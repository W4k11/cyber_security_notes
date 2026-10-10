# Kriptografi Temelleri: Asimetrik Şifreleme, RSA, Diffie-Hellman ve GPG

## 1. Temel Kriptografi Terimleri

- **Plaintext (Düz Metin):** Henüz şifrelenmemiş, herkes tarafından okunabilen orijinal veri (metin belgesi, görsel, ses kaydı, veritabanı yedeği vb.).
    
- **Ciphertext (Şifreli Metin):** Şifreleme algoritması uygulandıktan sonra anlamsız karakter yığınına dönüşen, ele geçirilse dahi okunamayan veri.
    
- **Cipher (Şifreleme Algoritması):** Düz metni şifreli metne dönüştüren ya da tam tersi işlemi yapan matematiksel adımlar bütünü.
    
- **Key (Anahtar):** Şifreleme ve şifre çözme süreçlerinde algoritmanın girdisi olarak kullanılan, yetkili tarafların elinde bulunan bit dizisi.
    
- **Encryption (Şifreleme):** Düz metni (Plaintext), bir algoritma (Cipher) ve anahtar (Key) kullanarak şifreli metne (Ciphertext) dönüştürme işlemi.
    
- **Decryption (Şifre Çözme):** Şifreli metni (Ciphertext) doğru anahtarı kullanarak tekrar orijinal düz metne (Plaintext) çevirme işlemi.
    

## 2. RSA Algoritması (Asimetrik Şifreleme)

RSA; çok büyük iki asal sayıyı çarpmanın son derece kolay, ancak ortaya çıkan devasa sonucu tekrar çarpanlarına ayırmanın (asal çarpanlara ayırma problemi) matematiksel olarak aşırı zor olmasına dayanan asimetrik bir şifreleme algoritmasıdır. Biri kilitlemek (**Public Key**), diğeri kilidi açmak (**Private Key**) için olmak üzere iki farklı anahtar çifti kullanır.

### Matematiksel Adımlar ve Örnek:

1. **Asal Sayıların Seçimi:** Bob iki gizli asal sayı belirler:
    
    $$p = 157, \quad q = 199$$
    
    Modülüs değeri hesaplanır:
    
    $$n = p \times q = 157 \times 199 = 31243$$
    
2. **Euler Totient Fonksiyonu $\phi(n)$ ve Anahtar Çifti:**
    
    $$\phi(n) = (p - 1)(q - 1) = n - p - q + 1 = 31243 - 157 - 199 + 1 = 30888$$
    
    - Bob, $\phi(n)$ ile aralarında asal olacak bir açık üs seçer:
        
        $$e = 163$$
        
    - Bob, $e \times d \equiv 1 \pmod{\phi(n)}$ denkliğini sağlayan gizli üssü hesaplar:
        
        $$d = 379 \quad (163 \times 379 = 61777, \quad 61777 \pmod{30888} = 1)$$
        
    - **Açık Anahtar (Public Key):** $(n, e) = (31243, 163)$ _(Herkese açık ilan edilir)_
        
    - **Özel Anahtar (Private Key):** $(n, d) = (31243, 379)$ _(Yalnızca Bob'da kalır)_
        
3. **Şifreleme (Alice Veri Gönderiyor):**
    
    Alice göndermek istediği düz metin sayısını $x = 13$ olarak belirler ve Bob'un açık anahtarını kullanarak şifreler:
    
    $$y = x^e \pmod n = 13^{163} \pmod{31243} = 16341$$
    
    Alice şifreli metin olarak **$16341$** değerini hatta iletir.
    
4. **Şifre Çözme (Bob Mesajı Okuyor):**
    
    Bob gelen $y = 16341$ değerini kendi özel anahtarıyla ($d=379$) çözer:
    
    $$x = y^d \pmod n = 16341^{379} \pmod{31243} = 13$$
    
    Bob, aradaki hiç kimse şifreyi kıramadan orijinal veri olan **$13$** değerini elde eder.
    

## 3. Diffie-Hellman Anahtar Değişimi (Key Exchange)

Diffie-Hellman, iki tarafın önceden ortak bir parolası olmasa bile, **güvenli olmayan açık bir hat üzerinden** dinleyicilere yakalanmadan ortak bir simetrik şifreleme anahtarı üretmesini sağlar.

Plaintext

```
    Alice (Gizli: a)                            Bob (Gizli: b)
           │                                           │
           ├──────── Herkese Açık: Asal p, Taban g ────┤
           │                                           │
   A = g^a mod p                               B = g^b mod p
           │                                           │
           │ ──────────── Açık Değer A ──────────────► │
           │ ◄─────────── Açık Değer B ─────────────── │
           │                                           │
 Ortak Anahtar:                              Ortak Anahtar:
  K = B^a mod p                               K = A^b mod p
  (K = g^(ab) mod p)                          (K = g^(ab) mod p)
```

### Sayısal Örnek:

1. **Açık Parametreler:** Alice ve Bob hatta herkesin görebileceği şu değerlerde anlaşır:
    
    $$p = 29 \quad (\text{Asal Sayı}), \quad g = 3 \quad (\text{Üreteç})$$
    
2. **Gizli Sayılar:** Taraflar kimseyle paylaşmayacakları özel sayılarını seçer:
    
    - Alice: $a = 13$
        
    - Bob: $b = 15$
        
3. **Açık Değerlerin Hesaplanması:**
    
    - Alice: $A = g^a \pmod p = 3^{13} \pmod{29} = 19$
        
    - Bob: $B = g^b \pmod p = 3^{15} \pmod{29} = 26$
        
4. **Anahtar Değişimi:** Alice hatta $19$ değerini, Bob ise $26$ değerini gönderir (Hattı dinleyen bir saldırgan sadece $19$ ve $26$'yı görür, gizli $a$ ve $b$'yi bilemez).
    
5. **Ortak Gizli Anahtarın Üretimi:**
    
    - Alice: $K = B^a \pmod p = 26^{13} \pmod{29} = \mathbf{10}$
        
    - Bob: $K = A^b \pmod p = 19^{15} \pmod{29} = \mathbf{10}$
        

Her iki taraf da kanaldan hiçbir parola iletmeden ortak anahtar olarak **$10$** değerine ulaştı. Artık AES gibi hızlı simetrik algoritmalarla bu anahtar üzerinden haberleşebilirler.

## 4. GPG (GnuPG) ile Pratik Şifreleme ve Anahtar Yönetimi

GPG, e-postaları, dosyaları ve hassas belgeleri asimetrik/simetrik olarak şifrelemek ve dijital imzalamak için kullanılan açık kaynaklı bir araçtır.

### 1. Anahtar İçe Aktarma (Import)

Daha önceden yedeklenmiş veya karşı taraftan alınmış bir anahtarı sisteme eklemek için:

Bash

```
gpg --import backup.key
```

### 2. Şifreli Dosyayı Çözme (Decrypt)

GPG ile şifrelenmiş `.gpg` uzantılı bir dosyanın şifresini çözüp ekrana veya yeni bir dosyaya basmak için:

Bash

```
gpg --decrypt confidential_message.gpg
```

_(Çıktıyı doğrudan yeni bir dosyaya kaydetmek için: `gpg --decrypt confidential_message.gpg > cozulmus_metin.txt`)_

### 3. Hızlı Şifreleme Komutları

- **Simetrik Parola ile Şifreleme (Tek parola belirleyerek):**
    
    Bash
    
    ```
    gpg -c gizli_belge.txt
    ```
    
- **Bir Alıcının Açık Anahtarıyla Şifreleme:**
    
    Bash
    
    ```
    gpg --encrypt --recipient alici@mail.com gizli_belge.txt
    ```