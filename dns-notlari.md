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



## Week 1 - DNS Pratik: nslookup & traceroute

### nslookup — DNS Sorgusu
Bir alan adının arkasındaki IP adresini ve sorgunun hangi DNS sunucusu üzerinden yapıldığını gösterir.

```bash
nslookup google.com
```
Server:   1.1.1.1

Address:  1.1.1.1#53
Non-authoritative answer:

Name:   google.com

Address: 142.251.142.238

Name:   google.com

Address: 2a00:1450:400f:811::200e

| Çıktı | Açıklama |
|-------|---------|
| `Server: 1.1.1.1` | Sorgunun yönlendirildiği DNS sunucusu (Cloudflare) |
| `#53` | DNS'in standart portu |
| `Non-authoritative answer` | Cevap doğrudan yetkili sunucudan değil, DNS önbelleğinden geldi — normal ve güvenilir bir durum |
| `142.251.142.238` | IPv4 adresi |
| `2a00:1450:400f:811::200e` | IPv6 adresi |

---

### traceroute — Paket Yolu İzleme
Bilgisayarımızdan çıkan paketin hedefe ulaşana kadar hangi yönlendiricilerden geçtiğini ve her duraktaki gecikme süresini (ms) gösterir. Ağdaki darboğazları tespit etmek için kullanılır.

```bash
traceroute google.com
```
1  10.0.2.1  ~1.5 ms        # ilk durak: sanal ağ geçidi

2  * * *

3  * * *

...

30  * * *

**`* * *` ne anlama gelir?**
Yol üzerindeki yönlendiriciler güvenlik duvarı (firewall) veya IDS/IPS kuralları nedeniyle UDP paketlerini yanıtlamaz. Ağ topolojisini gizlemek ve keşif (reconnaissance) faaliyetlerini engellemek için yaygın bir yapılandırmadır.

---

### Firewall'ı Aşmak: Alternatif Protokoller

**ICMP (ping) ile traceroute** — root yetkisi gerektirir
```bash
sudo traceroute -I google.com
```
Standart UDP'yi engelleyen ancak ICMP'ye açık güvenlik duvarlarında çalışır.

**TCP SYN ile traceroute** — web trafiğini taklit eder
```bash
sudo traceroute -T -p 80 google.com
```
Port 80 üzerinden normal tarayıcı trafiği gibi görünür. En sıkı firewall kurallarında bile çalışma ihtimali yüksektir.

---

### SOC Açısından Önemi

| Kullanım Alanı | Açıklama |
|---------------|---------|
| Keşif (Reconnaissance) | Hedef ağ topolojisini haritalamak için ilk adım |
| Firewall Profiling | Paketin hangi durakta düşürüldüğünü analiz ederek firewall/IDS konumunu tespit etme |
| Zafiyet Analizi | İfşa olan yönlendirici IP'leri üzerinden Nmap gibi araçlarla tarama genişletme |
| MitM Tespiti | Trafiğin standart rotadan sapıp sapmadığını denetleyerek yetkisiz yönlendirmeleri tespit etme |
