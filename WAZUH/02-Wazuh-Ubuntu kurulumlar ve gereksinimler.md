# SANAL MAKİNE, UBUNTU VE WAZUH HAKKINDA KISACA

Wazuh'un arama motoru (**Indexer**), ana yönetim merkezi (**Manager**) and web paneli (**Dashboard**) arka planda Linux çekirdeğine ve servislerine bağımlı olarak çalışacak şekilde kodlanmıştır. Bu sistemler Windows veya Mac üzerinde native (yerel) olarak çalışmaz.

Kendi bilgisayarımız (**Host**) sadece arayüze bağlanmak için bir istemcidir (**Client**). Wazuh sunucusunu kurmak, yapılandırmak ve servisleri başlatmak için o sunucunun **kendi içindeki işletim sistemine (Ubuntu)** talimat vermemiz gerekir. Bu talimatlar da Linux terminali üzerinden verilir. Komutları kendi Windows/Mac bilgisayarımızın komut satırına yazsaydık, sistem bu komutları tanımazdı.

---

## KURULUMDAN SONRA GEREKSİNİMLER

### 1. SAAT VE TARİHİN GÜNCEL OLMASI
SIEM sistemlerinde zaman her şeydir. Zaman kavramı (**NTP**) isteğe bağlı değil, zorunludur.

* **Örnek 1:** Bir saldırı olduğunda Wazuh Agent'lar logları sunucuya gönderir. Eğer sunucu ve istemcilerin saati farklıysa Wazuh olayların sırasını karıştıramaz veya yanlış eşleştirir.
* **Örnek 2:** Güvenlik ihlali incelemesinde *"Hangi işlem önce oldu?"* sorusunun cevabı zaman damgalarına bağlıdır. Yanlış zaman damgaları yasal süreçlerde geçersiz sayılabilir.
* **Örnek 3:** Wazuh bileşenleri (Indexer, Server, Dashboard) ve Agent'lar kendi aralarında şifreli **HTTPS/TLS** haberleşmesi yapar. Sistem saati kayarsa sertifikalar süresi dolmuş veya henüz geçerli değil olarak algılanır.

---

### 2. STATİK IP TANIMLAMA
Bir SIEM sunucusuna **Statik (Sabit) IP** tanımlamak ağ mimarisinin temel kuralıdır.

#### Tanımlamazsak Ne Olur?
Agent bağlantılarının kopması ve uğraştırıcı olması durumu yaşanır. Sanal makine her yeni başladığında veya DHCP kiralama süresi dolduğunda IP adresi değişebilir. Wazuh Agent'lar sunucuya IP adresiyle bağlıysa IP değiştiği anda tüm istemciler **Disconnected** durumuna düşer. Ayrıca her zaman farklı IP girilmek zorunda kalınır.

#### Zorunlu mu?
Eğer ortamda bir yerel Domain/DNS sunucusu varsa ve Wazuh Agent'larına IP yerine Domain/FQDN adresi tanımladıysak, teknik olarak IP değişse bile DNS güncellendiğinde iletişim devam edebilir.

> **Not:** Sunucu rolleri için her zaman statik IP tanımlamak en kararlı yöntemdir.

#### Adım Adım Statik IP Yapılandırması:
Terminal aç ve arabirim adını öğren: ip link
Mevcut IP, Gateway ve DNS bilgilerini öğren: ip route
Netplan içindeki dosyayı görüntüle: ls /etc/netplan/
Çıkan dosyaya gir: sudo nano /etc/netplan/50-cloud-init.yaml (Örnek isim verilmiştir, bunun yerine ls çıktısındaki dosya ismini girin)

Statik IP yapılandırmasını ekleyin: 
network:
  version: 2
  renderer: networkd
  ethernets:
    ens33:                  # 'ip link' komutundaki ağ kartı adınız
      dhcp4: no
      addresses:
        - 192.168.1.100/24  # Atamak istediğiniz Statik IP
      routes:
        - to: default
          via: 192.168.1.1  # Gateway / Modem IP adresi
      nameservers:
        addresses:
          - 8.8.8.8         # DNS adresleri
          - 1.1.1.1

ens33: ip link komutunda çıkan ağ kartı adınız olmalıdır.
via: Gateway / Modem IP adresiniz.
addresses: Atamak istediğiniz Statik IP adresi (192.168.x.x/x).

Kaydetme ve Çıkış: CTRL + O basıp ENTER ile kaydedin. CTRL + X ile çıkış yapın. 
Ayarları uygulayın: sudo netplan apply doğrula: ip a (Tanımladığınız Statik IP görünüyorsa işlem başarılıdır)

**Wazuh Arayüzüne Bağlanma**
Sonrasında Wazuh arayüzüne girmek için önce ip a ile kontrol edin ve çıkan IP adresini tarayıcınızda açın:

https://<ÇIKAN_IP_ADRESİ>

Örnek: ip a sonucunda 192.168.1.100 çıktıysa tarayıcıda https://192.168.1.100 adresine gidin. Güvenlik uyarısında Gelişmiş seçeneğini seçip siteye ilerleyin.

### 3. GEREKLİ AĞ PORTLARI
Wazuh bileşenlerinin haberleşebilmesi için Ubuntu'daki ufw veya ağ güvenlik duvarınızda şu portların açık olması gerekir.Ubuntu varsayılan olarak ufw güvenlik duvarını kullanır. İstemcilerin sunucuya ulaşabilmesi için bu portların açık olması şarttır.

1514 (UDP/TCP): Agent'ların Wazuh Server'a log gönderdiği ana port.
1515 (TCP): Yeni Agent kayıt (Enrollment) portu.
55000 (TCP): Wazuh REST API portu.
443 (TCP): Wazuh Dashboard (Web Arayüzü) erişim portu.

**Portları Açmak İçin Komutlar**:

sudo ufw allow 1514/tcp

sudo ufw allow 1514/udp

sudo ufw allow 1515/tcp

sudo ufw allow 55000/tcp

sudo ufw allow 443/tcp

sudo ufw reload

**Durum Doğrulama**:
sudo ufw status (Yukarıda izin verdiğimiz portlar ALLOW listesinde görünüyorsa işlem başarılıdır)

### 4. VARSAYILAN ADMİN PAROLASINI DEĞİŞTİRMEK
Wazuh kurulumu bittiğinde varsayılan parolayı değiştirmek güvenlik açısından önemlidir.

**Parola Değiştirme Komutu**:
sudo /usr/share/wazuh-indexer/plugins/opensearch-security/tools/wazuh-passwords-tool.sh -u admin -p 'YeniGüçlüParola123!'

**Parola Kuralları**: Parola en az 8 karakter uzunluğunda olmalı ve en az bir büyük harf barındırmalıdır. Daha sonra Wazuh arayüzüne yeni oluşturduğunuz parola ile giriş yapabilirsiniz.


