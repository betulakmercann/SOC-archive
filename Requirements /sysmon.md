sysmon (system monıtor) wındowstakı gerceklesen onemlı olayları ayrıntılı sekılde kaydeden sıstem aracıdır, mıcrosoft tarafından gelıstırılmıstır. event logsdan farkı; ozellıkle process, network, file ve registry aktıvıtelerı konularında daha degerlı telemetry saglar.

Normal loglarda:
PowerShell çalıştı.

Sysmon ile:
`WINWORD.EXE` → `powershell.exe` başlattı → belirli komut satırıyla çalıştı → belirli bir IP'ye bağlantı kurdu.

neden kullanılır?
ne calıstı, kim calıstırdı, ne zaman calıstı, hangi ip ile baglantı kuruldu, hangi registry alanı degisti, hangi dosya olusturuldu vb aktıvıtelerı telemetry olarak toplar.

                 WINDOWS
                    │
          ┌─────────┴─────────┐
          │                   │
      Kullanıcı            Sistem
          │                   │
          ▼                   ▼
      Programlar          Services
          │                   │
          └─────────┬─────────┘
                    │
                    ▼
                  SYSMON
                    │
          ┌─────────┼─────────┐
          ▼         ▼         ▼
       Process   Network    File
          │         │         │
          └─────────┼─────────┘
                    ▼
              Windows Event Log
                    │
                    ▼
                 SOC / SIEM


nerde calısır?
arkaplanda gozlemcı modunda calısır. antıvurus degıldır, malware vb engellemekten daha cok bu makınede ne oldu vb sorularına cevap verır.
bır SIEM degıldır.

 Sysmon
   ↓
Event üretir
   ↓
Windows Event Log
   ↓
Wazuh / SIEM
   ↓
Detection / Alert
   ↓
SOC Analyst

yanı sysmon verıyı uretır. SIEM ıse bu verılerı farklı kaynaklardan toplar, iliskilendirir, analiz tarafında kullanır.

sysmon olayları Event ID ıle tanımlar.
event id 1 : process creation
event id 3 : network connection
event id 11 : file creation
event id 12-13-14 : registry aktivitileri
vb gibi. 

ornegın event id 1 calıstı. ama sadece calısan programı gostermez, cesitli bilgiler de verir. orn:

Process:
powershell.exe

User:
Administrator

Parent Process:
cmd.exe

Command Line:
powershell.exe -Command ...

Time:
15:32

Path:
C:\Windows\System32\WindowsPowerShell\v1.0\

ayrıca bir process baska bir process calıstırabılır. orn:
explorer.exe (parent process)
      │
     ▼
powershell.exe (child process)

bu bıze bı process relatıonshıp verır. cunku tek process bazen yeterınce suphelı degıldır. olay zıncırıne bakılır burda, bu nedenle relatıonshıp kurması onemlıdır. 
orn:
PowerShell
    ↓
File Creation
    ↓
malicious.exe
    ↓
Process Creation
    ↓
Network Connection

sysmon hash nedir?
Sysmon bazı olaylarda dosyanın hash bılgısını de sağlayabılır.

sysmon configuration
bır congigurasyon dosyasıyla hangı olayların ızlenecegı ve nasıl fılteleencegı belırlenebılır. orn:
Process Creation → izle
Network Connection → izle
File Creation → izle
Registry → belirli aktiviteleri izle
cunku cok fazla event uretırsek gereksız verı ve gurultu olustur ve analız ısı zorlasır.

sysmon eventleri nerde gorunur?
wındows event log ıcınde gorulur.

event gordugumuzde sorgu sırası:
ne oldu, kim yaptı, hangi program, parent ne ve konumu nerde, ne zaman, event id ne gibi bi sıralama olabılır. sonrasında butun bunları bırlestırır.
WINWORD.EXE
     ↓
powershell.exe
     ↓
file creation
     ↓
network connection ve suphelı ıslem var mı arastırır.




görsel hafıza icin:

sysmon neyi gozlemler?

                         SYSMON
                           │
       ┌───────────┬───────┼────────┬───────────┐
       │           │       │        │           │
    PROCESS    NETWORK   FILE   REGISTRY   diğerleri
       │           │        │        │
       ▼           ▼        ▼        ▼
  Ne çalıştı?   Nereye?  Ne oluştu? Ne değişti?
      .│                             │
       ▼                             ▼
    PARENT                       Persistence?
       │
       ▼
  PROCESS CHAIN
       │
     └──────────────┬──────
                    ▼
                INVESTIGATION



soc investigation is akısı:

PROCESS
   ↓
NETWORK
   ↓
FILE
   ↓
REGISTRY
   ↓
Persistence?
   ↓
EVENT CHAIN
   ↓
SOC INVESTIGATION



bazı idler:

| Event ID | Anlamı                        |
| -------- | ----------------------------- |
| **1**    | Process Creation              |
| **3**    | Network Connection            |
| **11**   | File Creation                 |
| **12**   | Registry Object Create/Delete |
| **13**   | Registry Value Set            |
| **14**   | Registry Key/Value Rename     |
