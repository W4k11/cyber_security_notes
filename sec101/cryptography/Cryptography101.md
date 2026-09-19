- **Plaintext** şifrelenmeden önceki veridir. resim, text, document gibi.
- **Ciphertext** Şifrelemeden sonraki metindir
- **Cipher** Düz metni şifreleme veya şifrelenmiş metni düz metne çevirmedir.
- **Key** şifreleme veya şifre çözme için kullanılan bit dizisidir.
- **Encryption** düz metni "cipher" veya "key" kullanarak şifreleme işlemidir.
- **Decryption** "encryption" un tam tersidir.

RSA

RSA, büyük sayıların çarpanlarına ayrılmasının zorluğuna dayanarak, verileri şifrelemek için iki anahtar kullanan bir yöntemdir: biri kilitlemek (şifrelemek) diğeri ise kilidi açmak (şifreyi çözmek) içindir.

2 asal sayı girilir ve bunlar çarpılır.

örn:

1. Bob iki asal sayı seçiyor: p = 157 ve q = 199. n = p × q = 31243 diye hesaplıyor.
2. ϕ(n) = n − p − q + 1 = 31243 − 157 − 199 + 1 = 30888 olduğundan, Bob, ϕ(n) ile aralarında asal olan e = 163'ü seçer; ayrıca, e × d = 1 mod ϕ(n) olacak şekilde d = 379'u seçer, yani e × d = 163 × 379 = 61777 ve 61777 mod 30888 = 1. Açık anahtar (n,e), yani (31243,163) ve özel anahtar (n,d), yani (31243,379)'dur.
3. Şifrelemek istedikleri değerin x = 13 olduğunu varsayalım, o zaman Alice y = xe mod n = 13163 mod 31243 = 16341 değerini hesaplayıp gönderecektir.
4. Bob, x = yd mod n = 16341379 mod 31243 = 13 işlemini yaparak alınan değeri şifresini çözecektir. Bu şekilde Bob, Alice'in gönderdiği değeri kurtarır.


### Diffie-Hellman Anahtar Değişimi

**Anahtar değişimi,** iki taraf arasında ortak bir gizli anahtar oluşturmayı amaçlar. Bu yöntem, iki tarafın önceden var olan ortak bir gizli anahtara ihtiyaç duymadan ve bir gözlemcinin bu anahtarı elde edememesi koşuluyla, güvenli olmayan bir iletişim kanalı üzerinden ortak bir gizli anahtar oluşturmasına olanak tanır. Sonuç olarak, bu ortak anahtar, sonraki iletişimlerde simetrik şifreleme için kullanılabilir.

Şu senaryoyu ele alalım. Alice ve Bob güvenli bir şekilde iletişim kurmak istiyorlar. Simetrik şifreleme için ortak bir anahtar oluşturmak istiyorlar, ancak anahtar değişimi için asimetrik şifreleme kullanmak istemiyorlar. İşte burada Diffie-Hellman Anahtar Değişimi devreye giriyor.

Alice ve Bob birbirinden bağımsız olarak sırlar üretirler; bu sırlara A ve B diyelim. Ayrıca ortak, herkese açık bazı materyalleri de vardır; buna da C diyelim.

Bazı varsayımlarda bulunmamız gerekiyor. Birincisi, sırları birleştirdiğimizde, onları ayırmak pratikte imkansızdır. İkincisi, birleştirilme sırası önemli değildir. Alice ve Bob, sırlarını ortak malzemeyle birleştirerek AC ve BC'yi oluşturacaklar. Daha sonra bunları birbirlerine gönderecekler ve aldıkları kısmı kendi sırlarıyla birleştirerek her ikisi de ABC olan iki özdeş anahtar oluşturacaklar. Artık bu anahtarı kullanarak iletişim kurabilirler.

Önceki paragrafları çok soyut bulduysanız, gelin süreci ayrıntılı olarak inceleyelim.

1. Alice ve Bob, **kamuya açık değişkenler** konusunda anlaştılar : büyük bir asal sayı _p_ bir üreteç _g_ koşulunu sağlayan ve 0 < _g_ < _p_ değerlerini seçeceğiz . _p_ = 29 ve _g_ = 3 . Bu değerler iletişim kanalı üzerinden kamuya açık olarak açıklanacaktır. Güvenlik açısından küçük olsalar da, hesaplamalarımızı basitleştirmek için
2. Her iki taraf da özel bir tamsayı seçer. Sayısal bir örnek olarak, Alice _a_ = 13'ü , Bob ise _b_ = 15'i seçer . Bu değerlerin her biri **özel bir anahtarı** temsil eder ve açıklanmamalıdır.
3. hesaplama zamanı geldi. **genel anahtarlarını** Her iki tarafın da 2. adımda elde ettikleri özel anahtarlarını ve 1. adımda üzerinde anlaştıkları genel değişkenleri kullanarak _Alice, A_ = _g'yi hesaplıyor._ _A_ karşı _p_ = 3'e 13 mod 29 = 19 ve Bob _B_ = _g'yi hesaplıyor._ _B_ karşı _p_ = 3'e 15 mod 29 = 26. Bunlar açık anahtarlardır.
4. Alice ve Bob birbirlerine anahtarları gönderirler. Bob _A_ = _g alır._ _A_ mod _p_ = 19 , yani Alice'in açık anahtarı. Ve Alice _B_ = _g'yi alır._ _B_ mod _p_ = 26 , yani Bob'un açık anahtarı. Bu adıma **anahtar değişimi** denir .
5. hesaplayabilirler **nihayet ortak gizli anahtarı** Alice ve Bob, aldıkları açık anahtarı ve kendi özel anahtarlarını kullanarak _. Alice, B'yi hesaplar._ _A_ karşı _p_ = 26'ya 13 mod 29 = 10 ve Bob _A'yı hesaplıyor._ _B_ karşı _p_ = 19'a 15 mod 29 = 10. Her iki hesaplama da aynı sonucu verir, _g_ _a_ _b_ mod _p_ = 10 , paylaşılan gizli anahtar.

gpg ile şifreleme yapılabilir aynı zamanda key üretip şifreyide çözebilriz.

- Kullanırdınız `gpg --import backup.key`Anahtarınızı backup.key dosyasından içe aktarmak için
- Mesajlarınızın şifresini çözmek için bir komut vermeniz gerekiyor. `gpg --decrypt confidential_message.gpg`