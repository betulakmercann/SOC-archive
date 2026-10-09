Windows
│
├── User
│
├── Applications
│
├── Processes
│
├── Services
│
├── Registry
│
└── Windows Kernel

**kernel:** isletim sisteminin cekirdegidir. donanım ve sistem kaynaklarıyla uygulamalar arasında onemlı bı katman gorevı gorur. 
ornek: 
Chrome
  ↓
Windows
  ↓
CPU / RAM / Disk / Network
bi uygulama bunlardan bırını kullanmak ıstedıgınde ısletım sıstemı bu kaynakların yonetılmesını saglar. 

**User Mode ve Kernel Mode**
normal uygulamalar dogrudan donanım kısmına ıstedıgı gıbı erısemez.
bu ayrım guvenlık acısından onemlıdır. kernel sevıyesınde gerceklesen olaylar normal kullanıcı ıslemlerınden daha krıtık olabılır. 

**Windows Users/Groups**
linuxtakı mantık ıle aynı, farklı kullanıcılar ve gruplar olabılır, yetkılendırmeler olabılır, kullanıcılar gruplara dahıl edılebılır vs. 

Administrators: linuxta root ile bırebır aynı olmasa da yuksek yetkılı kullanıcıdır.

**Windows Processes**
linuxtakıyle aynı temel mantık. 
chrome.exe , explorer.exe , powershell.exe , cmd.exe vb process(calısan programlar) gorulebılır. 
her processin PID (process ID si vardir.)
orn:
chrome.exe      PID 4520
powershell.exe  PID 7812

**Windows Services**
service, arkaplanda calısan ve bellı gorevı yerıne getıren sıstem bılesenıdır.
orn: 
Windows Update
Windows Defender
Print Spooler
DNS Client

**PowerShell**
komut calıstırmak, sıstem yonetmek ve otomasyon yapmak ıcın kulalnılan guclu bır shell/scripting ortamıdır.
orn: 
Get-Process (calısan programları listeler)
Get-Service (servisleri gosterır)
Get-LocalUser (yerel kullanıcıları gosterır)

**Windows Registry**
onemlı yapılandırma bılgılerı tutulan merkezı verıtabanı.
Windows Registry
│
├── System settings
├── User settings
├── Software settings
├── Startup-related configuration
└── Other configuration data
bırcok sıstem ve uygulama regıstry ıle ılıskılıdır.

**Event Viewer**
wındowsun olusturdugu olay kayıtlarını goruntulemek ıcın kullanılan aractır.

Windows'ta olay gerçekleşir
        ↓
Event oluşturulur
        ↓
Windows Event Log'a kaydedilir
        ↓
Event Viewer'dan incelenebilir

Successful login
Failed login
Process-related event
Service event
System error gıbı olaylarla karsılasılabılır.

**Windows Event Logs**
farklı log kategorılerını gorebılırız
orn: security, system, application vb.

**Event ID**
olayların event id degerlerı olabılır.
orn: Event ID 4624 başarılı log olaylarını ıfade eder, 4625 basarısızları ıfade eder.

**Authentication**
kımlık dogrulama.
**Authorization**
nelere yetkın var?

User → Authentication
        ↓
      "betul"
        ↓
Authorization
        ↓
"Bu dosyaya erişebilir mi?"

**Windows Security Events**
wındowsta guvenlıkle ılgılı olaylar olusur.
Login
Failed login
Account created
Account changed
Privilege use
Process activity vb.

**Active Directory**
wındows kullanıcılarını, pcleri, grupları ve cesitli kaynakları merkezı olarak yonetmeye yarayan mıcrosoft dızın hızmetıdır.
orn:
Company
│
├── Users
│   ├── Ahmet
│   ├── Mehmet
│   └── Ayşe
│
├── Computers
│   ├── PC-01
│   ├── PC-02
│   └── PC-03
│
└── Groups
    ├── IT
    ├── HR
    └── Security

**Domain Controller (DC)**
active dırectory ortamının merkezı bılesenlerınden bırıdır. kullanıcıların ve pclerın domaın ıcındekı kımlık dogrulama ve yonetım sureclerınde rol oynar.
User
   ↓
Domain Controller
   ↓
Authentication
   ↓
Access


