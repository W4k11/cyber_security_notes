# FLARE VM (Windows Tabanlı Analiz Ortamı)

Mandiant (Google Cloud) tarafından geliştirilen **FLARE VM**, temiz bir Windows işletim sistemini dinamik ve statik tersine mühendislik laboratuvarına dönüştüren PowerShell tabanlı bir paket ekosistemidir.

### Temel Kullanım Alanları

- **Dinamik Analiz:** x64dbg/x32dbg, Process Hacker 2, Procmon, Regshot, Wireshark, Fiddler.
    
- **Statik & PE Analizi:** PEStudio, Detect It Easy (DiE), CFF Explorer, capa, FLOSS.
    
- **.NET & Script Analizi:** dnSpy, de4dot, CyberChef.
    
- **Zararlı Yazılım Çalıştırma Ortamı:** Windows PE dosyalarını yerel API çağrılarıyla çalıştırma ve davranışlarını gerçek zamanlı izleme.
    

### Kurulum Adımları (FLARE VM)

FLARE VM hazır bir `.iso` veya `.ova` olarak dağıtılmaz; temiz bir Windows sanal makinesi üzerine script ile çekilir.

#### 1. Ön Hazırlık (Temiz Windows VM)

1. VirtualBox veya VMware üzerinde temiz bir **Windows 10 / 11 (64-bit)** sanal makinesi kurun.
    
2. Donanım olarak en az **60-80 GB disk alanı**, **4-8 GB RAM** ve **2+ vCPU** verin.
    
3. Sanal makinenin tüm Windows güncellemelerini tamamlayın.
    

#### 2. Güvenlik ve Uyku Ayarlarını Kapatma

Yükleme esnasında araçların (özellikle exploit ve tersine mühendislik araçlarının) engellenmemesi için:

- **Windows Defender:** Real-time protection, Cloud-delivered protection ve Tamper protection ayarlarını kapatın.
    
- **Güç/Uyku:** Ekran kapanma ve uyku modunu "Hiçbir zaman" (Never) yapın.
    

#### 3. FLARE VM Kurulum Scriptini Çalıştırma

Sanal makine içinde **PowerShell**'i **Yönetici Olarak (Run as Administrator)** açın ve sırasıyla çalıştırın:

PowerShell

```
# Script çalıştırma politikasını esnetin
Set-ExecutionPolicy Unrestricted -Force

# Kurulum aracını indirin
cd C:\Users\<Kullanici_Adiniz>\Desktop
(New-Object System.Net.WebClient).DownloadFile("https://raw.githubusercontent.com/mandiant/flare-vm/main/install.ps1", "install.ps1")

# Kurulumu başlatın
.\install.ps1
```

- Script çalışırken sizden yönetici şifrenizi isteyebilir (yeniden başlatmalar sırasında oturumu otomatik açabilmek için).
    
- Kurulum internet hızınıza ve diskinize bağlı olarak **1 ila 3 saat** sürebilir; sistem bu süreçte birkaç kez otomatik yeniden başlayacaktır.
    

#### 4. Kurulum Sonrası

- Kurulum tamamlandığında masaüstünde ve Başlat menüsünde FLARE klasörlerini göreceksiniz.
    
- **Kritik Adım:** Makineyi kapatın ve sanal makine yöneticinizden **"Clean Base / Snapshot 1"** adıyla bir Snapshot alın. Analiz edeceğiniz her malware sonrası bu noktaya geri döneceksiniz.