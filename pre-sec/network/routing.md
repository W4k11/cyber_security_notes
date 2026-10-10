# Yönlendirme (Routing) ve Yönlendirme Protokolleri

## 1. Yönlendirme (Routing) Nedir?

Yönlendirme; bir ağdaki kaynaktan çıkan veri paketinin, hedefe ulaşana kadar **farklı ağlar ve yönlendiriciler (router) üzerinden geçerek en uygun yoldan iletilmesi sürecidir**.

Yönlendiriciler, bu kararı verirken ezbere hareket etmez; yönlendirme protokolleri sayesinde öğrendikleri **yönlendirme tablolarını (routing table)** kullanırlar.

## 2. İç Ağ Geçidi (IGP) vs. Dış Ağ Geçidi (EGP)

Yönlendirme protokolleri kullanım alanlarına göre iki ana gruba ayrılır:

Plaintext

```
               ┌────────────────────────────────────────────────────────┐
               │           İnternet / Küresel Ağlar (BGP)               │
               └──────────────────────────┬─────────────────────────────┘
                                          │
                  ┌───────────────────────┴───────────────────────┐
                  ▼                                               ▼
         [Şirket / Kampüs Ağı]                           [Başka Bir Kurum]
          (İç Ağ Protokolleri)                         (İç Ağ Protokolleri)
         OSPF, EIGRP veya RIP                         OSPF, EIGRP veya RIP
```

- **IGP (Interior Gateway Protocol):** Tek bir organizasyonun, şirketin veya kampüsün kendi iç ağında yönlendirme yapmak için kullanılır (_OSPF, EIGRP, RIP_).
    
- **EGP (Exterior Gateway Protocol):** Farklı bağımsız sistemleri (Autonomous System - AS), örneğin İnternet Servis Sağlayıcılarını (ISS) ve küresel internet omurgasını birbirine bağlamak için kullanılır (_BGP_).
    

## 3. Popüler Yönlendirme Protokolleri

### A) OSPF (Open Shortest Path First)

Büyük ve kurumsal ağlarda en çok tercih edilen açık standart yönlendirme protokolüdür.

- **Çalışma Mantığı (Link-State):** Yönlendiriciler ağdaki hatların (link) durumunu, hızını ve maliyetini sürekli paylaşır.
    
- **Ağ Haritası:** Her yönlendirici tüm ağın eksiksiz bir haritasını (topolojisini) hafızasında tutar ve Dijkstra algoritmasını çalıştırarak her hedefe giden **en verimli yolu** belirler.
    
- **Avantajı:** Ağda bir kablo koptuğunda veya hat hızlandığında saniyeler içinde yeni rotayı hesaplar (_hızlı yakınsama - convergence_).
    

### B) EIGRP (Enhanced Interior Gateway Routing Protocol)

Cisco tarafından geliştirilen, mesafe ve bağlantı durumu algoritmalarının güçlü yönlerini birleştiren gelişmiş hibrit bir protokoldür.

- **Metrik Hesabı:** En iyi yolu seçerken yalnızca router sayısına bakmaz; **bant genişliği (hız)** ve **gecikme (delay)** değerlerini harmanlayarak rotanın toplam maliyetini çıkarır.
    
- **Yedek Rota Gücü:** Ana rotanın yanında anında devreye girecek yedek rotaları (_feasible successor_) hazır tutar, bu sayede kesinti anında neredeyse anında rotayı değiştirir.
    

### C) BGP (Border Gateway Protocol)

İnternetin **küresel yönlendirme omurgasıdır**.

- **Kullanım Yeri:** Ev veya ofis içi ağlarda değil; dev telekomünikasyon şirketleri, büyük veri merkezleri ve İnternet Servis Sağlayıcıları (ISS) arasında kullanılır.
    
- **Yol Vektörü (Path-Vector):** Paketlerin dünyadaki binlerce bağımsız ağdan (AS) geçerken hangi ülkelerden ve sağlayıcılardan geçeceğini belirler. Hızdan ziyade **ağ politikaları, güvenlik ve kurumsal anlaşmalara** göre rota çizer.
    

### D) RIP (Routing Information Protocol)

Küçük ve basit ağlar için tasarlanmış en eski yönlendirme protokollerinden biridir.

- **Çalışma Mantığı (Distance-Vector):** Karar verirken tek bir kritere bakar: **Atlama Sayısı (Hop Count)**. Yani paketin hedefe giderken geçeceği router sayısı en az olan yolu seçer.
    
- **Eksisi:** Hattın kalitesini ve hızını dikkate almaz. 10 Mbps'lik tek atlamalık yavaş bir yolu, 1 Gbps'lik iki atlamalık fiber hatta tercih edebilir.
    
- **Sınırı:** En fazla **15 atlamayı** destekler; 16. durak "ulaşılamaz" sayılır. Bu nedenle büyük ağlarda kullanılmaz.
    

## 4. Karşılaştırma Özeti

|**Protokol**|**Türü**|**Karar Ölçütü (Metrik)**|**Ağ Büyüklüğü**|**Standart Durumu**|
|---|---|---|---|---|
|**RIP**|Distance-Vector|Atlama Sayısı (_Hop Count, maks: 15_)|Küçük Ağlar|Açık Standart|
|**OSPF**|Link-State|Hat Maliyeti (_Bant genişliği/Hız_)|Orta / Büyük Kurumsal Ağlar|Açık Standart|
|**EIGRP**|Gelişmiş Hibrit|Bant Genişliği + Gecikme|Büyük Ağlar|Cisco (Lisanslı/Açık Hibrit)|
|**BGP**|Path-Vector|AS Yolları ve Ağ Politikaları|Küresel İnternet / ISS'ler|Açık Standart|