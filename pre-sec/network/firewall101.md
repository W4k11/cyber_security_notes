Yetkisiz gelen ve giden trafiği durdurarak ağ trafiğini filtrelemek için kullanılan bir güvenlik aracı, donanım veya yazılım.
Trafik nereden geliyor?
Trafik nereye gidiyor?
bu trafik hangi porta ait?
trafik hangi protokolü kullanıyor?


Firewall lar 2 temel kategoriye ayrılırlar:

Stateful:

- Bu firewall tipi, tek tek paketleri incelemek yerine, bağlantının tamamından gelen bilgileri kullanarak cihazın davranışını belirler. Bu tür güvenlik duvarları, karar verme süreci dinamik olduğu için durumsuz güvenlik duvarlarına kıyasla çok daha fazla kaynak tüketir

- Bir sunucudan gelen bağlantı kötüyse, tüm cihazı bloke eder.

Stateless:
- Bu firewall tipi, tek tek paketlerin kabul edilebilir olup olmadığını belirlemek için statik bir kurallar kümesi kullanır.
- Örneğin, bir cihazın hatalı bir paket göndermesi, cihazın tamamının bloke edilmesi anlamına gelmez. 
- Bu güvenlik duvarları alternatiflerine göre çok daha az kaynak tüketse de, çok daha aptalca çalışıyorlar.
- Ancak, bu güvenlik duvarları, bir grup sunucudan gelen büyük miktarda trafiği (örneğin DDOS gibi) aldıklarında harika iş çıkarırlar.

