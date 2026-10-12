### 1. Active Directory Saldırı Yüzeyi (Attack Surface) ve Vektörler

Saldırganlar AD'ye nasıl sızar ve içeride nasıl ilerler?

- **İlk Erişim (Initial Access):** Parola püskürtme (Password Spraying), oltalama (Phishing) veya zayıf şifrelerle sızma.
- **Yetki Yükseltme (Privilege Escalation):**
    - **Kerberoasting:** AD içindeki servis hesaplarının Kerberos biletlerini isteyip bu biletleri kırarak şifrelerini ele geçirme.
    - **AS-REP Roasting:** "Do not require Kerberos preauthentication" seçeneği işaretli kullanıcıların şifre özetlerini çekme.
- **Yanal İlerleme (Lateral Movement):** `Pass-the-Hash` veya `Pass-the-Ticket` teknikleriyle ağdaki diğer sunuculara sıçrama.
- **Domain Dominance (Tam Kontrol):** `Golden Ticket` veya `DCSync` saldırılarıyla kendilerine sahte yetkili bilet üreterek AD üzerinde kalıcılık sağlama.

### 2. Tehdit Aktörlerinin Kullandığı Araç ve Teknikler (TTPs)

CTI raporlarında ve MITRE ATT&CK matrisinde AD'ye saldıran grupların en sık kullandığı araçlar şunlardır:

- **BloodHound:** AD içindeki gizli yetki yollarını ve güvenlik açıklarını haritalandıran en popüler keşif aracı.
- **Mimikatz:** Bellekten (RAM) ve LSASS sürecinden düz metin şifreleri veya hash'leri çeken araç.
- **Rubeus:** Kerberos odaklı bilet saldırıları (Kerberoasting, Golden Ticket) için kullanılan araç.
- **Impacket:** Linux üzerinden (`rpcclient`, `psexec`, `secretsdump`) AD'ye saldırmak ve veri çekmek için kullanılan Python kütüphanesi.

### 3. CTI Açısından Tehdit Avcılığı ve İzleme (Threat Hunting)

CTI analisti olarak AD üzerinde hangi olay loglarını (Event ID) takip etmen gerekir?

- **Event ID 4624 / 4625:** Başarılı ve başarısız oturum açma girişimleri.
- **Event ID 4768 / 4769:** Kerberos bilet istekleri (Kerberoasting tespiti için).
- **Event ID 4672:** Özel yetkili (Administrator vb.) oturum açma hareketleri.
- **Event ID 4720:** Yeni bir kullanıcı hesabı oluşturulması (Saldırganların kalıcılık sağlama çabası).

### Saldırganların Sızma Aşamaları Örnek Senaryo

#### Aşama 1: İlk Erişim (Initial Access & Phishing)

Saldırgan, İnsan Kaynakları departmanında çalışan Ahmet Bey'e sahte bir e-posta gönderir: _"Ekim Ayı Güncel Yemek Kartı Faturası.pdf.exe"_. 
Ahmet Bey dosyaya çift tıklar. Arka planda zararlı yazılım çalışır ve saldırgan, Ahmet Bey'in bilgisayarında  düşük yetkili bir komut satırı erişimi elde eder.
Saldırgan ağın içinde ama henüz sıradan bir kullanıcı yetkisine sahip.

#### Aşama 2: Yerel Keşif ve Haritalandırma (Reconnaissance & BloodHound)

Saldırgan ağda nereye gideceğini görmek için içeriye **BloodHound** adındaki keşif aracını sızdırır.
Araç, Active Directory'deki tüm kullanıcıları, bilgisayarları, grupları ve aralarındaki gizli yetki ilişkilerini haritalandırır.

BloodHound saldırgana şunu söyler: _"Ahmet kullanıcısının hiçbir yetkisi yok. Ancak ağdaki `SQL-SVC`adındaki servis hesabının şifresini kırarsan, o hesap üzerinden Domain Admin grubundaki Mehmet Bey'in bilgisayarına sıçrayabilirsin!"_

#### Aşama 3: Yetki Yükseltme (Privilege Escalation & Kerberoasting)

 Saldırgan hedef olarak `SQL-SVC` hesabını seçer ve **Kerberoasting** saldırısını başlatır.
    1. Ahmet'in oturumu üzerinden Domain Controller'dan `SQL-SVC` hesabı için bir Kerberos servisi bileti (TGS Ticket) ister.
    2. Domain Controller bilet verir. Bu bilet `SQL-SVC` hesabının şifresiyle şifrelenmiştir.
    3. Saldırgan bu bileti kendi bilgisayarına indirir ve offline olarak **Hashcat/John the Ripper** araçlarıyla şifreyi kırmaya çalışır.

 Servis hesabının şifresi zayıf olduğu için (`Sql2023!`) şifre 10 dakika içinde kırılır. Saldırgan artık `SQL-SVC`hesabının yetkilerine sahiptir.

#### Aşama 4: Yanal İlerleme (Lateral Movement & Pass-the-Hash)

Ele geçirilen `SQL-SVC` hesabı yetkileriyle ağdaki diğer sunuculara sıçranır.
Saldırgan **Impacket** veya **psexec** aracını kullanarak `SQL-SVC` kimliğiyle SQL Sunucusuna bağlanır. Burada bellekte (RAM) çalışan **LSASS** sürecini **Mimikatz** aracıyla okur.
SQL sunucusuna önceden bakım yapmak için giren Sistem Yöneticisi Mehmet Bey'in (Domain Admin) şifre hash'i bellekten çalınır.

#### Aşama 5: Domain Dominance (Tam Kontrol & DCSync)

Saldırgan artık Domain Admin yetkisine sahip olan Mehmet Bey'in kimliğine bürünmüştür.
    1. Saldırgan **DCSync** tekniğini kullanır; Domain Controller'a _"Ben de bir Domain Controller'ım, bana tüm kullanıcıların şifre hash'lerini senkronize et"_ komutunu gönderir.
    2. Active Directory veri tabanındaki  tüm CEO, İK, Sistem Yöneticisi şifrelerini çeker.
    3. Ardından **Golden Ticket** (Altın Bilet) oluşturur. Bu bilet sayesinde AD üzerindeki şifreler değişse bile saldırgan süresiz ve izlenemez bir arka kapı (Backdoor) elde etmiş olur.



Red Team (Saldırı)AD'ye sızar, Kerberoasting yapar, Golden Ticket üretir. 
"Bu AD'yi nasıl ele geçirebilirim?"

SOC / Blue Team (Savunma) Event ID 4769 logunu izler, Mimikatz'ı engeller.
"Şu an AD'ye bir saldırı var mı?"

CTI (İstihbarat) Saldıran grubun kim olduğunu, hangi araçları kullandığını, Dark Web'de şirket şifrelerinin satılıp satılmadığını takip eder ve savunmaya yön verir.  CTI ekibi saldırganın içeride bıraktığı izlere (IoC - Indicators of Compromise: IP adresleri, dosya hash'leri, kullandığı komutlar) bakar.
"Bize saldıran kim, hangi AD tekniklerini kullanıyor ve bir sonraki hamlesi ne olacak?"
