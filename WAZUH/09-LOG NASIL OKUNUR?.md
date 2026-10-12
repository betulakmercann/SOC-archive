# LOG OKUMAK

Wazuh veya herhangi bir SIEM/Linux ortamında Log Okuma (Log Analysis), ham veri halinde gelen karışık metin dizilerini anlamlı bir güvenlik olayına (incident) dönüştürme sürecidir.

Bir SIEM analisti için log okuma işlemi 4 temel aşamadan oluşur: **Yapıyı Anlama**, **Decoder (Çözümleme)**, **Kural Eşleştirme** ve **Süzme/Analiz**.

---

## 1. Bir Logun Anatomisi

Her sistem (Windows, Linux, Apache, Firewall) logları farklı formatta üretir. Ancak standart bir log satırı her zaman şu 4 temel soruya cevap verir:

> **Ne Zaman? + Nerede? + Kim? + Ne Yaptı?**

### Örnek Linux SSH Logu (Raw / Ham Log)
Oct 08 18:45:12 ubuntu-server 
sshd[2451]: Failed password for invalid user admin from 192.168.1.100 
port 51234 ssh2

Bu logu parçalarına ayıralım (Parsing):

Oct 08 18:45:12: Timestamp (Zaman).

ubuntu-server: Hostname (Kaynak cihaz).

sshd[2451]: Process ID - PID (Olayı üreten servis ve işlem kimliği).

Failed password: Action (Eylem).

invalid user admin: Target User (Hedef kimliği).

from 192.168.1.100: Source IP - srcip (Kaynak IP).

port 51234 ssh2: Port ve Protocol (Bağlantı detayları).

## 2. Log Okuma Süreci Nasıl İşler? (Wazuh Mimarisi)

A. Phase 1: Pre-decoding (Ön Çözümleme)
Logun genel bilgileri ayrıştırılır:
program_name: sshd
timestamp: Oct 08 18:45:12
hostname: ubuntu-server

B. Phase 2: Decoding (Alan Ayrıştırma - Extracted Fields)
Decoder regex kullanarak metin içindeki verileri değişkenlere atar:
srcip: 192.168.1.100
srcport: 51234
dstuser: admin

C. Phase 3: Rule Matching (Kural Eşleştirme)
Elde edilen veriler XML kuralları ile taranır:
srcip var mı? Evet (192.168.1.100).
Metinde Failed password geçiyor mu? Evet.
Sonuç: Rule ID: 5710 (Level 5 - SSH Failed Login) tetiklenir.

## 3. Linux Terminalinde Manuel Log Okuma Teknikleri

A. Canlı Akışı İzlemek (tail)
Dosyaya yeni yazılan logları anlık takip etmek için: sudo tail -f /var/log/auth.log

B. Belirli Bir İfadeyi Süzmek (grep)
Sadece başarısız girişleri veya spesifik bir IP'yi aramak için: sudo grep "Failed password" /var/log/auth.log

C. Sık Tekrar Eden Logları Saymak (uniq -c / sort)
Hangi IP adresinin kaç kez saldırdığını sıralı görmek için: sudo grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr

Çıktı Örneği: 145 192.168.1.100 (Bu IP'den 145 kez başarısız deneme yapılmış).

## 4. Windows Event Logları Nasıl Okunur?

Windows logları metin yerine XML / Event ID formatında tutulur:

Event ID 4624: Başarılı Oturum Açma (Successful Logon).

Event ID 4625: Başarısız Oturum Açma (Failed Logon).

Event ID 4672: Yönetici (Admin) Haklarının Kullanılması.

Event ID 4720: Yeni Kullanıcı Oluşturuldu.

Windows logunu okurken kritik alan Logon Type alanıdır:

Logon Type 2: Fiziksel klavyeden giriş.

Logon Type 3: Ağ üzerinden erişim (Ağ paylaşımı vb.).

Logon Type 10: Remote Desktop (RDP) üzerinden giriş.


## Windows Log Örneği ve Çözümleme
XML
<Event xmlns="[http://schemas.microsoft.com/win/2004/08/events/event](http://schemas.microsoft.com/win/2004/08/events/event)">
  <System>
    <Provider Guid="{54849a20-b447-4b1d-4e01-000000000000}" Name="Microsoft-Windows-Security-Auditing"/>
    <EventID>4625</EventID>
    <Version>0</Version>
    <Level>0</Level>
    <Task>12544</Task>
    <Opcode>0</Opcode>
    <Keywords>0x8010000000000000</Keywords>
    <TimeCreated SystemTime="2026-10-09T03:15:22.1234567Z"/>
    <EventRecordID>98452</EventRecordID>
    <Correlation/>
    <Execution ProcessID="688" ThreadID="1204"/>
    <Channel>Security</Channel>
    <Computer>WIN-SERVER2022.corp.local</Computer>
    <Security/>
  </System>
  <EventData>
    <Data Name="SubjectUserSid">S-1-0-0</Data>
    <Data Name="SubjectUserName">-</Data>
    <Data Name="SubjectDomainName">-</Data>
    <Data Name="TargetUserSid">S-1-0-0</Data>
    <Data Name="TargetUserName">Administrator</Data>
    <Data Name="TargetDomainName">WIN-SERVER2022</Data>
    <Data Name="Status">0xc000006d</Data>
    <Data Name="SubStatus">0xc000006a</Data>
    <Data Name="LogonType">10</Data>
    <Data Name="LogonProcessName">User32</Data>
    <Data Name="AuthenticationPackageName">Negotiate</Data>
    <Data Name="WorkstationName">ATTACKER-PC</Data>
    <Data Name="TransmitCommittedBytes">0</Data>
    <Data Name="IpAddress">192.168.1.150</Data>
    <Data Name="IpPort">53214</Data>
  </EventData>
</Event>


## Bu Logu Bir Analist Gibi Okuyalım:

Wazuh bu XML verisini aldığında içeriğindeki <Data> etiketlerini ayrıştırır:

EventID = 4625: Bir oturum açma denemesinin başarısız (Failed Logon) olduğunu gösterir.

TargetUserName = Administrator: Saldırganın girmeye çalıştığı kullanıcı adı.

LogonType = 10: Girişin Remote Desktop (RDP) üzerinden yapıldığını doğrular.

IpAddress = 192.168.1.150: Saldırıyı yapan kaynağın IP adresi (srcip).

SubStatus = 0xc000006a: Windows hata kodudur. Anlamı: "Kullanıcı adı doğru ancak parola yanlış (Unknown user name or bad password)".

## Bu Logun wazuh-logtest Çıktısı:

Phase 1: Completed pre-decoding.
	full event: '...'
	decoder: 'windows-eventchannel'
	
Phase 2: Completed decoding.
	name: 'eventchannel'
	win.system.eventID: '4625'
	win.eventdata.targetUserName: 'Administrator'
	win.eventdata.logonType: '10'
	win.eventdata.ipAddress: '192.168.1.150'

Phase 3: Completed filtering (rules).
	id: '60107'
	level: '5'
	description: 'Windows: Failed attempt to log in'

## 5. wazuh-logtest ile Log Okuma Pratiği

Wazuh sunucunuzda bir logun sistem tarafından nasıl okunduğunu görmek için wazuh-logtest aracını çalıştırıp ham logu yapıştırabilirsiniz:

sudo /var/ossec/bin/wazuh-logtest

Test Logu Örneği:

Feb 21 15:30:10 server1 sshd[999]: Failed password for root from 10.0.0.5 port 22 ssh2

## Çıktı:

Phase 1: Completed pre-decoding.
	full event: 'Feb 21 15:30:10 server1 sshd[999]: Failed password for root from 10.0.0.5 port 22 ssh2'
	program_name: 'sshd'

Phase 2: Completed decoding.
	name: 'sshd'
	dstuser: 'root'
	srcip: '10.0.0.5'

Phase 3: Completed filtering (rules).
	id: '5710'
	level: '5'
	description: 'sshd: Attempt to login using non-existent user or failed password.'
