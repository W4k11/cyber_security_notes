# Kriptografik Karma (Hash) Fonksiyonları ve Parola Güvenliği

## 1. Hash Fonksiyonu Nedir? (Şifrelemeden Farkı)

Hash (özet/karma) fonksiyonu; değişken boyuttaki herhangi bir veriyi (tek bir harf, metin belgesi, ISO kalıbı veya terabaytlarca dosya) alıp, **sabit uzunlukta** benzersiz bir karakter dizisine dönüştüren matematiksel bir algoritmadır.

### Şifreleme (Encryption) ile Temel Farkı:

- **Şifreleme:** İki yönlüdür. Bir anahtar (key) kullanılır; şifrelenen veri doğru anahtarla tekrar orijinal haline (**decryption**) döndürülebilir.
    
- **Hash:** Tek yönlüdür (**One-way function**). Bir anahtar yoktur; üretilen özet değerden orijinal girdiye geri dönmek matematiksel ve hesaplama açısından pratik olarak imkânsızdır.
    

## 2. Hash Fonksiyonlarının Temel Karakteristikleri

- **Tek Yönlü (Pre-image Resistance):** Hash değerine bakarak orijinal veriyi bulmak imkânsız kabul edilir.
    
- **Belirlenimci (Deterministic):** Aynı girdi, aynı hash algoritmasından geçirildiğinde dünyanın neresinde olursa olsun her zaman birebir aynı çıktıyı verir.
    
- **Sabit Boyutlu Çıktı:** Girdi boyutu ne olursa olsun çıktı uzunluğu sabittir (Örn: SHA-256 her zaman 256 bit / 64 hex karakter üretir).
    
- **Çığ Etkisi (Avalanche Effect):** Girdide yapılan en ufak bir değişiklik (tek bir harfin büyümesi, fazladan bir nokta konması) çıktıdaki bitlerin neredeyse tamamını rastgele değiştirir:
    
    - `merhaba` $\rightarrow$ `2b3a886f34589d903e00318610534289...`
        
    - `Merhaba` $\rightarrow$ `7e738c62c3e1e909bfeb98539659f81a...`
        
- **Çarpışma Direnci (Collision Resistance):** Matematiksel olarak farklı iki girdinin aynı hash çıktısını üretmesine **çarpışma (collision)** denir. Güvenli bir hash fonksiyonunda bu ihtimal ihmal edilebilir düzeyde sıfıra yakındır.
    

> **Olası Hash Değeri Sayısı:**
> 
> $x$ bitlik bir hash çıktısı için üretilebilecek benzersiz değer sayısı **$2^x$** kadardır. Örneğin SHA-256 için bu değer $2^{256}$ olup evrendeki atom sayısından fazladır.

## 3. Popüler Hash Algoritmaları Karşılaştırma Tablosu

|**Algoritma**|**Çıktı (Bit)**|**Çıktı (Bayt)**|**Hex Karakter Sayısı**|**Güvenlik Durumu**|
|---|---|---|---|---|
|**MD5**|128 bit|16 bayt|32 karakter|❌ Güvensiz (Çarpışma zafiyeti var)|
|**SHA-1**|160 bit|20 bayt|40 karakter|❌ Güvensiz (Kriptografik olarak kırıldı)|
|**SHA-224**|224 bit|28 bayt|56 karakter|⚠️ Güvenli (Az tercih edilir)|
|**SHA-256 (SHA-2)**|256 bit|32 bayt|64 karakter|✅ Standart ve Güvenli|
|**SHA-384**|384 bit|48 bayt|96 karakter|✅ Yüksek Güvenlik|
|**SHA-512 (SHA-2)**|512 bit|64 bayt|128 karakter|✅ Çok Yüksek Güvenlik|
|**SHA3-256**|256 bit|32 bayt|64 karakter|✅ Modern Standart (Keccak Mimarisi)|
|**SHA3-512**|512 bit|64 bayt|128 karakter|✅ Modern ve Çok Yüksek Güvenlik|
|**BLAKE2b**|512 bit _(özelleştirilebilir)_|64 bayt|128 karakter|✅ Çok Hızlı ve Güvenli|
|**BLAKE3**|256 bit _(özelleştirilebilir)_|32 bayt|64 karakter|✅ Aşırı Hızlı ve Güvenli|

## 4. Hash'leri Güvenli Kılan Mantık (P vs. NP İlişkisi)

Bilgisayar biliminde algoritmaların işlem karmaşıklığı P ve NP sınıflarıyla modellenir:

- **P Sınıfı (Polinom Zamanı):** Çözümü makul bir sürede hesaplanabilen problemlerdir. (Örneğin bir veri dizisini sıralamak).
    
- **NP Sınıfı (Deterministik Olmayan Polinom Zamanı):** Çözümü bulmak çok zor olsa bile, **verilen bir çözümün doğruluğunu test etmenin son derece hızlı olduğu** problemlerdir.
    
- **Hash İlişkisi:** Girdiyi alıp hash çıktısını hesaplamak son derece hızlıdır (doğrulama anlıktır). Ancak hash çıktısından yola çıkıp orijinal girdiyi bulmak NP benzeri tersine çevrilemez hesaplama duvarına çarpar.
    

## 5. Linux Sistemlerinde Parola Güvenliği (`/etc/shadow`)

Tarihsel süreçte Linux sistemlerde parolalar herkesin okuyabildiği `/etc/passwd` dosyasında tutulurken; modern sistemlerde yalnızca `root` kullanıcısının erişebildiği `/etc/shadow` dosyasına taşınmıştır.

### Parola Hash Formatı

Shadow dosyasındaki satırlar iki nokta (`:`) ile ayrılmış alanlardan oluşur. Parola alanı şu 4 bileşeni içerir:

Plaintext

```
$önek$seçenekler$tuz$karma_değer
($prefix$options$salt$hash)
```

- **Tuz (Salt):** Her kullanıcıya özel eklenen rastgele veri parçasıdır. Aynı parolayı kullanan iki kullanıcının hash değerlerinin farklı olmasını sağlar; Rainbow Table saldırılarını tamamen engeller.
    

### Algoritma Önekleri (Prefix Tablosu)

|**Önek**|**Algoritma**|**Açıklama**|
|---|---|---|
|**`$y$`**|**yescrypt**|Yeni Linux dağıtımlarında varsayılan ve önerilen ölçeklenebilir modern şema.|
|**`$gy$`**|**gost-yescrypt**|GOST R 34.11-2012 fonksiyonunu ve yescrypt yapısını kullanan model.|
|**`$7$`**|**scrypt**|Bellek-yoğun (memory-hard) parola türetme fonksiyonu.|
|**`$2b$`, `$2y$`, `$2a$`**|**bcrypt**|Blowfish tabanlı, yavaşlatma faktörüne sahip güçlü algoritma.|
|**`$6$`**|**sha512crypt**|SHA-512 tabanlı GNU libc standardı hash formatı.|
|**`$1$`**|**md5crypt**|Eski FreeBSD kaynaklı güvensiz MD5 türevi.|
|**`$md5$`**|**SunMD5**|Solaris sistemler için geliştirilmiş hash algoritması.|

## 6. Hash'ler Nasıl Kırılır? (Sözlük Saldırısı ve John the Ripper)

Hash algoritması matematiksel olarak tersine çevrilemez; fakat **kaba kuvvet (Brute-Force)** ve **Sözlük Saldırısı (Dictionary Attack)** ile kırılabilir.

Plaintext

```
[Wordlist (rockyou.txt)] ──► [Aynı Hash Fonksiyonu] ──► [Aday Hash]
                                                              │
                                                              ▼
           Eşleşti mi? ◄─────────────────────────── [Hedef Hash]
           ├── Evet ──► Parola Tespit Edildi!
           └── Hayır ──► Sıradaki Kelimeye Geç
```

Saldırgan elindeki parola listesindeki (örn: `rockyou.txt`) her bir kelimenin hedef algoritmayla hash değerini hesaplar. Elde edilen değer hedefin hash'i ile birebir eşleşirse, parolanın asıl metni çözülmüş olur.

### Pratik Linux Kelime Listesi Komutları

- **Dosya Satır Sayısını Öğrenme:**
    
    Bash
    
    ```
    wc -l rockyou.txt
    ```
    
- **İlk Belirli Sayıda Satırı Okuma:**
    
    Bash
    
    ```
    head -n 50 rockyou.txt
    ```