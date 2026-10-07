**SIEM (Security Information and Event Management)** farklı sistemlerden gelen güvenlik loglarını **tek bir yerde toplamak, anlamlandırmak, ilişkilendirmek ve şüpheli olaylar için alarm üretmek** amacıyla kullanılan güvenlik platformudur.

Normalde farklı makinelerde farklı loglar vardır örn:
Windows
 ├── Login logları
 ├── Process logları
 └── Security Events

Linux
 ├── SSH logları
 ├── Authentication
 └── System logs

Network
 ├── DNS
 ├── Firewall
 └── Network events
SIEM bunları merkezı bır noktada toplar.

Windows ──────┐
Linux ────────┤
Firewall ─────┤
Network ──────┤
               ▼
             SIEM
               │
       ┌───────┴───────┐
       ▼               ▼
    Events           Alerts
       │               │
       └───────┬───────┘
               ▼
        SOC Analyst


### SIEM'in temel görevleri

**1. Log toplama**
Farklı sistemlerden veri alır.

**2. Merkezi görüntüleme**
Her şeyi tek yerde görmeni sağlar.

**3. Korelasyon**
Farklı olayları birbiriyle ilişkilendirir.

**4. Alert / Alarm**
Belirli kurallara uyan olaylarda uyarı üretir.

**5. Investigation**
Analistin olayın detaylarını incelemesine yardımcı olur.

# Wazuh nedir?
**Wazuh**, açık kaynaklı bir security monitoring / SIEM ve XDR platformudur, sistemlerden güvenlik verisi toplayıp bunları analiz etmemize ve şüpheli aktiviteleri tespit etmemize yardımcı olur.

WAZUHUN BOLUMLERI VE BAZI KAVRAMLAR 
==1-WAZUH AGENT==
İzlenmek istenen bilgisayarlara küçük bir yazılım ajanı kurulur.
Bu ajan işletim sistemi, uygulamalar ve ağ hareketlerinin loglarını toplayıp ana sunucuya gönderir.
Kritik dosyaları  takibe alır. Bu dosyalarda kim, ne zaman, hangi değişikliği yaptı veya dosyayı sildi mi anında wazuh servera raporlar. 
SCA Güvenlik Analizi  Bilgisayarların güvenli kurulup kurulmadığını denetler. Örneğin "Gereksiz bir port açık kalmış" diye uyarır, raporlar ve yine servera gönderir
Aktif Yanıt Bir tehdit anında wazuhdan gelen bildiri üzerine  otomatik olarak komut çalıştırır

==2-WAZUH SERVER==
Tüm ajanlar buraya bağlıdır
Şirketin merkezi veya bulutta çalışır
Görünütleme dashboard tüm grafikleri analizleri gösterir
Ajanlardan gelen logları hızlı şekilde analiz eder anlamlaştırır ve kurallarla eşleştirir bir saldırı vs olduğunda kurallara bakarak anlar
Zararlı durum oldugunda alert verir
Ajanın gönderdiği dosya özetlerini siber tehdit istihbarat hazuvuzyla karşılaştırır ve dosyaya zafiyet varsa virüs teşhisi koyar

1-Log Dijital sistemlerde gerçekleşen her türlü hareketin tarih, saat ve kullanıcı bilgisiyle kaydedildiği dijital ayak izleridir. 
2-MITRE ID bize saldırganın hangi aşamada ne tür bir yöntem kullandığını gösterir. Saldırganın bir sonraki muhtemel hamlesini öngörmemizi ve doğru savunma stratejisini hızla uygulamamızı sağlar
3- True Positive  Sistemin ürettiği alarmın gerçek bir siber saldırı veya güvenlik ihlali olması durumudur. Saldırı anında sitem aktif response verir. L1 analisti true false tespiti yapar ve true ise l2 gönderir 
4-False Positive  Sistemin zararsız bir hareketi siber saldırı zannedip alarm üretmesidir.

==TRUE FALSE AYRIMI==
1- Alarmın kaynağı
Alarm üreten makine şirketin bir geliştirme (Dev) sunucusu mu, yoksa canlıda çalışan kritik (Production) bir sunucu mu? Dev sunucularında yazılımcılar test yaptığı için sıklıkla yanlış alarmlar tetiklenebilir.

2-IP Sorgulama
IP İncelemesi: Dış IP'yi VirusTotal, AbuseIPDB veya Shodan üzerinde aratırsın. Eğer IP bilinen bir zararlı botnet'e veya spam ağında kayıtlıysa, bu True Positive ihtimalini tavan yaptırır.
Hash / Dosya İncelemesi Çalıştırılan bir .exe veya script varsa hash'ini VirusTotal'de sorgulatırsın.

3-Kullanıcı detayları
Kim, zamanlama,şirket içi sorgulama

4-Saldırının başarı durumu
Örneğin 100 kere başarısız şifre denemesi yapılmış ama hepsi HTTP 401 dönmüşse ve içeri sızma yoksa tehdit seviyesi farklıdır; 101. denemede HTTP 200 (OK) alınıp içeri girilmişse bu kritik bir True Positivedir.


==ZAFIYETE GORE== 
Nmap, ddos - 3. Ve 4. Katman
Katman 3/4 seviyesinde bir tehdit varsa  IP'yi firewall seviyesinde bloklarsın.
Sql injection, XSS - 7.katman
Katman 7 seviyesinde bir tehdit varsa, kullanıcının Oturumunu sonlandırırsın


**WAZUH VE SIEM İLİSKİSİ**
SIEM bir kavram/kategori. Wazuh ise bu kategoride kullanılabilecek bir platform.

SIEM
│
├── Wazuh
├── Splunk
├── Microsoft Sentinel
├── IBM QRadar
└── Elastic Security

**Event Log:**
> "Ne oldu?"

**Sysmon:**
> "Ne oldu ve bunun teknik detayları ne?"

**SIEM:**
> "Farklı kaynaklardan gelen bu olayları merkezi olarak nasıl toplayıp ilişkilendiririz?"

**Wazuh:**
> "Bunu yapmamıza yardımcı olan açık kaynaklı platformlardan biri."
