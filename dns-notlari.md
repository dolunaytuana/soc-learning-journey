## Week 1 - DNS (Domain Name System)

### DNS Nedir?
DNS, IP adreslerini hatırlamak zorunda kalmadan internetteki cihazlarla iletişim kurmamızı sağlar. Her bilgisayarın kendine özgü bir IP adresi vardır (örneğin `104.26.10.229`). DNS bu sayılar yerine alan adlarını tanır.

---

### Alan Adı Hiyerarşisi

**TLD (Top-Level Domain — Üst Düzey Alan Adı)**
Alan adının en sağındaki kısımdır. Örneğin `github.com`'un TLD'si `.com`'dur.

| Tür | Açıklama | Örnek |
|-----|---------|-------|
| gTLD | Genel amaçlı | `.com`, `.org`, `.net` |
| ccTLD | Ülke bazlı | `.co.uk`, `.com.tr` |

**SLD (Second-Level Domain — İkinci Düzey Alan Adı)**
`github.com`'da `github` kısmıdır. Maksimum 63 karakter, yalnızca harf/rakam ve `-` içerebilir. `-` ile başlayıp bitemez, ardışık `-` içeremez.

**Alt Alan Adı (Subdomain)**
SLD'nin solunda yer alır. Örneğin `admin.tryhackme.com`'da `admin` alt alan adıdır. Birden fazla kullanılabilir: `jupiter.servers.tryhackme.com`. Alan adının tamamı maksimum 253 karakter olabilir.

---

### DNS Kayıt Türleri

| Kayıt | Açıklama | Örnek |
|-------|---------|-------|
| `A` | IPv4 adresine çözümlenir | `104.26.10.229` |
| `AAAA` | IPv6 adresine çözümlenir | `2606:4700:20::681a:be5` |
| `CNAME` | Başka bir alan adına yönlendirir | `store.tryhackme.com` → `shops.shopify.com` |
| `MX` | E-posta sunucusuna yönlendirir | `alt1.aspmx.l.google.com` |
| `TXT` | Serbest metin alanı; alan adı doğrulama ve spam koruması için kullanılır | |

---

### DNS Çözümleme Süreci
Bir URL yazdığımızda arka planda şu adımlar gerçekleşir:

1. **Yerel önbellek (cache)** → Bilgisayar bu adresi daha önce ziyaret edip etmediğini kontrol eder.
2. **Özyinelemeli DNS Sunucusu** → Önbellekte yoksa ISP'nin (veya Google DNS `8.8.8.8` gibi tercih edilen) sunucusuna istek gönderilir. Bu sunucunun da kendi önbelleği vardır.
3. **Kök Sunucular (Root Servers)** → Adresin tam IP'sini bilmez ama TLD uzantısına göre doğru sunucuya yönlendirir.
4. **TLD Sunucuları** → `.com`, `.org` gibi uzantılara ait kayıtları tutar, yetkili sunucunun adresini bilir.
5. **Yetkili DNS Sunucusu (Authoritative DNS)** → Alan adının asıl sahibine ait sunucudur. Gerçek IP adresini (A, AAAA, MX vb.) saklar ve geri gönderir.

> Yedeklilik için her sitenin genellikle birden fazla yetkili sunucusu bulunur.

Yetkili sunucudan gelen cevap özyinelemeli sunucuya iletilir, oradan da bilgisayara. Özyinelemeli sunucu bu bilgiyi önbelleğe alır.

**TTL (Time to Live — Yaşam Süresi)**
Her DNS kaydının saniye cinsinden bir TTL değeri vardır. Bu süre boyunca bilgisayar aynı adres için tekrar DNS sorgusuna çıkmaz, önbellekteki bilgiyi kullanır.

---

### Terminalde DNS Sorgulama

```bash
nslookup url                        # genel sorgu
nslookup --type=A url               # IPv4 adresi
nslookup --type=CNAME url           # CNAME kaydı
nslookup --type=MX url              # e-posta sunucusu
nslookup --type=TXT url             # TXT kaydı
```
