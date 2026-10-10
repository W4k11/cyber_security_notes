# Nmap (Network Mapper) Kılavuzu ve Komut Notları

## 1. Nmap Nedir?

Nmap; ağdaki aktif cihazları keşfetmek, açık portları tespit etmek, portlar üzerinde çalışan servisleri ve sürümlerini belirlemek ile işletim sistemi parmak izi analizi yapmak amacıyla kullanılan açık kaynaklı bir ağ güvenlik ve keşif aracıdır.

## 2. Hedef Belirleme Yöntemleri

|**Hedef Biçimi**|**Örnek Sözdizimi**|**Açıklama**|
|---|---|---|
|**Tekil IP**|`nmap 192.168.0.50`|Sadece belirtilen adresi tarar.|
|**IP Aralığı**|`nmap 192.168.0.1-10`|1 ile 10 arasındaki ilk 10 cihazı tarar.|
|**Alt Ağ (CIDR)**|`nmap 192.168.0.0/24`|`192.168.0.1` ile `192.168.0.254` arasındaki tüm ağı tarar.|

## 3. Ağ Keşif Komutları (Host Discovery)

- **`-sn` (Ping Taraması / No Port Scan):** Port taraması yapmadan yalnızca ağda hangi cihazların açık (online) olduğunu tespit eder.
    
    Bash
    
    ```
    nmap -sn 192.168.0.0/24
    ```
    
- **`-sL` (Liste Taraması):** Hedeflere tek bir paket bile göndermez; yalnızca DNS sorgusu yaparak hedef IP listesini ve alan adlarını ekrana döker.
    
    Bash
    
    ```
    nmap -sL 192.168.0.0/24
    ```
    
- **`-Pn` (Ping Atlaması):** ICMP ping paketlerini engelleyen güvenlik duvarlarını aşmak için hedefi baştan "açık" kabul eder ve doğrudan port taramasına geçer.
    
    Bash
    
    ```
    nmap -Pn 192.168.0.50
    ```
    

## 4. Tarama Türleri ve Port Seçenekleri

|**Parametre**|**Tarama Türü**|**Çalışma Mantığı**|
|---|---|---|
|**`-sS`**|**SYN (Stealth) Taraması**|TCP 3 yönlü el sıkışmayı tamamlamaz (SYN gönderir, SYN-ACK alır, RST gönderip keser). Hızlıdır ve loglarda daha az iz bırakır. Varsayılan tarama türüdür (root/admin yetkisi gerektirir).|
|**`-sT`**|**TCP Connect Taraması**|3 yönlü el sıkışmayı (`SYN -> SYN-ACK -> ACK`) tamamen kurar. Root yetkisi gerekmediğinde otomatik devreye girer; log kayıtlarında net iz bırakır.|
|**`-sU`**|**UDP Taraması**|DNS (53), DHCP (67), SNMP (161) gibi UDP tabanlı servisleri tarar. TCP'ye göre belirgin şekilde yavaştır.|
|**`-F`**|**Hızlı Tarama (Fast)**|Standart 1000 port yerine en yaygın kullanılan **100 portu** tarar.|
|**`-p`**|**Belirli Portlar**|Belirtilen portu veya aralığı tarar. _(Örn: `-p 80,443,8080` veya `-p 1-1000`)_|
|**`-p-`**|**Tüm Portlar**|1'den 65535'e kadar tüm portları eksiksiz tarar.|

## 5. Servis, Versiyon ve İşletim Sistemi Tespiti

|**Parametre**|**Özellik**|**Açıklama**|
|---|---|---|
|**`-sV`**|Servis / Sürüm Tespiti|Portta çalışan servisin adını ve tam sürümünü inceler _(Örn: Apache 2.4.49, OpenSSH 8.2)_. Zafiyet taraması için temel adımdır.|
|**`-O`**|İşletim Sistemi Tespiti|TCP/IP yığın parmak izine (fingerprinting) bakarak hedef cihazın işletim sistemini (Linux, Windows, macOS vb.) tahmin eder.|
|**`-A`**|Agresif Tarama|İşletim sistemi tespiti (`-O`), servis sürüm tespiti (`-sV`), temel script taraması (`-sC`) ve yol haritasını (`traceroute`) tek parametrede birleştirir.|

## 6. Zamanlama ve Hata Ayıklama Seçenekleri

- **Zamanlama Şablonları (`-T0` - `-T5`):** Taramayı hızlandırmak ya da IDS/IPS sistemlerini atlatmak için hız profili seçilir.
    
    - `-T0` (Paranoid) / `-T1` (Sneaky): Çok yavaştır, güvenlik sistemlerini atlatmak için kullanılır.
        
    - `-T2` (Polite) / `-T3` (Normal): Varsayılan dengeli moddur.
        
    - `-T4` (Aggressive): Yerel veya hızlı ağlarda önerilen en pratik hız seviyesidir.
        
    - `-T5` (Insane): Aşırı hızlıdır; ağda paket kaybına ve yanlış sonuçlara yol açabilir.
        
- **`-d` (Debugging):** Tarama sırasında arka planda dönen ham soket hatalarını ve paket ayrıntılarını adım adım izlemek için kullanılır (ayrıntıyı artırmak için `-d2`, `-d3` yazılabilir).
    

## 7. Örnek Kullanım Kalıpları

### Pratik ve Hızlı Servis Taraması:

Bash

```
nmap -sS -sV -T4 -p 21,22,80,443,8080 192.168.0.50
```

### Güvenlik Duvarı Arkasındaki Hedefe Detaylı Keşif:

Bash

```
nmap -Pn -sV -sC -O -T4 192.168.0.50
```

### Ağdaki Tüm Cihazların Sadece Açık Olup Olmadığını Görme:

Bash

```
nmap -sn 192.168.0.0/24
```