Commands:

echo: yanına ne yazdıysan onun çıktısını verir örneğin:
```
tryhackme@linux1:~$ echo Hello
Hello
tryhackme@linux1:~$ echo "Hello Friend!"
Hello Friend!
```

whoami: Oturumda hangi kullanıcının oturum açtığını gösterir.

```
tryhackme@linux1:~$ whoami
root
```

#### Intracting Filesystem
ls -> listeleme
cd -> dizin değiştirme
cat -> dosya okuma
pwd -> çalıştığın dizini ve alt dizinleri gösterme

```
┌──(k4d1r㉿DESKTOP-AGAM3GQ)-[~]
└─$ ls
lodur.txt

┌──(k4d1r㉿DESKTOP-AGAM3GQ)-[~]
└─$ cd ..

┌──(k4d1r㉿DESKTOP-AGAM3GQ)-[/home]
└─$ cd k4d1r/

┌──(k4d1r㉿DESKTOP-AGAM3GQ)-[~]
└─$ cat lodur.txt
lodurlo

┌──(k4d1r㉿DESKTOP-AGAM3GQ)-[~]
└─$ pwd
/home/k4d1r

```

### Searching for Files

find: dosyayı hangi dizinde olduğunu gösterme. find -name "*.txt"

grep: bir dosyanın içerisindeki içeriği aramak için kullanılır.
```
┌──(k4d1r㉿DESKTOP-AGAM3GQ)-[~]
└─$ grep "lo" lodur.txt
lodurlo
```

grep ile recursive arama

tek tek dosyalara bakmak yerine belirlenen dizinde arama yapmak.
```
┌──(k4d1r㉿DESKTOP-AGAM3GQ)-[~]
└─$ grep -R "lodur" /home/k4d1r
/home/k4d1r/lodur.txt:lodurlo
```

### Shell Operators

"&" : Bu operatör, terminalinizin arka planında komutlar çalıştırmanıza olanak tanır. Büyük dosyalarda iş yapar bence...

"&&" : Bu operatör, terminalinizde birden fazla komutu tek bir satırda birleştirmenizi sağlar. kodların hepsi başarılı olursa çalışır.

">" : Bu operatör bir yönlendiricidir; yani bir komutun çıktısını (örneğin cat kullanarak bir dosyaya çıktı almak gibi) alıp başka bir yere yönlendirebiliriz.

```
┌──(k4d1r㉿DESKTOP-AGAM3GQ)-[~]
└─$ echo hey > welcome

┌──(k4d1r㉿DESKTOP-AGAM3GQ)-[~]
└─$ cat welcome
hey

┌──(k4d1r㉿DESKTOP-AGAM3GQ)-[~]
└─$ ls
lodur.txt  welcome
```

">>" : Bu operatör, > operatörünün yaptığı aynı işlevi görür ancak çıktıyı değiştirmek yerine ekler (yani hiçbir şey üzerine yazılmaz).

```
┌──(k4d1r㉿DESKTOP-AGAM3GQ)-[~]
└─$ echo lodur >> welcome

┌──(k4d1r㉿DESKTOP-AGAM3GQ)-[~]
└─$ cat welcome
hey
lo
lodur
```