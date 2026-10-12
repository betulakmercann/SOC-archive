# TEMEL AĞ PROTOKOLLERİ, OSI MODELİ VE TRAFİK AKIŞI

## 1. TCP vs. UDP Protokolleri

### TCP (Transmission Control Protocol)
* Hızlı ve güvenilirdir. Bağlantı kurmadan veri göndermez, paketlerin sırasını takip eder ve ulaştığını doğrular.
* Bağlantı yönetimi ve akış kontrolü yapar; **3-Way Handshake** sürecini kullanır. *"Önce bağlantı kur, sonra veriyi gönder"* mantığıyla çalışır.
* Güvenilirlik gerektiren iletişimlerde tercih edilir (Örn: `HTTPS`, `SSH`, `FTP`, `SMTP`).

#### 3-Way Handshake Süreci:
1. **Client $\rightarrow$ Server:** Bağlantı kurmak istiyorum (`SYN`)
2. **Server $\rightarrow$ Client:** Hazırım (`SYN-ACK`)
3. **Client $\rightarrow$ Server:** Tamamdır (`ACK`)  
*(Bu aşamadan sonra TCP bağlantısı güvenli bir şekilde kurulmuştur.)*

---

### UDP (User Datagram Protocol)
* TCP'ye kıyasla çok daha hızlıdır ancak **garantisizdir** (low overhead). Sıra garantisi sunmaz ve paket kaybını kontrol etmez.
* Hızın kritik olduğu durumlarda, önce verinin gönderildiği senaryolarda kullanılır.
* Genellikle **DNS**, **DHCP**, video akışları (streaming), çevrim içi oyunlar ve `VoIP` servislerinde tercih edilir.

---

## 2. Port Kavramı ve Yaygın Portlar

Port, hedef bilgisayara ulaşılan verinin hangi servise (`process`) yönlendirileceğini belirleyen sayısal kimliktir.

### Yaygın Port Listesi:
* **`20 / 21`** $\rightarrow$ FTP (File Transfer Protocol)
* **`22`** $\rightarrow$ SSH (Secure Shell)
* **`23`** $\rightarrow$ Telnet
* **`25`** $\rightarrow$ SMTP (Simple Mail Transfer Protocol)
* **`53`** $\rightarrow$ DNS (Domain Name System)
* **`80`** $\rightarrow$ HTTP (Hypertext Transfer Protocol)
* **`110`** $\rightarrow$ POP3 (Post Office Protocol)
* **`139`** $\rightarrow$ NetBIOS
* **`143`** $\rightarrow$ IMAP (Internet Message Access Protocol)
* **`443`** $\rightarrow$ HTTPS (HTTP Secure)
* **`445`** $\rightarrow$ SMB (Server Message Block)
* **`3306`** $\rightarrow$ MySQL Veritabanı
* **`3389`** $\rightarrow$ RDP (Remote Desktop Protocol)
* **`5432`** $\rightarrow$ PostgreSQL Veritabanı

### Port Durumları (Tarama Sonuçları):
* **`Open` (Açık):** Port aktiftir, ilgili servis çalışıyor ve bağlantı kabul ediyor (Örn: `443/tcp open`).
* **`Closed` (Kapalı / Erişilebilir):** Porttan yanıt alınıyor ancak arkasında dinleyen (listening) bir servis yok (Örn: `9999`).
* **`Filtered` (Filtrelenmiş):** Firewall veya ağ filtreleme kuralları nedeniyle portun durumu tam olarak doğrulanamıyor (Örn: `445`).

---

## 3. DNS (Domain Name System)

DNS, insan dostu alan adlarını (`google.com`) bilgisayarların haberleşebilmesi için sayısal IP adreslerine (`142.250.x.x`) çeviren sistemdir. DNS olmasaydı internet sitelerine IP adresleriyle girmek zorunda kalırdık.

### Başlıca DNS Kayıt Türleri (Records):
* **`A`:** Alan adını IPv4 adresine çözer.
* **`AAAA`:** Alan adını IPv6 adresine çözer.
* **`CNAME`:** Bir alan adını başka bir alan adma yönlendirir (takma ad / alias).
* **`MX`:** E-posta trafiğinin yönlendirileceği mail sunucularını belirtir.
* **`NS` (Name Server):** İlgili alan adı için yetkili DNS sunucusunu tanımlar.
* **`TXT`:** Metin tabanlı ek bilgiler veya doğrulama (verification) kayıtlarını tutar.

---

## 4. DHCP (Dynamic Host Configuration Protocol)

Ağa katılan yeni cihazlara otomatik olarak IP adresi, alt ağ maskesi (`subnet mask`), ağ geçidi (`gateway`) ve DNS bilgilerini atayan protokoldür. Çalışma mantığı **DORA** kısaltmasıyla özetlenir:

```text
[Client]                      [DHCP Server]
   │                              │
   ├─── 1. DHCP Discover ────────>│ (Ağda DHCP sunucusu arar)
   │                              │
   │<── 2. DHCP Offer ────────────┤ (Sunucu IP teklif eder)
   │                              │
   ├─── 3. DHCP Request ─────────>│ (Teklif edilen IP'yi talep eder)
   │                              │
   │<── 4. DHCP ACK ──────────────┤ (Atamayı onaylar ve tamamlar)
```

## 5. IP Adresleri (IPv4, Private ve Public IP)
IP adresi, ağ üzerindeki cihazların birbirini bulmasını sağlayan benzersiz kimliktir. 
IPv4 standardı 32-bit uzunluğundadır.

A. Private IP (Özel IP)
Yalnızca yerel ağlarda (LAN) kullanılan, internet yönlendirilemeyen IP aralıklarıdır:
10.0.0.0 / 8
172.16.0.0 / 12
192.168.0.0 / 16

B. Public IP (Genel IP)
İnternet dünyasında benzersiz olan ve küresel çapta yönlendirilebilen IP adresleridir.
İnternet Erişim Zinciri: Domain → DNS → Public IP → Open Ports → Services → Applications

## 6. HTTP, HTTPS, NAT, Firewall, VPN ve Proxy

**HTTP**: Web üzerindeki metin, görsel ve veri iletişimini sağlayan şifresiz protokoldür.

**HTTPS**: HTTP protokolünün TLS (Transport Layer Security) ile şifrelenmiş ve güvenli hale getirilmiş versiyonudur.

**NAT (Network Address Translation)**: Yerel ağdaki cihazların sahip olduğu Private IP adreslerini Public IP'ye çevirerek internete çıkmalarını sağlar.

**Firewall (Güvenlik Duvarı)**: Belirli kurallara (Kaynak IP, Hedef IP, Port, Protokol, Yön vb.) göre ağ trafiğine izin veren veya engelleyen güvenlik mekanizmasıdır.

**VPN (Virtual Private Network)**: İki uç nokta arasında şifreli tünel oluşturarak güvenli iletişim kurmayı sağlar (Örn: Evdeki bilgisayardan şirket ağına 10.0.0.x bağlanmak).

**Proxy**: İstemci ile sunucu arasında aracı (intermediary) görevi gören sunucudur. Trafiği yönlendirebilir, filtreleme yapabilir, log tutabilir ve önbellekleme (cache) sağlayabilir.

**VPN vs. Proxy Farkı**:
Proxy: Sadece uygulama düzeyinde aracılık yapar, trafiğin yönünü değiştirir veya filtreler.
VPN: Tüm ağ trafiğini şifreleyerek güvenli bir tünelden geçirir.

## 7. OSI Modeli ve TCP/IP Katmanları Karşılaştırması
```text

Katman-Katman Adı	                  Açıklama ve Görevler	                   Karşılık Gelen TCP/IP Katmanı

7	Application         	Kullanıcı uygulamaları ve protokolleri	                    Application

6	Presentation	        Veri formatı, sıkıştırma ve şifreleme	

5	Session	                Oturum yönetimi ve sürekliliği	

4	Transport	            Uçtan uca güvenli ve güvenilir veri taşıma (TCP, UDP)	    Transport

3	Network	                Paket yönlendirme (IP), IP adresleme ve Router işlemleri	Internet

2	Data Link	            Fiziksel adresleme (MAC), Ethernet ve Switch işlemleri	    Network Access

1	Physical	            Kablolar, elektrik sinyalleri ve fiziksel ortam

```
## 8. Tüm Bileşenlerin Birlikte Çalışma Senaryosu (Özet Akış)

Bir web sitesine (Örn: https://example.com) giriş yaptığınızda arka planda sırasıyla şu adımlar gerçekleşir:

**Ağ Bağlantısı ve IP Alımı**: Bilgisayar açılıp Wi-Fi'a bağlandığında DHCP protokolü üzerinden ağ yapılandırma bilgilerini (IP, Subnet, Gateway, DNS) alır.

**DNS Sorgusu**: Tarayıcıya google.com yazıldığında, bilgisayar alan adının hangi IP adresine karşılık geldiğini öğrenmek için DNS sorgusu yapar.

**NAT ve Yönlendirme**: Bilgisayar yerel bir Private IP kullanıyorsa, internete çıkabilmek için NAT / Router mekanizmasından geçer.

**Güvenlik Kontrolleri**: Trafik, çıkış ve giriş aşamalarında Firewall kurallarından ve güvenlik denetimlerinden geçer.

**Bağlantı Kurulumu (TCP + TLS)**: Sunucunun IP adresiyle TCP 3-Way Handshake gerçekleştirilir. Ardından HTTPS olduğu için TLS üzerinden şifreli güvenli iletişim katmanı ayağa kaldırılır.

**Proxy (Varsa)**: İstek doğrudan sunucuya gitmek yerine aradaki bir Proxy sunucusu üzerinden yönlendirilebilir.

**İstek ve Yanıt**: HTTP isteği sunucuya ulaşır, sunucu talebi işler ve 200 OK koduyla birlikte sayfayı istemciye geri döndürür.

```text
Client ──> (Proxy) ──> (Firewall) ──> (NAT) ──> Internet ──> Server
