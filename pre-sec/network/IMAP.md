Internet Message Access Protocol: Birden fazla cihazdan bir posta kutusuna aynı anda senkronize olan protokoldür.

- `LOGIN <username> <password>` kullanıcıyı doğrular
- `SELECT <mailbox>` Çalışmak istediğin mail kutusunu seçiyorsun
- `FETCH <mail_number> <data_item_name>` Örneğin 3 `fetch 3 body[]` numaralı mesajın başlığını ve mesajını getirmek için kullanılır.
- `MOVE <sequence_set> <mailbox>` belirtilen mesajları başka bir posta kutusuna taşır.
- `COPY <sequence_set> <data_item_name>` başka bir mail kutusuna kopyalar
- `LOGOUT` çıkış yapar.

telnet "ip adresi" 143