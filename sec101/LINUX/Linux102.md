# Linux 102: SSH, İzin Yapısı ve Dizin Hiyerarşisi

## 1. SSH (Secure Shell) Protokolü

**SSH (Secure Shell)**, uzak sunuculara komut satırı üzerinden güvenli erişim sağlayan bir ağ protokolüdür (varsayılan port: `22`).

- **TELNET vs. SSH:** TELNET trafiği düz metin (_cleartext_) iletirken, SSH uçtan uca şifreleme (**kriptografik tünelleme**) kullanır. Kullanıcı adı, parola ve oturum komutları ağ üzerinde dinlense dahi okunamaz.
    

Plaintext

```
[Yerel PC] -------------> [ Güvensiz Ağ / İnternet ] -------------> [ Linux Sunucu ]
  "Parola"   -- (Şifreleme) -->   "a8f7c9e1... (Ciphertext)"   -- (Deşifre) -->   "Parola"
```

### SSH ile Bağlantı Kurma

Hedef sistemde `openssh-server` servisinin çalışır durumda olması ve erişilebilir bir IP adresi gerekir:

Bash

```
# Sözdizimi: ssh <kullanıcı_adı>@<hedef_ip>
ssh tryhackme@10.112.174.6

# Standart dışı port kullanımı durumunda (-p):
ssh tryhackme@10.112.174.6 -p 2222
```

## 2. Linux Yardım ve Dokümantasyon Komutları

- **`--help` parametresi:** Komutun desteklediği parametreleri ve temel kullanım özetini ekrana basar:
    
    Bash
    
    ```
    ls --help
    ```
    
- **`man` (Manual Pages):** Komutun kapsamlı resmi el kitabını sayfalar halinde açar (`q` ile çıkılır):
    
    Bash
    
    ```
    man ls
    ```
    

## 3. Dosya İzinleri ve Erişim Denetimi (Permissions)

`ls -lh` komutu dosya ve dizinleri ayrıntılı izin bilgileriyle birlikte listeler:

Bash

```
tryhackme@linux2:~$ ls -lh
-rw-r--r-- 1 cmnatic cmnatic 0 Feb 19 10:37 file1
drwxr-xr-x 2 cmnatic cmnatic 4.0K Feb 19 10:37 scripts/
```

### İzin Diziliminin Anatomisi

İlk 10 karakterlik blok (`drwxr-xr-x`) dört ana parçadan oluşur:

Plaintext

```
 [d]  [rwx]  [r-x]  [r-x]
  |     |      |      |
  |     |      |      +---> 4. Diğerleri (Others): Sistemdeki diğer tüm kullanıcılar
  |     |      +----------> 3. Grup (Group): Dosyanın ait olduğu grup üyeleri
  |     +-----------------> 2. Sahip (Owner/User): Dosyayı oluşturan kullanıcı
  +-----------------------> 1. Dosya Türü: (-) Normal dosya | (d) Dizin / Klasör
```

### Temel İzin Tipleri ve Sayısal (Sekizlik / Octal) Değerleri

|**İzin**|**Sembol**|**Sayısal Değer**|**Anlamı (Dosya İçin)**|**Anlamı (Dizin İçin)**|
|---|---|---|---|---|
|**Read**|`r`|**4**|Dosya içeriğini okuma|Dizin içeriğini listeleme (`ls`)|
|**Write**|`w`|**2**|Dosyayı düzenleme/kaydetme|Dosya/dizin oluşturma veya silme|
|**Execute**|`x`|**1**|Program/script olarak çalıştırma|Dizinin içine girme (`cd`)|

### İzinleri Değiştirme (`chmod`)

Sayısal modda izinler basamak değerlerinin toplamı ile ifade edilir:

- `7` = 4 + 2 + 1 (`rwx` - Tam yetki)
    
- `6` = 4 + 2 (`rw-` - Okuma ve yazma)
    
- `5` = 4 + 1 (`r-x` - Okuma ve çalıştırma)
    
- `4` = 4 (`r--` - Yalnızca okuma)
    
- `0` = Hiçbir yetki yok (`---`)
    

Bash

```
# Örnek: Sahip (7), Grup (5), Diğerleri (0)
chmod 750 file1
# Sonuç: -rwxr-x---
```

## 4. Kritik Linux Dizin Hiyerarşisi (FHS)

| **Dizin** | **Rolü ve Açıklaması**                                                                                                              |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `/etc`    | Sistem genelindeki yapılandırma (_configuration_) dosyalarını barındırır (örn. `/etc/passwd`, `/etc/shadow`, ağ ayarları).          |
| `/var`    | Değişken verileri (_variable data_) tutar; servis logları (`/var/log`), web sunucu kök dizinleri (`/var/www/html`) burada yer alır. |
| `/tmp`    | Geçici (_temporary_) dosyalar içindir; genellikle yeniden başlatmada temizlenir ve tüm kullanıcıların yazma yetkisi vardır.         |
| `/root`   | Sistem yöneticisi `root` hesabının özel ev dizinidir (standart `/home` altında yer almaz).                                          |