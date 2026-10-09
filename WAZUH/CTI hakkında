# CTI HAKKINDA

Wazuhta CTI ya göre kurallar yazıyoduk peki CTI tam olarak nedir?

Siber Tehdit İstihbaratı (CTI), siber saldırganların kim olduğunu, ne amaçladıklarını, hangi araç ve yöntemleri (TTP) kullandıklarını analiz ederek kurumu saldırılara karşı **önceden hazırlıklı kılma** sürecidir

### 1. CTI Seviyeleri (Üç Ana Katman)

CTI bilgileri hedef kitleye göre üç seviyeye ayrılır:

- **Stratejik (Strategic):** Üst düzey yöneticiler içindir. Yüksek seviyeli trendleri, saldırgan grupların motivasyonlarını ve finansal/risk boyutunu ele alır
- **Taktiksel (Tactical):** Tehdit aktörlerinin teknik yöntemlerini ifade eder. Saldırganların kullandığı teknikler **MITRE ATT&CK** matrisi üzerinden takip edilir 
- **Operasyonel/Teknik (Operational / Technical):** Anlık güvenlik operasyonları ve SOC analistleri içindir. Saldırı belirtilerini (**IoC - Indicators of Compromise**) içerir:
    
    - Zararlı IP adresleri, C2 domainleri
    - Dosya hash değerleri (MD5, SHA256)
    - Zararlı e-posta adresleri veya URL'ler

### 2. Tehdit Piramidi (Pyramid of Pain)

CTI ve SOC analizindeki en ikonik kavramdır. Saldırganın tespit edildiğinde yöntemlerini değiştirmesinin ne kadar zor olduğunu gösterir:

1. **Hash Values (En Kolay):** Saldırgan dosyayı 1 byte değiştirip hash'i değiştirebilir.
2. **IP Addresses:** Saldırgan yeni bir VPN/Proxy IP'sine geçebilir.
3. **Domain Names:** Yeni bir alan adı kaydettirebilir.
4. **Network / Host Artifacts:** Sistemde bıraktığı izleri (registry, log izleri) değiştirmesi biraz daha zordur.
5. **Tools:** Saldırganın kullandığı aracı (örneğin özel bir fidye yazılımı) değiştirmesi zordur.
6. **TTPs (Taktik, Teknik, Prosedürler - En Zor):** Saldırganın çalışma alışkanlığı ve davranış biçimidir. Değiştirmesi saldırgan için en sancılı (painful) olan kısımdır.

### 3. CTI Dili ve Biçimleri (SIEM ve Wazuh Entegrasyonu)

CTI verilerinin güvenlik araçlarına otomatik aktarılması için standart formatlar kullanılır:

- **STIX / TAXII:** CTI verilerinin standartlaştırılmış formatı (STIX) ve bu verinin aktarım protokolüdür (TAXII).
- **MISP (Open Source Threat Intelligence Platform):** SOC ve CTI ekiplerinin zararlı IP, Domain ve Hash listelerini paylaştığı ve topladığı en popüler açık kaynaklı platformdur.
- **Wazuh ile İlişkisi:** MISP veya CTI beslemelerinden (Threat Feeds) gelen zararlı IP ve Hash listeleri Wazuh'a `CDB Lists` veya API entegrasyonuyla yüklenir. Böylece loglarda bilinen zararlı bir IP görüldüğünde Wazuh otomatik olarak yüksek seviyeli alarm üretir.

