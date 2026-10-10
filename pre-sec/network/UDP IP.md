# UDP/IP (User Datagram Protocol) Temelleri

## 1. UDP Nedir?

UDP (User Datagram Protocol), taşıma katmanında (OSI Katman 4) çalışan; veri iletimi öncesinde iki cihaz arasında sürekli bir oturum açmayan, **bağlantısız (connectionless)** ve **durumsuz (stateless)** bir iletişim protokolüdür.

- **El Sıkışma Yoktur:** TCP'de görülen 3 yönlü el sıkışma (SYN -> SYN-ACK -> ACK) süreci UDP'de işletilmez. Cihazlar arasında önceden bir senkronizasyon kurulmaz; paket doğrudan hedefe fırlatılır (_"Fire-and-forget"_).
    
- **Durumsuz (Stateless) Yapı:** İletişimin geçmişini veya geleceğini takip eden bir oturum tablosu tutmaz.
    
- **Hız Avantajı:** Onay paketleri (ACK), sıra numarası takibi veya akış kontrol mekanizmaları bulunmadığı için gecikme (latency) minimum seviyededir.
    
- **Kullanım Alanları:** Veri kaybının hafif tolere edilebildiği ancak hızın ve anlık akışın kritik olduğu senaryolar:
    
    - DNS sorguları (Port 53)
        
    - Canlı ses ve video akışları (VoIP, Discord, Zoom)
        
    - Çevrim içi (Online) oyunlar
        
    - DHCP (Port 67/68) ve SNMP (Port 161)
        

## 2. UDP ve IP Başlıklarının (Headers) Dağılımı

UDP trafiğinde paket başlıkları iki seviyede ele alınır: Ağ katmanı (IP) ve Taşıma katmanı (UDP).

Plaintext

```
┌────────────────────────────────────────────────────────┐
│                   IP BAŞLIĞI (Ağ Katmanı)               │
│  • Source IP Address         • Destination IP Address  │
│  • Time to Live (TTL)        • Protokol Numarası (17)  │
├────────────────────────────────────────────────────────┤
│                   UDP BAŞLIĞI (Taşıma Katmanı)          │
│  • Source Port (16 bit)      • Destination Port (16 bit)│
│  • Length (Uzunluk)          • Checksum (Kontrol)      │
├────────────────────────────────────────────────────────┤
│                   DATA (Payload / Veri)                │
│                   Ham veri baytları                    │
└────────────────────────────────────────────────────────┘
```

### IP Seviyesindeki Başlıklar (Ağ Dolaşımı İçin):

- **Source Address (Kaynak IP):** Paketi gönderen cihazın IP adresi.
    
- **Destination Address (Hedef IP):** Paketin yönlendirileceği hedef cihazın IP adresi.
    
- **Time to Live (TTL):** Paketin ağda sonsuz döngüye girmesini önleyen router atlama sayacı.
    

### UDP Seviyesindeki Başlıklar (Servis Eşleşmesi İçin):

- **Source Port (Kaynak Port):** Gönderici cihazdaki uygulamanın/soketin port numarası (16 bit).
    
- **Destination Port (Hedef Port):** Hedef cihazda paketi karşılayacak uygulamanın port numarası (örneğin DNS için 53).
    
- **Length (Uzunluk):** UDP başlığı ve verinin toplam bayt cinsinden uzunluğu.
    
- **Checksum (Kontrol Toplamı):** İsteğe bağlı hata denetim alanı.
    
- **Data (Payload):** Uygulamadan gelen ham veri baytları.
    

## 3. TCP vs. UDP Karşılaştırma Tablosu

|**Özellik**|**TCP**|**UDP**|
|---|---|---|
|**Bağlantı Tipi**|Bağlantı odaklı (_Connection-oriented_)|Bağlantısız (_Connectionless_)|
|**Durum Bilgisi**|Durumlu (_Stateful_)|Durumsuz (_Stateless_)|
|**El Sıkışma (Handshake)**|Var (3 Yönlü: SYN, SYN-ACK, ACK)|Yok|
|**Güvenilirlik / Doğrulama**|%100 Garantili (Kayıp paket tekrar istenir)|Garanti yok (Paket düşerse telafi edilmez)|
|**Sıralama (Sequencing)**|Paketler sıra numarasıyla birleştirilir|Paketler sırasız ulaşabilir|
|**Hız / Gecikme**|Kontrollerden dolayı daha yavaş|Ek yük olmadığı için çok hızlı|
|**Tipik Protokoller**|HTTP/S, SSH, FTP, SMTP, IMAP|DNS, DHCP, VoIP, Online Oyunlar|