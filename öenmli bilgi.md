### Tor'u Benzersiz Yapan 3 Ana Mekanizma

**1. Çok Katmanlı Şifreleme (Soğan Modeli)** Paket bilgisayarından çıkmadan önce 3 farklı şifreleme katmanıyla sarılır.

- **Giriş Düğümü (Guard):** Sadece senin gerçek IP'ni bilir, ama nereye gittiğini ve veriyi göremez. İlk katmanı soyar ve paketi ortaya iletir.
    
- **Orta Düğüm (Relay):** Paketin nereden geldiğini ve nereye gittiğini bilmez; sadece bir önceki ve bir sonraki düğümü görür. İkinci katmanı soyar.
    
- **Çıkış Düğümü (Exit Node):** Son katmanı soyup veriyi hedef web sitesine iletir. Hedef sitenin IP'sini bilir ancak paketin asıl sahibinin (senin) kim olduğunu bilmez.
```bash
sudo anonsurf start
```

```bash
ollama run llama3.2:3b
nvidia-smi
python3 server.py
```

modeli derlemek için:
```bash
ollama create pentest_llama -f Modelfile
```

Modeller listesinde pentest_llama görünüyor mu kontrol et
```bash
ollama list
```


```bash
ollama run pentest_llama "Nmap ile versiyon tespiti nasıl yapılır?"
```
