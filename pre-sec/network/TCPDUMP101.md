# Tcpdump

## 1. Temel Parametreler (Flags)

|**Parametre**|**Açıklama**|
|---|---|
|**`-i eth0`**|Dinlenecek ağ arayüzünü (`eth0`, `any`, `wlan0` vb.) seçer.|
|**`-D`**|Sistemde dinlenebilecek tüm ağ arayüzlerini listeler.|
|**`-n`**|IP adreslerini alan adına çevirmez (DNS sorgusu atmaz, taramayı hızlandırır).|
|**`-nn`**|Hem IP adreslerini hem de port numaralarını isimlere çevirmez (ham sayısal gösterim).|
|**`-c 20`**|Belirtilen paket sayısına (örneğin 20) ulaştığında yakalamayı otomatik durdurur.|
|**`-A`**|Paket yükünü (payload) okunabilir düz ASCII metin formatında ekrana basar.|
|**`-X`**|Paket yükünü hem HEX (onaltılık) hem de ASCII olarak iki sütun halinde gösterir.|
|**`-v / -vv / -vvv`**|Ayrıntı düzeyini (TTL, paket kimliği, bayraklar vb.) kademeli olarak artırır.|
|**`-w dosya.pcap`**|Yakalanan ham paketleri Wireshark ile uyumlu `.pcap` dosyasına kaydeder.|
|**`-r dosya.pcap`**|Önceden kaydedilmiş `.pcap` dosyasını terminal üzerinde analiz etmek için okur.|

## 2. BPF (Berkeley Packet Filter) Filtreleri

### Konum ve IP Filtreleri

- `host 192.168.1.50` : Kaynağı veya hedefi bu IP olan tüm paketleri yakalar.
    
- `src 10.0.0.5` : Yalnızca kaynak IP adresi `10.0.0.5` olan paketleri yakalar.
    
- `dst 192.168.1.1` : Yalnızca hedef IP adresi `192.168.1.1` olan paketleri yakalar.
    
- `net 192.168.1.0/24` : Belirtilen alt ağ bloğuna ait tüm trafiği yakalar.
    

### Port Filtreleri

- `port 80` : Kaynak veya hedef portu 80 (HTTP) olan trafiği filtreler.
    
- `src port 53` : Sadece kaynak portu 53 olan paketleri (örneğin gelen DNS yanıtları) getirir.
    
- `dst port 22` : Sadece hedef portu 22 olan paketleri (örneğin giden SSH bağlantıları) yakalar.
    
- `portrange 100-200` : 100 ile 200 arasındaki port aralığına ait paketleri yakalar.
    

### Protokol Filtreleri

- `tcp`, `udp`, `icmp`, `arp` : İlgili protokole ait paketleri filtrelemek için protokol ismi doğrudan filtre olarak girilir.
    

## 3. Mantıksal Operatörler

Filtreleri birleştirerek karmaşık sorgular oluşturmak için kullanılır:

|**Operatör**|**Eşdeğeri**|**Görevi**|
|---|---|---|
|**`and`**|`&&`|İki koşulun da aynı anda sağlanmasını zorunlu kılar.|
|**`or`**|`\|`|Belirtilen koşullardan en az birinin sağlanmasını kabul eder.|
|**`not`**|`!`|Belirtilen koşulun dışındaki (hariç tutulan) trafiği yakalar.|

## 4. Pratik ve Sık Kullanılan Senaryolar

### 1. DNS Trafiğini ve Domain Çözümlemelerini İzleme

Bash

```
sudo tcpdump -nn -i eth0 udp port 53
```

### 2. HTTP Paket İçeriğini Açık Metin Olarak Okuma

Bash

```
sudo tcpdump -nn -A -i eth0 port 80
```

### 3. Ping (ICMP) Trafiğini İnceleme

Bash

```
sudo tcpdump -nn -i eth0 icmp
```

### 4. Gürültülü Trafiği Hariç Tutma (SSH ve DNS Hariç)

Bash

```
sudo tcpdump -nn -i eth0 not port 22 and not port 53
```

### 5. Belirli Bir IP ile Güvenli Web (HTTPS) İletişimini Yakalama

Bash

```
sudo tcpdump -nn host 192.168.1.50 and port 443
```

### 6. Yalnızca Yeni Bağlantı İsteklerini (TCP SYN Bayraklı Paketler) Yakalama

Bash

```
sudo tcpdump -nn "tcp[tcpflags] & (tcp-syn) != 0 and tcp[tcpflags] & (tcp-ack) == 0"
```

### 7. Trafiği Wireshark için PCAP Dosyasına Kaydetme

Bash

```
sudo tcpdump -nn -i eth0 -c 500 -w analiz.pcap
```

### 8. Kayıtlı PCAP Dosyasından Yalnızca UDP Trafiğini Filtreleyerek Okuma

Bash

```
tcpdump -nn -r analiz.pcap udp
```