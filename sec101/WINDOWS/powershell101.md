powershell e erişmek için cmd açıp ```powershell``` yazmak yeterlidir.

```Get-Content```: Dosyanın içeriğini alır ve konsolda kontrol eder.
```Set-Location```: Çalışma dizinini değiştirir (ayarlar).

Tüm fonksiyon ve cmdlet leri sıralamak için ```Get-Command``` yazmak yeterlidir.

Komut listesini filtreleme:
çıktıda 3 sütun var.
```powershell
Get-Command Name Version Source                                                    
```

örneğin commandType a göre filtrelemek istiyorum

Get-Command -CommandType "cmdlet"

Eğer bu komutların ne işe yaradığına bakmak istiyorsak

```powershell
Get-Help komut_ismi
```


##### Dizin

```powershell
Get-ChildItem
```
Bulunduğun dizindeki dosya ve dizinleri listeler.

```powershell
Set-Location -Path ".\dizin adı"
```
Change directory


```powershell
New-Item -Path ".\dizin1\yeniDizinAdı" -ItemType "Directory"
```

```powershell
Remove-Item -Path ".\captain-cabin\captain-wardrobe\captain-boots.txt"
```
rm

```powershell
Copy-Item -Path ".\lodur\lo1.txt" -Destination ".\user\lo2.txt"
```
Copy-Item (kopyala) ve Move-Item (taşı) komutlarını kullanarak dosyaları ve dizinleri kopyalayabilir veya taşıyabiliriz.

```powershell
Get-Content -Path ".\lodur\lo1.txt"
```
cat

```powershell
Get-ChildItem | Sort-Object Length
```
ls komutu ama dosyaları içeriğinin büyüklüğüne göre listeler.


```powershell
Get-ChildItem | Where-Object -Property "Extension" -eq ".txt"
```

txt dosyalarını listeler

```powershell
Get-ChildItem | Where-Object -Property "Name" -eq "lo*"
```
"lo" ile başlayan dosya isimlerine göre listeler.

```powershell
Select-String -Path ".\lodur.txt" -Pattern "lo"
```
"lo" içeren satırları getirir.

```powershell
Get-ComputerInfo
```
OS, BIOS, Donanım bilgilerini verir.

```powershell
Get-LocalUser
```
Bilgisayardaki kayıtlı kullanıcıları listeler.

```powershell
Get-NetIPConfiguration
```
IP, DNS sunucuları, ve gateway configlerini verir.

```powershell
Get-NetIPAddress
```
etkin olmayan IP ler dahil tüm IP leri listeler.

```powershell
Get-Process
```
Bilgisayardaki işlemleri verir. CPU RAM gibi kaynak kullanımlarını ayrıntılı olarak verir.

```powershell
Get-Service
```
Bilgisayardaki hizmetleri listeler.

```powershell
Get-NetTCPConnection
```
Bilgisayardaki aktif ağ bağlantılarını listeler.

```powershell
Get-FileHash -Path .\lodur\lo.txt
```
Dosyadaki hash karmalarını verir.

```powershell
Get-Item -Path "C:\lodur\lo.txt" -Stream  *
```
Dosya bağlantılarını verir.

