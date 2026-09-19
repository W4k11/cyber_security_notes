#### IP Adresleri
- Açılımı Internet Protocol.
- 4 oktetden oluşur ve hepsi 0 ile 255 aralığındadır.
- Aynı ağ içinde benzersiz olurlar ve cihazdan cihaza göre değişir.
- 2 ye ayrılırlar public ve private olmak üzere
- public IP internette herkese açık bilgisayarlarla iletişim kurmak için kullanılır.Genel IP adresleri, İnternet Servis Sağlayıcınız (veya ISS) tarafından aylık bir ücret karşılığında (faturanızda!) verilir.
- private IP aynı ağdaki bilgisayarlarla iletişim kurmak için kullanılır.
#### MAC Adresleri (Media Adress Control)

Bir ağdaki tüm cihazların, cihazın anakartında bulunan bir mikroçip kartı olan fiziksel bir ağ arayüzü vardır. Bu ağ arayüzüne, üretildiği fabrikada MAC (Medya Erişim Kontrolü) adresi adı verilen benzersiz bir adres atanır. MAC adresi, ikiye bölünmüş ve iki nokta üst üste ile ayrılmış on iki karakterli bir onaltılık sayıdır (bilgisayarda sayıları temsil etmek için kullanılan on altı tabanlı bir sayı sistemi). Bu iki nokta üst üste ayırıcı olarak kabul edilir. Örneğin, a4:c3:f0:85:ac:2d. İlk altı karakter ağ arayüzünü üreten şirketi, son altı karakter ise benzersiz bir sayıyı temsil eder.

#### PING 

Ping, elimizdeki en temel ağ araçlarından biridir. Ping, cihazlar arasındaki bağlantının performansını belirlemek için ICMP (Internet Control Message Protocol) paketlerini kullanır; örneğin, bağlantının mevcut olup olmadığını veya güvenilir olup olmadığını tespit eder.

```
ping -c 4 google.com
```

