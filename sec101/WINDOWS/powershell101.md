Markdown

# PowerShell Temel Komutları ve Cheat Sheet
---

# PowerShell Temelleri

PowerShell'e erişmek için **CMD** (`cmd.exe`) veya **Çalıştır** (`Win + R`) penceresine `powershell` yazmak yeterlidir.

> [!tip] Cmdlet Mantığı
> PowerShell komutları genellikle `Fiil-İsim` (*Verb-Noun*) yapısındadır (Örn: `Get-Process`, `Set-Location`). Bash/Linux komutlarının çoğuna karşılık gelen yerleşik alias'lar (takma adlar) bulunur.

---

## 🧭 Yardım ve Komut Keşfi

### Tüm Komutları Listeleme
```powershell
Get-Command
````

### Komut Türüne Göre Filtreleme

Çıktı varsayılan olarak `CommandType`, `Name`, `Version` ve `Source` sütunlarını gösterir. Yalnızca belirli türdeki komutları (örn. `cmdlet`) listelemek için:

PowerShell

```
Get-Command -CommandType "Cmdlet"
```

### Yardım Alma

Bir cmdlet'in parametrelerini ve ne işe yaradığını görmek için:

PowerShell

```
Get-Help <komut_ismi>
Get-Help Get-Process -Detailed
Get-Help Get-Process -Examples
```

## 📁 Dosya ve Dizin İşlemleri

|**PowerShell Cmdlet**|**Linux / CMD Karşılığı**|**Açıklama**|
|---|---|---|
|`Get-ChildItem`|`ls` / `dir`|Dizin içeriğini listeler|
|`Set-Location`|`cd`|Çalışma dizinini değiştirir|
|`New-Item`|`mkdir` / `touch`|Yeni dosya veya klasör oluşturur|
|`Remove-Item`|`rm` / `del`|Dosya veya klasör siler|
|`Copy-Item`|`cp` / `copy`|Dosya/klasör kopyalar|
|`Move-Item`|`mv` / `move`|Dosya/klasör taşır|
|`Get-Content`|`cat` / `type`|Dosya içeriğini okur|

### Dizin Değiştirme

PowerShell

```
Set-Location -Path ".\dizin_adi"
```

### Yeni Klasör Oluşturma

PowerShell

```
New-Item -Path ".\dizin1\yeniDizinAdi" -ItemType "Directory"
```

### Dosya Silme

PowerShell

```
Remove-Item -Path ".\captain-cabin\captain-wardrobe\captain-boots.txt"
```

### Kopyalama ve Taşıma

PowerShell

```
# Kopyalama
Copy-Item -Path ".\lodur\lo1.txt" -Destination ".\user\lo2.txt"

# Taşıma
Move-Item -Path ".\lodur\lo1.txt" -Destination ".\user\lo1.txt"
```

### Dosya İçeriği Okuma

PowerShell

```
Get-Content -Path ".\lodur\lo1.txt"
```

## 🔍 Filtreleme, Sıralama ve Arama (Pipeline)

### Boyuta Göre Sıralama

Dosyaları dosya boyutuna (`Length`) göre küçükten büyüğe listeler:

PowerShell

```
Get-ChildItem | Sort-Object Length
```

### Uzantıya Göre Filtreleme

Yalnızca `.txt` uzantılı dosyaları listeler:

PowerShell

```
Get-ChildItem | Where-Object -Property "Extension" -eq ".txt"
```

### İsim Deseni ile Filtreleme

`lo` ile başlayan dosyaları listeler:

PowerShell

```
Get-ChildItem | Where-Object -Property "Name" -like "lo*"
```

### Dosya İçinde Metin Arama (`grep` dengi)

Dosya içindeki belirli bir deseni içeren satırları getirir:

PowerShell

```
Select-String -Path ".\lodur.txt" -Pattern "lo"
```

## ⚙️ Sistem ve Kullanıcı Bilgisi Toplama

### Donanım ve İşletim Sistemi Özeti

OS sürümü, BIOS ve donanım detaylarını verir:

PowerShell

```
Get-ComputerInfo
```

### Yerel Kullanıcıları Listeleme

Bilgisayarda kayıtlı yerel kullanıcı hesaplarını listeler:

PowerShell

```
Get-LocalUser
```

### Süreçleri (Process) Listeleme

Çalışan işlemleri, CPU ve bellek (RAM) kullanımlarıyla birlikte listeler:

PowerShell

```
Get-Process
```

### Hizmetleri (Services) Listeleme

Sistemdeki servislerin durumlarını (`Running`, `Stopped`) listeler:

PowerShell

```
Get-Service
```

## 🌐 Ağ (Network) Analizi

### Temel IP Yapılandırması

Aktif arabirimlerin IP, alt ağ maskesi, varsayılan ağ geçidi ve DNS bilgilerini listeler:

PowerShell

```
Get-NetIPConfiguration
```