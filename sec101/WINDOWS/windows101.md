# Windows Ortam ve Kullanıcı Mimarisi

## 📁 Windows Dizini ve Değişken Mantığı

Geleneksel olarak Windows işletim sistemi dosyaları `C:\Windows` dizininde konumlanır. Ancak sistemin mutlaka `C:` sürücüsünde veya `Windows` adında bir dizinde yer alması zorunlu değildir.

Farklı sürücü ve dizin yapılandırmalarında yazılımların çekirdek dosyalara sorunsuz erişebilmesi için **Sistem Ortam Değişkenleri (System Environment Variables)** kullanılır.

> [!info] Ortam Değişkeni (Environment Variable) Nedir?
> İşletim sisteminin çalışma ortamı hakkında dinamik bilgiler depolayan değişkenlerdir. İşletim sistemi yolu, işlemci mimarisi, geçici klasör konumları (`%TEMP%`) ve kullanıcı profil yolları (`%USERPROFILE%`) bu değişkenlerde tutulur.

### Temel Sistem Değişkeni: `%windir%`
* Windows çekirdek dizinini dinamik olarak işaret eder.
* Sistem ister `C:\Windows` ister `D:\WinOS` altında kurulu olsun, betikler veya komutlar `%windir%` çağrısıyla doğrudan doğru dizine yönlenir.
* **PowerShell Karşılığı:** `$env:windir` veya `$env:SystemRoot`

---

## 👥 Windows Kullanıcı Hesap Türleri

Windows temel olarak iki ana yerel kullanıcı türü sunar:

| Hesap Türü | Yetki Seviyesi | Yapabileceği İşlemler | Kısıtlamalar |
| :--- | :--- | :--- | :--- |
| **Yönetici (Administrator)** | Tam Denetim (`Full Access`) | • Kullanıcı ekleme/silme<br>• Grup ilkelerini düzenleme<br>• Sürücü/yazılım yükleme<br>• Sistem ayarlarını değiştirme | Yok (UAC onayı ile yürütülür) |
| **Standart Kullanıcı** | Kısıtlı Erişim | • Kendisine tahsis edilen dosya/klasörleri yönetme<br>• Yetkilendirilmiş uygulamaları çalıştırma | • Sistem genelinde değişiklik yapamaz<br>• Yeni program yükleyemez<br>• Diğer kullanıcıların verilerine erişemez |

---

## 🔍 Sistemdeki Kullanıcıları İnceleme Yöntemleri

### 1. Grafiksel Arayüz (GUI)
* Başlat arama çubuğuna **"Diğer kullanıcılar"** (*Other users*) yazılarak Ayarlar menüsünden mevcut kullanıcılar listelenebilir.
* Alternatif olarak `Win + R` penceresine `lusrmgr.msc` yazılarak **Yerel Kullanıcılar ve Gruplar** paneli açılabilir.

### 2. Komut Satırı (CLI)

**PowerShell ile:**
```powershell
Get-LocalUser
````

**CMD ile:**

DOS

```
net user
```