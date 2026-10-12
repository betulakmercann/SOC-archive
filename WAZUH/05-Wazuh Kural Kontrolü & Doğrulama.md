# WAZUH'TA KURALLARI YAZDIKTAN SONRA KONTROL

1. Önce kuralınızın doğruluğundan emin olun
   sudo /var/ossec/bin/wazuh-analysisd -t
   Doğrulama: Çıktı olarak "Configuration OK" mesajını alıyorsanız veya ekran tamamen boş dönüyorsa dosyada yazım hatası yoktur.

2. Yeni kuralların yüklenebilmesi için servisi yeniden başlatın
   sudo systemctl restart wazuh-manager
   Doğrulama: Servis hatasız şekilde yeniden başladıysa komut satırına düşersiniz ve sudo systemctl status wazuh-manager komutuyla servis durumunun active (running) olduğunu görebilirsiniz.

3. Wazuh Dashboard üzerinden kontrol edin:
   Arayüze gidin.
   Sol menüden Management sekmesine gelin ve Rules kısmına tıklayın.
   Arama kısmına kendi kural ID'nizi yazın.
   Doğrulama: Kural listede görünüyor ve durumu Enabled (Etkin) olarak işaretliyse canlı sistemde çalışmaya başlamıştır.
