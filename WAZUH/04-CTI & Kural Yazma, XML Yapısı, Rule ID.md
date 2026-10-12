# CTI KURLARI YAZMA, XML YAPISI VE RULE ID

Wazuh'un kalbi sayılan **Kurallar (Rules)**, tespit motorunun gelen logları analiz edip bir uyarı (alert) üretip üretmeyeceğine karar verdiği mantıksal yapılardır.

---

## 1. Rule ID Mantığı (Kural Kimliği ve Arama Aralığı)

Wazuh'ta yazılan her kuralın unique/eşsiz bir kimlik numarası **(Rule ID)** olmak zorundadır. Wazuh sistemdeki karışıklığı önlemek için bu ID'leri belirli aralıklara bölmüştür:

* **0 - 99.999 arası (Varsayılan / Sistem Kuralları):** Wazuh ile hazır gelen, varsayılan sistem ve güvenlik kurallarıdır. Bu kuralların dosyaları doğrudan değiştirilmez/düzenlenmez.
* **100.000+ (Özel / User Kuralları):** Bizim (analistlerin/yöneticilerin) yazdığı tüm **özel (custom) kurallar** 100.000 ve üzeri bir ID almak zorundadır (Örn: `100001`, `100002`).

> **ÖNEMLİ:** Kendi yazdığınız veya düzenlediğiniz kurallar her zaman `/var/ossec/etc/rules/local_rules.xml` dosyasına yazılır ve ID'si `100000`'den başlar.

---

## 2. Wazuh Kuralının XML Yapısı

Wazuh kuralları standart XML formatında yazılır:
 
xml
<group name="syslog,sshd,">
  <rule id="100001" level="10">
    <if_sid>5716</if_sid>
    <match>Failed password</match>
    <description>Bir kullanıcı için üste üste hatalı SSH girişi tespit edildi!</description>
    <mitre>
      <id>T1110</id> 
    </mitre>
  </rule>
</group>

Temel Etiketlerin (Tag) Anlamları:
<group>: Kuralları kategorize eder (örn: windows, syslog, firewall, web).
<rule id="..." level="...">:
      id: Kuralın benzersiz numarası.
      level: Tehditlerin dereceleri (0: Bilgilendirme/Önemsiz, 1-3: Düşük seviye, 4-6: Orta seviye/Medium, 7-9: Yüksek seviye/High, 10-12: Çok yüksek seviye/Very high, 13-15: Kritik seviye/Critical).
<if_sid> (If Same ID / Bağımlılık): Kuralın tetiklenmesi için önce başka bir kuralın çalışmış olması gerektiğini belirtir (Kural hiyerarşisi, korelasyon sağlar).
<match> veya <regex>: Log metninin içinde hangi kelimenin veya kalıbın aranacağını belirler.
<description>: Kural tetiklendiğinde Wazuh Dashboard ekranında yazacak olan açıklama metnidir.
<mitre>: Kuralın hangi MITRE ATT&CK tekniğine (Örn: T1110 - Brute Force) karşılık geldiğini etiketler.


## 1.Hazır Sistem Kuralları Örnekleri (ID: 0 – 99.999)

5710 | SSH (Linux) | Level: 5 | Açıklama: SSH bağlantı denemesi başarısız oldu (Failed SSH login). Yanlış parola girildiğinde tetiklenir.

5712 | SSH (Linux) | Level: 10 | Açıklama: SSHD Brute Force Saldırısı. Üst üste birden fazla başarısız SSH girişi yapıldığında tetiklenir.

60106 | Windows Event | Level: 3 | Açıklama: Kullanıcı oturum açtı (Event ID 4624). Başarılı bir Windows oturum açma olayıdır.

60107 | Windows Event | Level: 5 | Açıklama: Hatalı parola ile oturum açma denemesi (Event ID 4625). Windows'ta başarısız giriş denemesidir.

60115 | Windows Event | Level: 10 | Açıklama: Kullanıcı hesabı kilitlendi (Event ID 4740). Çok fazla hatalı deneme sonucu hesabın kilitlendiğini bildirir.

550 | FIM (Dosya İzleme) | Level: 7 | Açıklama: İzlenen bir dosyanın içeriği değişti. Kritik bir dosyanın (/etc/passwd vb.) değiştirildiğini haber verir.

31101 | Web Server | Level: 5 | Açıklama: HTTP 404 - Sayfa Bulunamadı. Web sunucusunda olmayan bir sayfaya istek atıldığını gösterir.

## 2. Özel (Custom) Kural ID Örnekleri (ID: 100.000+)

100001 | SSH / Güvenlik | Level: 8 | Açıklama: Yüksek Öncelikli SSH Başarısız Giriş: Varsayılan 5710 kuralının önem seviyesi yükseltilerek kritik sunuculara yapılan başarısız SSH denemeleri için alarm üretilir.

100002 | Ransomware / FIM | Level: 14 | Açıklama: Yem (Canary) Dosya Değişikliği: Sahte finans dosyası (000_FINANS_2026.xlsx) değiştirildiği an fidye yazılımı şüphesiyle kritik seviye alarm tetiklenir.

100003 | Windows / Hesap | Level: 12 | Açıklama: Kritik Yedek Hesabının Silinmesi: Windows üzerinde admin_backup isimli kritik yönetici hesabı silindiğinde güvenlik ekibine bildirim düşer.

100004 | Web / Güvenlik | Level: 10 | Açıklama: Web Sunucusuna SQL Injection Denemesi: Web loglarında UNION SELECT veya ' OR '1'='1 kalıpları tespit edildiğinde veritabanı saldırısı uyarısı verilir.

100005 | Sistem / Yetki | Level: 11 | Açıklama: Mesai Saatleri Dışında sudo Kullanımı: Gece saatlerinde (00:00 - 06:00) bir kullanıcının root yetkisi alması durumunda alarm üretilir.
