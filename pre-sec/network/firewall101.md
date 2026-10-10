# Güvenlik Duvarı (Firewall) Temelleri

## 1. Firewall Nedir ve Ne İşe Yarar?

Güvenlik Duvarı (Firewall); bir iç ağ ile dış dünya (örneğin internet) arasında veya farklı ağ segmentleri arasında duran, önceden belirlenmiş güvenlik kurallarına göre **gelen ve giden trafiği denetleyen, filtreleyen ve yetkisiz erişimleri engelleyen** yazılımsal veya donanımsal bir güvenlik mekanizmasıdır.

### Bir Paketi İncelerken Sorduğu 4 Temel Soru:

1. **Kaynak (Source IP):** Trafik tam olarak nereden geliyor?
    
2. **Hedef (Destination IP):** Trafik nereye gitmek istiyor?
    
3. **Port (Port Numarası):** Trafik hangi servise ulaşmaya çalışıyor? _(Örn: Web için 80/443, SSH için 22)_
    
4. **Protokol (Protocol):** Hangi iletişim kuralı kullanılıyor? _(TCP, UDP, ICMP vb.)_
    

## 2. Temel Türler: Stateful vs. Stateless

Güvenlik duvarları ağ bağlantılarını ele alma şekillerine göre iki ana mimariye ayrılır:

Plaintext

```
┌────────────────────────────────────────────────────────────────────────┐
│                        STATELESS FIREWALL                              │
│   Paket 1 ──► [Kural Listesi] ──► Geçti / Reddedildi (Hafıza yok)      │
│   Paket 2 ──► [Kural Listesi] ──► Geçti / Reddedildi (Önceki paketi    │
│ bilmez)                                                                │
└────────────────────────────────────────────────────────────────────────┘

┌────────────────────────────────────────────────────────────────────────┐
│                        STATEFUL FIREWALL                               │
│   Paket 1 ──► [El Sıkışma / Bağlantı Tablosu] ──► Oturum Açıldı        │
│   Paket 2 ──► Bağlantı güvenilir oturuma ait ──► Otomatik Geçti        │
│   Şüpheli ──► Oturumu ve tüm cihazı engelle                            │
└────────────────────────────────────────────────────────────────────────┘
```

### A) Stateless Firewall (Durumsuz Güvenlik Duvarı)

Bağlantının geçmişini, oturumun önceki adımlarını veya geleceğini takip etmez. Her bir paketi birbirinden tamamen bağımsız birer parça olarak ele alır.

- **Çalışma Prensibi:** Statik kurallar tablosuna bakar. Eğer paket _"IP:Port:Protokol"_ kuralına uyuyorsa geçirir, uymuyorsa atar.
    
- **Hafıza Tutmaz:** 10 saniye önce gelen paket ile şu an gelen paket arasında mantıksal bir bağ kuramaz. Tek bir paket hatalı veya zararlı olsa bile tüm cihazı bloklamaz; yalnızca o anki paketi düşürür.
    
- **Performans Avantajı:** Durum takibi (bağlantı tablosu) yapmadığı için CPU ve RAM tüketimi son derece düşüktür.
    
- **İdeal Kullanım Alanı:** Büyük miktarda veri trafiğinin hızlıca filtrelenmesi gereken yerlerde ve yüksek hacimli **DDoS saldırılarını** karşılamada harika çalışır.
    

### B) Stateful Firewall (Durum Bilgisi Tutan Güvenlik Duvarı)

Paketleri tek tek değerlendirmek yerine, iletişim oturumunun tamamını (**connection state**) canlı bir tabloda izler ve cihazın genel davranışına bakar.

- **Çalışma Prensibi:** İletişimin TCP el sıkışmasıyla (3-way handshake) kurallara uygun başlayıp başlamadığını, devam eden veri akışının mantıklı olup olmadığını kontrol eder.
    
- **Dinamik Karar Alma:** Bir bağlantıdan zararlı veya anormal bir davranış sezilirse, yalnızca o paketi değil ilgili oturumu ve gerekiyorsa o cihazdan gelen tüm trafiği dinamik olarak engeller.
    
- **Performans Yükü:** Ağdaki aktif her oturumun durumunu RAM üzerinde bir bağlantı tablosunda (state table) tuttuğu için Stateless sistemlere göre çok daha fazla sistem kaynağı tüketir.
    
- **Güvenlik Avantajı:** Oturum taklidi (session spoofing) veya el sıkışma dışı yetkisiz paket geçişlerini doğrudan engellediği için modern kurumsal ağların temel koruma katmanıdır.
    

## 3. Stateful ve Stateless Karşılaştırma Özeti

|**Özellik**|**Stateless Firewall**|**Stateful Firewall**|
|---|---|---|
|**İnceleme Kapsamı**|Yalnızca tekil paket başlıkları|Tüm bağlantı oturumu ve bağlam|
|**Bağlantı Takibi (State Table)**|Yok (Hafızasız)|Var (Aktif oturum tablosu)|
|**Kaynak Tüketimi (CPU/RAM)**|Çok düşük ve hafif|Yüksek|
|**Hız / Veri İşleme Kapasitesi**|Çok yüksek|Durum tablosu boyutuna bağlı|
|**DDoS Dayanıklılığı**|Yüksek (Kaynak tüketmeden eler)|Durum tablosu şişebilir (State table exhaustion)|
|**Güvenlik Derinliği**|Temel düzey|İleri düzey|