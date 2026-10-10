### Tor'u Benzersiz Yapan 3 Ana Mekanizma

**1. Çok Katmanlı Şifreleme (Soğan Modeli)** Paket bilgisayarından çıkmadan önce 3 farklı şifreleme katmanıyla sarılır.

- **Giriş Düğümü (Guard):** Sadece senin gerçek IP'ni bilir, ama nereye gittiğini ve veriyi göremez. İlk katmanı soyar ve paketi ortaya iletir.
    
- **Orta Düğüm (Relay):** Paketin nereden geldiğini ve nereye gittiğini bilmez; sadece bir önceki ve bir sonraki düğümü görür. İkinci katmanı soyar.
    
- **Çıkış Düğümü (Exit Node):** Son katmanı soyup veriyi hedef web sitesine iletir. Hedef sitenin IP'sini bilir ancak paketin asıl sahibinin (senin) kim olduğunu bilmez.
```bash
sudo anonsurf start
```

| Firewalls                 | Characteristics                                                                                                                                                                                |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Stateless firewalls       | - Basic filtering  <br>- No track of previous connections  <br>- Efficient for high-speed networks                                                                                             |
| Stateful firewalls        | - Recognize traffic by patterns  <br>- Complex rules can be applicable  <br>- Monitor the network connections                                                                                  |
| Proxy firewalls           | - Inspect the data inside the packets as well  <br>- Provides content filtering options  <br>- Provides application control  <br>- Decrypts and inspects SSL/TLS data packets                  |
| Next-generation firewalls | - Provides advanced threat protection  <br>- Comes with an intrusion prevention system  <br>- Identify anomalies based on heuristic analysis  <br>- Decrypts and inspects SSL/TLS data packets |