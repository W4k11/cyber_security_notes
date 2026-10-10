# MAP (Internet Message Access Protocol) Temelleri

## 1. IMAP Nedir?

IMAP; e-postaları yerel bilgisayara indirip sunucudan silmek yerine, **doğrudan posta sunucusu üzerinde tutan ve birden fazla cihaz arasında eşzamanlayan (senkronize eden)** e-posta erişim protokolüdür.

- **Port Bilgisi:** Düz metin bağlantılar için **Port 143**, SSL/TLS ile şifrelenmiş güvenli sürümü (IMAPS) için **Port 993** kullanılır.
    
- **POP3 ile Temel Farkı:**
    
    - **POP3:** Postaları cihaza indirir ve genellikle sunucudan siler (tek cihaz için uygundur).
        
    - **IMAP:** Postalar sunucuda kalır; telefon, tablet ve bilgisayardan açtığınızda okundu durumu, klasörler ve silinen mailler tüm cihazlarda senkronize kalır.
        

## 2. Temel IMAP Komutları

IMAP oturumu sırasında istemci sunucuyla etkileşime geçerken şu anahtar komutları kullanır:

|**Komut**|**Sözdizimi**|**Ne İşe Yarar?**|
|---|---|---|
|**`LOGIN`**|`LOGIN <kullanıcı_adı> <şifre>`|Kimlik doğrulama yapar ve posta hesabını açar.|
|**`SELECT`**|`SELECT <posta_kutusu>`|İşlem yapılacak klasörü seçer (Örn: `SELECT INBOX`).|
|**`FETCH`**|`FETCH <mesaj_no> <veri_alanı>`|Seçili kutudaki belirli bir mesajın başlığını veya gövdesini getirir.|
|**`MOVE`**|`MOVE <mesaj_no> <hedef_kutu>`|Belirtilen e-postayı başka bir klasöre taşır.|
|**`COPY`**|`COPY <mesaj_no> <hedef_kutu>`|E-postayı başka bir klasöre kopyalar.|
|**`LOGOUT`**|`LOGOUT`|Sunucu ile olan oturumu güvenli şekilde sonlandırır.|

## 3. Terminal (Telnet / Netcat) Üzerinden IMAP Oturumu

Manuel olarak IMAP sunucusuna bağlanıp bir mesajı okuma adımları şöyledir:

### Adım 1: Sunucuya Bağlanma

Varsayılan 143 numaralı porta `telnet` veya `nc` ile bağlanılır:

Bash

```
telnet 192.168.1.50 143
```

_Bağlantı kurulduğunda sunucu hazır olduğunu belirten bir karşılama mesajı döner (`* OK IMAP4rev1 Service Ready`)._

### Adım 2: Oturum Açma (`LOGIN`)

IMAP komutlarının başına genellikle istemci tarafından bir etiket (örneğin `A01`, `A02`) eklenir:

Plaintext

```
A01 LOGIN ahmet parola123
```

_Yanıt: `A01 OK LOGIN completed`_

### Adım 3: Gelen Kutusunu Seçme (`SELECT`)

İşlem yapmak için ana gelen kutusu seçilir:

Plaintext

```
A02 SELECT INBOX
```

_Sunucu bu klasördeki toplam mesaj sayısını, okunmamış bildirimlerini döner._

### Adım 4: E-Posta İçeriğini Getirme (`FETCH`)

Örneğin gelen kutusundaki 3 numaralı e-postanın hem başlıklarını hem de metin gövdesini görüntülemek için:

Plaintext

```
A03 FETCH 3 BODY[]
```

### Adım 5: Klasörler Arası Taşıma veya Çıkış

Mesajı çöp kutusuna taşımak ve ardından oturumu kapatmak için:

Plaintext

```
A04 MOVE 3 Trash
A05 LOGOUT
```

## 4. Güvenlik Notu: Port 143 vs Port 993

- **Port 143 (Şifresiz):** `LOGIN` komutu sırasında kullanıcı adı ve şifre ağ üzerinden **açık metin (cleartext)** geçer. Ortadaki Adam (MitM) veya paket dinleme (sniffing) saldırılarına karşı tamamen savunmasızdır.
    
- **Port 993 (IMAPS):** TLS/SSL katmanı üzerinden şifrelenir; modern sistemlerde güvenlik standardı olarak bu port tercih edilmelidir.