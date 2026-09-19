
## 1. Değişkenler (Variables)

Değişkenler, veri değerlerini saklamaya yarayan **kapsayıcılardır (containers)**. Bir kovaya eşya koyup üzerine etiket yapıştırmak gibidir; saklanan veriye daha sonra bu isimle erişilir.

- **`var`:** Fonksiyon kapsamlıdır (`function-scoped`). Eski yöntemdir.
    
- **`let`:** Blok kapsamlıdır (`block-scoped`). Değeri sonradan değiştirilebilir.
    
- **`const`:** Blok kapsamlıdır (`block-scoped`). Sabittir, değeri sonradan yeniden atanamaz.
    

> [!NOTE] `let` ve `const`, kod blokları içinde değişken görünürlüğünü (`scope`) daha iyi kontrol etmeyi sağlar.

## 2. Veri Tipleri (Data Types)

Bir değişkenin tutabileceği verinin türünü belirler:

- **`string`:** Metin ifadeleri (`"merhaba"`).
    
- **`number`:** Sayısal değerler (`42`, `3.14`).
    
- **`boolean`:** Mantıksal doğruluk (`true` / `false`).
    
- **`null`:** Kasıtlı olarak boş bırakılmış değer.
    
- **`undefined`:** Tanımlanmış ama henüz değer atanmamış değişken.
    
- **`object`:** Diziler (`arrays`) ve nesneler gibi karmaşık veri yapıları.
    

## 3. Fonksiyonlar (Functions)

Belirli bir görevi yerine getirmek için tasarlanmış, tekrar kullanılabilir **kod bloklarıdır**. Aynı kodu defalarca yazmak yerine fonksiyon haline getirip çağırırız.

JavaScript

```
// Öğrenci sonucunu ekrana basan fonksiyon
function PrintResult(rollNum) {
    alert("Username with roll number " + rollNum + " has passed the exam");
}
```

## 4. Döngüler (Loops)

Belirli bir koşul doğru (`true`) olduğu sürece bir kod bloğunu **tekrar tekrar çalıştırmaya** yarar. En yaygın döngüler: `for`, `while` ve `do...while`.

JavaScript

```
const rollNumbers = [101, 102, 103];

// Fonksiyonu 100 kez tek tek çağırmak yerine döngü kullanımı:
for (let i = 0; i < 100; i++) {
    PrintResult(rollNumbers[i]); 
}
```

## 5. İstek-Yanıt Döngüsü (Request-Response Cycle)

Web geliştirmede istemci ile sunucu arasındaki temel iletişim modelidir:

1. **Request (İstek):** Kullanıcının tarayıcısı (Client), web sunucusuna bir istek gönderir.
    
2. **Response (Yanıt):** Sunucu isteği işler ve istenen veriyi (HTML sayfası, JSON verisi, medya dosyası vb.) tarayıcıya geri döner.

# HTML Entegrasyonu

## 1. Genel Bakış

JavaScript, genellikle doğrudan içerik oluşturmak yerine **HTML** ve **CSS** ile birlikte çalışarak sayfalara dinamizm ve etkileşim kazandırır. HTML içine iki temel yöntemle entegre edilir: **Dahili (Internal)** ve **Harici (External)**.

## 2. Dahili JavaScript (Internal JS)

JS kodlarının doğrudan HTML dosyası içindeki `<script>` etiketleri arasına yazılması yöntemidir.

- **`<head>` içine yazılırsa:** Sayfa içeriği yüklenmeden önce çalışması gereken scriptler için kullanılır.
    
- **`<body>` içine yazılırsa:** Sayfadaki öğeler yüklendikçe onlarla etkileşime giren scriptler için tercih edilir.
    

HTML

```
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Internal JS</title>
</head>
<body>
    <h1>Addition of Two Numbers</h1>
    <p id="result"></p>

    <!-- Dahili JS Kodu -->
    <script>
        let x = 5;
        let y = 10;
        let result = x + y;
        // HTML öğesini seçip içeriğini güncelleme
        document.getElementById("result").innerHTML = "The result is: " + result;
    </script>
</body>
</html>
```

## 3. Harici JavaScript (External JS)

JS kodlarının `.js` uzantılı ayrı bir dosyada saklanıp HTML dosyasına **`src` niteliği (attribute)** ile bağlanması yöntemidir.

- **Avantajı:** HTML dokümanını temiz tutar, kodun bakımını kolaylaştırır ve büyük projelerde düzen sağlar.
    
- **Konum:** Yerel sunucuda veya harici bir bulut/sunucu üzerinde barındırılabilir.
    

**`script.js`:**

JavaScript

```
let x = 5;
let y = 10;
let result = x + y;
document.getElementById("result").innerHTML = "The result is: " + result;
```

**`external.html`:**

HTML

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>External JS</title>
</head>
<body>
    <h1>Addition of Two Numbers</h1>
    <p id="result"></p>

    <!-- Harici JS dosyasını bağlama -->
    <script src="script.js"></script>
</body>
</html>
```

## 4. Pentest Perspektifi: Dahili ve Harici JS Tespiti

Bir web uygulaması sızma testinde (pen-testing), sayfanın nasıl JS yüklediğini anlamak için kaynak kodu incelenir:

- **Kaynak Kodunu İnceleme:** Sayfaya sağ tıklayıp `View Page Source` (Sayfa Kaynağını Görüntüle) seçilir.
    
- **Dahili JS:** `src` niteliği olmayan doğrudan açılıp kapanan `<script> ... </script>` bloklarıdır.
    
- **Harici JS:** `<script src="dosya_yolu.js"></script>` şeklinde tanımlanmış dosya referanslarıdır.

# JavaScript İletişim Kutuları ve Güvenlik Tehditleri

## 1. Dahili İletişim Kutuları (Dialogue Boxes)

JavaScript, kullanıcıyla basit etkileşimler kurmak için üç temel yerleşik fonksiyon sunar:

### `alert()`

- **Amacı:** Kullanıcıya bir bilgi veya uyarı mesajı göstermek.
    
- **Arayüz:** Sadece bir mesaj ve **"Tamam" (OK)** butonu içerir.
    

JavaScript

```
alert("Hello THM");
```

### `prompt()`

- **Amacı:** Kullanıcıdan metin girdisi (input) almak.
    
- **Dönüş Değeri:** Kullanıcı "Tamam" derse girilen metni (`string`), "İptal" derse `null` döner.
    

JavaScript

```
let name = prompt("What is your name?");
alert("Hello " + name);
```

### `confirm()`

- **Amacı:** Kullanıcıdan bir işlem için onay almak.
    
- **Dönüş Değeri:** Kullanıcı "Tamam" derse `true`, "İptal" derse `false` döner.
    

JavaScript

```
let response = confirm("Do you want to proceed?");
```

## 2. Güvenlik ve İstismar (Security & Exploitation)

Bu fonksiyonlar güvenli bir şekilde kontrol edilmez veya filtrelenmezse saldırganlar tarafından istismar edilebilir.

- **XSS (Cross-Site Scripting):** Saldırganlar hedef web sayfasına zararlı JS enjekte ettiklerinde, genellikle zafiyetin çalıştığını doğrulamak (PoC - Proof of Concept) veya kullanıcı oturumlarını hedeflemek için `alert()` gibi fonksiyonları tetikler.
    
- **Hizmet Reddi / Tarayıcı Kilitleme (DoS / Annoyance):** İletişim kutuları tarayıcının o sekmesindeki etkileşimi bloklar (senkrondur). Bir döngü içerisine sokulduklarında kullanıcının sekmeyi veya tarayıcıyı normal yollarla kapatmasını engelleyebilirler.
    

HTML

```
<!-- Kullanıcıyı döngüye sokan zararlı script örneği -->
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Hacked</title>
</head>
<body>
    <script>
        // 3 kez yerine 500 kez çalıştırıldığında tarayıcıyı kilitler
        for (let i = 0; i < 3; i++) {
            alert("Hacked");
        }
    </script>
</body>
</html>
```

> [!WARNING] E-posta eklerinde veya güvenilmeyen kaynaklarda gelen HTML/JS dosyaları açıldığında arka planda istemci tarafını kilitleyen ya da zararlı eylemler yürüten scriptler çalışabilir.

# JavaScript Akış Kontrolü ve İstemci Taraflı Kimlik Doğrulama Zafiyeti

## 1. Akış Kontrolü (Control Flow)

JavaScript'te akış kontrolü, kod bloklarının ve ifadelerin belirli koşullara bağlı olarak hangi sırayla çalıştırılacağını belirler:

- **Karar Yapıları:** `if-else`, `switch` (farklı koşullara göre farklı kod blokları çalıştırma).
    
- **Döngüler:** `for`, `while`, `do...while` (eylemleri tekrar etme).
    

## 2. Koşullu İfadeler (`if-else`)

Bir koşulun doğruluğuna (`true` veya `false`) göre alternatif kod bloklarını yürütür.

HTML

```
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Age Verification</title>
</head>
<body>
    <h1>Age Verification</h1>
    <p id="message"></p>

    <script>
        let age = prompt("What is your age");
        
        // Koşul kontrolü: 18 ve üzeri mi?
        if (age >= 18) {
            document.getElementById("message").innerHTML = "You are an adult.";
        } else {
            document.getElementById("message").innerHTML = "You are a minor.";
        }
    </script>
</body>
</html>
```

## 3. Pentest Perspektifi: İstemci Taraflı Doğrulama Atlatma (Client-Side Auth Bypass)

Geliştiriciler bazen kimlik doğrulama (kullanıcı adı/şifre kontrolü) mekanizmasını doğrudan JavaScript ile **istemci tarafında (tarayıcıda)** uygular.

- **Güvenlik Riski:** Tarayıcıya indirilen JavaScript kodları son kullanıcı tarafından tamamen okunabilir ve düzenlenebilir.
    
- **Kaynak Kodu Analizi:** Sayfa kaynağı (`View Page Source`) veya Developer Tools (`F12`) incelenerek JS kodundaki `admin` kullanıcı adı, beklenen sabit şifre veya kontrolü sağlayan `if-else` mantığı doğrudan açığa çıkar.
    
- **İstismar:** Şifre kaynak koddan okunabilir veya tarayıcı konsolundan (`Console`) ilgili değişkenler/fonksiyonlar manipüle edilerek giriş kontrolü kolayca atlatılabilir (`bypass`).
    

> [!CAUTION] Kimlik doğrulama, yetkilendirme ve hassas iş mantığı kontrolleri **asla** yalnızca istemci taraflı (JS) yapılmamalıdır; mutlaka **sunucu tarafında (Server-Side)** doğrulanmalıdır.

# JavaScript Kod Küçültme (Minification) ve Gizleme (Obfuscation)

## 1. Kod Küçültme (Minification)

Kodun dosya boyutunu küçülterek web sayfalarının yüklenme süresini optimize etmek amacıyla gereksiz tüm karakterlerin temizlenmesi işlemidir.

- **Temizlenen Öğeler:** Boşluklar, satır sonları, yorum satırları.
    
- **Değişken Kısaltma:** Değişken adları tek veya çift karakterli isimlere dönüştürülür.
    
- **Amaç:** Üretim (production) ortamlarında ağ trafiğini azaltmak ve performansı artırmak.
    
- **İşlevsellik:** İnsanlar için okunması zorlaşsa da tarayıcı tarafında kod tamamen aynı şekilde çalışır.
    

## 2. Kod Gizleme (Obfuscation)

Kaynak kodun insanlar ve tersine mühendisler tarafından **anlaşılmasını zorlaştırmak** amacıyla kasıtlı olarak karmaşıklaştırılması işlemidir.

- **Uygulanan Yöntemler:**
    
    - Değişken ve fonksiyon adlarını anlamsız rastgele dizgilerle (ör. `_0x114713`) değiştirme.
        
    - Sahte / ölü kod blokları (dummy code) ekleme.
        
    - Dizeleri (strings) hex/onaltılık veya kodlanmış formatlara (`\x20THM`) çevirme.
        
    - Akış mantığını karmaşık matematiksel ifadelere ve dizi indekslerine bağlama.
        
- **Amaç:** Fikri mülkiyeti korumak, kaynak kodun kopyalanmasını veya güvenlik analizcileri tarafından hızlıca çözülmesini engellemek (aynı zamanda kötü amaçlı yazılımlar/malware tarafından tespiti zorlaştırmak için de sıkça kullanılır).
    

## 3. Güvenlik Analizi: Kaynak Kodu İnceleme ve Geri Çözme (Deobfuscation)

Sızma testlerinde veya zararlı yazılım analizinde karmaşıklaştırılmış scriptlerle karşılaşıldığında uygulanan adımlar:

- **Tarayıcıda İnceleme:** `F12` > **Sources** sekmesinden yüklenen `.js` dosyaları doğrudan görüntülenebilir.
    
- **Prettify (Pretty Print):** Geliştirici araçlarındaki `{}` simgesi ile sıkıştırılmış kod biçimlendirilip satırlara ayrılabilir.
    
- **Deobfuscation Araçları:** Gizlenmiş ve anlamsızlaştırılmış kodlar, `deobfuscator` araçları kullanılarak analiz edilebilir ve orijinal mantığına yakın okunabilir bir JS formatına dönüştürülebilir.
    

> [!NOTE] Obfuscation bir **şifreleme (encryption) değildir**. Kodun istemci (tarayıcı) tarafında çalışabilmesi için tarayıcı motoru tarafından çözülebilir olması gerekir; bu nedenle yeterli analiz ile deobfuscate edilip geri çözülebilir.