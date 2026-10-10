# TLS (Transport Layer Security) ve SSL Tarihçesi

## 1. TLS Nedir ve Nasıl Ortaya Çıktı?

TLS (Transport Layer Security), istemci ile sunucu arasındaki ağ trafiğini uçtan uca şifreleyen, veri bütünlüğünü sağlayan ve kimlik doğrulaması sunan endüstri standardı bir güvenlik protokolüdür.

### Tarihsel Gelişim Süreci:

- **1990'ların Başı (Netscape & SSL Doğuşu):** Netscape Communications, web trafiğindeki açık metin (cleartext) riskini fark ederek **SSL (Secure Sockets Layer)** protokolünü geliştirdi. İlk halka açık sürüm olan **SSL 2.0**, 1995 yılında kullanıma sunuldu.
    
- **1999 (IETF ve TLS 1.0):** Protokolün yönetimini üstlenen IETF (The Internet Engineering Task Force), SSL 3.0'ın yerini alan, daha güçlü şifreleme ve ek güvenlik özellikleri barındıran **TLS 1.0**'ı yayımladı.
    
- **Modern Çağ (TLS 1.3):** 20 yılı aşkın süren optimizasyon ve güvenlik yamalarının ardından, 2018 yılında **TLS 1.3** yayımlandı. Bu sürüm; eski ve güvensiz şifreleme algoritmalarını tamamen terk etti ve bağlantı kurma (handshake) gecikmesini 1 gidiş-dönüşe (1-RTT) düşürdü.
    

> **Güvenlik Notu:** Günümüzde SSL'in tüm sürümleri (SSL 2.0 ve 3.0) ile erken TLS sürümleri (TLS 1.0 ve 1.1) güvensiz kabul edilir; modern sistemlerde yalnızca **TLS 1.2** ve **TLS 1.3** kullanılmalıdır.

## 2. Şifresiz (Düz Metin) vs. Güvenli (TLS/SSL) Port Karşılaştırması

Standart ağ servislerinin açık metin çalışan geleneksel sürümleri ile TLS/SSL ile güvenceye alınmış modern sürümlerinin port tablosu:

|**Servis / Protokol**|**Düz Metin (Açık Metin) Portu**|**Güvenli (TLS / SSL) Portu**|**Açıklama**|
|---|---|---|---|
|**Web Trafiği**|**HTTP** → `80`|**HTTPS** → `443`|Tarayıcı ile sunucu arasındaki web trafiği|
|**E-Posta Gönderimi**|**SMTP** → `25`|**SMTPS** → `465` / `587`|Posta aktarımı (`587` STARTTLS ile şifrelenir)|
|**E-Posta İndirme**|**POP3** → `110`|**POP3S** → `995`|Yerel istemciye mail çekme ve depolama|
|**E-Posta Senkronizasyonu**|**IMAP** → `143`|**IMAPS** → `993`|Çoklu cihaz arasında anlık posta kutusu eşleme|

## 3. TLS Sağladığı 3 Temel Güvenlik Sütunu

1. **Gizlilik (Encryption):** Veri şifrelenerek taşınır. Araya giren bir saldırgan (paket dinleme / sniffing) yalnızca anlamsız karakter dizileri görür.
    
2. **Bütünlük (Integrity):** Aktarılan verinin yolda tahrif edilip edilmediği (kurcalama / tampering) kriptografik özetlerle (HMAC) denetlenir.
    
3. **Kimlik Doğrulama (Authentication):** Dijital sertifikalar sayesinde istemci, bağlandığı sunucunun gerçekten hedeflediği meşru kurum olduğunu doğrular (sahte sunucuları / MitM engeller).