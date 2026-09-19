## Linux metin düzenleyicileri

echo ve pipe operatörleri: echo > , echo >>

nano ve vim

nano dosya.txt

vim dosya.txt

## Linuxta Kullanışlı Araçlar

wget: http üzerinden web den dosya indirmeyi sağlar.
ÖRN:
`wget https://assets.tryhackme.com/additional/linux-fundamentals/part3/myfile.txt`

hosta dosya paylaşma 
SCP

`scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt`

WEB Sunucusunda Dosya Sunma

python3 -m http.server

## İşlemler 101

İşlemler, makinenizde çalışan programlardır. Çekirdek tarafından yönetilirler ve her işlemin kendisine atanmış bir kimliği (ID) vardır; bu kimlik aynı zamanda işlem adı olarak da bilinir. Bu kimlik, işlemin başlama sırasını artırır. Örneğin, 60. işlemin kimlik numarası 60 olacaktır.

işlemleri görmek için: ps
sistem işlemlerini görmek için: ps aux
anlık işlemleri görmek için: top
işlemleri sonlandırmak için: kill (PID)

### Bazı işlemi sonlandırma komutları:
SIGTERM : İşlemi sonlandırın, ancak öncesinde bazı temizleme işlemlerini yapmasına izin verin.
SIGKILL : İşlemi sonlandır - sonrasında herhangi bir temizleme işlemi yapmaz.
SIGSTOP : Bir işlemi durdur/askıya al

## Sistem Açılışında İşlemlerin/Hizmetlerin Başlatılmasını Sağlama

`systemctl [option] [service]`

Örn: `systemctl start apache2`

options:
start
stop
enable
disable
status

# Linuxta planlama yapma

Crontab, önyükleme sırasında başlatılan ve cron görevlerini kolaylaştırmak ve yönetmekten sorumlu olan süreçlerden biridir.

MIN -> Hangi dakikada çalıştırılcak
HOUR -> Hangi saatte çalıştırılcak
DOM -> Ayın hangi gününde çalıştırıcak
MON -> Hangi ayda çalıştırıcak
DOW -> Haftanın hangi gününde çalıştırılıcak
CMD -> Yürütülcek komut

Dosya yedekleme örneğini ele alalım. "cmnatic"in "Belgeler" klasörünü her 12 saatte bir yedeklemek isteyebilirsiniz. Bunun için aşağıdaki formatı kullanırız:
```
crontab -e
`0 */12 * * * cp -R /home/cmnatic/Documents /var/backups/`
```

### Arka Planda İşlem Başlatma
```
tryhackme@linux3:~$ echo salam &
[1] 3642
tryhackme@linux3:~$ salam
```
```
root@ip-10-114-119-206:~# ps
    PID TTY          TIME CMD
  18950 pts/1    00:00:00 bash
  19069 pts/1    00:00:00 ps
```

arka plandaki işlemi durdurmak için: fg
