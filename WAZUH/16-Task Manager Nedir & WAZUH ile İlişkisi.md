# TASK MANAGER
**Task Manager (Görev Yöneticisi)**, Windows işletim sistemlerinde çalışan uygulamaları, arka plan süreçlerini (processes), performans tüketimini (CPU, RAM, Disk, Ağ) ve başlangıç programlarını izlemenizi ve yönetmenizi sağlayan en temel yerleşik sistem izleme ve yönetim aracıdır.

Sistem performansını izlemek, yanıt vermeyen (donan) programları kapatmak ve zararlı yazılımları veya kaynak sömüren süreçleri tespit etmek için kritik bir rol oynar.

### Task Manager Hakkında Bilinmesi Gereken Kritik Detaylar ve Sekmeler

Görev Yöneticisi (`Ctrl + Shift + Esc` tuş kombinasyonuyla hızlıca açılabilir), işlevlerine göre kategorize edilmiş sekmelerden oluşur:

1. **Processes (İşlemler) Sekmesi:**
    
    - O an çalışan tüm uygulamaları, arka plan süreçlerini ve Windows hizmetlerini listeler.
    - Her bir sürecin anlık olarak ne kadar **CPU, Bellek (RAM), Disk ve Ağ (Network)** kaynağı tükettiğini yüzde veya miktar olarak gösterir.
    - **Donan Bir Uygulamayı Kapatmak:** Yanıt vermeyen bir programı kapatmak için üzerine sağ tıklayıp **End Task (Görevi Sonlandır)** seçeneğini kullanabilirsiniz.
    
2. **Performance (Performans) Sekmesi:**
    
    - Bilgisayarınızın donanım bileşenlerinin (CPU, Bellek, Diskler, Wi-Fi/Ethernet ve Grafik Kartı/GPU) anlık kullanım grafiklerini ve genel durumunu gösterir.
    - İşlemcinin çekirdek sayısını, çalışma hızını (GHz), bellek hızını (MHz) ve çalışma süresini (Up time) buradan takip edebilirsiniz.

3. **App History (Uygulama Geçmişi) Sekmesi:**
    
	- Modern (Windows Mağazası) uygulamalarının zaman içinde ne kadar kaynak tükettiğini ve işlemci süresi harcadığını gösterir.

4. **Startup Apps (Başlangıç Uygulamaları) Sekmesi:**
    
	- Bilgisayarınız açıldığında otomatik olarak arka planda çalışmaya başlayan uygulamaları listeler.
    - Bilgisayarın açılış süresini uzatan gereksiz programların durumunu buradan **Disable (Devre Dışı Bırak)**yaparak sistemi hızlandırabilirsiniz.
        
5. **Users (Kullanıcılar) Sekmesi:**
    
    - Birden fazla kullanıcı oturumu açılmışsa, hangi kullanıcının hangi kaynakları tükettiğini gösterir.
        
6. **Details (Ayrıntılar) Sekmesi:**
    
    - Çalıştırılabilir dosyaların (`.exe`) işletim sistemindeki süreç kimlik numaralarını (**PID**), kullanıcı haklarını ve işlem önceliklerini (Priority) detaylıca gösterir. Gelişmiş sorun giderme veya süreç sonlandırma işlemleri burada yapılır.
        
7. **Services (Hizmetler) Sekmesi:**
    
    - Windows arka plan hizmetlerinin durumunu (`Running` veya `Stopped`) gösterir ve bunları yönetmenize olanak tanır.


# WAZUH İLE İLİŞKİSİ

### 1. Görsel Arayüz (Task Manager) vs. Anomali Tespit Motoru (Wazuh)

- **Task Manager:** Makinede o an çalışan bir `.exe` dosyasını, tükettiği kaynakları (CPU, RAM) ve PID (Process ID) değerini anlık olarak **manuel** incelemenizi sağlar.
- **Wazuh:** Task Manager'da gördüğünüz tüm bu süreçlerin başlatılma, durdurulma ve kaynak kullanım geçmişini **otomatik** olarak toplar.

- **İlişki:** Task Manager'da şüpheli bir süreç (örneğin ismi `svchost.exe` taklit edilmiş ama imzası olmayan zararlı bir dosya) gördüğünüzde, bunun ne zaman başladığını, hangi ebeveyn süreç (Parent Process) tarafından tetiklendiğini ve ağda nereye bağlandığını geriye dönük sorgulamak için Wazuh kullanılır.

### 2. Sysmon ve Windows Event Log Entegrasyonu

Task Manager'da anlık çalışan bir uygulamayı görürsünüz ancak detayını bilemezsiniz. Wazuh ise Windows Event Log'ları ve **Sysmon** (System Monitor) entegrasyonu sayesinde Task Manager'ın sunduğu verileri derinleştirir:

- **Event ID 1 (Process Creation):** Task Manager'da yeni beliren bir uygulamanın tam dosya yolunu, komut satırı parametrelerini (Command Line) ve hash değerini Wazuh'a bildirir.
- **Process Injection / Hollowing:** Saldırganlar Task Manager'da meşru görünen bir sürecin (örn: `explorer.exe`) içine zararlı kod enjekte edebilir. Wazuh bu tür bellek müdahalelerini tespit ederek uyarı üretir.

### 3. Kaynak Kullanım Mimarisi (Wazuh Agent Etkisi)

Wazuh'un kendisi de izlediği bilgisayarlarda bir **Wazuh Agent** hizmeti (`wazuh-agent.exe`) olarak çalışır.

- **Önemli Detay:** Task Manager'ı açtığınızda `Wazuh Agent` sürecinin çalışır durumda olduğunu ve ne kadar CPU/RAM tükettiğini görürsünüz.
- **Performans İzleme:** Wazuh agent'ının sistemi yormaması (örneğin kök dizin taramalarında veya FIM - File Integrity Monitoring sırasında işlemciyi kilitlememesi) için Task Manager üzerinden Wazuh'un kaynak kullanımı takip edilir ve konfigürasyondan limitler belirlenir.

### 4. Saldırı Tespiti (Threat Hunting) Örnekleri

Siber saldırganların ilk yaptığı şeylerden biri, yakalanmamak için güvenlik araçlarını kapatmaktır:

- **Görev Yöneticisinden Süreç Kapatma:** Bir saldırgan Task Manager veya `taskkill` komutuyla güvenlik servislerini kapatmaya çalışırsa, Wazuh bu eylemi anında tespit eder (Event ID 7036 / Sysmon Event ID 5) ve SOC merkezine alarm düşürür.
- **Gizli Süreçler (Rootkit):** Bazen Task Manager'da görünmeyen ama sistemde çalışan zararlı süreçler (Rootkit) olabilir. Wazuh Syscheck ve Rootcheck modülleri ile sistem çağrılarını tarayarak Task Manager'dan gizlenen bu süreçleri ortaya çıkarır.
