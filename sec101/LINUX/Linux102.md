## SSH

**SSH, Secure Shell** açılımının kısaltması olarak kullanılan bir uzak sunucu bağlantı protokolüdür. Temelde aynı işlevselliğe sahip olsalar da **SSH**, **TELNET**’ten farklı olarak **şifreli bağlantı** gerçekleştirme olanağı sunmaktadır. SSH yani Güvenli Kabuk” üzerinden bağlantı gerçekleştirmek istediğinizde kullanıcı adı ve şifreler açık metin olarak değil şifrelenmiş olarak iletilmektedir.

ÖRN:

  PC------------> INTERNET -------------------> LINUX SERVER
  Salam               asnfksjnfksanjf                         Salam
  
## SSH ile Bağlanma

Hedef makinada OpenSSH gibi yazılımların yüklü olması lazım ve statik ya da local IP yi bilmemiz lazım.
örn:
```
root@ip-10-112-120-117:~# ssh tryhackme@10.112.174.6
```

## Linux Yardım Komutları

ls parametrelerini görmek için : "ls --help" ya da ls -h

kılavuz olarak görmek için : "man ls"

# Erişim 101

ls komutu listeleme yapar bunu biliyoruz peki ls -l parametresiyle nasıl bir listeleme yapar?
```shell-session
tryhackme@linux2:~$ ls -lh
-rw-r--r-- 1 cmnatic cmnatic 0 Feb 19 10:37 file1
-rw-r--r-- 8 cmnatic cmnatic 0 Feb 19 10:37 file2
```

-rwx nedir?
r-- sadece okuma
-w- sadece yazma
--x sadece çalıştırma yetkileridir.
### Önemli: 
Başlarındaki d ya da - şu anlama gelir:
d: dizin demek
-: dosya anlamına gelir.

peki bunların 3 tanesi neden art arda yazılır?

Birinci rwx: Owner
İkinci rwx: grup
Üçüncü rwx: diğerleri

peki bu yetkiler nasıl değişir?
r-- : 4 , -w- : 2 , --x : 1 

chmod 750 file1
yüzler basamağındaki owner
onlar basamağındaki grup
birler basamağındaki diğerleri

# Linux Dizin Yapısı

/etc : Sistem Dosyalarını depolar.

/var : Çalışan servisler ve uygulamalar için sık sık kullanılan veya erişilen bir dizindir.

/tmp : Bu dizin geçici dosyaları saklar temporaryden gelir windowsdaki temp den farkı yok.

/root : root home directory

