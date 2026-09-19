Durumlu bir iletişim protokolüdür.
tcp/ıp 4 katmandan oluşur ve OSI modeline çok benzer.

bu 4 katman:
- uygulama
- taşıma
- internet
- ağ arayüzü

Bu protokolü kullanan bir paketin, ağ üzerinden gönderilen verilere ek bilgi parçaları içeren bir dizi başlığı olacaktır:

- Time to Live: ağınızı tıkamasını önlemek için bir süre sonu zamanlayıcısı ayarlar!
- Checksum: Bu alan,TCP/IP gibi protokoller için kontrol sağlar. Herhangi bir veri değiştirilirse, bu değer beklenenden farklı olacak ve dolayısıyla hatalı olacaktır.
- Source Address: Paketin gönderildiği cihazın IP adresi, böylece verinin nereye geri döneceği bilinir.
- Destination Address: Paketin gönderildiği cihazın IP adresi, böylece verinin bir sonraki adımda nereye gideceği bilinir.
- Sequence Number: Bağlantı kurulduğunda, iletilen ilk veri parçasına rastgele bir sayı atanır.
- Acknowledgement Number: Bir veri parçasına sıra numarası verildikten sonra, bir sonraki veri parçasının numarası, sıra numarası + 1 olacaktır.
- Checksum: Bu değer tcp bütünlüğünü veren değerdir.
- Data: Verinin baytları burda saklanır.
- Flag: el sıkışma işlemi sırasında paketin her iki cihaz tarafından nasıl işlenmesi gerektiğini belirler.

3 yönlü handshake adım adım:

1. SYN: handshake için ilk gönderilen pakettir. 2 cihazı senkronize etmek için kullanılır.
2. SYN/ACK: Senkronizasyonu onaylamak için karşıdan gelen mesajdır.
3. ACK: her iki taraftada kullanılır. Onay Paketidir.
4. DATA: dosya baytları bu mesaj ile gönderilir.
5. FIN: Bağlantıyı sonlandırmak için kullanılan pakettir.
#.RST: Bu paket bağlantıyı aniden sonlandırmak için var.



Bağlantı başlatmak için:
örn: syn -> syn/ack -> ack

Bağlantıyı sonlandırmak için:
örn: fın -> ack -> fın -> ack
