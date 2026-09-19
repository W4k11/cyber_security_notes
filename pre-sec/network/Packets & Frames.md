
Paketler OSI nin 3.katmanında çerçeveler 2.katmanında kullanılır.

### Paketler
Ağa bağlı cihazlar arasında veri iletmenin verimli bir yoludur. Bu veriler küçük parçalar halinde değiş tokuş edildiğinden, büyük mesajların tek seferde gönderilmesine kıyasla ağda darboğaz oluşma olasılığı daha düşüktür.

Bu protokolü kullanan bir paketin, ağ üzerinden gönderilen verilere ek bilgi parçaları içeren bir dizi başlığı olacaktır:

- Time to Live: ağınızı tıkamasını önlemek için bir süre sonu zamanlayıcısı ayarlar!
- Checksum: Bu alan,TCP/IP gibi protokoller için kontrol sağlar. Herhangi bir veri değiştirilirse, bu değer beklenenden farklı olacak ve dolayısıyla hatalı olacaktır.
- Source Address: Paketin gönderildiği cihazın IP adresi, böylece verinin nereye geri döneceği bilinir.
- Destination Address: Paketin gönderildiği cihazın IP adresi, böylece verinin bir sonraki adımda nereye gideceği bilinir.