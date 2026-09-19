The Post Office Protocol Version 3:
Clientin mail sunucusuyla iletişim kurup e-maillerin alınmasını sağlar.


- `USER <username>` Kullanıcıyı tanımlar.
- `PASS <password>` Kullanıcı şifresini sağlar.
- `STAT` Mesaj sayısını ve toplam boyutu talep eder.
- `LIST` tüm mesajları ve boyutlarını listeler.
- `RETR <message_number>` Belirtilen mesajı alır
- `DELE <message_number>` silinecek bir mesajı işaretler
- `QUIT` Silme gibi değişiklikleri uygulayarak POP3 oturumunu sonlandırır.

telnet "ip" 110
