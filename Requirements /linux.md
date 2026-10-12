# LINUX TEMELLERİ VE SİSTEM YÖNETİMİ

## 1. Linux File System (Dosya Sistemi Mimarisi)

Linux işletim sisteminde her şey bir dosyayla başlar ve tüm dizinler tek bir kök dizin (`/`) altında hiyerarşik olarak toplanır:

```text
/
├── home/       # Normal kullanıcıların kişisel dosyaları (betul, user vb.)
├── root/       # Root (en yetkili) kullanıcısının kişisel dizini
├── etc/        # Sistem yapılandırmaları (passwd, shadow, ssh vb.)
├── var/log/    # Sistem logları (auth.log, syslog vb.)
├── tmp/        # Geçici dosyalar için kullanılır
├── usr/        # Kullanıcı programları, kütüphaneler, çeşitli sistem bileşenleri
├── bin/        # Temel sistem komutları ve yürütülebilir dosyalar
├── sbin/       # Sistem yöneticisine özel temel komutlar ve ikililer
├── dev/        # Donanım ve cihazla ilgili özel dosyalar
└── opt/        # Opsiyonel üçüncü taraf yazılımlar için kullanılabilir
```

## 2. Users / Groups (Kullanıcılar ve Gruplar)

Root: Sistemdeki en yetkili kullanıcıdır. Kullanıcı oluşturabilir, dosya erişim yetkilerini değiştirebilir, sistem yapılandırmalarını düzenleyebilir, güvenlik kısıtlamalarını aşabilir ve servisleri yönetebilir.

Groups: Kullanıcıları mantıksal olarak gruplandırarak toplu yetkilendirme ve dosya paylaşım yönetimi yapılmasını sağlar.

## 3. Permissions (Dosya ve Dizin İzinleri)
Linux'ta dosya izinleri üç ana gruptan oluşur: User (Sahip), Group (Grup) ve Others (Diğerleri).

```text
-rwxr-xr--
rwx (user)  r-x (group)  r--(others)
```

## Temel İzin Harfleri:
```text
r (Read): Okuma
w (Write): Yazma / Değiştirme
x (Execute): Çalıştırma / Dizin içine girme
```

## 4. Processes (Süreçler / İşlemler)
Terminalden firefox çalıştırdığında sistemde çalışan bir process (süreç) oluşur. Her process'in benzersiz bir kimlik numarası vardır ve bu PID (Process ID) olarak adlandırılır.

```text
Örnekler:
PID 1234 → Firefox
PID 5678 → SSHD
PID 9012 → Nginx
```

## 5. Services (Servisler)
Arka planda çalışan ve belirli bir işi sürekli yapan sistem uygulamalarıdır. Servislerin durumunu kontrol etmek, başlatmak veya durdurmak için systemctl komutu kullanılır:
```text
systemctl status <servis_adi>
```

## 6. SSH (Secure Shell)

Başka bir bilgisayara ağ üzerinden güvenli bir şekilde bağlanıp komut çalıştırmamızı sağlayan protokoldür. 

Genellikle TCP 22 numaralı portu kullanır; iletişim tamamen şifrelidir ve kimlik doğrulama için parola veya SSH anahtarları (ssh-key) kullanılabilir.

## Bağlantı Örneği: ssh user@192.168.1.10

ssh: SSH bağlantısı başlatır.

user: Bağlanılacak kullanıcı adıdır.

192.168.1.10: Hedef makinenin IP adresidir.

## 7. Loglar

/var/log/ dizini altında Linux dağıtımına göre farklı log dosyaları bulunur. Log okumak için genellikle cat veya tail komutları kullanılır (tail dosyanın son kısımlarını gösterir).

/var/log/auth.log: Kimlik doğrulama ile ilgili olaylar (SSH login, başarısız login denemeleri, sudo kullanımı vb.).

/var/log/syslog: Çeşitli sistem olayları ve uygulama mesajları (Servis başlatma hataları, sistem hataları, ağ olayları vb.).

## 8. Bash Temel Komutlar ve Kullanımları
```text
pwd: Bulunduğun dizini tam yol olarak gösterir.
less: Büyük dosyaları sayfalar halinde, aşağı yukarı kaydırarak incelemeni sağlar.
head: Dosyanın başlangıç kısımlarını (ilk satırlarını) gösterir.
tail: Dosyanın son kısmını gösterir (Örn: tail -n 50 son 50 satırı gösterir).
tail -f: Log dosyalarındaki hareketleri canlı (real-time) olarak takip etmeni sağlar.
grep: Belirli bir kelimeyi veya kalıbı bir dosyada arar (Örn: grep "failed" /var/log/auth.log).
find: Sistemde dosya veya klasör aramak için kullanılır.
cp: Dosya veya dizin kopyalama işlemlerini yapar.
mv: Dosya taşıma veya yeniden adlandırma (rename) işlemlerini yapar.
mkdir: Yeni bir klasör (dizin) oluşturur.
touch: Yeni ve boş bir dosya oluşturur.
whoami: Şu anda terminalde hangi kullanıcı ile işlem yaptığını gösterir.
id: Kullanıcının UID, GID ve dahil olduğu grup bilgilerini listeler.
ps: O an çalışan process'leri listeler.
ps aux: Sistemdeki tüm process'leri ayrıntılı ve geniş formatta gösterir.
ss -tuln: Aktif ağ bağlantılarını ve dinlenen (listening) portları listeler.
systemctl: Servisleri kontrol eder (Örn: systemctl status ssh).
sudo: Bir komutu en yetkili kullanıcı (root) haklarıyla çalıştırmanı sağlar (Örn: sudo systemctl status ssh).
```
