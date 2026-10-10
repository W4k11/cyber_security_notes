# İnternet Varlık Keşfi ve Zafiyet Yönetimi

---

## 🌐 Arama ve İstihbarat Motorları

### 1. Shodan
İnternete bağlı **donanım ve sistem odaklı** bir arama motorudur. Sunucular, yönlendiriciler (routers), web kameraları, IoT cihazları ve Endüstriyel Kontrol Sistemleri (ICS/SCADA) gibi varlıkların başlık (banner) bilgilerini indeksler.

> [!example] Örnek Kullanım
> Hangi sunucuların belirli bir sürümü çalıştırdığını ve bu sistemlerin ülkelere göre dağılımını analiz etmek için:
> ```text
> apache 2.4.1
> ```
> *Bu sorgu, yanıt başlığında (HTTP banner) "2.4.1" geçen tüm Apache sunucularını listeler.*

---

### 2. Censys
İnternete açık varlıkları tarayan bir platformdur; Shodan'a kıyasla daha çok **web siteleri, TLS/SSL sertifikaları, alan adları (domains) ve açık servis haritalandırmasına** odaklanır.

- **Temel Kullanım Alanları:**
  - Bir kuruluşa ait aktif alan adlarını ve alt alan adlarını (subdomains) çıkarma
  - Açık port ve servisleri denetleme
  - SSL/TLS sertifika kayıtları üzerinden altyapı korelasyonu yapma
  - Ağ içerisindeki kötü amaçlı veya sahte (phishing) altyapıları keşfetme

---

### 3. Shodan vs. Censys Karşılaştırması

| Kriter | Shodan | Censys |
| :--- | :--- | :--- |
| **Ana Odak** | IoT, ICS/SCADA, sunucular, ağ cihazları | Web varlıkları, TLS sertifikaları, domain/host ilişkileri |
| **Veri Kaynağı** | Servis başlıkları (Service banners) | Sertifika şeffaflık günlükleri (CT logs), aktif taramalar |
| **Yaygın Senaryo** | Açık IP kameralar, savunmasız servis sürümleri | Saldırı yüzeyi yönetimi (ASM), sertifika analizi |

---

## 🛡️ Dosya ve Tehdit Analizi

### VirusTotal
Şüpheli dosya, URL, IP adresi ve alan adlarını analiz etmek için **onlarca farklı antivirüs motorunu ve dinamik analiz aracını** eşzamanlı çalıştıran çevrimiçi bir tehdit istihbarat platformudur.

- **Dosya Taraması:** Yüklenen dosyanın imzasını (hash) ve davranışını birden fazla güvenlik çözümüyle doğrular.
- **Topluluk ve Sandbox:** Dosyanın bıraktığı ağ trafiği, açtığı kayıt defteri anahtarları ve güvenlik uzmanlarının paylaştığı yorumları içerir.

---

## ⚠️ Zafiyetler ve İstismarlar (Vulnerabilities & Exploits)

### CVE (Common Vulnerabilities and Exposures)

> [!info] Tanım
> **CVE:** Yazılım ve donanım ürünlerinde tespit edilen kamusal güvenlik açıklarını standartlaştıran evrensel bir güvenlik açığı kataloğudur.

* **Format Yapısı:** `CVE-YIL-NUMARA` (Örn: `CVE-2024-29988`)
* **Amacı:** Güvenlik araştırmacıları, yazılım geliştiricileri ve sistem yöneticileri arasında ortak bir dil oluşturarak herkesin aynı zafiyete kesin olarak aynı kimlikle (`ID`) atıfta bulunmasını sağlar.