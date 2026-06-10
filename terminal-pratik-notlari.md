## Week 1 - Terminal Komut Pratiği
### Operatörler
- `&&` → iki komutu birleştirir, ilk komut başarılıysa ikincisini çalıştırır
- `|` (pipe) → bir komutun çıktısını diğer komuta girdi olarak verir
- `>` → komut çıktısını dosyaya yönlendirir (üzerine yazar)

### echo
- `echo "metin" > dosyaadi` → dosya oluşturup içine metin yazar
- `echo -e` → `\n` (yeni satır), `\t` (tab) gibi özel kaçış karakterlerini yorumlar
- `dquote>` → tırnak açıldı ama kapatılmadı demek, `Ctrl + C` ile çıkılır

### grep
- `grep -i "hata" sunucu.log` → büyük/küçük harf duyarlılığını ortadan kaldırır
- `grep -n "hata" sunucu.log` → eşleşen satırları satır numarasıyla gösterir

### tail
- `tail -n 2 sunucu.log` → dosyanın son 2 satırını gösterir
- `tail -f sunucu.log` → canlı akan log dosyasını anlık takip eder, yeni satır eklenince terminalde görünür (`Ctrl + C` ile çıkış)

### find
- `find . -name "*.txt"` → `.` ile gösterilen mevcut klasörde arama yapar

### rm
- `rm -r klasöradi` → klasörün içine özyinelemeli olarak girip her şeyi siler
- `rm -f dosyaadi` → onay sormadan zorla siler
- `rm -rf klasöradi` → içi dolu klasörü soru sormadan tamamen siler

> ⚠️ `rm -rf` geri alınamaz, dikkatli kullan!
