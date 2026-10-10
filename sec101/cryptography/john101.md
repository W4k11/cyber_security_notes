# John the Ripper (JtR) Kapsamlı Not Kartı ve Kullanım Kılavuzu

## 1. Temel Sözdizimi ve Parametreler

Bash

```
john [seçenekler] [hash_dosyası]
```

- **`john`**: Aracı çalıştıran ana komut.
    
- **`--wordlist=<dosya_yolu>`**: Sözlük saldırısında kullanılacak kelime listesini belirtir _(Örn: `/usr/share/wordlists/rockyou.txt`)_.
    
- **`--format=<format_adı>`**: Hedef hash algoritmasını manuel olarak tanımlar _(Örn: `raw-md5`, `raw-sha256`, `bcrypt`)_.
    
- **`--show <hash_dosyası>`**: Daha önce kırılmış parolaları ve kullanıcı eşleşmelerini ekrana listeler.
    

### Standart Sözlük Saldırısı Örneği:

Bash

```
john --wordlist=/usr/share/wordlists/rockyou.txt --format=raw-md5 hash07.txt
```

## 2. Tekil Mod (`--single`) Mantığı

`--single` modu; hedef dosyadaki kullanıcı adları, gecos alanları veya kullanıcı kimlik bilgilerinden yola çıkarak akıllı mutasyonlar üretir.

- **Kullanım Amacı:** Kalabalık bir parola dosyasında, kullanıcı adına benzer tahmin edilebilir parolaları hızlıca yakalamak veya yalnızca tek bir girdiyi analiz etmek için tercih edilir.
    

Bash

```
john --single --format=raw-md5 hash07.txt
```

## 3. Özel Kurallar (Custom Rules) ve Kural Tabanlı Saldırılar

Kullanıcılar kurumsal parola politikalarına uyum sağlamak için genellikle sabit bir şablon izler:

$$\text{polopassword} \longrightarrow \text{Polopassword1!}$$

_(İlk harf büyük + taban kelime + rakam + özel sembol)_

Saldırganlar bu insan davranışını kırmak için John yapılandırma dosyasına özel kural kümeleri ekler.

### Yapılandırma Dosyası Konumu (`john.conf`):

- **Paket Yöneticisi Kurulumu (Debian/Ubuntu/Kali):** `/etc/john/john.conf`
    
- **Kaynak Koddan Derleme / TryHackMe:** `/opt/john/john.conf`
    

### Kural Tanımlama Sözdizimi:

Yeni bir kural kümesi tanımlamak için `john.conf` dosyasına başlık ve değiştirici kalıpları yazılır:

Ini, TOML

```
[List.Rules:THMRules]
c Az"[0-9]" Az"[!£$%@]"
```

- **`[List.Rules:KuralAdi]`**: Komut satırında çağrılacak kuralın etiketidir.
    
- **`c`**: İlk karakteri büyük harfe dönüştürür (_capitalize_).
    
- **`Az`**: Belirtilen karakter kümesini kelimenin **sonuna** ekler (_append_).
    
- **`A0`**: Belirtilen karakter kümesini kelimenin **başına** ekler (_prepend_).
    

### Karakter Kümeleri:

|**Karakter Kümesi**|**Açıklama**|
|---|---|
|`[0-9]`|0 ile 9 arasındaki tüm rakamları dener.|
|`[0]`|Sadece 0 rakamını ekler.|
|`[A-z]`|Hem büyük hem küçük tüm Latin harflerini dener.|
|`[A-Z]`|Sadece büyük harfleri dener.|
|`[a-z]`|Sadece küçük harfleri dener.|
|`[!£$%@]`|Belirtilen özel sembolleri sırayla dener.|

### Özel Kuralı Çalıştırma:

Bash

```
john --wordlist=rockyou.txt --rule=THMRules hash.txt
```

## 4. Dosya Formatı Dönüştürücüleri (*2john Araçları)

John the Ripper şifreli dosyaları doğrudan kıramaz; önce dosyanın başlığındaki parola doğrulama karmasını John'un anlayacağı formata döken dönüştürücüler (`*2john`) kullanılır.

Plaintext

```
[Şifreli Dosya] ──► [*2john Dönüştürücü] ──► [Hash Çıktısı (TXT)] ──► [john --wordlist] ──► Parola
```

### A) Parola Korumalı ZIP Dosyalarını Kırma (`zip2john`)

Bash

```
# 1. ZIP dosyasından hash değerini çıkart
zip2john secure.zip > zip_hash.txt

# 2. Hash dosyasını John ile kır
john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt

# 3. Kırılan şifreyi kullanarak arşivden dosyaları çıkar
unzip secure.zip
```

### B) Parola Korumalı RAR Dosyalarını Kırma (`rar2john`)

Bash

```
# 1. RAR dosyasından hash değerini çıkart
rar2john secure.rar > rar_hash.txt

# 2. Hash dosyasını kır
john --wordlist=/usr/share/wordlists/rockyou.txt rar_hash.txt

# 3. Arşivi aç
unrar x secure.rar
```

### C) Parola Korumalı SSH Private Key (id_rsa) Kırma (`ssh2john`)

SSH oturumlarına giriş yapmak için kullanılan şifreli özel anahtar dosyalarını (`id_rsa`) kırmak için kullanılır:

- **Sistemde Doğrudan Mevcutsa:**
    
    Bash
    
    ```
    ssh2john id_rsa > ssh_hash.txt
    ```
    
- **Python Scripti Olarak Çağrılması Gerekiyorsa:**
    
    - **Kali Linux:**
        
        Bash
        
        ```
        python /usr/share/john/ssh2john.py id_rsa > ssh_hash.txt
        ```
        
    - **TryHackMe / AttackBox:**
        
        Bash
        
        ```
        python3 /opt/john/ssh2john.py id_rsa > ssh_hash.txt
        ```
        
- **Kırma Aşaması:**
    
    Bash
    
    ```
    john --wordlist=/usr/share/wordlists/rockyou.txt ssh_hash.txt
    ```