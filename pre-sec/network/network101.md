# IP Adresleri, MAC Adresleri ve Ping Aracı

## 1. IP (Internet Protocol) Adresi Nedir?

IP adresi, ağa bağlı her cihaza atanan ve cihazların birbirini bulup haberleşmesini sağlayan **mantıksal bir kimlik numarasıdır**.

### IPv4 Yapısı ve Özellikleri

- **Oktet Yapısı:** IPv4 adresi nokta ile ayrılmış 4 bölümden (oktet) oluşur:
    
    `192 . 168 . 1 . 50`
    
- **Değer Aralığı:** Her oktet 8 bitlik veri taşır; dolayısıyla her bir sayı **0 ile 255** arasında bir değer alabilir.
    
- **Benzersizlik (Uniqueness):** Aynı yerel ağ içinde iki cihaz aynı IP adresini alamaz; aksi takdirde **IP çakışması (IP Conflict)** meydana gelir ve iletişim kopar.
    

### Public IP (Genel IP) vs. Private IP (Özel IP)

|**Özellik**|**Public IP (Genel IP)**|**Private IP (Özel IP)**|
|---|---|---|
|**Kullanım Alanı**|İnternet ortamında küresel iletişim|Ev, ofis, okul gibi yerel ağ (LAN) içi iletişim|
|**Kim Tahsis Eder?**|İnternet Servis Sağlayıcısı (ISS)|Yerel ağdaki yönlendirici (DHCP servisi)|
|**Maliyet**|ISS abonelik faturanıza dahildir|Tamamen ücretsizdir|
|**Erişilebilirlik**|Tüm internetten doğrudan erişilebilir|Yalnızca aynı yerel ağdaki cihazlar erişebilir|
|**Örnek Aralıklar**|`142.250.180.206` (Google sunucusu)|`192.168.1.0/24`, `10.0.0.0/8`, `172.16.0.0/12`|

## 2. MAC (Media Access Control) Adresi Nedir?

MAC adresi; cihazın ağ kartına (Ethernet kartı, Wi-Fi çipi vb.) üretim aşamasında fabrikada kazınan, değiştirilemez nitelikteki **fiziksel (donanım) kimlik numarasıdır**.

- **Biçim:** İki nokta üst üste (`:`) veya tire (`-`) ile ayrılmış 6 çiftten oluşan 12 karakterlik onaltılık (Hexadecimal - 0-9, A-F) bir değerdir.
    
    - _Örnek:_ `A4:C3:F0:85:AC:2D`
        

Plaintext

```
       A4 : C3 : F0  :  85 : AC : 2D
      └──────┬──────┘  └──────┬──────┘
             │                │
        OUI Bölümü       Cihaz Kimliği
     (Üretici Şirket)    (Seri Numarası)
```

- **İlk 6 Karakter (OUI - Organizationally Unique Identifier):** Donanım kartını üreten şirketi (Intel, Apple, Realtek, Cisco vb.) tanımlar.
    
- **Son 6 Karakter (NIC-Specific):** Üreticinin o karta özel verdiği benzersiz seri numarasıdır.
    

> **IP vs. MAC Farkı:**
> 
> IP adresi bulunduğunuz konuma göre değişen geçici ev adresiniz gibidir (taşındıkça değişir). MAC adresi ise cihazın değişmeyen T.C. kimlik numarası / parmak izidir.

## 3. `ping` Komutu ve Kullanımı

`ping`, iki cihaz arasındaki fiziksel/mantıksal bağlantının çalışıp çalışmadığını ve hattın gecikme performansını denetleyen en temel ağ analiz aracıdır. Arka planda **ICMP (Internet Control Message Protocol)** paketlerini kullanır.

### Temel Kullanım:

Bash

```
ping -c 4 google.com
```

### Parametre Açıklamaları ve Çıktı Analizi

- **`-c 4` (Count):** Linux ve macOS sistemlerinde sonsuza kadar ping atmak yerine işlemi **4 paket ile sınırlandırır** (Windows sistemlerinde varsayılan olarak zaten 4 paket atılır).
    
- **RTT (Round-Trip Time):** Paketin hedefe varıp geri dönmesi arasında geçen süreyi milisaniye (`ms`) cinsinden belirtir.
    
- **Paket Kaybı (Packet Loss):** Gönderilen paketlerin kaçının yanıt alamadan kaybolduğunu gösterir; %0 olması hattın sağlıklı ve stabil olduğunu kanıtlar.