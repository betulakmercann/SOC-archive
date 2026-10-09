SANAL MAKINA, UBUNTU VE WAZUH HAKKINDA KISACA
Wazuh'un arama motoru (Indexer), ana yönetim merkezi (Manager) ve web paneli (Dashboard) arka planda Linux çekirdeğine ve servislerine bağımlı olarak çalışacak şekilde kodlanmıştır. Bu sistemler Windows veya Mac üzerinde native (yerel) olarak çalışmaz.

Kendi bilgisayarımız (Host) sadece arayüze bağlanmak için bir istemcidir (Client). Wazuh sunucusunu kurmak, yapılandırmak ve servisleri başlatmak için o sunucunun **kendi içindeki işletim sistemine (Ubuntu)** talimat vermemiz gerekir. Bu talimatlar da Linux terminali üzerinden verilir. Komutları kendi Windows/Mac bilgisayarımızın komut satırına yazsaydık, sistem bu komutları tanımazdı.

KURULUMDAN SONRA GEREKSİNİMLER 

1- SAAT VE TARİHİN GÜNCEL OLMASI
SIEM sistemlerinde zaman her şeydir. zaman kavramı (NTP) isteğe bağlı değil zorunludur.

örn-1: bir saldırı olduğunda wazuh agentlar logları sunucuya gönderir.  eğer sunucu ve istemcilerin saati farklıysa Wazuh olayların sırasını karıştıramaz yada yanlış eşleştirir. 
örn-2: güvenlik ihlali incelemesinde "hangi işlem önce oldu"  sorusunun cevabı zaman damgalarına bağlıdır. yanlış zaman dalgaları yasal süreçlerde geçersiz sayılabilir.
örn-3: wazuh bileşenleri (indexer,server,dashboard) ve agentlar kendi aralarında şifreli HTTPS/TLS haberleşmesi yapar. sistem saati kayarsa sertifikalar süresi dolmuş veya henüz geçerli değil olarak algılanır. 

2-STATİK İP TANIMLAMA
Bir SIEM sunucusuna **Statik (Sabit) IP** tanımlamak ağ mimarisinin temel kuralıdır.  

tanımlamazsak?
agent bağları kopması ve uğraştırıcı olması: sanal makina her yeni başladığında veya dhcp kiralama süresi dolduğunda ip adresi değişebilir. wazuh agentlar sunucuya ip adresiyle bağlıysa ip dğeiştiği anda tüm istemciler disconnected durumuna düşer. ayrıca her zaman farklı ip girilmek zorunda kalırız. 

zorunlu mu?
eğer ortamda bi yerel domain/dns sunucusu varsa ve wazuh agentlarına ip yerine domain/FQDN adresi tanımladıysak teknik olarak ip değişse bile dns güncellendiğinde iletişim devam edebilir. 

ANCAK sunucu rolleri için her zaman statik ip tanımlamak en kararlı yöntem.

nasıl tanımlanır?
1-terminal aç ve arabirim adını öğren : ip link
2-mevcut IP, gateway ve dns bilgilerini öğren : ip route
3-netplan içindeki dosyayı görüntüle : ls /etc/netplan/
4-çıkan dosyaya gir : sudo nano /etc/netplan/50-cloud-init.yaml (örnek isim verdim bunun yerine listedeki ismi gir)
5- statik ip yapılandır :
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

ens33: ip link komutunda çıkan ağ kartı adın olmalı.
via: gateway/modem ip adresi
addresses: 192.168.x.x/x atamak istediğin statik ip

6-CTRL + O kaydet ve enter ile onayla. CTRL + X ile çıkış
7-ayarları uygula : sudo netplan apply
8- doğrula : ip a (tanımladığın statik ip görünüyorsa başarılı)
9- sonrasında wazuh arayüzüne girmek için önce ip a ile kontrol ve çıkan ip adresini gir ve urlyi herhangi bir tarayıcıda kullanabilirsin

https://<ÇIKAN__IP_ADRESİ> 

örn:  ip a sonucunda 192.168.1.100 çıktı. tarayıcıda https://192.168.1.100 adresine git. güvenlik uyarısında gelişmiş seç ve siteye ilerle. 


3- GEREKLİ AĞ PORTLARI 
wazuh bileşenlerinin haberleşebilmesi için ubuntudaki ufw veya ağ güvenlik duvarınızda şu portların açık olması gerekir. ubuntu varsayılan olarak ufw güvenlik duvarını kullanır. istemcilerin sunucuya ulaşabilmesi için bu portların açık olması şarttır.

- **`1514` (UDP/TCP):** Agent'ların Wazuh Server'a log gönderdiği ana port.
- **`1515` (TCP):** Yeni Agent kayıt (enrollment) portu.
- **`55000` (TCP):** Wazuh REST API portu.
- **`443` (TCP):** Wazuh Dashboard (Web Arayüzü) erişimi

açmak için komutlar:
sudo ufw allow 1514/tcp
sudo ufw allow 1514/udp
sudo ufw allow 1515/tcp
sudo ufw allow 55000/tcp
sudo ufw allow 443/tcp
sudo ufw reload
sudo ufw status ile doğrula ve yukarıda izin verdiğimiz portlar allow listesinde varsa başarılı

4- VARSAYILAN ADMİN PAROLASINI DEĞİŞTİRMEK
Wazuh kurulumu bittiğinde varsayılan parolayı değiştirmek güvenlik açısından önemli

komut:
sudo /usr/share/wazuh-indexer/plugins/opensearch-security/tools/wazuh-passwords-tool.sh -u admin -p 'YeniGüçlüParola123!' 

burda parolada bazı şartlar istiyor: en az 8 karakter, bir tane büyük harf.daha sonra wazuh arayüzüne yeni oluşturduğuz parola ile girin. 
