# Windows Saldırı Teknikleri
Windows işletim sistemlerinde güvenlik testleri, sızma testleri veya yetki yükseltme (privilege escalation) senaryolarında kullanılan temel saldırı teknikleri, sistemin zafiyetlerine veya yanlış yapılandırmalarına dayanır. Bu teknikler genellikle keşif aşamasından sonra yetki artırmak veya ağ içinde yatay hareket (lateral movement) yapmak amacıyla uygulanır.

Windows ortamlarında en sık karşılaşılan ve kritik olan bazı saldırı teknikleri şunlardır:

### 1. Kimlik Bilgisi Hırsızlığı ve Bellek Saldırıları (Credential Dumping)

- **LSASS Bellek Dökümü (`lsass.exe` Dump):** Windows'un kimlik doğrulama süreçlerini yöneten bu sürecin belleği kopyalanarak içerisindeki NTLM hash'leri ve düz metin parolalar hedeflenir.
- **Mimikatz Kullanımı:** Bellekten (RAM) biletlerin, PIN'lerin ve parolaların çekilmesinde kullanılan en popüler araçtır.

### 2. Kimlik Bilgisi Yeniden Kullanımı (Pass-the-Hash / Pass-the-Ticket)

- **Pass-the-Hash (PtH):** Parolayı düz metin olarak bilmeye gerek kalmadan, ele geçirilen NTLM hash değeri doğrudan kimlik doğrulama sürecinde kullanılır.
- **Pass-the-Ticket (PtT):** Kerberos biletleri (`TGT` veya `TGS`) çalınarak başka bir oturumda veya makinede yetkiliymiş gibi hareket edilir.

### 3. Etki Alanı Hakimiyeti (Domain Dominance)

- **Golden Ticket (Altın Bilet):** Active Directory veritabanının kalbi olan `krbtgt` hesabının hash'i ele geçirilerek etki alanında sınırsız yetkiye sahip sahte biletler üretilir.
- **Silver Ticket (Gümüş Bilet):** Belirli bir servisin (örneğin CIFS, MSSQL) servis hesabı hash'i kullanılarak o servise yönelik sahte biletler oluşturulur. Domain Controller ile iletişime geçilmediği için tespiti zordur.

### 4. Yetki Yükseltme ve Yanay Hareket (Privilege Escalation & Lateral Movement)

- **Yetki Yükseltme (Privilege Escalation):** Standart bir kullanıcı hesabıyla sisteme sızdıktan sonra, yanlış yapılandırılmış servisler, zafiyetli sürücüler (`BYOVD`) veya yerel yönetim hakları kullanılarak `SYSTEM` veya `Administrator` yetkilerine ulaşılır.
- **Yatay Hareket (Lateral Movement):** Ele geçirilen yetkilerle aynı ağdaki diğer sunuculara ve iş istasyonlarına sızmak için `PsExec`, `WMI`, `WinRM` veya `SMB` protokolleri kullanılır.
