# SYSMON KURULUM
- **Sysmon Executable Dosyasını İndirin:**
    - Microsoft Sysinternals resmi sitesinden en güncel **Sysmon** paketini indirin (`Sysmon.zip`) ve sunucuda/istemcide güvenli bir dizine (örn: `C:\Tools\Sysmon`) çıkartın.
- **Kapsamlı Bir Yapılandırma (Config) Dosyası Edinin:**
    - Sysmon, varsayılan ayarlarla kurulursa çok fazla gürültü (gürültülü loglar) üretebilir. Bu yüzden endüstri standartlarında filtrelenmiş bir config dosyası kullanmalısınız (örneğin SwiftOnSecurity veya Neo23x0 Olaf Hartong Sysmon config). Config dosyasını `sysmonconfig.xml` adıyla aynı dizine kaydedin.
- **Sysmon'u Kurulum Komutu ile Çalıştırın:**
    - Komut İstemi'ni (Command Prompt) **Yönetici olarak (Run as Administrator)** açın, Sysmon klasörüne gidin ve şu komutu çalıştırarak yükleyin:

        ```
        Sysmon64.exe -i sysmonconfig.xml
        ```
        
    - _Verification:_ Kurulum tamamlandığında ekranda "Sysmon installed" mesajı görümeli ve `services.msc` üzerinden **Sysmon64** servisinin _Running_  olduğu doğrulanmalıdır.
