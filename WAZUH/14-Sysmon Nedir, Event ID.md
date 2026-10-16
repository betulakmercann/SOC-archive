# Sysmon Nedir
**Sysmon (System Monitor)**, Microsoft’s Sysinternals paketinde yer alan ve Windows işletim sistemlerinde gerçekleşen detaylı sistem aktivitelerini izleyip Windows Event Log'larına (Olay Günlükleri) kaydeden son derece güçlü, ücretsiz bir güvenlik aracıdır.

### Sysmon Neden Bu Kadar Önemlidir?

Windows'un varsayılan olay günlükleri (Event Viewer) genellikle bir saldırıyı veya şüpheli davranışı derinlemesine anlamak için yetersiz kalır. Sysmon ise arka planda bir Windows Servisi ve Aygıt Sürücüsü (Device Driver) olarak çalışarak sistemdeki en ufak hareketleri bile detaylıca yakalar:

- **Süreç Oluşturma (Process Creation):** Çalıştırılan her programı, komut satırı parametreleriyle 
(`CommandLine`) ve dosya özetleriyle (MD5/SHA256 hash) kaydeder.
    
- **Ağ Bağlantıları (Network Connections):** Hangi sürecin, hangi IP ve porta bağlandığını tespit eder.
    
- **Bellek ve Enjeksiyon Hareketleri:** Süreçlerin birbirinin belleğine erişmesini veya kod enjekte etmesini yakalar.
    
- **Kayıt Defteri (Registry) ve Dosya Değişiklikleri:** Zararlı yazılımların kalıcılık (persistence) sağlama girişimlerini izler.
    

### En Kritik Sysmon Event ID'leri (Olay Kimlikleri)

Sysmon, yakaladığı olayları özel kategorilere ayırır. Bir güvenlik analistinin bilmesi gereken en temel Olay Kimlikleri şunlardır:

|Event ID|Olay Adı|Ne İşe Yarar? / Kullanım Alanı|
|---|---|---|
|**Event ID 1**|Process Creation|Yeni bir süreç (exe) başladığında tetiklenir. Çalıştırılan komut satırı ve parent process bilgisini verir.|
|**Event ID 3**|Network Connection|Sistemdeki bir uygulamanın dışarıya veya içeriye yaptığı ağ bağlantılarını kaydeder.|
|**Event ID 7**|Image Loaded|Bir sürecin içine yüklenen DLL dosyalarını takip eder (DLL Hijacking tespiti için).|
|**Event ID 8**|CreateRemoteThread|Bir sürecin başka bir sürece kod enjekte etmesini (Process Injection) yakalar.|
|**Event ID 10**|ProcessAccess|Bir sürecin başka bir sürecin belleğine okuma/yazma erişimi istemesini yakalar. _(Örn: LSASS Dump tespiti)_|
|**Event ID 11**|FileCreate|Sistemde yeni bir dosya oluşturulduğunda (özellikle `.exe`, `.dll`, `.bat` gibi) kaydeder.|
|**Event ID 12 / 13 / 14**|Registry Events|Windows Kayıt Defteri'ndeki ekleme, silme ve değiştirme işlemlerini izler.|
|**Event ID 22**|DNSEvent|Uygulamaların yaptığı DNS sorgularını kaydeder (C2 / Komuta Kontrol sunucu tespitinde kritik).|

### Sysmon Yapılandırması (Configuration Files)

Sysmon varsayılan olarak her şeyi loglamaya çalışırsa çok fazla gürültü (false positive) ve log kirliliği yaratabilir. Bu yüzden topluluk tarafından kabul görmüş **XML yapılandırma dosyaları (rules/config)** ile filtreleme yapılarak kurulur.

- **SwiftOnSecurity Config:** En popüler ve genel kullanıma uygun hazır Sysmon konfigürasyonlarından biridir
- **Olaf Hartong (sysmon-modular):** Modüler yapıda, MITRE ATT&CK matrisi ile eşleştirilmiş oldukça kapsamlı bir kural setidir.

