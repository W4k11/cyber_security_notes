# Linux 103: Editörler, Dosya Transferi, Süreç Yönetimi ve Görev Planlama

## 1. Metin Düzenleyicileri (Text Editors)

- **`echo` ve Yönlendirme:** Terminalden doğrudan dosya oluşturmak veya ekleme yapmak için kullanılır:
    
    - `echo "metin" > dosya.txt` (Mevcut içeriği siler, üzerine yazar)
        
    - `echo "metin" >> dosya.txt` (Mevcut içeriği korur, son satıra ekler)
        
- **`nano`:** Yeni başlayanlar için uygun, klavye kısayolları (`Ctrl + O` kaydet, `Ctrl + X` çık) alt kısımda listelenen basit terminal editörüdür:
    
    Bash
    
    ```
    nano dosya.txt
    ```
    
- **`vim`:** Mod tabanlı (Normal mod, Insert modu `:w`, `:q!` komutları) gelişmiş ve kaynak tüketimi düşük terminal editörüdür:
    
    Bash
    
    ```
    vim dosya.txt
    ```
    

## 2. Ağ Üzerinden Dosya Transferi

### `wget` ile Web Üzerinden İndirme

HTTP, HTTPS ve FTP protokolleri üzerinden uzak sunuculardan dosya çekmek için kullanılır:

Bash

```
wget https://assets.tryhackme.com/additional/linux-fundamentals/part3/myfile.txt
```

### `scp` (Secure Copy Protocol) ile Güvenli Dosya Aktarımı

SSH altyapısını kullanarak yerel makine ile uzak sunucu arasında şifreli dosya aktarımı yapar:

Bash

```
# Yereldeki bir dosyayı uzak sunucuya gönderme:
scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt

# Uzaktaki bir dosyayı yerel makineye çekme:
scp ubuntu@192.168.1.30:/home/ubuntu/remote_file.txt ./
```

### Hızlı Geçici Web Sunucusu Başlatma (`python3`)

Bulunulan dizindeki dosyaları ağa açmak için tek satırlık pratik HTTP servisi oluşturur (varsayılan port `8000`):

Bash

```
python3 -m http.server 8000
```

## 3. Süreç (Process) Yönetimi

İşlemler, Linux çekirdeği (Kernel) tarafından yönetilen ve bellekte koşan programlardır. Her işleme benzersiz bir Süreç Kimliği (**PID** - _Process ID_) atanır.

### Süreçleri Görüntüleme

|**Komut**|**Açıklama**|
|---|---|
|`ps`|O anki aktif terminal oturumuna bağlı süreçleri gösterir.|
|`ps aux`|Sistem genelinde çalışan tüm süreçleri kullanıcı ve kaynak tüketimleriyle listeler.|
|`top`|CPU ve RAM kullanımını gerçek zamanlı (canlı) olarak izletir (`q` ile çıkılır).|

### Süreçleri Sonlandırma (`kill` ve Sinyaller)

Süreçleri durdurmak için çekirdeğe sinyaller (Signals) gönderilir:

Bash

```
# Sözdizimi: kill -<SİNYAL> <PID>
kill -9 1304
```

- **`SIGTERM` (Sinyal 15):** Standart sonlandırma isteğidir. Programın verileri kaydetmesine ve geçici dosyaları temizlemesine izin verir.
    
- **`SIGKILL` (Sinyal 9):** Süreci çekirdek seviyesinde anında ve zorla öldürür; temizleme işlemine izin vermez.
    
- **`SIGSTOP` (Sinyal 19):** Süreci dondurur/askıya alır (daha sonra `SIGCONT` ile devam ettirilebilir).
    

### Arka Plan ve Ön Plan Yönetimi

- **`&` (Arka planda başlatma):** Komutun sonuna eklendiğinde terminali kilitlemeden arka planda çalıştırır:
    
    Bash
    
    ```
    sleep 100 &
    # [1] 3642 -> [İş Numarası] PID Numarası
    ```
    
- **`jobs`:** Arka planda çalışan aktif işleri listeler.
    
- **`fg` (Foreground):** Arka plandaki bir işlemi tekrar terminalin önüne (ön plana) getirir.
    
- **`Ctrl + Z`:** Ön planda çalışan bir işlemi geçici olarak dondurup arka plana atar.
    
- **`bg`:** Askıya alınmış işlemi arka planda yürütmeye devam ettirir.
    

## 4. Servis Yönetimi (`systemd` / `systemctl`)

İşletim sistemi servislerini (daemon) başlatmak, durdurmak ve sistem açılışına eklemek için `systemctl` kullanılır:

Bash

```
# Servisi anlık olarak başlatma
systemctl start apache2

# Servisi anlık olarak durdurma
systemctl stop apache2

# Servisin durumunu kontrol etme
systemctl status apache2

# Sistemin her açılışında otomatik başlamasını sağlama
systemctl enable apache2

# Açılışta otomatik başlamasını iptal etme
systemctl disable apache2
```

## 5. Görev Planlama (`cron` ve `crontab`)

Sistemde belirli aralıklarla tekrarlanacak görevleri otomatikleştirmek için `cron` servisi kullanılır. Görev tablosunu açmak için:

Bash

```
crontab -e
```

### Zamanlama Sözdizimi Anatomisi

Plaintext

```
 *     *     *     *     *     <Çalıştırılacak Komut>
 |     |     |     |     |
 |     |     |     |     +---- Haftanın Günü (DOW: 0 - 6, 0 = Pazar)
 |     |     |     +---------- Ay (MON: 1 - 12)
 |     |     +---------------- Ayın Günü (DOM: 1 - 31)
 |     +---------------------- Saat (HOUR: 0 - 23)
 +---------------------------- Dakika (MIN: 0 - 59)
```

### Örnek Planlama

`cmnatic` kullanıcısının `Documents` klasörünü her 12 saatte bir `/var/backups/` dizinine kopyalama:

Plaintext

```
0 */12 * * * cp -R /home/cmnatic/Documents /var/backups/
```