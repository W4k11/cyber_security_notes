# Ağ Paketleri (Packets) ve Çerçeveler (Frames)

## 1. Paket vs. Çerçeve (Katman Farkı)

Ağ üzerinde veri tek parça halinde değil, katman katman paketlenerek (**kapsülleme / encapsulation**) iletilir. İletişim sırasında verinin bulunduğu katmana göre ismi ve yapısı değişir:

|**Katman**|**Veri Birimi (PDU)**|**Taşıdığı Adres Türü**|**İşleyen Cihaz**|
|---|---|---|---|
|**Katman 3 (Ağ Katmanı)**|**Paket (Packet)**|Mantıksal Adres (**IP Adresi**)|Router (Yönlendirici)|
|**Katman 2 (Veri Bağlantısı)**|**Çerçeve (Frame)**|Fiziksel Adres (**MAC Adresi**)|Switch (Ağ Anahtarı)|

> **Özet:** Katman 3'te hazırlanan bir **IP Paketi**, Katman 2'ye indiğinde başına ve sonuna fiziksel bilgiler eklenerek bir **Çerçeve (Frame)** içine yerleştirilir.

## 2. Neden Veriler Paketler Halinde Gönderilir?

Büyük bir dosyayı (örneğin 1 GB'lık bir videoyu) tek bir blok halinde göndermek ağ açısından risklidir:

- **Darboğazı Önleme:** Ağ hattını tek bir dosya tamamen meşgul etmez; küçük paketler halinde farklı kullanıcıların verileriyle sıralı paylaşılır.
    
- **Hata Telafisi:** İletim sırasında kabloda 1 bitlik bir bozulma yaşanırsa tüm 1 GB'lık dosyayı baştan indirmek yerine yalnızca **bozulan o küçük paket** yeniden istenir.
    
- **Esnek Rota:** Paketler internet üzerinde hedefe giderken aynı anda farklı güzergâhlardan geçerek hedefe ulaşıp orada tekrar birleşebilir.
    

## 3. Bir IP Paketinin Temel Başlık (Header) Alanları

Paketin içinde taşınan asıl verinin (payload) önünde, yönlendiricilerin paketi doğru yönetebilmesi için eklenen etiketler (başlıklar) bulunur:

Plaintext

```
┌─────────────────┬──────────────────┬─────────────────┬──────────────────────┐
│  Source IP      │  Destination IP  │  Time-to-Live   │  Checksum            │
│  (Kaynak Adres) │  (Hedef Adres)   │  (TTL Sayacı)   │  (Hata Denetimi)     │
├─────────────────┴──────────────────┴─────────────────┴──────────────────────┤
│                         ASIL VERİ (PAYLOAD)                                 │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 1. Source Address (Kaynak IP Adresi)

- Paketi ilk gönderen cihazın IP adresidir.
    
- Hedef sunucunun cevabı kime geri göndereceğini bilmesini sağlar.
    

### 2. Destination Address (Hedef IP Adresi)

- Paketin ulaşması gereken nihai cihazın veya sunucunun IP adresidir.
    
- Ağdaki router'lar bu adrese bakarak paketi bir sonraki en uygun durağa iletir.
    

### 3. Time-to-Live (TTL - Yaşam Süresi)

- Paketin ağda sonsuz döngüye girip hattı kilitlemesini engelleyen **süre sonu sayacıdır**.
    
- Saniye cinsinden değil, **geçebileceği maksimum router sayısı** üzerinden çalışır.
    
- Paket her bir router'dan geçtiğinde TTL değeri **1 azaltılır**. Sayaç 0 olduğunda router paketi düşürür ve kaynak cihaza bir `ICMP Time Exceeded` uyarısı yollar.
    

### 4. Checksum (Sağlama / Hata Kontrol Toplamı)

- Paketin yolda giderken bozulup bozulmadığını denetleyen matematiksel bir doğrulama alanıdır.
    
- Gönderici cihaz paketin içeriğine göre bir sayı (özet) hesaplar.
    
- Alıcı cihaz paketi aldığında aynı hesabı tekrar yapar. Eğer iletim sırasında fiziksel bir gürültüden ötürü veride 1 bit bile değişmişse sonuç eşleşmez; paket **hatalı** sayılarak çöpe atılır ve yenisi istenir.