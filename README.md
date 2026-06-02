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
