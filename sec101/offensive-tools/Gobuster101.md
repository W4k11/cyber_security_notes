# Gobuster: Web Dizin, DNS ve Sanal Sunucu (VHost) Numaralandırma

Gobuster, web sunucularında gizli dizin ve dosyaları, DNS kayıtlarını ve sanal sunucuları (_Virtual Hosts_) sözlük tabanlı (_wordlist brute-force_) yöntemlerle keşfetmek amacıyla Go diliyle geliştirilmiş bir numaralandırma aracıdır.

## 1. Numaralandırma Modları Genel Bakış

Gobuster farklı hedef katmanlarına göre ayrılmış bağımsız modüller üzerinden çalışır:

|**Mod**|**Komut**|**Kullanım Amacı**|**Temel Mantık**|
|---|---|---|---|
|**Dizin Keşfi**|`gobuster dir`|Web dizin ve dosya taraması|HTTP GET/HEAD istekleriyle durum kodlarını (`200`, `301`, `403` vb.) kontrol eder.|
|**DNS Keşfi**|`gobuster dns`|Alt alan adı (_subdomain_) keşfi|Hedef alan adı için belirtilen kelime listesiyle DNS `A` kayıtlarını sorgular.|
|**VHost Keşfi**|`gobuster vhost`|Sanal sunucu (_Virtual Host_) keşfi|Aynı IP adresine farklı HTTP `Host` başlıkları göndererek yanıt boyutu ve durumlarını inceler.|

## 2. Dizin Numaralandırma (`dir` Modu)

Web sunucusundaki indekslenmemiş veya gizlenmiş sayfaları, yönetim panellerini ve yedek dosyaları tespit etmek için kullanılır.

### Sözdizimi

Bash

```
gobuster dir -u <HEDEF_URL> -w <WORDLIST_YOLU> [PARAMETRELER]
```

### Örnek Kullanım ve Yaygın Parametreler

Bash

```
gobuster dir -u "http://example.com" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 50 -x php,txt,html
```

- `-u, --url`: Hedef web adresi (URL).
    
- `-w, --wordlist`: Kullanılacak kelime listesinin tam dosya yolu.
    
- `-t, --threads`: Eşzamanlı iş parçacığı sayısı (varsayılan: 10).
    
- `-x, --extensions`: Aranacak dosya uzantıları (örn. `php`, `html`, `bak`, `txt`).
    
- `-s, --status-codes`: Pozitif kabul edilecek HTTP durum kodları (varsayılan: `200,204,301,302,307,401,403`).
    
- `-k, --no-tls-validation`: Kendinden imzalı (_self-signed_) SSL/TLS sertifikalarında doğrulamayı atlar.
    

## 3. DNS Numaralandırma (`dns` Modu)

Bir etki alanına ait alt alan adlarını (FQDN) brute-force yöntemiyle tespit eder.

### Sözdizimi

Bash

```
gobuster dns -d <ALAN_ADI> -w <WORDLIST_YOLU> [PARAMETRELER]
```

### Örnek Kullanım

Bash

```
gobuster dns -d example.com -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -r 1.1.1.1
```

- `-d, --domain`: Numaralandırma yapılacak kök etki alanı (_root domain_).
    
- `-r, --resolver`: Sorguların yönlendirileceği özel DNS sunucusu (örn. `1.1.1.1` veya `8.8.8.8`).
    
- `-c, --show-cname`: Alt alan adlarına ait `CNAME` kayıtlarını da terminal çıktısında gösterir.
    
- `--wildcard`: Wildcard DNS yapılandırmasını tespit ederek sahte pozitiflerin filtrelenmesini sağlar.
    

## 4. Sanal Sunucu Numaralandırma (`vhost` Modu)

Sanal sunucular (_Virtual Hosts_), tek bir fiziksel sunucu ve tek bir IP adresi üzerinde birden fazla web sitesinin barındırılmasına imkan tanır. Sunucu, gelen isteğin HTTP `Host` başlığına bakarak ilgili siteye yönlendirme yapar.

### DNS Modu ile VHost Modu Arasındaki Temel Fark

Plaintext

```
DNS Modu:   [Kelime] + [Alan Adı]  --->  DNS Sunucusuna 'A' Kaydı Sorgusu (FQDN Çözümleme)
VHost Modu: [Hedef IP/URL]         --->  HTTP GET İsteği (Host: [Kelime].[Alan Adı])
```

- **DNS Modu:** Doğrudan DNS sunucularına sorgu atar; yalnızca DNS kayıtlarında tanımlı olan adresleri bulabilir.
    
- **VHost Modu:** Doğrudan hedef web sunucusunun IP adresine bağlanır ve HTTP istek başlığındaki `Host` değerini kelime listesinden gelen girdilerle değiştirir. DNS üzerinde herkese açık olarak yayınlanmamış iç/test sanal sunucularını bulmak için tercih edilir.
    

### Sözdizimi ve Parametreler

Bash

```
gobuster vhost -u <HEDEF_URL_VEYA_IP> --domain <ALAN_ADI> -w <WORDLIST_YOLU> [PARAMETRELER]
```

### Örnek Kullanım

Bash

```
gobuster vhost \
  -u "http://192.0.2.10" \
  --domain example.com \
  -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt \
  --append-domain \
  --exclude-length 250-320
```

- `-u, --url`: Hedef IP adresi veya ana sunucu adresi.
    
- `--domain`: Alt alan adı eklenecek ana etki alanı.
    
- `--append-domain`: Kelime listesindeki girdinin sonuna `--domain` ile tanımlanan alan adını ekler (örn. `test` girdisi `test.example.com` haline gelir).
    
- `--exclude-length`: Belirtilen yanıt uzunluklarını filtreler. Varsayılan hata sayfaları veya standart yönlendirmeler genelde sabit boyutta döner; bu aralık elenerek yalnızca farklı içerik döndüren geçerli sanal sunucular ayıklanır.
    

## 5. Savunma ve Tespit Mekanizmaları (Defensive Context)

- **Hız ve İstek Sınırlama (Rate Limiting):** Web sunucusu veya ters vekil (Nginx, Cloudflare) seviyesinde IP başına saniyelik istek limiti uygulanarak otomatik kaba kuvvet araçları engellenir.
    
- **WAF ve Log Analizi:** Yüksek hacimli HTTP `404 Not Found` yanıtları ve standart dışı `User-Agent` başlıkları SIEM sistemleri üzerinden alarm olarak izlenir.
    
- **DNS Wildcard Yönetimi:** Açıkta kalan veya yetkisiz alt alan adlarının harici sistemlerce haritalandırılmasını zorlaştırmak için merkezi DNS kayıt politikaları denetlenir.