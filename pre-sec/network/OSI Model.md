# OSI Referans Modeli (Open Systems Interconnection)

## 1. OSI Modeli Nedir ve Neden Kullanılır?

Farklı üreticilere ait bilgisayarların, donanımların ve işletim sistemlerinin bir ağ üzerinde sorunsuz bir şekilde haberleşebilmesi için ISO tarafından geliştirilmiş **7 katmanlı standart bir mimaridir**.

- **Evrensel Uyumluluk:** Donanım veya yazılım nasıl tasarlanmış olursa olsun, OSI standartlarına uyan her cihaz birbiriyle konuşabilir.
    
- **Modüler Yapı:** Bir katmanda yapılan değişiklik veya geliştirme (örneğin kablonun fiber optikle değiştirilmesi), üst katmanlardaki yazılımları (örneğin tarayıcınızı) doğrudan etkilemez.
    

## 2. 7 Katmanın Özeti

Plaintext

```
 Katman No   Katman Adı       Veri Birimi (PDU)   Temel Donanım / Örnek
──────────────────────────────────────────────────────────────────────────
 Katman 7    Uygulama         Veri (Data)         Tarayıcı, HTTP, DNS
 Katman 6    Sunum            Veri (Data)         SSL/TLS, JPEG, ASCII
 Katman 5    Oturum           Veri (Data)         NetBIOS, RPC
 Katman 4    Taşıma           Segment / Datagram  TCP, UDP
 Katman 3    Ağ               Paket (Packet)      Router, IP, ICMP
 Katman 2    Veri Bağlantısı  Çerçeve (Frame)     Switch, MAC, NIC
 Katman 1    Fiziksel         Bit (0 ve 1'ler)    Kablo, Hub, Sinyaller
```

## 3. Katmanların Detaylı İncelemesi (Aşağıdan Yukarıya)

### Katman 1: Fiziksel Katman (Physical Layer)

Verinin saf elektriksel, optik veya radyo sinyallerine dönüştürülüp fiziksel ortam üzerinden aktarıldığı katmandır.

- **Veri Tipi:** İkili sistemdeki bitler (`0` ve `1`ler).
    
- **Bileşenler:** Ethernet (Bakır/RJ45) kabloları, fiber optik kablolar, Wi-Fi radyo frekansları, hub'lar ve tekrarlayıcılar (repeater).
    

### Katman 2: Veri Bağlantısı Katmanı (Data Link Layer)

Aynı yerel ağda (LAN) bulunan iki donanım arasında hatasız veri aktarımından sorumludur.

- **İşleyiş:** Ağ katmanından gelen paketleri alır, fiziksel donanım adreslerini (**MAC Adresi**) ekleyerek **çerçevelere (frame)** dönüştürür.
    
- **Bileşenler:** Ağ arayüz kartları (Ethernet/Wi-Fi NIC) ve Ağ Anahtarları (**Switch**).
    

### Katman 3: Ağ Katmanı (Network Layer)

Paketlerin farklı ağlar arasında kaynaktan hedefe en verimli rotadan ulaştırılmasını (**Routing / Yönlendirme**) sağlar.

- **Temel Yapıtaşı:** Mantıksal adresleme yani **IP adresleridir**. Bu katmanın merkez cihazı **Router (Yönlendirici)**'dır.
    
- **En İyi Yol (Rota) Nasıl Seçilir?** Yönlendirme protokolleri (OSPF, RIP, BGP) şu kriterlere göre karar verir:
    
    - **Kestirme Yol (Mesafe / Hop Count):** Paketin geçeceği router sayısının azlığı.
        
    - **Hız ve Bant Genişliği:** Yolun bakır kablo mu yoksa yüksek hızlı fiber optik hat mı olduğu.
        
    - **Güvenilirlik ve Kalite:** Güzergâhta paket kaybı veya aşırı gecikme olup olmadığı.
        

### Katman 4: Taşıma Katmanı (Transport Layer)

İki uç cihaz arasındaki uçtan uca veri akışının kurallarını belirler. İhtiyaca göre iki ana protokolden biri devreye girer:

|**Kriter**|**TCP (Transmission Control Protocol)**|**UDP (User Datagram Protocol)**|
|---|---|---|
|**Bağlantı Türü**|Bağlantı odaklı (3 Yönlü El Sıkışma / Handshake şarttır)|Bağlantısız (Handshake yok, doğrudan gönderir)|
|**Güvenilirlik**|**%100 Güvenilir:** Paket kaybolursa tekrar istenir, sıra kontrolü ve akış denetimi yapar.|**Güvenilirlik garantisi yok:** Paketin gidip gitmediği kontrol edilmez.|
|**Hız**|Kontrol mekanizmaları nedeniyle daha yavaş|Ek kontrol başlıkları olmadığı için aşırı hızlı|
|**Kullanım Alanları**|Veri kaybının kabul edilemeyeceği durumlar: HTTP/HTTPS, SSH, FTP, SMTP, IMAP|Hız ve anlık iletişimin önemli olduğu durumlar: Canlı yayınlar, sesli/görüntülü aramalar, online oyunlar, DNS sorguları|

### Katman 5: Oturum Katmanı (Session Layer)

İki sistem arasındaki iletişimin başlamasını, yönetilmesini ve güvenli şekilde sonlandırılmasını koordine eder.

- **Oturum Yönetimi:** Cihazlar arasında benzersiz bir oturum (_session_) açar, veri akışı bitene kadar bu oturumu canlı tutar.
    
- **Zaman Aşımı ve Kapatma:** İletişim koptuğunda veya işlem bittiğinde oturumu kapatıp kaynakları serbest bırakır.
    

### Katman 6: Sunum Katmanı (Presentation Layer)

Ağın **çevirmeni ve tercümanıdır**. Uygulama katmanının anlayacağı dil ile ağın anlayacağı biçim arasındaki dönüşümü sağlar.

- **Veri Çevirisi (Formatlama):** Farklı işletim sistemleri arasındaki karakter kodlama farklarını (ASCII, UTF-8) çözer.
    
- **Şifreleme (Encryption / Decryption):** Güvenli veri trafiği için şifreleme ve şifre çözme işlemlerini (SSL/TLS) yönetir.
    
- **Sıkıştırma (Compression):** Ağ üzerinden iletilecek veriyi boyut olarak küçültür.
    

### Katman 7: Uygulama Katmanı (Application Layer)

Kullanıcının doğrudan etkileşim kurduğu yazılımların ağ hizmetlerine eriştiği en üst katmandır.

- **İşlevi:** Web tarayıcıları, e-posta istemcileri ve dosya yöneticileri gibi uygulamalara ağ servisleri sağlar.
    
- **Protokoller:** Web için **HTTP/HTTPS**, alan adı çözme için **DNS**, dosya aktarımı için **FTP**, posta erişimi için **IMAP/SMTP**.