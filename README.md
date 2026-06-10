# soc-learning-journey
This repository tracks my SOC learning journey.

## Week 1 - Linux Fundamentals Part 1
### Dosya Arama
- `find -name password.txt` → belirli bir dosyayı arar
- `find -name *.txt` → tüm .txt dosyalarını arar (`*` joker karakter)

### Metin Arama & Sayma
- `wc -l access.log` → dosyadaki satır/kayıt sayısını verir
- `grep "81.143.211.90" access.log` → belirli bir IP'ye ait kayıtları bulur
- `grep -R "PRETTY_NAME" /etc/` → tüm dosya ve alt dizinlerde özyinelemeli arama yapar

### Operatörler
- `&` → komutu arka planda çalıştırır
- `&&` → birden fazla komutu tek satırda çalıştırır
- `>` → komut çıktısını bir dosyaya yönlendirir (üzerine yazar)
- `>>` → komut çıktısını dosyaya ekler (mevcut içeriği korur)


## Week 1 - Linux Fundamentals Part 2
### SSH
- `ssh kullanıcıadı@IP` → başka bir cihazda uzaktan komut çalıştırmayı sağlar

### Dosya Listeleme
- `ls` → çalışma dizininin içeriğini listeler
- `ls -a` → gizli dosyaları da gösterir (`.` ile başlayanlar)
- `ls -l` → long format, dosya hakkında detaylı bilgi gösterir
- `ls -lh` → dosya boyutlarını insan tarafından okunabilir formatta gösterir (KB, MB vb.)

### Yardım Komutları
- `--help` → komutun kısa açıklamasını ve kullanım örneğini verir
- `man <komut>` → o komutun kılavuz sayfasına erişir, tüm seçenekleri listeler

### Dosya & Klasör İşlemleri
- `touch <dosya>` → boş dosya oluşturur (içerik eklemek için `echo` veya `nano` kullanılır)
- `cp <mevcutdosya> <yenidosya>` → dosyayı kopyalar
- `mv <dosya> <hedef>` → dosya veya klasörü taşır, aynı zamanda yeniden adlandırmak için de kullanılır
- `rm <dosya>` → dosyayı siler
- `rm -R <klasör>` → klasörün içine girip her şeyi siler, ardından klasörü de siler
- `file <dosya>` → dosya türünü belirler

> Tüm bu komutlarda tam dosya yolu kullanılabilir: `dizin1/dizin2/dosya`

### İzinler
Linux'ta her dosya ve dizinin okuma, yazma ve çalıştırma izinleri vardır.
-rwxr-xr-x
| Konum | Kimin için |
|-------|-----------|
| İlk 3 | Mal sahibi (owner) |
| Orta 3 | Grup |
| Son 3 | Diğerleri |

Her iznin bir sayısal değeri vardır.
- `r` (read) → 4
- `w` (write) → 2
- `x` (execute) → 1

Her grup için değerler toplanır: `rwxr-xr-x` = **755**
(Sahibi her şeyi yapabilir, diğerleri sadece okuyup çalıştırabilir)

### Kullanıcı Değiştirme
- `sudo su` → root yetkilerine geçiş yapar
- `su <kullanıcıadı>` → belirtilen kullanıcıya geçer (o kullanıcının şifresi gerekir)
- `su -l` → kullanıcının ana dizinine yönlendirerek geçiş yapar

### Kök Dizinler
| Dizin | Açıklama |
|-------|---------|
| `/etc` | Sistem yapılandırma dosyaları |
| `/var` | Servisler tarafından sık erişilen/yazılan veriler (veritabanı vb.) |
| `/root` | Root kullanıcısının ana dizini |
| `/tmp` | Geçici veriler; sistem yeniden başlatılınca içerik silinir — sızma testi için önemli |
| `/var/log` | Günlük (log) dosyaları burada tutulur |


## Week 1 - Linux Fundamentals Part 3
### Terminal Metin Editörleri
- `nano <dosyaadı>` → dosya oluşturur veya düzenler. Çıkış için `Ctrl + X`
- `vim` → daha gelişmiş editör; sözdizimi vurgulama, özelleştirilebilir kısayollar, nano kurulu olmayan terminallerde de çalışır

> `echo` çok satırlı dosyalarda verimli değildir, bunun yerine metin editörü kullanılır.

### Dosya İndirme & Aktarım
**wget** → HTTP üzerinden web'den dosya indirir
```bash
wget https://web-adresi
```

**SCP (SSH ile Güvenli Kopyalama)** → SSH protokolünü kullanarak iki bilgisayar arasında dosya aktarır

Kendi makineden uzak makineye kopyalama:
```bash
scp important.txt ubuntu@IP:/home/ubuntu/transfered.txt
```

Uzak makineden kendi makineye kopyalama:
```bash
scp ubuntu@IP:/home/ubuntu/documents.txt notes.txt
```

**Python HTTP Server** → bilgisayarı hızlıca web sunucusuna dönüştürür, diğer cihazlar `wget` veya `curl` ile dosya indirebilir
```bash
python3 -m http.server        # 8000 portunda başlatır
wget http://IP:8000/myfile    # dosyayı indirir
curl http://IP:8000/myfile    # dosyayı ekrana yazdırır (cat gibi)
```
> wget çalıştırırken Python sunucusunu ayrı bir terminalde başlatmalısın.

### Süreçler (Processes)
Her işlemin kendine ait bir **PID** (Process ID) vardır. Sistemdeki işlemler başlama sırasına göre artan PID değeri alır.

**İşlemleri Görüntüleme**
- `ps` → mevcut oturumdaki işlemleri listeler (PID, TTY, TIME, CMD)
- `ps aux` → tüm kullanıcıların ve sistem işlemlerinin listesi (USER, PID, CPU, MEM vb.)
- `top` → gerçek zamanlı istatistikler, 10 saniyede bir yenilenir

**İşlem Sonlandırma**
```bash
kill 1337    # PID 1337'yi sonlandırır
```

| Sinyal | Açıklama |
|--------|---------|
| `SIGTERM` | İşlemi sonlandır, öncesinde temizlik yapmasına izin ver |
| `SIGKILL` | İşlemi anında sonlandır, temizlik yok |
| `SIGSTOP` | İşlemi durdur, askıya al |

**Arka Plan & Ön Plan İşlemleri**
- `komut &` → komutu arka planda çalıştırır
- `Ctrl + Z` → çalışan işlemi arka plana alır ve duraklatır
- `fg` → arka plandaki işlemi ön plana getirir

### Sistem Başlangıcında Servis Yönetimi
```bash
systemctl start apache2      # servisi başlat
systemctl stop apache2       # servisi durdur
systemctl enable apache2     # sistem açılışında otomatik başlat
systemctl disable apache2    # otomatik başlatmayı kapat
systemctl status apache2     # servis durumunu göster
```

> PID 0 → sistem başlayınca ilk başlatılan işlem (Ubuntu'da `systemd`). Diğer tüm işlemler onun alt işlemi olarak başlar.

### Crontab (Görev Zamanlama)
Belirli zamanlarda otomatik görev çalıştırmayı sağlar.

| Değer | Tanım |
|-------|-------|
| MIN | Hangi dakikada |
| HOUR | Hangi saatte |
| DOM | Ayın hangi günü |
| MON | Yılın hangi ayı |
| DOW | Haftanın hangi günü |
| CMD | Çalıştırılacak komut |

`*` → joker karakter, "her zaman" anlamına gelir

Örnek — her 12 saatte bir yedekleme:
```bash
0 */12 * * * cp -R /home/cmnatic/Documents /var/backups
```

```bash
crontab -e                        # crontab düzenle
crontab -l                        # mevcut kullanıcının crontab'ını listele
sudo crontab -u kullanıcıadı -l   # belirli kullanıcının crontab'ını listele
```

### Paket Yönetimi
**apt** → gelişmiş paket yönetim sistemi

Yazılım kurulurken **GPG anahtarı** ile bütünlük doğrulaması yapılır. Anahtar eşleşmezse yazılım indirilmez.

Örnek kurulum adımları:
```bash
# 1) GPG anahtarı ekle
wget -qO- https://... | sudo apt-key add -

# 2) Depo dosyasını oluştur
sudo nano /etc/apt/sources.list.d/program.list

# 3) Depo adresini dosyaya yapıştır, kaydet ve çık

# 4) Sistemi güncelle
sudo apt update

# 5) Programı kur
sudo apt install program-adi
```

Kaldırma:
```bash
sudo apt remove program-adi                          # programı kaldır
sudo rm /etc/apt/sources.list.d/program.list         # depoyu manuel kaldır
sudo add-apt-repository --remove ppa:PPA_ADI/PPA     # PPA deposunu kaldır
```

### Sistem Kayıtları
- `/var/log` → tüm sistem ve servis kayıtları (log dosyaları) bu dizinde tutulur
