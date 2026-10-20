# APPLOCKER VE DİKKAT EDİLMESİ GEREKENLER

**AppLocker**, Windows işletim sistemlerinde yöneticilerin hangi uygulamaların, betiklerin ve kurulum dosyalarının çalıştırılabileceğini merkezi olarak denetlemesine olanak tanıyan güçlü bir uygulama kontrol  mekanizmasıdır. 

Yanlış yapılandırıldığında sistemin kilitlenmesine (kritik servislerin çalışmaması vb.) yol açabileceği için kuralları tanımlarken ve uygulatırken belirli mühendislik disiplinlerine dikkat etmek gerekir.

### AppLocker Kuralları Oluştururken Dikkat Edilmesi Gereken Kritik Detaylar

1. **Önce "Audit Only" (Yalnızca Denetim) Modu ile Başlayın:**
    
    - Yeni bir AppLocker ilkesi devreye alırken asla doğrudan **Enforce rules** (Kuralları uygula) moduyla başlamayın. İlk aşamada kuralları `Audit Only` modunda çalıştırarak Event Viewer (Olay Görüntüleyicisi) üzerinden hangi uygulamaların engelleneceğini izleyin. Bu, kullanıcıların iş akışını bozan gizli bağımlılıkları ve meşru yazılımları önceden görmenizi sağlar.
        
2. **Varsayılan Kuralların (Default Rules) Önemini Unutmayın:**
    
    - AppLocker yapılandırırken sihirbazın sunduğu varsayılan kuralları mutlaka oluşturun. Bu kurallar, yerleşik Windows dizinlerindeki (örn. `C:\Windows`, `C:\Program Files`) sistem dosyalarının ve güvenilir yöneticilerin çalışmasını garanti altına alır. Varsayılan kuralları atlamak, işletim sisteminin kendisinin çalışmasını engelleyebilir.
        
3. **Doğru Kural Koleksiyonunu Seçin:** AppLocker beş ana kural kategorisine ayrılır. İhtiyacınıza göre doğru koleksiyonu hedeflemelisiniz:
    
    - **Executable Rules:** `.exe` ve `.com` uzantılı çalıştırılabilir dosyalar.

    - **Script Rules:** PowerShell (`.ps1`), VBScript (`.vbs`), Batch (`.bat`), JS dosyaları.

    - **Windows Installer Rules:** `.msi`, `.msp`, `.mst` kurulum dosyaları.

    - **DLL Rules:** Dinamik bağlantı kütüphaneleri (Performans maliyeti yüksek olduğu için dikkatli kullanılmalıdır).

    - **Packaged app / Appx Rules:** Windows Mağazası (Store) uygulamaları ve modern evrensel uygulamalar.

4. **Güvenilir Kural Koşulları (Rule Conditions) Kullanımı:** Dosyaları tanımlarken üç farklı koşul türü kullanabilirsiniz. Güvenilirlik sırasına göre tercih edilmelidir:
    
    - **Publisher (Yayıncı) Koşulları:** En güvenilir yöntemdir. Dijital sertifikaya (Publisher name, Product name, Version) dayandığı için uygulama güncellemelerinde kuralların bozulmasını engeller. "At least" (en azından bu sürüm) veya "Exact" (tam sürüm) seçimlerine dikkat edilmelidir.

    - **Path (Yol) Koşulları:** Klasör yollarına (`C:\Program Files\...`) dayalıdır. En zayıf yöntemdir çünkü kullanıcıların yazma izni olan dizinlere (örn. `C:\Temp` veya `AppData`) zararlı yazılımlar aynı isimle kopyalanarak bu kurallar bypass edilebilir. Yol koşulu kullanılacaksa, dizin izinlerinin sıkı denetlendiğinden emin olunmalıdır.

    - **File Hash (Dosya Hash) Koşulları:** Dosyanın SHA256 hash değerine bakar. Çok güvenlidir ancak dosya her güncellendiğinde (yama aldığında) hash değeri değişeceği için kuralın da güncellenmesi gerekir; bu da büyük ortamlarda yönetimi zorlaştırır.

5. **"Allow" (İzin Ver) ve "Deny" (Engelle) Mantığı:**
    - AppLocker varsayılan olarak bir kural eşleşmezse **Implicit Deny** (Örtülü Engelleme) mantığıyla çalışır; yani açıkça izin verilmeyen her şey engellenir. Bu yüzden "Deny" kuralları yerine, sadece güvenilen yayıncılara veya yollara "Allow" kuralları yazmak en temiz ve sürdürülebilir yaklaşımdır.
    
6. **Uygulama Kimliği (Application Identity) Servisi:**
    
    - AppLocker kurallarının çalışabilmesi için hedef makinelerde **Application Identity** (`AppIDSvc`) servisinin çalışır durumda ve başlangıç türünün _Automatic_ (Otomatik) olarak ayarlanmış olması şarttır. Bu servis kapalıysa AppLocker kuralları işlev görmez.
        
7. **İstisnalar (Exceptions) Tanımlama:**
    
    - Geniş kapsamlı kurallarda (örneğin tüm Program Files dizinine izin verirken belirli bir alt klasördeki zararlı/istenmeyen aracı hariç tutmak gibi) **Exception** parametrelerini aktif olarak kullanın. Bu, kural karmaşasını önler.
### AppLocker Bypass Teknikleri ve "Path Condition" Tuzakları

AppLocker kullanırken yapılan en büyük hata, **Path (Yol) tabanlı kurallar** oluştururken kullanıcıların yazma yetkisi olan Windows dizinlerini gözden kaçırmaktır.

- **Problem:** Saldırganlar veya yetkisiz kullanıcılar, `C:\Windows\Tasks`, `C:\Windows\Temp` veya `C:\Windows\Tracing` gibi dizinlere yazma hakkına sahiptir.

- **Önlem:** Eğer AppLocker'da `C:\Windows\*` için tam yol izni verdiyseniz, zararlı bir zararlı `.exe` veya script bu alt klasörlere atılarak AppLocker kolayca atlatılabilir (bypass). Bu tür standart dışı yazılabilir dizinlere istisna (Exception) eklenmeli veya "Publisher" tabanlı kurallar tercih edilmelidir.

### 2. DLL Rules Maliyeti ve Performans Etkisi

DLL kontrolü (`DLL Rules`), sistemdeki her bir `.dll` dosyasının yüklenme anında doğrulanmasını sağlar.

- **Performans Uyarısı:** DLL kurallarını aktif etmek, makinedeki disk I/O ve işlemci kullanımını belirgin şekilde artırır. Çok büyük yapılarda ve zayıf donanımlı istemcilerde ciddi yavaşlamalara (sistem performans düşüşüne) yol açabilir.

- **Öneri:** Gerçekten kritik seviyede sıkılaştırılmış sunucular haricinde genelde ortam genelinde DLL denetimi yerine **Executable** ve **Script** kurallarına odaklanılır.

### 3. PowerShell Constrained Language Mode (CLM) Entegrasyonu

AppLocker Script kuralları ile PowerShell arasında doğrudan bir bağ vardır.

- Bir makinede AppLocker Script kuralları **Enforce** moduna alındığında, yetkisiz (non-admin) bir kullanıcı PowerShell çalıştırdığında PowerShell otomatik olarak **Constrained Language Mode (Sınırlı Dil Modu)** ortamına geçer.

- Bu durum, saldırganların PowerShell üzerinden `.NET` sınıflarını çağırmasını, belleğe erişmesini veya zararlı kod derlemesini (örneğin Win32 API çağrılarını) doğrudan engeller.

### 4. Olay Günlükleri (Event Logs) ve İzleme

AppLocker süreçlerini sorunsuz yönetmek için izlemeniz gereken Windows Event Log yolları şunlardır:

- **Konum:** `Applications and Services Logs` -> `Microsoft` -> `Windows` -> `AppLocker`
- **Log Kanalları:**
    - **EXE and DLL:** `.exe` ve `.dll` engellemeleri/audit kayıtları.
    - **MSI and Script:** Kurulum paketleri ve betik engel/audit kayıtları.
    - **Packaged app-Deployment / Execution:** Store uygulamaları kayıtları.
- **Kritik Olay Kimlikleri (Event IDs):**
    - `8002`: Uygulama çalışmasına izin verildi.
    - `8004`: Uygulama engellendi (**Enforce** modu aktifken).
    - `8003`: Uygulama engellenecekti ama sadece denetlendi (**Audit** modu aktifken).

### 5. AppLocker vs Windows Defender Application Control (WDAC)

Geleceğe yönelik planlama yaparken AppLocker'ın konumunu bilmek önemlidir:

- **AppLocker:** GPO ile yönetimi çok kolaydır ve kullanıcı/grup bazlı (Örn: "Muhasebe grubu haricinde kimse çalıştırmasın") esnek kurallar sunar.

- **WDAC (Windows Defender Application Control):** AppLocker'a göre çok daha çekirdek (Kernel) seviyesinde çalışan, admin haklarına sahip kişilerin bile bypass etmesini zorlaştıran, ancak yapılandırması ve yönetimi AppLocker'a kıyasla daha karmaşık olan yeni nesil bir çözümdür.



