OSI modelinin başlıca avantajlarından biri, cihazların bir ağ üzerinde farklı işlevlere ve tasarımlara sahip olup diğer cihazlarla iletişim kurabilmesidir. OSI modelinin tekdüzeliğine uyan bir ağ üzerinden gönderilen veriler, diğer cihazlar tarafından anlaşılabilir.

1. Fiziksel
2. Veri Bağlantısı
3. Ağ
4. Taşıma
5. Oturum
6. Sunum
7. Uygulama

### 1. Fiziksel Katman
- Ethernet Kablosu var bu katmanda
- Cihazlar binary(1-0) ile iletişim kurar

### 2. Veri Bağlantısı Katmanı
- Verileri fiziksel katmandan alıp(uzak pc nin IP adreside dahil olmak üzere) MAC adresine ekler.
- Onu tanımlamak için bir wifi card bulunudurur.


### 3. Ağ Katmanı
Bu katmandaki bazı protokoller, verilerin bir cihaza ulaşmak için izlemesi gereken "en uygun" yolu tam olarak belirlerken, ağ modülünün bu aşamasında yalnızca bu protokollerin varlığından haberdar olmalıyız. Kısaca, bu protokoller arasında OSPF (Open Shortest Path First) ve RIP (Routing Information Protocol) yer almaktadır.

Hangi rotanın izleneceğine karar veren faktörler şunlardır:
hız, kestirme ve güvenlik
- En kısa yol hangisidir? Yani, paketin geçmesi gereken cihaz sayısı en az olan yol hangisidir? 
- En güvenilir yol hangisi? Yani, o yolda daha önce paket kaybı yaşandı mı? 
- Hangi yol daha hızlı fiziksel bağlantıya sahip? Yani bir yol bakır bağlantı (daha yavaş) kullanırken, diğer yol fiber optik bağlantı (önemli ölçüde daha hızlı) kullanıyor?

### 4. Taşıma Katmanı
Burda veri gönderirken bazı faktörlere bağlı olarak 2 prokolden biri seçilir:

TCP (Transmission Control Protocol)
UDP (User Datagram Protocol)

##### TCP
- 3 yönlü handshake i vardır.
- Güvenilir veri aktarımı yapar. (Akış kontrol, tıkanıklık kontrolü)
- HTTP, POP3, IMAP ve SMTP gibi protokolleri kullanır.
- Yavaştır ve bir veri karşıya ulaşmadığında tüm veri iptal olur.
##### UDP
Bağlantı kurulmasını gerektirmez. UDP, DNS gibi hızlı sorgulara dayanan protokoller ve sesli/görüntülü konferans ve yayın gibi gerçek zamanlı iletişime öncelik veren protokoller için uygundur.
- Verinin karşıya tam bir şekilde ulaşıp ulaşmaması umrunda değil.

### 5. Oturum Katmanı
Veriler sunum katmanından (katman 6) doğru şekilde çevrildikten veya biçimlendirildikten sonra, oturum katmanı (katman 5), verilerin hedeflendiği diğer bilgisayarla bağlantıyı kurmaya ve sürdürmeye başlar.
Bağlantı kurulduğunda oturum oluşturur ve aktif kaldıkça oturum devam eder.
oturumlar benzersiz
Aynı zamanda bağlantı aktif değilse bağlantıyı koparmaktanda sorumludur.

### 6. Sunum Katmanı

7.katmana gelen giden verileri çevirmen görevi görür.


### 7. Uygulama Katmanı
Grafik Kullanıcı Arayüzü (GUI) sağlar. Diğer protokoller arasında, web sitesi adreslerinin IP adreslerine nasıl çevrildiğini açıklayan Alan Adı Sistemi (DNS) de bulunur.
