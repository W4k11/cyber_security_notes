# TCP/IP Modeli ve TCP Protokolü Derinlemesine İnceleme

## 1. TCP/IP Modeli Nedir?

TCP/IP, günümüz internetinin ve modern bilgisayar ağlarının üzerinde çalıştığı pratik ve durumlu (**stateful**) iletişim protokolü yığınıdır. OSI referans modelinin 7 katmanını daha sade ve uygulanabilir **4 katmanda** birleştirir:

Plaintext

```
  OSI Modeli (7 Katman)                  TCP/IP Modeli (4 Katman)
 ┌──────────────────────┐               ┌──────────────────────┐
 │ Katman 7: Uygulama   │               │                      │
 │ Katman 6: Sunum      │ ────────────► │ Katman 4: Uygulama   │
 │ Katman 5: Oturum     │               │                      │
 ├──────────────────────┤               ├──────────────────────┤
 │ Katman 4: Taşıma     │ ────────────► │ Katman 3: Taşıma     │
 ├──────────────────────┤               ├──────────────────────┤
 │ Katman 3: Ağ         │ ────────────► │ Katman 2: İnternet   │
 ├──────────────────────┤               ├──────────────────────┤
 │ Katman 2: Veri Bağl. │ ────────────► │ Katman 1: Ağ         │
 │ Katman 1: Fiziksel   │               │ Arayüzü              │
 └──────────────────────┘               └──────────────────────┘
```

- **Uygulama Katmanı (Application):** Kullanıcının gördüğü servisler ve protokoller (HTTP, DNS, SSH, FTP, SMTP).
    
- **Taşıma Katmanı (Transport):** Uçtan uca güvenilir veya hızlı veri akışını yöneten katman (TCP, UDP).
    
- **İnternet Katmanı (Internet):** Mantıksal adresleme ve paket yönlendirmesini yapan katman (IP, ICMP, ARP).
    
- **Ağ Arayüzü Katmanı (Network Access / Link):** Fiziksel kablolar, Wi-Fi sinyalleri, Ethernet çerçeveleri ve MAC adresleri.
    

## 2. TCP Segment ve IP Paket Başlıkları

İletişim sırasında veriye hem ağ katmanında (IP) hem de taşıma katmanında (TCP) kritik kontrol başlıkları eklenir:

|**Alan**|**Hangi Katmanda?**|**Ne İşe Yarar?**|
|---|---|---|
|**Source / Destination IP**|IP (İnternet)|Paketin çıktığı kaynak cihazı ve ulaşacağı hedef adresi tanımlar.|
|**TTL (Time to Live)**|IP (İnternet)|Paketin sonsuz döngüye girmesini önleyen router atlama sayacıdır (her durakta 1 düşer).|
|**Source / Destination Port**|TCP (Taşıma)|Paketin hangi uygulamaya veya servise (80, 22, 443 vb.) ait olduğunu gösterir.|
|**Sequence Number (Sıra No)**|TCP (Taşıma)|İlk bağlantıda rastgele (ISN) üretilir; paketlerin alıcı tarafta doğru sırayla birleştirilmesini sağlar.|
|**Acknowledgment No (ACK No)**|TCP (Taşıma)|Alıcının bir sonraki adımda beklediği bayt numarasını gösterir (`Alınan Sıra No + 1`).|
|**Flags (Bayraklar)**|TCP (Taşıma)|Paketin türünü ve bağlantının durumunu belirler (SYN, ACK, FIN, RST, PSH, URG).|
|**Checksum (Kontrol Toplamı)**|TCP & IP|Verinin yolda bozulup bozulmadığını kontrol eden matematiksel özet/bütünlük alanıdır.|
|**Data (Payload)**|TCP (Taşıma)|Taşınan asıl ham verinin baytlarıdır.|

## 3. TCP Üç Yönlü El Sıkışma (Three-Way Handshake)

TCP "durumlu" bir protokol olduğu için rastgele veri yollamaz; önce istemci ile sunucu arasında güvenli bir hat kurulur:

Plaintext

```
      İSTEMCİ (Client)                                   SUNUCU (Server)
             │                                                  │
             │ ──────── 1. SYN (Seq=X) ───────────────────────► │
             │                                                  │
             │ ◄─────── 2. SYN/ACK (Seq=Y, Ack=X+1) ─────────── │
             │                                                  │
             │ ──────── 3. ACK (Seq=X+1, Ack=Y+1) ────────────► │
             │                                                  │
             ├──────────────── BAĞLANTI KURULDU ────────────────┤
             │                                                  │
             │ ════════ 4. DATA (Veri Aktarımı Başlar) ═══════► │
```

1. **Adım (SYN):** İstemci, sunucuya bir senkronizasyon isteği gönderir ve kendi başlangıç sıra numarasını (`Seq=X`) bildirir:
    
    > _"Seninle konuşmak istiyorum, başlangıç numaram X."_
    
2. **Adım (SYN-ACK):** Sunucu isteği kabul eder, istemcinin numarasını onaylar (`Ack=X+1`) ve kendi sıra numarasını iletir (`Seq=Y`):
    
    > _"İsteğini aldım (X+1 bekliyorum), ben de seninle konuşmaya hazırım, benim numaram Y."_
    
3. **Adım (ACK):** İstemci sunucunun yanıtını onaylar (`Ack=Y+1`):
    
    > _"Senin numaranı da aldım (Y+1 bekliyorum). Bağlantı hazır!"_
    
4. **Veri Aktarımı (DATA):** Karşılıklı anlaşma sağlandıktan sonra asıl dosya ve veri baytları iletilmeye başlar.
    

## 4. Bağlantıyı Sonlandırma Süreci: FIN vs. RST

TCP oturumu tamamlandığında iki farklı senaryoyla kapatılabilir:

### A) Normal Kapatma (Four-Way Handshake - FIN / ACK)

İki tarafın da gönderecek verisi bittiğinde bağlantı nezaket kurallarıyla, 4 adımda temiz şekilde sonlandırılır:

Plaintext

```
İstemci ──► FIN (Gönderecek verim bitti)        ──► Sunucu
İstemci ◄── ACK (Anlaşıldı, aldım)              ◄── Sunucu
İstemci ◄── FIN (Benim de gönderecek verim bitti) ◄── Sunucu
İstemci ──► ACK (Tamamdır, güle güle)           ──► Sunucu
```

### B) Aniden Kapatma (RST - Reset Bayrağı)

- Normal kapanma adımlarını beklemeden **bağlantıyı anında koparmak** için kullanılır.
    
- Port kapalıysa, beklenmeyen bir paket geldiyse veya bir güvenlik duvarı bağlantıyı zorla kesmek istiyorsa karşı tarafa `RST` bayraklı paket gönderilir ve oturum tek hamlede düşürülür.