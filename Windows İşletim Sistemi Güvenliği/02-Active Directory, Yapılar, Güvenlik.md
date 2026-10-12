# WINDOWS ACTIVE DIRECTORY
AD, microsoft tarafından tasarlanmıştır. İçinde sunucu, client pc, kullanıcı ve yazıcı gibi bilgileri tutar. Özellikle windows server ve client pc sistemleri için tasarlanmıştır

bu servis içerisinde Group Policy yönetim aracı bulunur. Bu araçla çeşitli kısıtlamalar yapılabilir, tek bir noktadan istediğimiz uygulamanın dağıtımını yapabiliriz

AD servisi, "domain controller" olarak adlandırılan sunucular üzerinde tutulur. Veritabanı dosya ismi *ntds.dit* dosyasıdır. ayrıca *edb.log* dosyası da kritiktir. bu dosyada tüm işlemler yer alır, veritabanına yazılmadan önce burda saklanır. 

## AD MANTIKSAL YAPISI
ADnin mantıksal katmanı veri deposunda bulunan bilgilerin nasıl görüleceğini belirler ve o bilgilere erişimi denetler. 

domain, domain tree, forest, organizational unit, global catalog, LDAP yapısı bulunur.

LDAP: tcp/ip üzerinde çalışır. servisleri sorgulama ve değiştirmek için kullanılan uyglama protokoludur. 

## AD FİZİKSEL YAPI
AD içinde fiziksel yapı mantıksak yapıdan bağımsız mimariye sahiptir. mantıksal yapıyla network kaynakları organize edilir, fiziksel yapıyla ise ağ tarfiğinin kontrolü ve konfigürasyonu gerçekleştirilebilir. 

site ve domain controller bulunur.

DC: AD servisinin kurulu olduğu sunucudur, burda tüm kullanıcı ve pclerin kimlik doğrulamaları yapılır
SİTE: farklı yerlerde bulunan domain controllerlar arasındaki veri akışını optimize eder

## AD GÜVENLİK SORUNLARI
Doğru kurgulanmadığında güvenlik sorunları oluşabilir. parola politikaları, hesap yetkileri, logloma yöntemleri ve aralıklarına dikkat edilmelidir.

## AD GÜVENLİĞİ SAĞLAMA YÖNTEMLERİ
hesap bilgilerinin kontrol edilmesi ve değiştirilmesi
kritik grup üyeliklerinin kontrol edilmesi
yapı içindeki kullanıcıların hesaplarının takibi
kimlik doğrulama 
lackout yöntemi

## AD Loglama
yapılan tüm eylemlerin kayıt altına alınmasına logloma yöntemi denir. bu sayede tüm eylemler denetlenebilir e kritik durumların tespiti yapılabilir. bu yapıyı belirli aralıklarla takip etmemiz ve incelememiz gerekir
