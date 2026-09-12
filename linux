**Lİnux File System**
/
├── home/betul normal kullanıcıların kisisel dosyaları (betul, user vb.)
├── root/ root kullanıcısının kisisel dizini
├── etc/ sistem yapılandırmaları (passwd, shadow,ssh vb.)
├── var/log sıstem logları burda olabılır (auth.log, syslog vb.)
├── tmp/ gecici dosyalar icin kullanılır 
├── usr/ kullanıcı programları, kutuphaneler, cesitli sistem bilesenleri
├── bin/ temel sistem komutları bulunur
├── sbin/ temel sistem komutları bulunur
├── dev/ donanım ve cıhazla ılgılı ozel dosyalar
└── opt/ opsıyonel ucuncu taraf yazılımlar ııcn kullanılabılır

**Users/Groups**
root : en yetkılı kullanıcı, kullanıcı olusturabılır,dosya erısımı,sıstem yapılandırması degıstırme, guvenlık kısıtlamalrını asabılır, servıslerı degıstırme
groups : kullanıcıları gruplandırarak yetkılendırme vs.


**Permissions**
-rwxr-xr--
rwx (user) r-x (group) r--(others)
r: okuma w:yazma x:çalıştırma

**Processes**
termınalden fırefox calıstırdın, calısırken sıstemde bir process vardır ve her processin bir PIDsi (process id) vardır. 
örn:
PID 1234 firefox 
PID 5678 sshd
PID 9012 nginx

**Services**
arkaplanda calısan ve belırlı bı ısı sureklı yapan sıstem uygulaması, 
systemctl status __ (durumunu gormek ıcın)

**SSH (secure shell)**
baska pcye ag uzerınden guvenlı sekılde baglanıp komut calıstırmamızı saglayan protokol, genellikle tcp 22 kullanılır, iletisim sifreli, authentication icin password veya ssh key kullanılabılır

- `ssh` → SSH bağlantısı başlat
- `user` → bağlanacağın kullanıcı
- `192.168.1.10` → hedef makinenin IP adresi

**Loglar**
/var/log/ burada dagıtıma gore farklı log dosyaları bulunabılır
log okumak icin:  cat veya tail  /var/log/auth.log komutu kullanılır
tail dosyanın son kısmını gosterır

orn:
/var/log/auth.log : Authentication ıle ılgılı olaylar (ssh login, basarısız login, sudo kullanımı)
/var/log/syslog : cesıtlı olaylar ve uyg mesajları (service started, system error, network event)

**Bash Temel Komutlar** 
pwd bulundugun dızını gosterır
less buyuk dosyaları sayfa sayfa ıncelemek
head dosya baslangıcını gosterır
tail dosya sonunu gosterır (tail -n 50 son 50 satırı gosterır)
tail -f canlı olarak log takıbı
grep "failed" belırlı kelımeyı arar bı dosyada (grep "failed" /var/log/auth.log)
find dosya-klasor aramak ıcın kullanılır
cp kopyalama 
mv tasıma-yenıden adlandırma
mkdır yenı klasor olusturma
touch yenı bos dosya olusturma
whoami suanda hangı kullanıcı calıstıgını gorme
id kullanıcının UID,GID ve grup bılgılerını gosterır
ps calısan processlerı gosterır
ps aux detaylı gosterım
ss -tuln network baglantılarını ve dınlenen portları gosterır
systemctl servıslerı kontrol eder (systemctl status ssh)
sudo bı komutu daha yuksek yetkıyle calıstırmak (sudo systemctl status ssh)








