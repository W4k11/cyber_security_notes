# Active Directory (AD) ve Organizational Unit (OU) Temelleri

## 🏢 Active Directory (AD) Nedir?

> [!info] Tanım
> **Active Directory (AD):** Microsoft tarafından Windows etki alanı (Domain) ağları için geliştirilmiş merkezi bir **dizin hizmetidir (Directory Service)**.

Ağ üzerindeki kullanıcılar, bilgisayarlar, yazıcılar ve paylaşılan kaynaklar gibi tüm nesneleri tek bir hiyerarşik veri tabanında depolar.

### Temel Görevleri
* **Merkezi Kimlik Doğrulama (Authentication):** Kullanıcıların tek bir kimlik bilgisiyle ağdaki yetkili tüm kaynaklara erişmesini sağlar (Kerberos / NTLM protokolleri üzerinden).
* **Yetkilendirme (Authorization):** Hangi kullanıcının veya grubun hangi kaynağa ne düzeyde erişebileceğini belirler.
* **Merkezi Yönetim:** Sistem yöneticilerinin tüm ağ ilkelerini tek bir noktadan (Domain Controller) dağıtmasına olanak tanır.

---

## 🗂️ Organizational Unit (OU) Nedir?

> [!abstract] Tanım
> **Organizational Unit (OU - Organizasyon Birimi):** Active Directory etki alanı içinde kullanıcılar, gruplar ve bilgisayarlar gibi nesneleri hiyerarşik olarak düzenlemek için kullanılan **mantıksal kapsayıcılardır (containers)**.

### OU'ların Temel Amaçları
* **Departman / Konum Eşlemesi:** Çoğu kurumsal yapıda OU'lar şirket departmanlarına (Örn: *Muhasebe*, *İnsan Kaynakları*, *IT*) veya fiziksel lokasyonlara (Örn: *Istanbul-Ofis*, *Ankara-Sube*) göre ayrılır.
* **Grup İlkesi (GPO) Dağıtımı:** Benzer güvenlik ve yapılandırma politikalarının (Group Policy Objects) tek bir hamlede belirli bir kullanıcı/bilgisayar kümesine uygulanmasını sağlar.
* **Yetki Devri (Delegation of Control):** Bir yöneticinin, tüm domain yetkisini vermeden yalnızca belirli bir departmanın/OU'nun şifre sıfırlama veya kullanıcı ekleme gibi görevlerini yerel destek ekiplerine devretmesine olanak tanır.

---

## 🔗 Hiyerarşik Yapı

```

Domain (ornek.local) │ ├── 📁 OU: Yonetim │ └── 👤 Kullanıcı: CEO │ ├── 📁 OU: Bilgi Teknolojileri (IT) │ ├── 👤 Kullanıcı: Sistem Yoneticisi │ └── 💻 Bilgisayar: IT-Laptop-01 │ └── 📁 OU: Muhasebe ├── 👥 Grup: Muhasebe-Erisim └── 💻 Bilgisayar: Finans-PC-01

```

> [!tip] Obsidian İpuçları
> - İlgili konularla bağ kurmak için: `[[Domain Controller]]`, `[[Group Policy Object (GPO)]]`, `[[Kerberos]]` wikilink'lerini ekleyebilirsiniz.
