
Şirketler sanallaştırmayı tek tek serverlar kurmak yerine tek bir server alıp ordan yönetmek için sanallaştırma kullanırlar.
Maaliyetleri büyük oranda azaltır.

### Sanallaştırma neler var?

##### Hypervisor
Sanal makineleri oluşturup yöneten kısımdır.
Ayrı ayrı sanal makineler oluşturup hepsini belirli sayıda cpu ve ram verir.

2 tür hypervisor var:
- **Tip 1** hipervizörler doğrudan fiziksel donanım üzerinde çalışır, bu da onları hızlı, verimli ve sunucular ile profesyonel ortamlar için ideal hale getirir.
- **Tip 2** hipervizörler mevcut bir işletim sistemi içinde çalışır, bu da kurulumlarını kolaylaştırır ve öğrenme, test etme veya küçük ölçekli kurulumlar için ideal hale getirir.

tip 1 temel farkı:
- üretim sunucusu
- veritabanı sunucusu
- veri merkezi

tip 2 temel farkı:
- Zararlı dosyaları test et
- Yazılım testi
- Kali linux

##### Container
Hafif, yalıtılmış bir ortam olup tek bir uygulamayı ve onu desteklemek için gerekli tüm bileşenleri çalıştırır. Ayrı bir işletim sistemi getirmek yerine, Mevcut sistemin çekirdeğini, yani donanımla iletişim kuran ve bellek ve çalışan programlar gibi kaynakları yöneten işletim sisteminin bir parçası olan çekirdeği kullanarak ödünç alır.
