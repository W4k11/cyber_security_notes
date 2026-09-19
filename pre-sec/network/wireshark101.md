Piyasada kullanılan en güçlü network analiz araçlarından biridir. 
Not: Saldırı tespit aracı değildir.

Ağ sorunlarını bulur çözüm önerileri yapar.
Güvenlikle alakalı anormallikleri tespit eder.
Yanıt kodları ve veri yükü gibi protokol detaylarını araştırmak ve öğrenmek.

|                                   |                                                                                                                                                                                                                 |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Toolbar**                       | filtreleme, sıralama, özetleme, dışa aktarma ve birleştirme dahil olmak üzere paket yakalama ve işleme için birden fazla menü ve kısayol içerir.                                                                |
| **Display Filter Bar**            | Ana sorgulama ve filtreleme bölümü.                                                                                                                                                                             |
| **Recent Files**                  | Son zamanlarda incelenen dosyaların listesi. Listelenen dosyaları çift tıklayarak geri çağırabilirsiniz.                                                                                                        |
| **Capture Filter and Interfaces** | Yakalama filtreleri ve mevcut izleme noktaları (ağ arayüzleri). Ağ arayüzü, bir bilgisayar ile ağ arasındaki bağlantı noktasıdır. Yazılım bağlantısı (örneğin, lo, eth0 ve ens33), ağ donanımını etkinleştirir. |
| **Status Bar**                    | Alet durumu, profil ve sayısal paket bilgileri.                                                                                                                                                                 |

|                      |                                                                                                                                                                                                              |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Packet List Pane     | Her paketin özeti (kaynak ve hedef adresleri, protokol ve paket bilgileri). Daha detaylı incelemek için listeden bir paket seçebilirsiniz. Bir paket seçtiğinizde, ayrıntılar diğer panellerde görünecektir. |
| Packet Details Panel | Seçilen paketin ayrıntılı protokol dökümü.                                                                                                                                                                   |
| Packet Bytes Pane    | Seçilen paketin onaltılık ve çözümlenmiş ASCII gösterimi. Ayrıntılar bölmesinde tıklanan bölüme bağlı olarak paket alanını vurgular.                                                                         |

Wireshark paketleri renklendirerek sorun ya da güvenlikle alakalı bir sorun olduğunda daha iyi görünmesi için renklendirir.

View ---> coloring rules

#### Traffic Sniffing:
köpekbalığı yüzgeçine basarak trafiği izlemeye başlar kırmızı kareye basarak trafiği izlemeyi bırakır.


#### Merge PCAP Files:
file ---> merge basarak herhangi bir PCAP dosyasıyla seçtiğiniz diğer PCAP dosyalarını birleştirebilirsiniz.


#### View File Details:
Statistics ---> Capture File Proporties


#### Packet Dissection:
Paket ayrıştırması, mevcut protokolleri ve alanları çözerek paket ayrıntılarını inceleyen bir yöntemdir ve protokol ayrıştırması olarak da bilinir.


#### Packet Details:
Çift tıklamayla paketin detaylarını görebiliriz. OSI'nin 5 ile 7 arasındaki katmanlarından oluşur.

frame/packet
source [MAC]
source [IP]
protcols
protocol errors
application protocol
application data

bunlar gözükür.

#### Packet Numbers:
No. Sütununun altındadır. Detaylarda Frame olarak yazar.

#### Go to Packet:
Herhangi bir Paketin numarasını girerek hızlıca bulmamızı sağlar.
Go ---> Go to Packet...

#### Find Packets:
Paketin içeriğine göre arama yapmak için "Edit ---> Find Packet"


#### Mark Packets:
Paketi işaretlemek "Sağ tık --> mark/unmark".

#### Packet Comments:
Paket yorumları details de gözükür ama "sağ tık Packet Comments ---> edit packet comments"

#### Export Packets:
Seçilen paketleri analiz etmek için paketleri dışarı çıkartır.
File ---> Export Specified Packets

#### Export Objects (Files):
Ağ üzerinden aktarılan dosyaları çıkartabilir.

#### Time Display:
Zamana göre filtreleme ve sıralama.
view ---> Time Display Format

#### Expert Info:
Analistlerin çıktıyı daha rahat okuyabilmeleri için bazı sorunları renklendirmişlerdir veya sınıflandırmışlardır.

|              |            |                                                         |
| ------------ | ---------- | ------------------------------------------------------- |
| **Severity** | **Colour** | **Info**                                                |
| **Chat**     | **Blue**   | Normal iş bilgileri.                                    |
| **Note**     | **Cyan**   | Uygulama hata kodları.                                  |
| **Warn**     | **Yellow** | Normalin dışındaki hata kodları veya sorun bildirimleri |
| **Error**    | **Red**    | Hatalı paketler gibi sorunlar                           |

#### Packet Filtering:
Wireshark, analistlerin trafiği daraltmasına ve ilgilenilen olaya odaklanmasına yardımcı olan güçlü bir filtre motoruna sahiptir. Wireshark'ın iki tür filtreleme yaklaşımı vardır: yakalama ve görüntüleme filtreleri. Yakalama filtreleri, yalnızca kullanılan filtre için geçerli olan paketleri "yakalamak" için kullanılır. Görüntüleme filtreleri ise kullanılan filtre için geçerli olan paketleri "görüntülemek" için kullanılır.

Filtreler, Wireshark'ın resmi protokol referansında bulunan protokoller için tasarlanmış özel sorgulardır. Filtreler yalnızca ilgilenilen olayı inceleme seçeneği olsa da, trafiği filtrelemenin ve yakalama dosyasındaki gürültüyü gidermenin iki farklı yolu vardır. Birincisi sorguları, ikincisi ise sağ tıklama menüsünü kullanır. Wireshark güçlü bir araç sunar ve temel görevler için sorgu yazmak istemeyen analistler için altın bir kural vardır: "Üzerine tıklayabiliyorsanız, filtreleyebilir ve kopyalayabilirsiniz."


NOT:

Bir pakette spesifik bir kelime aratmak için follow ---> html olarak seçip find dan bulabiliriz.