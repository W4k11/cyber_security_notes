# Linux 101: Temel Komutlar ve Shell Operatörleri

## 1. Temel Sistem Komutları

- **`echo`**: Belirtilen metni standart çıktıya (terminale) basar.
    
    Bash
    
    ```
    tryhackme@linux1:~$ echo Hello
    Hello
    tryhackme@linux1:~$ echo "Hello Friend!"
    Hello Friend!
    ```
    
- **`whoami`**: Mevcut kabuk oturumunu çalıştıran aktif kullanıcının adını döner.
    
    Bash
    
    ```
    tryhackme@linux1:~$ whoami
    root
    ```
    

## 2. Dosya Sistemiyle Etkileşim (Interacting with Filesystem)

|Komut|Açıklama|
|---|---|
|`pwd`|_Print Working Directory_ — Bulunulan mevcut mutlak dizin yolunu gösterir.|
|`ls`|Dizin içeriğini (dosya ve alt klasörleri) listeler.|
|`cd`|_Change Directory_ — Çalışma dizinini değiştirir.|
|`cat`|_Concatenate_ — Dosya içeriğini terminale yazdırır veya dosyaları birleştirir.|

### Örnek Terminal Akışı

Bash

```
# 1. Mevcut dizindeki dosyaları listele
k4d1r@DESKTOP-AGAM3GQ:~$ ls
lodur.txt

# 2. Bir üst dizine çık
k4d1r@DESKTOP-AGAM3GQ:~$ cd ..

# 3. Belirtilen kullanıcı ev dizinine dön
k4d1r@DESKTOP-AGAM3GQ:/home$ cd k4d1r/

# 4. Dosya içeriğini oku
k4d1r@DESKTOP-AGAM3GQ:~$ cat lodur.txt
lodurlo

# 5. Aktif dizin yolunu doğrula
k4d1r@DESKTOP-AGAM3GQ:~$ pwd
/home/k4d1r
```

## 3. Dosya ve İçerik Arama (Searching for Files)

### `find` (Dosya Arama)

Belirtilen kriterlere göre dosya sisteminde dosya ve dizin arar.

Bash

```
# Bulunulan dizin ve alt dizinlerde uzantısı .txt olanları bul
find -name "*.txt"

# Belirli bir dizinde arama yapmak için:
find /home/k4d1r -name "*.txt"
```

### `grep` (İçerik Arama)

Dosya veya veri akışları içerisinde metin kalıplarını filtreler.

Bash

```
# Tek bir dosya içinde arama yapma
k4d1r@DESKTOP-AGAM3GQ:~$ grep "lo" lodur.txt
lodurlo

# Belirtilen dizinde özyinelemeli (recursive) arama yapma (-R)
k4d1r@DESKTOP-AGAM3GQ:~$ grep -R "lodur" /home/k4d1r
/home/k4d1r/lodur.txt:lodurlo
```

## 4. Shell Operatörleri ve Çıktı Yönlendirme

|Operatör|İşlev|Davranış Mantığı|
|---|---|---|
|`&`|Arka Plan (_Background_)|Komutu arka planda çalıştırır; terminali hemen bir sonraki girdi için boşa çıkarır.|
|`&&`|Mantıksal VE (_AND_)|Solundaki komut başarıyla (`exit code 0`) tamamlanırsa sağındaki komutu çalıştırır.|
|`>`|Yönlendirme (_Overwrite_)|Standart çıktıyı hedef dosyaya yazar; hedef dosya varsa **üzerine yazar** (_truncate_).|
|`>>`|Ekleme (_Append_)|Standart çıktıyı hedef dosyanın sonuna ekler; mevcut veriyi silmez.|

### Yönlendirme Örnekleri

Bash

```
# 1. 'welcome' dosyasını oluştur ve içeriğe yaz (önceki veri silinir)
k4d1r@DESKTOP-AGAM3GQ:~$ echo hey > welcome
k4d1r@DESKTOP-AGAM3GQ:~$ cat welcome
hey

# 2. 'welcome' dosyasına yeni satır ekle (üzerine yazmaz)
k4d1r@DESKTOP-AGAM3GQ:~$ echo lodur >> welcome
k4d1r@DESKTOP-AGAM3GQ:~$ cat welcome
hey
lodur
```