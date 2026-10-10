# Active Directory (AD) Nedir 
Active Directory (AD), Microsoft tarafından geliştirilen, bir kurumun veya şirketin ağındaki tüm kaynakları **tek bir merkezden yönetmeyi, kimlik doğrulamayı ve yetkilendirmeyi** sağlayan dizin hizmetidir (Directory Service)

Büyük bir şirkette yüzlerce bilgisayar ve çalışan olduğunu düşün:
- AD olmadığında, her çalışana her bilgisayarda tek tek kullanıcı hesabı açmak ve kuralları elle ayarlamak gerekir.
- AD olduğunda ise tüm bilgiler merkezi bir veri tabanında tutulur.

#### Active Directory'nin Temel Görevleri:
- Merkezi Kimlik Yönetimi (Authentication): Bir kullanıcı  oluşturulduğunda, bu kullanıcı domain'e bağlı herhangi bir bilgisayardan veya yetki verilen bir Linux istemciden kendi şifresiyle oturum açabilir.
- Yetkilendirme (Authorization): Hangi kullanıcının hangi klasörlere, sunuculara veya ağ kaynaklarına erişebileceğini belirler.
- Merkezi Politika Uygulama (Group Policy - GPO): Tüm bilgisayarlara tek tıkla güvenlik kuralları, şifre politikaları veya çalışma saati kısıtlamaları (Logon Hours) dağıtır.
- Dizin Yapısı (Directory Objects): Ağ üzerindeki kullanıcıları, grupları, bilgisayarları ve yazıcıları hiyerarşik bir düzende (Organizational Unit - OU) organize eder.

### AD Kurulumu Öncesindeki Hazırlık Süreci
Active Directory (AD) yapısının sorunsuz çalışabilmesi için altyapı adımlarının sırasıyla tamamlanmış olması gerekir

1. **Sanal Makine Altyapısının Kurulması:**
    - VirtualBox veya VMware üzerinde iki ayrı sanal makine oluşturulur: Biri Windows Server, diğeri Linux(Ubuntu, Kali, Wazuh Server vb.).
2. **Ağ Yapılandırması (Networking):**
    - Her iki sanal makinenin birbiriyle konuşabilmesi için ağ modları ayarlanır.
    - Windows Server makinesine **Sabit (Static) IP** tanımlanır (örneğin `192.168.x.x`). AD kurulacak bir sunucunun IP adresi asla değişmemelidir.
3. **Bağlantı Doğrulaması (Ping & Connectivity):**
    - Linux istemci (`192.168.x.x) ile Windows Server (`192.168.x.x`) arasında karşılıklı `ping` atılarak ağ seviyesindeki haberleşme doğrulanır.
4. **DNS ve Sunucu Adı Düzenlemesi:**
    - Windows Server'a anlamlı bir bilgisayar adı verilir ve varsayılan DNS adresi olarak kendi IP'si gösterilir.

Tüm bu altyapı ve ağ erişimi sağlandıktan sonra Windows Server üzerinde Active Directory Domain Services (AD DS) rolünün kurulumuna geçilir.

