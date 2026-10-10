# Bulut Bilişim (Cloud Computing) Temelleri

## 1. Bulut Bilişimin Temel Faydaları

Bulut bilişim, kendi sunucularınızı fiziksel bir odada barındırmak yerine bilgi işlem gücünü internet üzerinden kiralama modelidir.

- **Ölçeklenebilirlik (Scalability):** Kullanıcı sayısı aniden fırladığında kaynakları saniyeler içinde artırabilir, talep düştüğünde gereksiz maliyetleri kısmak için küçültebilirsiniz.
    
- **İsteğe Bağlı Self-Servis (On-Demand Self-Service):** Donanım satın alma, kargolama veya montaj beklemeden bir tıkla yeni sunucu ve depolama alanı açabilir ya da kapatabilirsiniz.
    
- **Kullandıkça Öde (Pay-As-You-Go):** Yüksek başlangıç yatırımı yerine yalnızca tükettiğiniz saat, bant genişliği veya depolama alanı kadar ödeme yaparsınız.
    
- **Güvenlik (Security):** Bulut sağlayıcıları fiziksel tesis güvenliği, DDoS koruması ve güncel yamaları profesyonel güvenlik ekipleriyle sağlar.
    
- **Yüksek Erişilebilirlik (High Availability):** Veri merkezlerinden birinde fiziksel bir arıza olsa dahi sistemleriniz yedekli sunucular üzerinden kesintisiz çalışmaya devam eder.
    
- **Küresel Erişim (Global Reach):** Uygulamalarınızı dünyanın dört bir yanındaki veri merkezlerine yayarak kullanıcılara en yakın noktadan düşük gecikmeyle hizmet verebilirsiniz.
    

## 2. Dağıtım Türleri (Deployment Models)

|**Model**|**Ne Anlama Gelir?**|**En Yaygın Kullanım Alanı**|
|---|---|---|
|**Public Cloud (Genel Bulut)**|Kaynaklar sağlayıcıya aittir ve birden fazla müşteri tarafından paylaşılır. Maliyeti düşüktür, altyapı bakımı gerekmez.|Girişimler (start-up'lar), bloglar, e-ticaret siteleri ve genel web uygulamaları.|
|**Private Cloud (Özel Bulut)**|Altyapı tamamen tek bir kuruluşa tahsis edilmiştir. En üst düzey kontrol, izolasyon ve yasal uyumluluk sağlar.|Bankalar, savunma sanayii, sağlık kuruluşları ve regülasyona tabi kamu kurumları.|
|**Hybrid Cloud (Hibrit Bulut)**|Public ve Private bulutun birleşimidir. Kritik veriler özel alanda tutulurken, ani yük artışları genel buluta aktarılır.|Kritik müşteri verisini şirket içinde tutup kampanya dönemlerindeki web trafiğini genel bulutta karşılayan büyük e-ticaret platformları.|

## 3. Bulut Hizmet Modelleri (IaaS, PaaS, SaaS)

Bulut modelleri, sağlayıcının ne kadarını yönettiğine ve sizin ne kadarından sorumlu olduğunuza göre ayrılır:

Plaintext

```
Sorumluluk Dağılımı:
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│     IaaS     │     │     PaaS     │     │     SaaS     │
├──────────────┤     ├──────────────┤     ├──────────────┤
│ Uygulama     │     │ Uygulama     │     │              │
│ Veri         │     │ Veri         │     │ Her Şeyi     │
│ İşletim Sis. │     ├──────────────┤     │ Sağlayıcı    │
├──────────────┤     │ İşletim Sis. │     │ Yönetir      │
│ Sanallaştırma│     │ Sanallaştırma│     │ (Kullanıcı   │
│ Donanım/Ağ   │     │ Donanım/Ağ   │     │ sadece aracı │
│ (Sağlayıcı)  │     │ (Sağlayıcı)  │     │ kullanır)    │
└──────────────┘     └──────────────┘     └──────────────┘
```

### A) IaaS (Infrastructure as a Service - Altyapı Hizmeti)

- **Mantık:** Sağlayıcıdan "boş bir sanal makine, disk ve ağ" kiralarsınız.
    
- **Sorumluluk:** Fiziksel donanım sağlayıcıdadır; işletim sistemi kurulumu, yamalar, güvenlik ayarları ve uygulamanın bakımı sizdedir.
    
- **Örnek:** AWS EC2, Azure Virtual Machines, DigitalOcean Droplets.
    

### B) PaaS (Platform as a Service - Platform Hizmeti)

- **Mantık:** Sunucu yönetimi, işletim sistemi ve çalışma ortamı (runtime) sağlayıcı tarafından hazır sunulur. Siz yalnızca kodunuzu yazar ve yüklersiniz.
    
- **Sorumluluk:** Sunucu yapılandırmasıyla uğraşmazsınız; yalnızca uygulamanızdan ve verilerinizden sorumlusunuzdur.
    
- **Örnek:** Heroku, Google App Engine, AWS Elastic Beanstalk.
    

### C) SaaS (Software as a Service - Yazılım Hizmeti)

- **Mantık:** İnternet üzerinden doğrudan çalışan eksiksiz bir yazılımdır.
    
- **Sorumluluk:** Altyapı, kod ve güncellemeler tamamen sağlayıcıya aittir. Kullanıcı yalnızca tarayıcıdan veya istemciden giriş yapar.
    
- **Örnek:** Gmail, Zoom, Microsoft 365, Google Drive.
    

## 4. Popüler Bulut Sağlayıcıları ve Öne Çıkan Güçlü Yönleri

- **Microsoft Azure:** Windows ekosistemi, Active Directory entegrasyonu ve kurumsal hibrit altyapılarda pazar liderlerindendir.
    
- **Google Cloud Platform (GCP):** Büyük veri analitiği, Kubernetes, yapay zekâ ve makine öğrenimi alanlarında son derece güçlüdür.
    
- **AWS (Amazon Web Services):** En geniş ürün yelpazesine, en büyük küresel pazar payına ve olgun topluluk desteğine sahip öncü sağlayıcıdır.
    
- **Alibaba Cloud:** Asya-Pasifik bölgesinde pazar lideridir; özellikle Uzak Doğu odaklı küresel projeler için uygun maliyetli çözümler sunar.
    
- **IBM Cloud:** Kurumsal şirketlerin hibrit bulut geçişlerine, regülasyon uyumluluğuna ve kurumsal AI çözümlerine odaklanır.
    
- **Oracle Cloud (OCI):** Büyük kurumsal veritabanları (Oracle DB), yüksek performanslı hesaplama ve ERP altyapıları için optimize edilmiştir.