`john [options] [file path]`

- `john`John the Ripper programını devreye sokar.
- `[options]`Kullanmak istediğiniz seçenekleri belirtir.
- `[file path]`Kırmak istediğiniz hash değerini içeren dosya; eğer aynı dizinde bulunuyorsa, dosya yolunu belirtmenize gerek yok, sadece dosya adını vermeniz yeterli.

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt --format=raw-md5 hash07.txt
```

single kullanımı:

diyelim ki içerisinde bir sürü hash bulunan bir dosya var ve bundan sadece bir hashi çözümlemek istiyoruz ona bir başlık verip --single ile çözebiliriz.


örnek:

```bash
john --single --format=raw-md5 hash07.txt
```

### Genel Özel Kurallar

Birçok kuruluş, sözlük saldırılarıyla mücadele etmek için belirli bir parola karmaşıklığı seviyesi gerektirir; bu da bir yerde hesap oluştururken parola oluşturma bölümüne gidip şu şifreyi girmeniz gerektiği anlamına gelir:

polopassword

Şifrelerinizin aşağıdakilerden en az birini içermesi gerektiğini belirten bir uyarı alabilirsiniz:

- Büyük harf
- Sayı
- Sembol

Bu harika! Ancak, çoğu kullanıcının bu sembollerin yerini tahmin edilebilir bir şekilde belirleyebileceği gerçeğinden yararlanabiliriz. Yukarıdaki kriterler için, birçok kullanıcı aşağıdaki gibi bir şey kullanacaktır:

Polopassword1!

İlk harfi büyük, ardından rakam ve son olarak bir sembol gelen bir parola. Bu tanıdık parola kalıbı, başına ve sonuna eklenen değiştiricilerle (büyük harf veya semboller gibi) birlikte, insanların parola oluştururken kullanacağı ve tekrar tekrar kullanacağı akılda kalıcı bir kalıptır. Bu kalıp, parola karmaşıklığının tahmin edilebilirliğinden yararlanmamızı sağlayabilir.

Bu yöntem parola karmaşıklığı gereksinimlerini karşılıyor olsa da, bir saldırgan olarak bu eklenen öğelerin muhtemel konumunu bildiğimiz gerçeğinden yararlanarak kelime listelerimizden dinamik parolalar oluşturabiliriz.

### Özel Kurallar Nasıl Oluşturulur?

Özel kurallar şurada tanımlanır: `john.conf`dosya, genellikle şu konumda bulunur: `/etc/john/john.conf`John'u bir paket yöneticisi kullanarak yüklediyseniz veya kaynak koddan derlediyseniz... `make`ve içinde `/opt/john/john.conf`TryHackMe Saldırı Kutusu'nda.

Yukarıdaki örneği hedef kalıp olarak kullanarak, bu özel kuralların sözdizimini inceleyelim. Bu kurallarda tanımlayabileceğiniz çok ayrıntılı bir kontrol düzeyi olduğunu unutmayın; [buradaki wiki'ye göz atmanızı öneririm.](https://www.openwall.com/john/doc/RULES.shtml) kullanabileceğiniz değiştirici türlerinin yanı sıra kural uygulamasına ilişkin daha fazla örnek görmek için

İlk satır:

`[List.Rules:THMRules]`- Kuralınızın adını tanımlamak için kullanılır; bu, özel kuralınızı John argümanı olarak çağırmak için kullanacağınız şeydir.

Ardından, kelimenin neresinin değiştirileceğini belirlemek için düzenli ifade tarzı bir kalıp eşleştirmesi kullanırız; yine de burada sadece temel ve en yaygın değiştiricileri ele alacağız:

`Az`- Belirttiğiniz karakterlerle kelimeyi sonuna ekler.

`A0`- Belirttiğiniz karakterleri kelimeye ekler.

`c`- Karakteri konum olarak büyük harfle yazar.

Bunlar, kelimede neyi ve nerede değiştirmek istediğinizi belirlemek için birlikte kullanılabilir.

Son olarak, hangi karakterlerin ekleneceğini, başına ekleneceğini veya başka şekilde dahil edileceğini tanımlamamız gerekiyor; bunu köşeli parantezler içinde karakter kümeleri ekleyerek yapıyoruz. `[ ]`Kullanılmaları gereken sırayla. Bunlar, çift tırnak içindeki değiştirici kalıplarını doğrudan takip eder. `" "`İşte bazı yaygın örnekler:

`[0-9]`- 0-9 arası rakamları içerecektir.

`[0]`- Sadece 0 rakamlarını içerecektir.

`[A-z]`- Hem büyük hem de küçük harfleri içerecektir.

`[A-Z]`- Sadece büyük harfleri içerecektir.

`[a-z]`- Sadece küçük harfleri içerecektir.

`[a]`- Sadece bir tanesini içerecektir

`[!£$%@]`- !£$%@ sembollerini içerecektir

### Parola Korumalı Zip Dosyalarını Kırma

`zip2john [options] [zip file] > [output file]`

Örn:
`zip2john zipfile.zip > zip_hash.txt`
```bash
unzip secure.zip
```

sonra da txt dosyasının hashini çözüyoruz ordan şifreyi kırmış oluyoruz.


### Parola Korumalı Rar Dosyalarını Kırma

`rar2john [rar file] > [output file]`
```bash
unrar x secure.rar
```
`john --wordlist=/usr/share/wordlists/rockyou.txt rar_hash.txt`

### SSH Anahtar şifrelerini kırma


Kim tahmin edebilirdi ki, bir başka dönüştürme aracı? İşte John ile çalışmanın özü de bu. Adından da anlaşılacağı gibi, `ssh2john`dönüştürür `id_rsa`SSH oturumuna giriş yapmak için kullanılan özel anahtarı, John'un çalışabileceği bir karma biçime dönüştürüyor. Şaka bir yana, bu John'un çok yönlülüğünün bir başka güzel örneği. Sözdizimi beklediğiniz gibi. Eğer yoksa, `ssh2john`Yüklendikten sonra kullanabilirsiniz. `ssh2john.py`, içinde yer almaktadır `/opt/john/ssh2john.py`Eğer bunu AttackBox üzerinde yapıyorsanız, şunu değiştirin: `ssh2john`komutla `python3 /opt/john/ssh2john.py`veya Kali'de, `python /usr/share/john/ssh2john.py`.

`ssh2john [id_rsa private key file] > [output file]`



