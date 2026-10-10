# Wireshark Ağ Analiz ve Paket İnceleme Rehberi

## 1. Wireshark Nedir?

Wireshark, ağ trafiğini anlık yakalayan ve derinlemesine paket analizi sunan sektör standardı açık kaynaklı bir ağ analiz aracıdır.

- **Saldırı Tespit Sistemi (IDS/IPS) Değildir:** Trafiği otomatik olarak engellemez veya alarm üretmez; analiste ham veriyi inceleme imkânı sunar.
    
- **Kullanım Amaçları:**
    
    - Ağ tıkanıklıklarını, gecikmeleri ve iletişim kopukluklarını tespit etmek.
        
    - Şüpheli paketleri, olağan dışı bağlantıları ve güvenlik anormalliklerini araştırmak.
        
    - Protokollerin başlık alanlarını, durum kodlarını ve veri yüklerini (payload) incelemek.
        

## 2. Arayüz Bileşenleri (GUI Mimarisi)

|**Bölüm Adı**|**Konumu / Tanımı**|**Temel Görevi**|
|---|---|---|
|**Toolbar (Araç Çubuğu)**|En Üst Menü|Başlat, durdur, yeniden başlat, filtrele, kaydet ve birleştir işlevlerine hızlı erişim sağlar.|
|**Display Filter Bar**|Üst Arama Çubuğu|Yakalanmış paketler üzerinde protokol, IP veya port bazlı filtreleme sorgularının yazıldığı alandır.|
|**Recent Files**|Karşılama Ekranı|Daha önce incelenmiş olan `.pcap` / `.pcapng` dosyalarının hızlı erişim listesidir.|
|**Capture Filter & Interfaces**|Ana Başlangıç Paneli|Dinlenecek ağ kartlarını (`eth0`, `ens33`, `lo`, Wi-Fi) seçer ve yakalama öncesi BPF filtreleri uygular.|
|**Packet List Pane**|Üst Tablo Paneli|Yakalanan paketlerin sıra no, zaman, kaynak IP, hedef IP, protokol ve özet bilgilerini satır satır listeler.|
|**Packet Details Panel**|Orta Ağaç Paneli|Seçilen tek bir paketin OSI katmanlarına (Katman 2'den 7'ye kadar) göre ayrıştırılmış başlık alanlarını açar.|
|**Packet Bytes Pane**|Alt Ham Veri Paneli|Seçili paketin baytlarını sol tarafta HEX (onaltılık), sağ tarafta ise okunabilir ASCII formatında gösterir.|
|**Status Bar**|En Alt Çubuk|Toplam paket sayısı, görüntülenen paket sayısı ve aktif profil bilgisini gösterir.|

## 3. Temel Operasyonel İşlemler

- **Trafik Yakalamayı Başlatma / Durdurma:**
    
    - **Başlat:** Mavi köpekbalığı yüzgeci ikonuna tıklanır.
        
    - **Durdur:** Kırmızı kare ikonuna tıklanır.
        
- **PCAP Dosyalarını Birleştirme (Merge):** İki farklı analizi tek dosyada toplamak için `File -> Merge` yolu izlenir.
    
- **Dosya Özelliklerini İnceleme:** Trafiğin süresi, ortalama bant genişliği ve arayüz bilgisi için `Statistics -> Capture File Properties` sekmesi kullanılır.
    
- **Renklendirme Kuralları (Coloring Rules):** Hatalı veya şüpheli paketleri renklerle ayırt etmek için `View -> Coloring Rules` menüsü kullanılır.
    

## 4. Paket Ayrıştırma (Packet Dissection) ve Detaylar

Bir pakete tıklandığında **Packet Details** panelinde incelenebilen katman başlıkları:

Plaintext

```
┌────────────────────────────────────────────────────────┐
│ Frame / Packet      : Fiziksel katman boyutu ve zamanı │
│ Source / Dest MAC   : Katman 2 (Ethernet) donanım adres│
│ Source / Dest IP    : Katman 3 (IPv4 / IPv6) mantıksal │
│ Protokol Detayları  : Katman 4 (TCP / UDP portları vb.)│
│ Application Protocol: Katman 7 (HTTP, DNS, TLS vb.)    │
│ Application Data    : Taşınan asıl içerik (Payload)    │
└────────────────────────────────────────────────────────┘
```

## 5. Pratik Analiz Kısayolları

- **Numaraya Göre Pakete Gitme:** `Go -> Go to Packet...` (veya `Ctrl + G`).
    
- **Paket İçinde Arama:** `Edit -> Find Packet...` (veya `Ctrl + F`) ile metin, hex veya regex araması yapılır.
    
- **Paketi İşaretleme:** Seçili pakete `Sağ Tık -> Mark/Unmark Packet` (veya `Ctrl + M`).
    
- **Yorum Ekleme:** Ekip incelemeleri için `Sağ Tık -> Packet Comments -> Edit Packet Comment`.
    
- **Belirli Paketleri Dışa Aktarma:** Yalnızca filtrelenmiş veriyi kaydetmek için `File -> Export Specified Packets`.
    
- **Dosya Çıkarma (Export Objects):** Ağdan akan dosyaları (HTTP, SMB, TFTP vb.) disk üzerine kaydetmek için `File -> Export Objects -> [Protokol]`.
    
- **Zaman Biçimini Değiştirme:** `View -> Time Display Format` ile UTC saati, epoch veya yakalama anından itibaren geçen süre seçilebilir.
    
- **Stream Takibi ve Kelime Arama:** Paket üzerindeki oturum akışını düz metin görmek için `Sağ Tık -> Follow -> TCP Stream / HTTP Stream` seçilir, açılan penceredeki arama çubuğuyla anahtar kelimeler bulunur.
    

## 6. Uzman Bilgisi (Expert Info) Bildirim Seviyeleri

Wireshark ağdaki anormallikleri otomatik tespit edip `Analyze -> Expert Info` sekmesinde 4 seviyede raporlar:

|**Önem Derecesi**|**Renk**|**Anlamı ve Kapsamı**|
|---|---|---|
|**Chat**|Mavi|Standart iş akış bilgileri (TCP bağlantı açılışı, kapanışı vb.).|
|**Note**|Camgöbeği (Cyan)|Dikkat edilmesi gereken uygulama seviyesi durum bildirimleri veya HTTP kodları.|
|**Warn**|Sarı|Normal dışı durumlar, yeniden iletimler (retransmissions) veya gecikme alarmları.|
|**Error**|Kırmızı|Bozuk paketler, geçersiz sağlama toplamları (checksum error) ve kritik kesintiler.|

## 7. Yakalama (Capture) vs. Görüntüleme (Display) Filtreleri

- **Capture Filters (Yakalama Filtreleri):** Trafik kaydedilmeden önce uygulanır. Diske yazılacak veriyi baştan sınırlar; sonradan geri döndürülemez. BPF formatı kullanır _(Örn: `host 192.168.1.1 and port 80`)_.
    
- **Display Filters (Görüntüleme Filtreleri):** Yakalanmış tüm verinin içerisinden sadece istenenleri ekranda göstermek için kullanılır. İstendiğinde kaldırılıp tüm trafik tekrar görülebilir _(Örn: `http.request.method == "POST"`)_.
    

> **Altın Kural:** Sorgu yazmakla uğraşmak istemiyorsanız, Packet Details bölmesinde incelediğiniz herhangi bir alanın üzerine gelin ve **`Sağ Tık -> Apply as Filter -> Selected`** seçeneğini kullanın.