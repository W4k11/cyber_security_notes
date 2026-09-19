Network Address Translation

IPV4 maximum 4 milyar cihaza adres verebilir. Cihazların sayısı arttıkça adres sayısı hızla azalmaya başladı. Bu azalan adreslerin bir diğer çözümü ise NAT'dır.

NAT'ın ardındaki fikir, birçok özel IP adresine internet erişimi sağlamak için tek bir genel IP adresi kullanmaktır. Başka bir deyişle, yirmi bilgisayarı olan bir şirketi bağlıyorsanız, yirmi genel IP adresi yerine tek bir genel IP adresi kullanarak tüm yirmi bilgisayara internet erişimi sağlayabilirsiniz.

Not: Teknik olarak, IP adreslerinin sayısı her zaman ikinin kuvveti olarak ifade edilir. Teknik olarak doğru olmak gerekirse, NAT ile otuz iki yerine iki genel IP adresi ayırırsınız. Sonuç olarak, otuz genel IP adresinden tasarruf etmiş olursunuz.

Yönlendirici, dahili IP adresi ve port numarasını harici IP adresi ve port numarasıyla eşleştiren bir tablo tutar. Örneğin, dizüstü bilgisayar bir web sunucusuyla bağlantı kurabilir. Dizüstü bilgisayar açısından, bağlantı 192.168.0.129 IP adresinden 15401 TCP kaynak port numarasıyla başlatılır; ancak web sunucusu, çeviri tablosunda gösterildiği gibi, aynı bağlantının 212.3.4.5 IP adresinden ve 19273 TCP port numarasıyla kurulduğunu görecektir. Yönlendirici bu adres çevirisini sorunsuz bir şekilde gerçekleştirir.