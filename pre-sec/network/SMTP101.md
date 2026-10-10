# SMTP (Simple Mail Transfer Protocol) Temelleri

## 1. SMTP Nedir?

SMTP; bir e-posta istemcisi (client) ile posta sunucusu arasında veya **iki posta sunucusunun kendi arasında e-posta göndermek/aktarmak** için kullanılan standart uygulama katmanı protokolüdür.

- **Temel Görevi:** E-postaları **göndermek (push)** için çalışır. (Gelen kutusundaki postaları **okumak/çekmek** için ise **POP3** veya **IMAP** kullanılır.)
    
- **Port Standartları:**
    
    - **Port 25:** Sunucudan sunucuya aktarım (şifresiz/geleneksel, genellikle ISS'ler tarafından spam engellemek için ev kullanıcılarına kapatılır).
        
    - **Port 587:** İstemcilerin sunucuya mail teslim etmesi için modern güvenli standart (STARTTLS destekli).
        
    - **Port 465:** SSL/TLS üzerinden doğrudan şifreli eski SMTP portu (SMTPS).
        

## 2. Temel SMTP Oturum Komutları

Bir SMTP istemcisi sunucuyla iletişim kurarken şu anahtar komutları sırayla yürütür:

|**Komut**|**Sözdizimi / Kullanım**|**Açıklama**|
|---|---|---|
|**`HELO` / `EHLO`**|`HELO client.domain`|İstemcinin kendini sunucuya tanıtarak oturumu başlattığı selamlama komutudur (`EHLO`, gelişmiş özellikleri destekler).|
|**`MAIL FROM`**|`MAIL FROM:<gonderen@mail.com>`|E-postayı gönderen kişinin adresini belirtir.|
|**`RCPT TO`**|`RCPT TO:<alici@mail.com>`|E-postanın ulaşacağı alıcı adresini belirtir. Birden fazla alıcı için art arda çağrılabilir.|
|**`DATA`**|`DATA`|Sunucuya _"Konu, başlıklar ve mesaj metnini girmeye başlıyorum"_ bilgisini verir.|
|**`.` (Tek Nokta)**|`.` _(tek başına boş bir satırda)_|Mesaj gövdesinin bittiğini sunucuya bildirir; sunucu bu işareti görünce postayı kuyruğa alır.|
|**`QUIT`**|`QUIT`|Oturumu sonlandırır.|

## 3. Terminal Üzerinden Manuel E-Posta Gönderimi (Adım Adım)

Linux terminalinden `telnet` veya `nc` ile bir SMTP sunucusuna bağlanıp doğrudan e-posta gönderme akışı:

### Adım 1: Bağlantı Açma

Bash

```
telnet 192.168.1.50 25
```

_Sunucu karşılama kodu döner: `220 mail.sirket.local ESMTP Postfix`_

### Adım 2: Oturumu Başlatma (`EHLO`)

Plaintext

```
EHLO kali.local
250-mail.sirket.local
250 OK
```

### Adım 3: Gönderen ve Alıcıyı Tanımlama

Plaintext

```
MAIL FROM:<saldirgan@hedef.com>
250 2.1.0 Ok

RCPT TO:<kurban@hedef.com>
250 2.1.5 Ok
```

### Adım 4: Mesaj İçeriğini Girme (`DATA` ve `.`)

Plaintext

```
DATA
354 End data with <CR><LF>.<CR><LF>
Subject: Acil Sistem Bildirimi
From: Destek Ekibi <destek@hedef.com>

Merhaba, sistem bakımı nedeniyle şifrenizi yenilemeniz gerekmektedir.
.
250 2.0.0 Ok: queued as 4Tz9L
```

_(Tek başına yazılan `.` işaretinden sonra sunucu `250 OK` mesajıyla mektubun kuyruğa alındığını teyit eder.)_

### Adım 5: Oturumu Kapatma

Plaintext

```
QUIT
221 2.0.0 Bye
```

## 4. Güvenlik Notu: E-Posta Sahteciliği (Email Spoofing)

Geleneksel SMTP protokolü kimlik doğrulama zorunluluğuna sahip olmadan tasarlandığı için, saldırganlar `MAIL FROM` alanına istedikleri herhangi bir sahte adresi yazarak **Email Spoofing (Sahte E-Posta)** gönderebilirler.

Modern sistemlerde bu sahteciliği engellemek için DNS kayıtları üzerinden 3 güvenlik kalkanı kullanılır:

- **SPF (Sender Policy Framework):** Domain adına hangi IP adreslerinin mail atabileceğini TXT kaydında listeler.
    
- **DKIM (DomainKeys Identified Mail):** E-postanın yolda değiştirilmediğini kanıtlayan kriptografik bir imza ekler.
    
- **DMARC:** SPF ve DKIM doğrulamalarından geçemeyen maillerin çöpe atılmasını veya karantinaya alınmasını sağlar.