# **Lsass Dump ile Parola Elde Edilmesi**
Windows işletim sistemlerinde kullanıcı oturum açma işlemlerini, parola doğrulamalarını ve güvenlik belirteçlerini (tokens) yöneten kritik bir sistem sürecidir (`lsass.exe`).

Belleğinde (RAM) aktif oturum açmış kullanıcıların NTLM hash'lerini, düz metin parolalarını ve Kerberos biletlerini tutar.

**Saldırı Türü (Dump İşlemi):** Saldırganlar veya sızma testi uzmanları, sistemde yüksek yetki (örneğin `Administrator`veya `SYSTEM`) elde ettikten sonra LSASS sürecinin belleğini bir dosyaya kopyalarlar (dump ederler). Bu işlem için `Procdump`, Görev Yöneticisi (Task Manager), `comsvcs.dll` gibi yerleşik araçlar veya özel komutlar kullanılabilir.

 Elde edilen döküm dosyası (`lsass.dmp`), saldırganın kendi bilgisayarına aktarılarak Mimikatz gibi araçlarla çevrimdışı (offline) olarak analiz edilir ve sistemdeki kullanıcıların parola veya hash değerleri açığa çıkarılır.

- **Korunma Yöntemleri:**
    - **LSASS PPL Koruması:** `RunAsPPL` (Protected Process Light) etkinleştirilerek LSASS sürecinin belleğine dışarıdan (imzasız veya düşük yetkili süreçler tarafından) erişim engellenmelidir.
    
    - **Credential Guard:** Windows Defender Credential Guard aktif edilerek kimlik bilgilerinin sanallaştırma tabanlı güvenli bir alanda izole edilmesi sağlanmalıdır.
    
    - **EDR / Güvenlik Çözümleri:** Bellek okuma girişimlerini (özellikle LSASS sürecine yönelik API çağrılarını) anında tespit edip bloklayan modern EDR (Endpoint Detection and Response) sistemleri kullanılmalıdır.
