# LINUX TEMELLERİ VE SİSTEM YÖNETİMİ

## 1. Linux File System (Dosya Sistemi Mimarisi)

Linux işletim sisteminde her şey bir dosyayla başlar ve tüm dizinler tek bir kök dizin (`/`) altında hiyerarşik olarak toplanır:

```text
/
├── home/       # Normal kullanıcıların kişisel dosyaları (betul, user vb.)
├── root/       # Root (en yetkili) kullanıcısının kişisel dizini
├── etc/        # Sistem yapılandırmaları (passwd, shadow, ssh vb.)
├── var/log/    # Sistem logları (auth.log, syslog vb.)
├── tmp/        # Geçici dosyalar için kullanılır
├── usr/        # Kullanıcı programları, kütüphaneler, çeşitli sistem bileşenleri
├── bin/        # Temel sistem komutları ve yürütülebilir dosyalar
├── sbin/       # Sistem yöneticisine özel temel komutlar ve ikililer
├── dev/        # Donanım ve cihazla ilgili özel dosyalar
└── opt/        # Opsiyonel üçüncü taraf yazılımlar için kullanılabilir
