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
