Karma fonksiyonlar şifrelemeden farklıdır. Anahtar yoktur ve çıktının girdiye geri dönmesinin imkansız (veya hesaplama açısından pratik olmayan) olması amaçlanmıştır. 

Bir karma fonksiyonu, herhangi bir boyuttaki girdi verilerini alır ve bu verilerin bir özetini veya özetlenmiş halini oluşturur.

Çıktının boyutu sabittir. Herhangi bir girdi için çıktıyı tahmin etmek zordur ve bunun tersi de geçerlidir. İyi karma algoritmaları, hesaplama açısından nispeten hızlı, ancak tersine çevirme açısından (yani çıktıdan girdiyi belirleme açısından) son derece yavaş olacaktır. Girdi verilerindeki en ufak bir değişiklik, hatta tek bir bit bile, çıktıda önemli bir değişikliğe neden olmalıdır.

| **Karma Algoritması** | **Çıktı Boyutu (Bit)**    | **Çıktı Boyutu (Bayt)**   | **Hex Karakter Sayısı** | **Mevcut Güvenlik Durumu**               |
| --------------------- | ------------------------- | ------------------------- | ----------------------- | ---------------------------------------- |
| **MD5**               | 128 bit                   | 16 bayt                   | 32 karakter             | ❌ Güvensiz (Çarpışma zayıflığı var)      |
| **SHA-1**             | 160 bit                   | 20 bayt                   | 40 karakter             | ❌ Güvensiz (Kriptografik olarak kırıldı) |
| **SHA-224**           | 224 bit                   | 28 bayt                   | 56 karakter             | ⚠️ Güvenli (Az tercih edilir)            |
| **SHA-256** (SHA-2)   | 256 bit                   | 32 bayt                   | 64 karakter             | ✅ Standart / Güvenli                     |
| **SHA-384**           | 384 bit                   | 48 bayt                   | 96 karakter             | ✅ Yüksek Güvenlik                        |
| **SHA-512** (SHA-2)   | 512 bit                   | 64 bayt                   | 128 karakter            | ✅ Çok Yüksek Güvenlik                    |
| **SHA3-256**          | 256 bit                   | 32 bayt                   | 64 karakter             | ✅ Modern Standart (Keccak)               |
| **SHA3-512**          | 512 bit                   | 64 bayt                   | 128 karakter            | ✅ Modern / Çok Yüksek Güvenlik           |
| **BLAKE2b**           | 512 bit _(ayarlanabilir)_ | 64 bayt _(ayarlanabilir)_ | 128 karakter            | ✅ Çok Hızlı & Güvenli                    |
| **BLAKE3**            | 256 bit _(ayarlanabilir)_ | 32 bayt _(ayarlanabilir)_ | 64 karakter             | ✅ Aşırı Hızlı & Güvenli                  |

olası karma değeri hesaplamak için:

x bitlik karma değeri varsa.

2^x
dir.

linuxta bir dosyayı okumak istediğimizde çok fazla satırdan oluşuyorsa ve ilk satır ile spesifik seçtiğimiz satır aralığındaki dosyaları getirmek istiyorsak. 

```bash
head -n satır_sayısı rockyou.txt
```

kaç satırdan oluştuğunu öğrenmek istiyorsak

```bash
wc -l rockyou.txt
```

Sistemde parola karmaları, normalde yalnızca root tarafından okunabilen /etc/shadow dosyasında saklanır. Eskiden ise herkes tarafından okunabilen /etc/passwd dosyasında saklanırdı.
Shadow dosyası parola bilgilerini içerir. Her satırda iki nokta üst üste (:) ile ayrılmış dokuz alan bulunur. İlk iki alan, kullanıcı adı ve şifrelenmiş paroladır. Diğer alanlar hakkında daha fazla bilgi için Linux sisteminde `man 5 shadow` komutunu çalıştırabilirsiniz. 
Şifrelenmiş parola alanı, dört bileşenden oluşan karma parolayı içerir: önek (algoritma kimliği), seçenekler (parametreler), tuz ve karma değer. `$prefix$options$salt$hash` biçiminde kaydedilir. Önek, Unix ve benzeri parolaları kolayca tanımayı sağlar; karma değeri oluşturmak için kullanılan karma algoritmasını belirtir.

|Prefix|Algorithm|
|---|---|
|`$y$`|yescrypt is a scalable hashing scheme and is the default and recommended choice in new systems|
|`$gy$`|gost-yescrypt uses the GOST R 34.11-2012 hash function and the yescrypt hashing method|
|`$7$`|scrypt is a password-based key derivation function|
|`$2b$`, `$2y$`, `$2a$`, `$2x$`|bcrypt is a hash based on the Blowfish block cipher originally developed for OpenBSD but supported on a recent version of FreeBSD, NetBSD, Solaris 10 and newer, and several Linux distributions|
|`$6$`|sha512crypt is a hash based on SHA-2 with 512-bit output originally developed for GNU libc and commonly used on (older) systems|
|`$md5`|SunMD5 is a hash based on the algorithm originally developed for Solaris|
|`$1$`|md5crypt is a hash based on the MD5 algorithm originally developed for FreeBSD|

## Hash'leri Güvenli Kılan Nedir?

Karma fonksiyonları tek yönlü fonksiyonlar olarak tasarlanmıştır. Başka bir deyişle, verilen bir girdinin karma değerini hesaplamak kolaydır; ancak, karma değeri verildiğinde orijinal girdiyi bulmak zor bir problemdir. Basitçe ifade etmek gerekirse, zor bir problem bilgisayar biliminde hızla hesaplama açısından imkansız hale gelir. Bu hesaplama problemi, matematikte P ve NP problemlerine dayanmaktadır.

Bilgisayar biliminde, P ve NP, algoritmaların verimliliğini anlamamıza yardımcı olan iki problem sınıfıdır:

- **P (Polinom Zamanı)** : P sınıfı, çözümü polinom zamanında bulunabilen problemleri kapsar. Artan sırada bir listeyi sıralamayı düşünün. Liste ne kadar uzun olursa, sıralama o kadar uzun sürer; ancak, zamandaki artış üstel değildir.
- **NP (Belirsiz Polinom Zamanlı)** : NP sınıfındaki problemler, çözümün kendisinin bulunması zor olsa bile, verilen bir çözümün hızlı bir şekilde kontrol edilebildiği problemlerdir. Aslında, çözümün hızlı bir şekilde bulunup bulunmadığını bile bilmiyoruz.

**Hash algoritması**, değişken uzunluktaki herhangi bir veriyi (metin, dosya, şifre vb.) alıp, **sabit uzunlukta** benzersiz bir karakter dizisine (özet/hash değeri) dönüştüren matematiksel bir fonksiyondur.

Girdi ne kadar büyük olursa olsun (tek bir harf veya terabaytlarca veri), elde edilen hash çıktısının uzunluğu kullanılan algoritmaya göre her zaman sabittir.

### Temel Özellikleri

- **Tek Yönlüdür (Geri Dönüştürülemez):** Hash değerinden yola çıkarak orijinal veriyi elde etmek matematiksel olarak imkansız kabul edilir. Şifreleme (encryption) ile arasındaki en büyük fark budur; şifreli veri çözülebilir ancak hash çözülemez.
    
- **Kararlı ve Deterministiktir:** Aynı veri, aynı hash algoritmasıyla işlendiğinde **her zaman** birebir aynı çıktıyı verir.
    
- **Çığ Etkisi (Avalanche Effect):** Girdide yapılan en küçük değişiklik bile (örneğin bir nokta eklemek veya bir harfi büyütmek) hash çıktısının tamamen değişmesine neden olur.
    
- **Çakışma Direnci (Collision Resistance):** İki farklı verinin aynı hash çıktısını üretmesi durumu "çakışma" olarak adlandırılır. Güvenli bir hash algoritmasında çakışma ihtimali neredeyse sıfırdır.

örnek: sha256 algoritması

```bash
Girdi:  merhaba
Çıktı: 2b3a886f34589d903e00318610534289... (64 karakter)

Girdi:  Merhaba
Çıktı: 7e738c62c3e1e909bfeb98539659f81a... (64 karakter)
```

## John'un devreye girdiği yer burası.

Algoritmanın tersine çevrilebilir olmaması, hash'leri kırmanın imkansız olduğu anlamına gelmez. Örneğin, bir parolanın hash'lenmiş versiyonuna sahipseniz ve hash algoritmasını biliyorsanız, bu hash algoritmasını kullanarak sözlük adı verilen çok sayıda kelimeyi hash'leyebilirsiniz. Daha sonra bu hash'leri kırmaya çalıştığınız hash ile karşılaştırarak eşleşip eşleşmediklerine bakabilirsiniz. Eşleşiyorsa, o hash'e karşılık gelen kelimeyi biliyorsunuz demektir - yani şifreyi kırdınız!