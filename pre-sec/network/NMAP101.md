**Nmap** (Network Mapper), bilgisayar ağlarını keşfetmek ve ağ güvenlik denetimleri yapmak için kullanılan açık kaynaklı, ücretsiz bir ağ tarama aracıdır.

IP aralığını taramak için: [192.168.0.1-10] 192.168.0.1 ile 10 aralığındaki IP leri tarar

IP alt ağı taramak için: [192.168.0.1/24] bu da buna eş değer: [192.168.0.1-255].

Diyelim ki bir ağdaki çevrimiçi sunucuları keşfetmek istiyorsunuz.
```bash
nmap -sn 
```

IP listesini çıkarmak için. 
```bash
nmap -sL
```

| Option      | Explanation                                                 |
| ----------- | ----------------------------------------------------------- |
| `-sT`       | TCP bağlantı taraması – üç yönlü el sıkışmanın tamamlanması |
| `-sS`       | TCP SYN – üç aşamalı el sıkışmanın yalnızca ilk adımı       |
| `-sU`       | UDP taraması                                                |
| `-F`        | Hızlı mod – en sık kullanılan 100 bağlantı noktasını tarar. |
| `-p[range]` | Bir port numarası aralığı belirtir – -p- tüm portları tarar |

| Option | Explanation                                               |
| ------ | --------------------------------------------------------- |
| `-O`   | İşletim sistemi tespiti                                   |
| `-sV`  | Servis ve sürüm tespiti                                   |
| `-A`   | İşletim sistemi tespiti, sürüm tespiti ve diğer eklemeler |
| `-Pn`  | Çalışmayan sunucuları tarayın                             |

hız için 
```bash
-T0 ile -T4
```
aralığında parametre eklememiz gerekiyor en hızlısı için -T4

hata ayıklama için

 ```bash
 -d
 ```
 parametresi eklememiz gerekiyor.