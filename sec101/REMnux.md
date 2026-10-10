# REMnux (Linux Tabanlı Zararlı Yazılım Analiz Ortamı)

Lenny Zeltser tarafından sürdürülen **REMnux**, Linux tabanlı tersine mühendislik ve statik/dinamik zararlı yazılım analizi için önceden yapılandırılmış yüzlerce araç barındıran Ubuntu tabanlı bir dağıtımdır.

### Temel Kullanım Alanları

- **Tersine Mühendislik & Statik Analiz:** Ghidra, Radare2/Cutter, capa, strings, FLOSS, Yara.
    
- **Belge & Script Analizi:** Şüpheli PDF, Office makroları (olevba, pdfid, pdf-parser), JavaScript/VBScript deobfuscation.
    
- **Ağ Simülasyonu:** FakeDNS, INetSim (Windows analiz makinesinin internete çıkışını taklit ederek C2 iletişimini izole ortamda yakalama).
    
- **Bellek (Memory) Analizi:** Volatility 2 ve Volatility 3.
    

### Kurulum Adımları (REMnux)

En kararlı ve hızlı yöntem **hazır OVA (Sanal Makine)** kalıbını kullanmaktır.

#### Yöntem A: Hazır Sanal Makine (Önerilen)

1. **İndirme:** [remnux.org](https://remnux.org) adresinden resmi `.ova` kalıbını (VirtualBox/VMware uyumlu) indirin.
    
2. **İçe Aktarma:**
    
    - VirtualBox / VMware Workstation açın -> `File` -> `Import Appliance` seçeneğiyle indirdiğiniz `.ova` dosyasını seçin.
        
    - En az **4 GB RAM** ve **2 CPU** çekirdeği atayın.
        
3. **İlk Çalıştırma:**
    
    - Varsayılan kullanıcı adı: `remnux`
        
    - Varsayılan şifre: `malware`
        
4. **Sistemi Güncelleme:** Terminali açıp dağıtımı son sürüme çekin:
    
    Bash
    
    ```
    remnux upgrade
    remnux update
    ```
    

#### Yöntem B: Mevcut Ubuntu 20.04/22.04 Üzerine Yükleme

Mevcut bir temiz Ubuntu makineniz varsa CLI yükleyici ile kurabilirsiniz:

Bash

```
# Kurulum aracını indirin ve çalıştırılabilir yapın
wget https://remnux.org/remnux-cli
chmod +x remnux-cli
sudo mv remnux-cli /usr/local/bin/

# REMnux paketlerini sisteme enjekte edin
sudo remnux install
```

> **Önemli:** Kurulum biter bitmez sanal makinenin temiz durumdayken bir **Snapshot (Anlık Görüntü)** kopyasını alın.