# POP3 (Post Office Protocol Version 3) Temelleri

## 1. POP3 Nedir?

POP3 (Post Office Protocol 3), istemcinin (Thunderbird, Outlook vb.) uzak bir posta sunucusuna bağlanarak gelen kutusundaki e-postaları yerel bilgisayara indirmesini sağlayan uygulama katmanı protokolüdür.

- **Port Bilgisi:** Düz metin (şifresiz) iletişim için **Port 110**, TLS/SSL ile şifrelenmiş güvenli sürümü (POP3S) için **Port 995** kullanılır.
    
- **Çalışma Mantığı ("İndir ve Sil"):** E-postaları sunucudan bilgisayara indirir ve istemci aksini belirtmedikçe sunucudaki kopyasını siler. Bu nedenle tek bir cihazdan çalışan kullanıcılar için uygundur; fakat cihazlar arası senkronizasyon sunmadığı için günümüzde yerini çoğunlukla **IMAP** protokolüne bırakmıştır.
    

## 2. Temel POP3 Protokol Komutları

POP3 oturumu yetkilendirme (Authorization) ve işlem (Transaction) olmak üzere iki ana evreden oluşur. Bu süreçte kullanılan temel komutlar şunlardır:

|**Komut**|**Sözdizimi**|**Ne İşe Yarar?**|
|---|---|---|
|**`USER`**|`USER <kullanıcı_adı>`|Giriş yapılacak posta kutusu sahibini tanımlar.|
|**`PASS`**|`PASS <şifre>`|Kullanıcı parolasını sunucuya iletir.|
|**`STAT`**|`STAT`|Posta kutusundaki toplam mesaj sayısını ve toplam boyutu (oktet/bayt) döner.|
|**`LIST`**|`LIST`|Tüm mesajları sıra numaraları ve boyutlarıyla tek tek listeler.|
|**`RETR`**|`RETR <mesaj_no>`|Belirtilen sıra numaralı e-postanın tüm içeriğini (başlıklar ve gövde) istemciye getirir.|
|**`DELE`**|`DELE <mesaj_no>`|Belirtilen mesajı silinmek üzere işaretler (hemen silinmez).|
|**`QUIT`**|`QUIT`|Oturumu kapatır; işaretlenmiş tüm silme işlemlerini (`DELE`) sunucuda kalıcı hale getirir.|

## 3. Terminal (Telnet / Netcat) Üzerinden POP3 Oturumu

Manuel olarak bir POP3 sunucusuna bağlanıp e-posta çekme ve silme adımları şu şekildedir:

### Adım 1: Sunucuya Bağlanma

Varsayılan 110 numaralı porta `telnet` ile bağlanılır:

Bash

```
telnet 192.168.1.50 110
```

_Sunucu bağlantıyı kabul ettiğinde `+OK POP3 server ready` şeklinde bir karşılama yanıtı verir._

### Adım 2: Kimlik Doğrulama (`USER` & `PASS`)

Plaintext

```
USER ahmet
+OK User accepted
PASS gizli123
+OK Mailbox open, 2 messages
```

### Adım 3: Posta Kutusunun Durumunu Kontrol Etme (`STAT` & `LIST`)

Kutuda kaç mail olduğunu ve boyutlarını görmek için:

Plaintext

```
STAT
+OK 2 3200

LIST
+OK 2 messages (3200 octets)
1 1200
2 2000
.
```

_(1 numaralı mesaj 1200 bayt, 2 numaralı mesaj 2000 bayt yer kaplamaktadır.)_

### Adım 4: Mesajı Okuma (`RETR`)

1 numaralı mesajın başlık ve metin içeriğini getirmek için:

Plaintext

```
RETR 1
+OK 1200 octets
From: guvenlik@sirket.local
Subject: Acil Parola Sıfırlama
Merhaba Ahmet, parolanızı sıfırlamanız gerekmektedir...
.
```

### Adım 5: Mesajı Silme ve Oturumu Sonlandırma (`DELE` & `QUIT`)

Okunan 1 numaralı mesajı silmek ve sunucu üzerindeki değişiklikleri kaydedip çıkmak için:

Plaintext

```
DELE 1
+OK Message 1 marked for deletion
QUIT
+OK Logging off, message deleted
```

## 4. IMAP vs. POP3 Karşılaştırması

|**Özellik**|**POP3 (Port 110 / 995)**|**IMAP (Port 143 / 993)**|
|---|---|---|
|**Depolama Alanı**|E-postalar yerel cihaza indirilir; sunucudan temizlenir|E-postalar sunucuda tutulur|
|**Çoklu Cihaz Senkronizasyonu**|Yoktur (telefonda indirilen mail bilgisayarda görünmeyebilir)|Tam senkronize (okundu, taslak, klasör durumu her yerde aynı)|
|**Bant Genişliği Kullanımı**|Tüm e-posta gövdesi ve ekler doğrudan indirilir|İstemci yalnızca seçtiği veya ihtiyaç duyduğu kısımları çeker|