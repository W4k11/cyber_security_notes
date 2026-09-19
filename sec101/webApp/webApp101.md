| **Katman**              | **Web Bileşeni**         | **Gezegen Analojisi**     | **Görevi / İşlevi**                                     |
| ----------------------- | ------------------------ | ------------------------- | ------------------------------------------------------- |
| **Front End** (Ön Yüz)  | **HTML**                 | Canlının DNA'sı           | Sayfanın temel iskeletini ve içeriğini oluşturur.       |
|                         | **CSS**                  | Dış görünüş (renk, doku)  | Tasarım, düzen ve görsel stili belirler.                |
|                         | **JavaScript**           | Canlının beyni            | Dinamik kararlar alır ve etkileşimi yönetir.            |
| **Back End** (Arka Yüz) | **Veritabanı**           | Kütüphane / Dosya dolabı  | Verileri depolar, günceller ve sunar.                   |
|                         | **Altyapı (Sunucu vb.)** | Yollar, arabalar ve yakıt | Sistemin çalışmasını sağlayan donanım ve ağ desteğidir. |
|                         | **WAF**                  | Gezegenin atmosferi       | Zararlı istekleri filtreleyerek koruma sağlar.          |

### URL (Uniform Resource Locator) Nedir?

**URL (Tekdüze Kaynak Bulucu)**; web sayfası, video, görsel veya diğer medyalar dahil olmak üzere internet üzerindeki her türlü kaynağa erişmeyi sağlayan web adresidir. Tarayıcının internet üzerinde doğru hedefe ulaşması için bir yol gösterici görevi görür.

Plaintext

```
scheme://user:password@host:port/path?query#fragment
└─ 1 ─┘   └──── 2 ────┘ └─ 3 ─┘ └4┘ └──5─┘ └──6──┘ └───7────┘
```

### Bir URL'in Anatomisi (Temel Bileşenler)

|**Bileşen**|**Örnek**|**Açıklama**|**Güvenlik & Önemli Detay**|
|---|---|---|---|
|**1. Scheme (Protokol)**|`https://`|Kaynağa erişmek için kullanılan iletişim protokolüdür (en yaygınları **HTTP** ve **HTTPS**).|**HTTPS**, verileri şifrelediği için standarttır ve güvenlik açısından zorunlu tutulmalıdır.|
|**2. User (Kullanıcı Bilgisi)**|`admin@`|Kimlik doğrulama gerektiren kaynaklarda kullanıcı adı/şifre bilgisini taşır.|**Güvenlik Riski:** Hassas bilgileri açık metin olarak gösterdiğinden günümüzde neredeyse hiç kullanılmaz.|
|**3. Host / Domain (Alan Adı)**|`example.com`|Erişilmek istenen web sitesini belirten benzersiz adrestir.|**Typosquatting / Phishing:** Sahte siteler orijinal domaine çok benzer harfler kullanarak oltalama saldırıları düzenler.|
|**4. Port (Bağlantı Noktası)**|`:443`|Sunucu üzerindeki hangi servis/kapı ile iletişim kurulacağını belirler (1–65.535 arası).|Standart portlar genellikle gizlenir: **HTTP için 80**, **HTTPS için 443** kullanılır.|
|**5. Path (Yol / Dizin)**|`/blog/post-1`|Sunucu üzerinde erişilmek istenen dosya veya sayfanın tam konumunu gösterir.|Yetkisiz erişimleri engellemek için hassas dizin yollarının korunması (yetkilendirme) gerekir.|
|**6. Query String (Sorgu Dizesi)**|`?id=10&sort=asc`|`?` işaretiyle başlar; arama parametreleri, filtreler veya form verilerini taşır.|**Enjeksiyon Riski:** Kullanıcı tarafından kolayca değiştirilebildiği için sunucu tarafında mutlaka doğrulanmalıdır (Input Validation).|
|**7. Fragment (Çapa / Bölüm)**|`#section-2`|`#` işaretiyle başlar; sayfa içindeki belirli bir başlığa, tabloya veya konuma doğrudan atlamayı sağlar.|Tarayıcı tarafında işlenir; ancak DOM tabanlı güvenlik açıklarına yol açmaması için temizlenmelidir.|

> 💡 **Özet:** URL, sadece bir web adresi değil; protokol, hedef sunucu, iletişim portu, dosya dizini ve istek parametrelerini tek bir satırda birleştiren yapılandırılmış bir iletişim haritasıdır.

### HTTP Mesajları (HTTP Messages)

**HTTP mesajları**, istemci (kullanıcı/tarayıcı) ile web sunucusu arasında alınıp verilen veri paketleridir. İstemcinin taleplerinin ve sunucunun yanıtlarının nasıl iletildiğini belirleyerek istemci-sunucu (client-server) etkileşimini mümkün kılar.

İki tür HTTP mesajı bulunur:

- **HTTP Request (İstek):** Kullanıcı/istemci tarafından bir eylemi tetiklemek için sunucuya gönderilir.
    
- **HTTP Response (Yanıt):** İstemcinin isteğine karşılık sunucu tarafından üretilip geri gönderilir.
    

### HTTP Mesaj Yapısı (Anatomisi)

Her HTTP mesajı, iletişimin hatasız gerçekleşmesi için standart 4 temel bölümden oluşur:

Plaintext

```
┌──────────────────────────────────────────────┐
│ 1. Start Line (Başlangıç Satırı)             │  <- İstek/Yanıt türü ve durumu
├──────────────────────────────────────────────┤
│ 2. Headers (Başlıklar) (Key: Value)          │  <- Meta veriler, talimatlar
├──────────────────────────────────────────────┤
│ 3. Empty Line (Boş Satır)                    │  <- Ayırıcı (CRLF)
├──────────────────────────────────────────────┤
│ 4. Body (Gövde / Yük)                        │  <- Asıl veri (JSON, HTML vb.)
└──────────────────────────────────────────────┘
```

|Bölüm|Görevi / İşlevi|Örnek / Detay|
|---|---|---|
|**1. Start Line**|Mesajın türünü ve temel işleme detaylarını belirten giriş satırıdır.|**İstekte:** Metot + Yol + Protokol (`GET /index.html HTTP/1.1`)<br><br>  <br><br>**Yanıtta:** Protokol + Durum Kodu (`HTTP/1.1 200 OK`)|
|**2. Headers**|Anahtar-değer (`Key: Value`) çiftlerinden oluşan meta veri ve talimat alanıdır.|Güvenlik politikaları, içerik türü (`Content-Type`), çerezler (`Cookies`), yetkilendirme (`Authorization`) vb. taşır.|
|**3. Empty Line**|Başlıklar ile gövde arasındaki ayrımı gösteren tek satırlık boşluktur.|Tarayıcı ve sunucunun başlıkların bittiğini ve gövdenin başladığını anlamasını sağlar; olmaması durumunda mesaj ayrıştırılamaz (parse error).|
|**4. Body**|Mesajın taşıdığı asıl veri içeriğidir (isteğe bağlıdır).|**İstekte:** Gönderilen form veya JSON verisi.<br><br>  <br><br>**Yanıtta:** İstenen HTML sayfası, görsel veya API verisi.|

### HTTP Mesajlarını Anlamanın Önemi

- **Hata Ayıklama ve Teşhis:** İletişim akışını, durum kodlarını ve başlıkları inceleyerek performans sorunlarını veya kırık istekleri hızla tespit etmeyi sağlar.
    
- **Web Güvenliği:** Ağ üzerinden iletilen veriyi analiz ederek enjeksiyon saldırılarını, yetkisiz erişimleri veya güvenli olmayan veri iletimlerini engellemek için doğru güvenlik önlemlerini (ör. Güvenlik başlıkları, şifreleme) uygulamaya yardımcı olur.

### HTTP İstekleri (HTTP Requests)

**HTTP İsteği (HTTP Request)**, bir kullanıcının (istemcinin) web uygulamasıyla etkileşime geçmek ve sunucuda bir eylem tetiklemek için gönderdiği ilk temas mesajıdır. Özellikle **siber güvenlik** açısından isteklerin nasıl yapılandığını ve manipüle edildiğini anlamak kritik öneme sahiptir.

### Request Line (İstek Satırı)

HTTP isteğinin ilk satırıdır (`Start Line`). Sunucuya ne tür bir istek geldiğini, hangi kaynağı hedeflediğini ve hangi protokolün kullanıldığını bildirir.

Plaintext

```
[METHOD] [URL Path] [HTTP Version]
Örnek: POST /api/login HTTP/1.1
```

### HTTP Metotları (HTTP Methods) ve Güvenlik Riskleri

HTTP metotları, URL yoluyla belirtilen kaynak üzerinde hangi eylemin gerçekleştirilmek istendiğini tanımlar:

|Metot|İşlev / Açıklama|Güvenlik Uyarısı & Risk|
|---|---|---|
|**GET**|Sunucudan veri çekmek (okumak) için kullanılır; sunucuda durum değişikliği yapmaz.|URL'de açık metin olarak göründüğü için şifre, token veya hassas veriler asla `GET` parametrelerine konulmamalıdır.|
|**POST**|Sunucuya yeni bir kaynak oluşturmak veya veri göndermek için kullanılır.|SQL Injection (SQLi) ve XSS gibi saldırıları önlemek için gelen girdi mutlaka doğrulanmalı ve temizlenmelidir (Sanitize/Validation).|
|**PUT**|Sunucudaki kaynağı tamamen güncellemek veya yenisiyle değiştirmek için kullanılır.|Yalnızca kimliği doğrulanmış ve yetkili kullanıcıların değişiklik yapmasına izin verilmelidir (Broken Access Control önlemi).|
|**DELETE**|Belirtilen kaynağı sunucudan siler.|Yetkisiz silme işlemlerini engellemek için sıkı yetkilendirme kontrolleri gerektirir.|
|**PATCH**|Kaynağın tümünü değiştirmeden yalnızca belirli bir kısmını günceller.|Veri tutarsızlıklarını önlemek için kısmi veriler titizlikle doğrulanmalıdır.|
|**HEAD**|`GET` ile aynıdır ancak yanıt gövdesini (`body`) döndürmez, yalnızca başlıkları (`headers`) çeker.|Tam indirme yapmadan meta verileri ve durum kodlarını kontrol etmek için kullanılır.|
|**OPTIONS**|Belirli bir kaynak için sunucunun desteklediği HTTP metotlarını listeler.|Gerekmediği durumlarda kapatılmalı veya bilgi ifşasını (Information Disclosure) önlemek için sınırlandırılmalıdır.|
|**TRACE**|İstemciden gelen isteği sunucudan aynen geri yansıtır (debug/hata ayıklama amaçlı).|**XST (Cross-Site Tracing)** saldırılarına yol açabileceğinden modern sunucularda genellikle devre dışı bırakılır.|
|**CONNECT**|Şifreli iletişim kanalları (ör. HTTPS proxy tünellemesi) kurmak için kullanılır.|İki uç nokta arasında çift yönlü tünel açılmasını sağlar.|

### URL Path (Yol / Dizin)

Sunucuda hangi kaynağın talep edildiğini gösteren adrestir. _(Örnek: `[https://tryhackme.com/api/users/123](https://tryhackme.com/api/users/123)` adresinde dizin yolu `/api/users/123` ifadesidir)._

- **Saldırı Vektörleri:** Saldırganlar dizin atlama (**Directory Traversal / LFI**), yetkisiz erişim veya enjeksiyon saldırıları için URL yollarını manipüle etmeye çalışır.
    
- **Savunma:** URL yolları yetkilendirme mekanizmalarıyla doğrulanmalı, girdi temizliği yapılmalı ve hassas dizinler doğrudan dışarıya açılmamalıdır.
    

### HTTP Versiyonları

|Versiyon|Yıl|Temel Özellikler & Evrim|
|---|---|---|
|**HTTP/0.9**|1991|Yalnızca `GET` metodunu destekleyen en ilkel versiyon.|
|**HTTP/1.0**|1996|Başlıklar (`Headers`), durum kodları ve farklı dosya türleri için önbellekleme desteği eklendi.|
|**HTTP/1.1**|1997|Kalıcı bağlantılar (`Keep-Alive`), parçalı veri transferi (`Chunked Transfer`) ve gelişmiş önbellekleme geldi. Halen en yaygın desteklenen sürümdür.|
|**HTTP/2**|2015|Aynı TCP bağlantısı üzerinden çoklama (`Multiplexing`), başlık sıkıştırma (`HPACK`) ve akış önceliklendirme ile performans artışı sağlandı.|
|**HTTP/3**|2022|TCP yerine **UDP tabanlı QUIC** protokolünü kullanarak bağlantı kurulum hızını ve güvenliğini en üst düzeye çıkardı.|

### Request Headers (İstek Başlıkları)

**İstek Başlıkları**, gönderilen istek hakkında web sunucusuna ek meta veriler ve talimatlar iletmek için kullanılır.

|**Başlık (Header)**|**Örnek Değer**|**Açıklama**|
|---|---|---|
|**`Host`**|`Host: tryhackme.com`|İsteğin hangi hedef alan adına (web sunucusuna) gittiğini belirtir. (HTTP/1.1'de zorunludur).|
|**`User-Agent`**|`User-Agent: Mozilla/5.0`|İsteği gönderen tarayıcı, işletim sistemi veya istemci yazılımı hakkında bilgi verir.|
|**`Referer`**|`Referer: [https://www.google.com/](https://www.google.com/)`|Kullanıcının bu isteğe hangi bağlantı/URL üzerinden yönlendiğini gösterir.|
|**`Cookie`**|`Cookie: user_type=student; room=intro`|Sunucunun daha önce tarayıcıda saklanmasını istediği durum/oturum bilgilerini taşır.|
|**`Content-Type`**|`Content-Type: application/json`|Gövdede (`Body`) sunucuya gönderilen verinin türünü ve formatını belirtir.|

### Request Body (İstek Gövdesi) ve Veri Formatları

`POST` ve `PUT` gibi istemciden sunucuya veri aktaran isteklerde veriler **HTTP Request Body** alanında taşınır. Format türü başlıkta yer alan `Content-Type` ile tanımlanır:

#### 1. URL Encoded (`application/x-www-form-urlencoded`)

- **Yapı:** Standart HTML formlarının varsayılan formatıdır. Veriler `anahtar=değer` çiftleri halinde iletilir, çiftler `&` işaretiyle ayrılır. Özel karakterler yüzde kodlamasına (percent-encoded / URL encoded) dönüştürülür.
    
- **Format:** `key1=value1&key2=value2`
    

HTTP

```
POST /profile HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 33

name=Aleksandra&age=27&country=US
```

#### 2. Form Data (`multipart/form-data`)

- **Yapı:** Birden fazla bağımsız veri bloğunun aynı anda gönderilmesini sağlar. Her blok, `boundary` (sınır çizgisi) dizesiyle birbirinden ayrılır.
    
- **Kullanım:** Web sunucusuna dosya, resim veya ikili (binary) veri yüklerken tercih edilir.
    

HTTP

```
POST /upload HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="username"

aleksandra
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="profile_pic"; filename="aleksandra.jpg"
Content-Type: image/jpeg

[İkili / Binary Görsel Verisi]
----WebKitFormBoundary7MA4YWxkTrZu0gW--
```

#### 3. JSON (`application/json`)

- **Yapı:** Veriler JSON (_JavaScript Object Notation_) standardına göre `{"anahtar": "değer"}` ikilileri olarak süslü parantezler `{ }` içinde tutulur. Çiftler virgül ile ayrılır.
    
- **Kullanım:** Modern RESTful API'lar ve SPA (Single Page Application) mimarilerinde en yaygın standarttır.
    

HTTP

```
POST /api/user HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/json
Content-Length: 62

{
  "name": "Aleksandra",
  "age": 27,
  "country": "US"
}
```

#### 4. XML (`application/xml`)

- **Yapı:** Veriler açılış ve kapanış etiketleri (`<etiket>veri</etiket>`) içine yerleştirilir ve iç içe (nested) hiyerarşik yapılar oluşturabilir.
    
- **Kullanım:** SOAP web servisleri, kurumsal entegrasyonlar ve eski web uygulamalarında yaygındır.
    

HTTP

```
POST /api/user HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/xml
Content-Length: 124

<user>
  <name>Aleksandra</name>
  <age>27</age>
  <country>US</country>
</user>
```


### HTTP Yanıtları (HTTP Responses)

Bir web uygulamasıyla etkileşime girdiğinizde sunucu, isteğin başarılı olup olmadığını veya bir sorun çıkıp çıkmadığını bildirmek için bir **HTTP Yanıtı (HTTP Response)** döndürür. Bu yanıtlar bir **durum kodu (Status Code)** ve sunucunun isteği nasıl işlediğini açıklayan kısa bir metin (**Reason Phrase**) içerir.

### Status Line (Durum Satırı)

Her HTTP yanıtının ilk satırıdır (`Start Line`). Üç temel bileşenden oluşur:

Plaintext

```
[HTTP Version] [Status Code] [Reason Phrase]
Örnek: HTTP/1.1 200 OK
```

- **HTTP Version:** Kullanılan HTTP protokol versiyonunu belirtir (ör. `HTTP/1.1`, `HTTP/2`).
    
- **Status Code:** İsteğin sonucunu belirten 3 haneli sayısal koddur.
    
- **Reason Phrase:** Durum kodunun insanlar tarafından okunabilir kısa açıklamasıdır (ör. `OK`, `Not Found`).
    

### Durum Kodu Aralıkları (Status Code Categories)

HTTP durum kodları işlevlerine göre 5 ana kategoriye ayrılır:

|**Kod Aralığı**|**Kategori Adı**|**Açıklama / Anlamı**|
|---|---|---|
|**`100 - 199`**|**Informational** (Bilgilendirici)|Sunucu isteğin bir kısmını almıştır ve devamını beklemektedir ("Devam et" sinyali).|
|**`200 - 299`**|**Successful** (Başarılı)|İstek başarıyla alınmış, anlaşılmış ve işlenmiştir. Talep edilen veri döndürülür.|
|**`300 - 399`**|**Redirection** (Yönlendirme)|İstenen kaynağın başka bir konuma taşındığını belirtir; genellikle yeni URL bilgisi verilir.|
|**`400 - 499`**|**Client Error** (İstemci Hatası)|İstekte bir sorun olduğunu gösterir (yanlış URL, eksik parametre, yetkisiz erişim vb.). Hata istemci kaynaklıdır.|
|**`500 - 599`**|**Server Error** (Sunucu Hatası)|İstemcinin isteği doğru olsa bile sunucunun isteği işlerken dahili bir sorunla karşılaştığını gösterir.|

### En Yaygın HTTP Durum Kodları

- **`100 Continue`:** Sunucu isteğin başlangıç kısmını almıştır ve kalan kısmı almaya hazırdır.
    
- **`200 OK`:** İstek başarılıdır; sunucu istenen içeriği veya yanıtı eksiksiz iletiyordur.
    
- **`301 Moved Permanently`:** Kaynak kalıcı olarak yeni bir URL'e taşınmıştır. Bundan sonra yeni adres kullanılmalıdır.
    
- **`404 Not Found`:** Sunucu belirtilen adreste (URL) ilgili kaynağı bulamamıştır.
    
- **`500 Internal Server Error`:** Sunucu tarafında beklenmeyen bir hata meydana gelmiş ve istek işlenememiştir.

### Response Headers (Yanıt Başlıkları)

Bir web sunucusu bir HTTP isteğine yanıt verdiğinde, **HTTP Yanıt Başlıkları (Response Headers)** adı verilen anahtar-değer (`Key: Value`) çiftlerini mesaja dahil eder. Bu başlıklar yanıt hakkında meta veriler sağlar ve istemciye (genellikle tarayıcıya) veriyi nasıl işlemesi gerektiğini bildirir.

### Temel / Önemli Yanıt Başlıkları

İstemci ve sunucunun yanıtı doğru şekilde işleyebilmesi için kullanılan başlıca yanıt başlıkları şunlardır:

- **`Date`**
    
    - **Örnek:** `Date: Fri, 23 Aug 2024 10:43:21 GMT`
        
    - **İşlev:** Yanıtın sunucu tarafından oluşturulduğu tam tarih ve saat bilgisini (GMT formatında) belirtir.
        
- **`Content-Type`**
    
    - **Örnek:** `Content-Type: text/html; charset=utf-8`
        
    - **İşlev:** İstemciye dönen verinin türünü (HTML, JSON, XML, görsel vb.) ve karakter kodlamasını (ör. UTF-8) bildirerek tarayıcının içeriği doğru şekilde yorumlamasını sağlar.
        
- **`Server`**
    
    - **Örnek:** `Server: nginx`
        
    - **İşlev:** İsteği işleyen web sunucusu yazılımı ve sürümü hakkında bilgi verir.
        
    - **Güvenlik Uyarısı (Information Disclosure):** Hata ayıklama için faydalı olsa da saldırganlara hedef sistemin teknolojisini açığa çıkardığı için güvenlik uzmanları tarafından bu başlığın gizlenmesi veya kaldırılması önerilir.
        

### Diğer Yaygın Yanıt Başlıkları

Tarayıcı davranışını ve güvenlik politikalarını yönetmek için kullanılan ek başlıklar:

|Başlık (Header)|Örnek Değer|Açıklama ve Güvenlik Önlemleri|
|---|---|---|
|**`Set-Cookie`**|`Set-Cookie: sessionId=38af...`|Sunucudan istemciye durum/oturum çerezi iletir.<br><br>  <br><br>**Güvenlik:** Çerez güvenliği için XSS erişimini engelleyen `HttpOnly` ve yalnızca şifreli bağlantıda iletim sağlayan `Secure` bayrakları mutlaka eklenmelidir.|
|**`Cache-Control`**|`Cache-Control: max-age=600`|Yanıtın istemci/tarayıcı önbelleğinde ne kadar süre (`max-age`) saklanacağını belirler. Hassas veriler için önbelleğe almayı engelleyen `no-cache` / `no-store` direktifleri kullanılır.|
|**`Location`**|`Location: /index.html`|Yönlendirme (`3xx`) durum kodlarında kullanılır; istemcinin yönlendirileceği yeni hedef URL'i belirtir.<br><br>  <br><br>**Güvenlik:** Kullanıcı girdisiyle manipüle edilirse **Open Redirect** (açık yönlendirme) açıklarına yol açabilir; bu nedenle doğrulanmalıdır.|

### Response Body (Yanıt Gövdesi)

Sunucunun istemciye geri gönderdiği asıl veri içeriğidir (HTML sayfaları, JSON/XML verileri, görseller, dosyalar vb.).

- **Güvenlik (XSS Koruması):** Yanıt gövdesine (özellikle kullanıcı kaynaklı dinamik veriler) veri basılmadan önce **XSS (Cross-Site Scripting)** ve içerik enjeksiyonu açıklarını önlemek amacıyla tüm veriler mutlaka filtrelenmeli (_sanitize_) ve uygun şekilde kaçış karakterleri uygulanmalıdır (_escape_).

### Güvenlik Başlıkları (Security Headers)

**Security Headers (Güvenlik Başlıkları)**, web uygulamasının genel güvenlik seviyesini artırarak **XSS (Cross-Site Scripting)**, **Clickjacking** ve **MIME-sniffing** gibi yaygın saldırılara karşı tarayıcı seviyesinde koruma sağlayan HTTP yanıt başlıklarıdır.

### 1. Content-Security-Policy (CSP)

Tarayıcının hangi kaynaklardan (script, stil, görsel vb.) veri yükleyebileceğini kısıtlayarak **XSS** ve veri enjeksiyonu saldırılarını hafifletir.

- **`'self'`:** Yalnızca web sitesinin kendi barındırıldığı alan adından kaynak yüklenmesine izin verir.
    
- **`default-src`:** Özel olarak belirtilmeyen tüm kaynak türleri için geçerli olan varsayılan kuraldır.
    
- **`script-src`:** JavaScript kodlarının hangi alan adlarından yüklenebileceğini belirler.
    
- **`style-src`:** CSS stil dosyalarının hangi alan adlarından yüklenebileceğini belirler.
    

HTTP

```
Content-Security-Policy: default-src 'self'; script-src 'self' https://cdn.tryhackme.com; style-src 'self'
```

### 2. Strict-Transport-Security (HSTS)

Tarayıcının web sitesiyle **yalnızca ve her zaman HTTPS** üzerinden güvenli/şifreli iletişim kurmasını zorunlu kılar; SSL/TLS Stripping ve Man-in-the-Middle (MitM) saldırılarını engeller.

- **`max-age`:** Kuralın tarayıcı önbelleğinde saniye cinsinden ne kadar süre geçerli kalacağını belirtir.
    
- **`includeSubDomains`:** (İsteğe bağlı) Kuralın ana domaine bağlı tüm alt alan adlarına (subdomain) da uygulanmasını sağlar.
    
- **`preload`:** (İsteğe bağlı) Sitenin tarayıcıların gömülü HSTS Preload listesine dahil edilmesini sağlayarak siteye ilk ziyarette bile doğrudan HTTPS zorunluluğu uygular.
    

HTTP

```
Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
```

### 3. X-Content-Type-Options

Tarayıcıların sunucu tarafından bildirilen `Content-Type` değerini göz ardı edip dosya türünü tahmin etmesini (**MIME sniffing**) engeller.

- **`nosniff`:** Tarayıcıya MIME türünü tahmin etmemesini, yalnızca sunucunun `Content-Type` başlığında belirttiği türe sadık kalmasını emreder (ör. zararlı bir betiğin görsel dosyası gibi çalıştırılmasını önler).
    

HTTP

```
X-Content-Type-Options: nosniff
```

### 4. Referrer-Policy

Kullanıcı bir bağlantıya tıkladığında hedef sunucuya gönderilecek `Referer` (yönlendiren kaynak URL) bilgisinin miktarını ve kapsamını kontrol eder.

|**Direktif**|**Davranış / Açıklama**|
|---|---|
|**`no-referrer`**|`Referer` başlığını tamamen devre dışı bırakır; hiçbir yönlendirme bilgisi iletilmez.|
|**`same-origin`**|Yönlendirme bilgisi yalnızca aynı kaynak (same-origin / aynı site içi) içerisindeki gezinmelerde gönderilir; dış sitelere gönderilmez.|
|**`strict-origin`**|Yalnızca güvenlik protokolü korunduğunda (HTTPS -> HTTPS) kaynak alan adı gönderilir; HTTPS'den güvensiz HTTP'ye geçişte hiçbir bilgi gönderilmez.|
|**`strict-origin-when-cross-origin`**|Aynı kökenli isteklerde tam URL yolunu gönderir; harici sitelere (farklı köken) yalnızca HTTPS korunduğu sürece sadece alan adını (origin) iletir.|

HTTP

```
Referrer-Policy: strict-origin-when-cross-origin
```

