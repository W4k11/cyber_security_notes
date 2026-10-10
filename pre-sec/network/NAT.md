# NAT (Network Address Translation - Ağ Adresi Çevirisi)

## 1. NAT Nedir ve Neden Ortaya Çıktı?

IPv4 protokolü 32 bitlik bir yapıya sahiptir ve teorik olarak dünyada en fazla yaklaşık **4.3 milyar ($2^{32}$)** benzersiz IP adresi barındırabilir. İnternete bağlı cihaz sayısı bu sınırı hızla aşınca küresel bir IP tükenmesi krizi başladı.

Kalıcı çözüm **IPv6** protokolüne geçiş olsa da, geçiş sürecinde IPv4 adreslerinin tükenmesini önleyen en kritik geçici cankurtaran **NAT (Network Address Translation)** oldu.

- **Temel Fikir:** Yerel ağda bulunan onlarca, yüzlerce hatta binlerce cihazın internete çıkarken **tek bir genel (Public) IP adresini** ortak kullanmasını sağlamaktır.
    
- **Tasarruf:** Örneğin 30 kullanıcılı bir ofis için 30 ayrı genel IP satın almak yerine tek bir genel IP kullanılır. Kalan 29 adres küresel havuzda boşa çıkmış olur.
    

## 2. Özel IP (Private) vs. Genel IP (Public)

NAT mimarisi IP adreslerini iki gruba ayırır:

|**Tür**|**Kullanım Alanı**|**Yönlendirilebilirlik**|**Örnek Aralıklar (RFC 1918)**|
|---|---|---|---|
|**Private IP (Özel IP)**|Ev, ofis, şirket içi yerel ağlar|İnternette doğrudan dolaşamaz, router'lar tarafından engellenir|`192.168.0.0/16`<br><br>  <br><br>`172.16.0.0/12`<br><br>  <br><br>`10.0.0.0/8`|
|**Public IP (Genel IP)**|Tüm küresel internet omurgası|Dünyanın her yerinden erişilebilir ve yönlendirilebilir|`212.3.4.5` gibi ISS tarafından atanan adresler|

## 3. NAT Nasıl Çalışır? (PAT / NAT Aşırı Yükleme Mantığı)

Yerel ağdaki bir cihaz internetteki bir sunucuya gitmek istediğinde, router gelen paketin yerel kaynak adresini kendi genel IP adresiyle değiştirir ve bu işlemi **NAT Çeviri Tablosunda (Translation Table)** saklar.

### Akış Şeması

Plaintext

```
[Yerel Ağ - LAN]                                     [Dış Ağ - İnternet]
Dizüstü Bilgisayar
192.168.0.129:15401 ────► [ Router / NAT ] ────────► Web Sunucusu
                           Genel IP: 212.3.4.5        (Hedef: 93.184.216.34:80)
                           (Portu 19273'e çevirdi)
```

### 1. İstek Giderken (Giden Trafik):

1. Dizüstü bilgisayar `192.168.0.129` yerel IP'si ve `15401` kaynak portuyla web sunucusuna bir istek gönderir.
    
2. Paket yönlendiriciye (Router) ulaştığında yönlendirici paketin kaynak adresini kendi genel IP'si olan `212.3.4.5` ile değiştirir ve rastgele boş bir kaynak portu (`19273`) atar.
    
3. Yönlendirici bu eşleşmeyi hafızasındaki tabloya yazar.
    
4. Web sunucusu isteği alır; istek sanki doğrudan `212.3.4.5:19273` adresinden gelmiş gibi görünür.
    

## 4. NAT Çeviri Tablosu (Translation Table)

Yönlendiricinin paketleri doğru bilgisayara teslim etmek için tuttuğu tablo şu biçimdedir:

|**İç Kaynak IP (Local IP)**|**İç Port (Local Port)**|**Dış Kaynak IP (Public IP)**|**Dış Port (Translated Port)**|**Hedef Sunucu (Destination)**|
|---|---|---|---|---|
|`192.168.0.129`|`15401`|`212.3.4.5`|`19273`|`93.184.216.34:80`|
|`192.168.0.45`|`54210`|`212.3.4.5`|`19274`|`142.250.180.206:443`|

### 2. Yanıt Gelirken (Gelen Trafik):

1. Web sunucusu cevabı `212.3.4.5:19273` adresine gönderir.
    
2. Yönlendirici paketi karşılar ve port numarasına (`19273`) bakar.
    
3. Tablosunu kontrol eder: _"Bu port `192.168.0.129:15401` cihazına aitti."_
    
4. Hedef IP'yi tekrar dizüstü bilgisayarın yerel IP'sine çevirir ve paketi iç ağa teslim eder.
    

## 5. Güvenlik Katkısı

NAT bir güvenlik duvarı değildir; ancak doğal bir gizleme avantajı sunar:

- Dış dünyadaki hiçbir sunucu iç ağdaki cihazların gerçek yerel IP adreslerini (`192.168.0.129`) göremez.
    
- Dışarıdan izinsiz başlatılan doğrudan bağlantı istekleri, router üzerindeki NAT tablosunda önceden açılmış bir karşılığı yoksa otomatik olarak düşürülür.