# Yaygın Ağ Portları ve Temel Servisler Kılavuzu

## 1. Hızlı Başvuru Tablosu

Ağ taramalarında, CTF yarışmalarında ve sistem yönetiminde en sık karşılaşılan temel protokoller ve standart portları:

|**Port**|**Protokol**|**Tam Adı**|**Temel Görevi**|**Güvenlik Durumu**|
|---|---|---|---|---|
|**21**|**FTP**|File Transfer Protocol|Sunucu ve istemci arasında dosya alıp gönderme|Güvensiz (Şifresiz / Cleartext)|
|**22**|**SSH**|Secure Shell|Uzak sunucuya şifreli komut satırı (CLI) erişimi|Güvenli (Uçtan Uca Şifreli)|
|**80**|**HTTP**|HyperText Transfer Protocol|Web sitelerinin düz metin içeriğini tarayıcıya iletme|Güvensiz (Şifresiz)|
|**443**|**HTTPS**|HTTP Secure (over TLS/SSL)|Web trafiğini şifreleyerek iletme|Güvenli (Şifreli)|
|**445**|**SMB**|Server Message Block|Dosya, yazıcı ve paylaşılan kaynaklara erişim|Güvenli yapılandırma gerekir|
|**3389**|**RDP**|Remote Desktop Protocol|Uzak bilgisayara tam grafik arayüzlü (GUI) masaüstü erişimi|Şifreli (Açık porta dikkat edilmeli)|

## 2. Servislerin Detaylı Açıklamaları

### Port 21 — FTP (File Transfer Protocol)

- **İşlevi:** Uzak bir sunucuya dosya yüklemek veya sunucudan dosya indirmek için kullanılır.
    
- **Önemli Nokta:** Giriş bilgileri (`USER`, `PASS`) ve aktarılan dosya içerikleri ağ üzerinden açık metin olarak akar. Güvenli alternatifleri **SFTP** (Port 22) ve **FTPS**'tir.
    

### Port 22 — SSH (Secure Shell)

- **İşlevi:** Uzaktaki bir Linux/Unix veya Windows sunucuya şifreli bir terminal tüneli üzerinden güvenle bağlanıp komut çalıştırmayı sağlar.
    
- **Eski Alternatifi:** Güvensiz ve şifresiz olan **Telnet (Port 23)** protokolünün yerini almıştır.
    
- **Temel Kullanım:**
    
    Bash
    
    ```
    ssh kullanici@192.168.1.50
    ```
    

### Port 80 — HTTP (HyperText Transfer Protocol)

- **İşlevi:** World Wide Web (WWW) üzerindeki web sayfalarının (HTML, CSS, resimler vb.) tarayıcıya aktarılmasını sağlayan temel protokoldür.
    
- **Önemli Nokta:** Veri trafiği şifrelenmez; oturum çerezleri (cookie) veya formlara girilen parolalar ağ dinleyicileri (sniffing) tarafından okunabilir.
    

### Port 443 — HTTPS (HyperText Transfer Protocol Secure)

- **İşlevi:** HTTP protokolünün **TLS/SSL sertifikaları** ile şifrelenmiş modern ve güvenli sürümüdür.
    
- **Önemli Nokta:** İstemci ile sunucu arasındaki tüm trafik şifrelendiğinden, ortadaki adam (MitM) saldırılarına ve paket dinlemeye karşı koruma sağlar. Günümüz web sitelerinin zorunlu standardıdır.
    

### Port 445 — SMB (Server Message Block)

- **İşlevi:** Yerel ağda (özellikle Windows ortamlarında) cihazlar arasında dosya paylaşımı, dizin eşleme ve **yazıcı gibi ağ kaynaklarını ortak kullanıma açma** protokolüdür.
    
- **FTP ile Farkı:** FTP yalnızca dosya aktarımı yaparken; SMB uzaktaki bir dosyayı indirmeden doğrudan ağ üzerinden açıp düzenleme, yazıcıya çıktı gönderme ve süreçler arası haberleşme (RPC) imkânı sunar.
    
- **Güvenlik Notu:** Ağ sızma testlerinde en kritik hedeflerden biridir (örneğin EternalBlue / MS17-010 zafiyeti bu portu hedef almıştır).
    

### Port 3389 — RDP (Remote Desktop Protocol)

- **İşlevi:** Microsoft tarafından geliştirilen; yalnızca komut satırı değil, uzaktaki bilgisayarın **grafik masaüstü arayüzünü (GUI)** birebir kendi ekranınızdaymış gibi fare ve klavyeyle kontrol etmenizi sağlayan protokoldür.
    
- **Kullanım Senaryosu:** SSH'ın metin tabanlı sınırlamalarının aksine, sunucu masaüstünü doğrudan görerek yönetim gerektiren durumlarda kullanılır.
    
- **Güvenlik Notu:** İnternete doğrudan açık bırakılması durumunda kaba kuvvet (Brute-Force) saldırılarına ve fidye yazılımlarına (Ransomware) sıkça hedef olur; genellikle VPN arkasında tutulması önerilir.