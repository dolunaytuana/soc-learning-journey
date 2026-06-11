## Week 1 - Log Analizi Pratiği

### /var/log Klasörü
Linux'ta işletim sistemi, servisler ve programlarla ilgili her şey `/var/log` altına kaydedilir.

```bash
cd /var/log
ls -lh    # dosyaları boyutlarıyla birlikte okunabilir formatta listeler
```

> `.gz` uzantılı ve kırmızı görünen dosyalar, Linux'un yer kazanmak için eski log dosyalarını otomatik sıkıştırmasından kaynaklanır.

---

### Örnek 1: macchanger.log (cat & grep)
`macchanger` → ağ kartının MAC adresini değiştirmek için kullanılır (ağ gizliliği ve iz bırakmama).

```bash
cat macchanger.log                  # dosya içeriğini ekrana basar
grep -i "changed" macchanger.log    # "changed" geçen satırları filtreler
grep -i "wlan0" macchanger.log      # belirli ağ arayüzüne ait kayıtları bulur
```

---

### Örnek 2: boot.log (tail)
`boot.log` → sistem açılırken yüklenen servislerin kaydını tutar.

```bash
tail -n 15 boot.log    # en son 15 satırı gösterir
```

---

### Örnek 3: .log Dosyalarını Bulmak (find)
`/var/log` altında nginx, postgresql gibi alt klasörler de bulunur (mavi renkli olanlar klasördür).

```bash
find . -name "*.log"    # mevcut klasör ve alt klasörlerdeki tüm .log dosyalarını listeler
```

> `*` joker karakteri — başı ne olursa olsun, sonu `.log` ile bitenleri bul demek.

---

### Örnek 4: dpkg.log (pipe | ile filtreleme)
`dpkg.log` → sistemdeki paket yüklemelerinin günlüğüdür. Hangi programlar yüklendi, silindi veya güncellendi — hepsi burada.

```bash
cat dpkg.log | grep "installed"    # sadece başarıyla kurulmuş paketleri gösterir
```

---

### nginx Klasörü (Web Sunucu Logları)
`/var/log/nginx` → nginx web sunucusuna gelen tüm istekler, saldırılar ve taramalar `access.log` ve `error.log` dosyalarına yazılır.

```bash
tail -f nginx/access.log    # web sitesine gelen istekleri canlı olarak izler
```

---

### inetsim Klasörü (Zararlı Yazılım Analizi)
`inetsim` → internet servisleri simülatörü. Lab ortamında zararlı yazılım analiz ederken, yazılımın internete bağlanmaya çalışıp çalışmadığını simüle etmek için kullanılır.

```bash
find nginx/ inetsim/ -name "*.log"    # her iki klasördeki log dosyalarını listeler
```

---

### btmp & wtmp (Giriş Kayıtları)
Bu dosyalar binary (ikili) formattadır, `cat` ile okunamaz.

| Dosya | İçerik | Komut |
|-------|--------|-------|
| `wtmp` | Başarılı giriş geçmişi | `last` |
| `btmp` | Başarısız giriş denemeleri (brute force tespiti için) | `lastb` |

---

### Kısayollar
| Kısayol | Açıklama |
|---------|---------|
| `q` | `less`, `journalctl` gibi araçlardan çıkış |
| `Ctrl + C` | `tail -f` gibi canlı takip komutlarını durdurur |
