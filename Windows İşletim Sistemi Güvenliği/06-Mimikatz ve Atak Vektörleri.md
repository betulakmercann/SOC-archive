## Mimikatz
windows işletim sistemlerinde lssas.exe prosesine erişimlesini ve bu parolaları açık metin haline getirmeyi sağlayan araç

**Korunma Yöntemleri:**
- **LSASS Koruması:** `RunAsPPL` (Protected Process Light) aktifleştirilerek LSASS prosesinin belleğine dışarıdan yetkisiz erişimler engellenmelidir.
- **Güncel Yama ve Yazılımlar:** İşletim sistemleri güncel tutulmalı, zafiyetler kapatılmalıdır.

## mimikatz atak vektörleri
pass the hash saldırısı
golden ticket
silver ticket-kerberos
pass the ticket

#### **bazı kavramlar** 

**kerberos**: güvenli olmayan bir ağda haberleşen kaynaklar bilet mantığıyla kendi kimliklerini tanıtır bu sayede iletişim kurarlar, birnevi kimlik doğrulama protokolüdür

**KDC(key distribution center:**  kerberos kimlik doğrulamasında kullanılan bir servis. authentication service ve ticket granting service (TGC) olmak üzere iki bölümden oluşur.

**TGC:** kerberos bileti veren servis

**Lsass:** lsass prosesi windowsta kullanıcı kimlik doğrulamasını parola değişikliklerini ve güvenlik ilkelerinin uygulamasını yöneten süreç
### 1. Pass the Hash (PtH)
Saldırgan ele geçirdiği NTLM veya LM hash değerlerini düz metin parola yerine doğrudan kimlik doğrulama sürecinde kullanır (parolayı kırmaya gerek kalmaz)

**Korunma Yöntemleri:**
Yüksek yetkili hesapların son kullanıcı istasyonlarında oturum açması kesinlikle engellenmelidir 

SMB gibi servislerin gereksiz sistemler arasında iletişimi kısıtlanmalıdır.

### 2. Pass the Ticket (PtT)
Kerberos biletlerinin  bellekten çalınarak başka bir oturumda veya makinede kimlik doğrulama amacıyla yetkisiz bir şekilde kullanılmasıdır. Hash yerine biletler taklit edilir.

**Korunma Yöntemleri:**
Kerberos bilet ömürleri ve yenileme süreleri minimumda tutulmalıdır.

İstasyonlarda oturum kilitlenmeleri ve bellek güvenliği sıkılaştırılmalıdır.

### 3. Golden Ticket (Altın Bilet)
Active Directory'nin kalbi olan **krbtgt** hesabının parola hash'i ele geçirildiğinde, saldırgan tüm etki alanında sınırsız yetkiye sahip sahte bir TGT üretir. İstediği süreyi ve yetkiyi bilet içine yazabilir.

**Korunma Yöntemleri:**
`krbtgt` hesabının parolası düzenli aralıklarla değiştirilmelidir. ve parola hashine ulaşabilcek hesap sayısı azaltılmalıdır

 Anormal yetki yükseltmeleri ve Kerberos logları sıkı bir şekilde izlenmelidir.

bal küpü nesneleri oluşturulmalıdır

### 4. Silver Ticket (Gümüş Bilet - Kerberos)
Golden Ticket'a benzer; ancak bu kez tüm domain yerine belirli bir servisin NTLM hash'i kullanılarak o servis için sahte bir TGS üretilir. Domain Controller ile iletişime geçilmediği için tespiti daha zordur.

**Korunma Yöntemleri:**
Servis hesaplarının parolaları karmaşık olmalı ve periyodik olarak değiştirilmelidir.

Servis hesaplarına sadece işlerini yapacak kadar yetki verilmelidir.

**DCSYNC Saldırısı**
domain controller gibi davranır, diğer domain controllerlardan kullanıcı adı ve parola bilgisini elde etmek için kullanılır

**NTDS.dit Saldırısı**
sunculurda active directory verilerinin default olarak toplanır, toplandığı dosya adı NTDS.dit dosyasıdır. içerisinde kimlik bilgileri gibi kritik bilgileri barındırır

