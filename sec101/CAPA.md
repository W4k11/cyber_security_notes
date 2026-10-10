# capa (Mandiant) — Malware Capability Analysis

Mandiant tarafından geliştirilen **capa**, PE, ELF veya shellcode formatındaki şüpheli/zararlı ikili (binary) dosyaların **hangi yeteneklere (capabilities) sahip olduğunu** otomatik tespit eden açık kaynaklı bir statik analiz aracıdır.

Dosyanın disassembly çıktısını, API çağrılarını ve string verilerini kural tabanlı (rule-based YAML) motoruyla tarayarak bulguları doğrudan **MITRE ATT&CK** ve **MBC (Malware Behavior Catalog)** çerçeveleriyle eşleştirir.

---

## 🚀 Kurulum (Installation)

### 1. Hazır Binary ile Kurulum (En Hızlı & Önerilen)
Sisteminizde Python/bağımlılık karmaşası yaşamamak için en temiz yöntem GitHub Releases üzerinden derlenmiş binary'yi indirmektir:

- **GitHub Release:** [mandiant/capa releases](https://github.com/mandiant/capa/releases)

#### Linux / WSL:
```bash
# En son release sürümünü indirin (örnek v7.x)
wget [https://github.com/mandiant/capa/releases/download/v7.0.1/capa-v7.0.1-linux.zip](https://github.com/mandiant/capa/releases/download/v7.0.1/capa-v7.0.1-linux.zip)
unzip capa-v7.0.1-linux.zip
chmod +x capa
sudo mv capa /usr/local/bin/

# Kurulum testi
capa --version


#### Windows (FlareVM / PowerShell):

- Releases sayfasından `capa-v*-windows.zip` dosyasını indirin ve bir dizine (örn. `C:\Tools\capa`) çıkarın.
    
- Bu dizini ortam değişkenlerindeki (`System PATH`) listesine ekleyin.
    
- _FlareVM_ kullanıyorsanız genellikle yüklü gelir (`capa` yazıp terminalde çağırabilirsiniz).
    

### 2. Python / pip ile Kurulum

Sanal ortam (venv) içinde çalıştırmak isterseniz:

Bash

```
# Sanal ortam oluşturup kurma
python3 -m venv capa-env
source capa-env/bin/activate
pip install flare-capa

# İlk çalıştırmada kuralları otomatik indirir
capa --help
```

## 🛠️️ Temel Kullanım & Komut Parametreleri

### Standart Tarama (Özet Görünüm)

Bash

```
capa malware_sample.exe
```

### Detaylı Analiz (Ayrıntılı Fonksiyon & Adres Bilgisi)

TryHackMe odalarında veya tersine mühendislik aşamasında hangi capability'nin hangi adreste (`0x401000` vb.) tetiklendiğini görmek için:

Bash

```
# Verbose mod: Eşleşen kuralların hangi fonksiyonlarda olduğunu listeler
capa -v malware_sample.exe

# Very Verbose mod: Kuralın içindeki her bir alt ifadenin (API çağrısı, string vb.) adresini döker
capa -vv malware_sample.exe
```

### Çıktıyı JSON / Rapor Olarak Alma

Otomasyon betikleri veya log saklama için:

Bash

```
capa -j malware_sample.exe > capa_report.json
```

## 📊 Çıktı Mimarisi (Neye Bakmalı?)

capa çalıştırıldığında çıktıyı 3 ana blokta özetler:

|**Bölüm**|**Açıklama**|**Örnek Bulgu**|
|---|---|---|
|**Analysis**|Dosya formatı, mimari ve packer tespiti|`format: pe`, `arch: i386`|
|**ATT&CK Tactic**|Yüksek seviyeli saldırı fazları|`DEFENSE EVASION`, `PERSISTENCE`|
|**MBC / Capabilities**|Yazılımın gerçekleştirebildiği somut eylemler|`check for debugger`, `create registry key`|