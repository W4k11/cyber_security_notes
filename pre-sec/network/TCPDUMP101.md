# TCPDUMP HIZLI REFERANS VE NOT KARTI

## 1. Temel Seçenekler (Flags)
tcpdump -i eth0               # eth0 arayüzünü dinler
tcpdump -D                    # Tüm ağ arayüzlerini listeler
tcpdump -n                    # IP adreslerini çözmez (hızlıdır)
tcpdump -nn                   # IP ve Port numaralarını isimlere çevirmez
tcpdump -c 20                 # Sadece 20 paket yakalar ve durur
tcpdump -A                    # Paket içeriğini ASCII (okunabilir metin) gösterir
tcpdump -X                    # Paket içeriğini HEX ve ASCII olarak gösterir
tcpdump -v / -vv / -vvv       # Ayrıntı seviyesini artırır
tcpdump -w dosya.pcap         # Paketleri PCAP dosyasına kaydeder
tcpdump -r dosya.pcap         # Kayıtlı PCAP dosyasını okur

---

## 2. Temel Filtreler

### Konum / IP Filtreleri
host 192.168.1.50             # Gelen/giden IP 192.168.1.50 olanlar
src 10.0.0.5                  # Kaynak IP'si 10.0.0.5 olanlar
dst 192.168.1.1               # Hedef IP'si 192.168.1.1 olanlar
net 192.168.1.0/24            # Belirtilen alt ağa ait paketler

### Port Filtreleri
port 80                       # Port 80 (HTTP) trafiği
src port 53                   # Kaynak portu 53 olanlar
dst port 22                   # Hedef portu 22 (SSH) olanlar
portrange 100-200             # 100 ile 200 arasındaki portlar

### Protokol Filtreleri
tcp / udp / icmp / arp / dns

---

## 3. Mantıksal Operatörler
and / &&                     # Ve
or / ||                      # Veya
not / !                      # Değil

---

## 4. Pratik ve Sağlam Örnek Komutlar

# 1. DNS Sorgularını Yakalama (Domain Çözümlemeleri)
sudo tcpdump -nn -i eth0 udp port 53

# 2. HTTP İçeriğini / Metinlerini Anlık Okuma (Passwords, URLs vb.)
sudo tcpdump -nn -A -i eth0 port 80

# 3. ICMP (Ping) Trafiğini İzleme
sudo tcpdump -nn -i eth0 icmp

# 4. SSH ve DNS Dışındaki Tüm Trafiği Yakalama
sudo tcpdump -nn -i eth0 not port 22 and not port 53

# 5. Belirli Bir IP ile Belirli Bir Port Arasındaki İletişim
sudo tcpdump -nn host 192.168.1.50 and port 443

# 6. Sadece Yeni Bağlantı İsteklerini (TCP SYN) Yakalama
sudo tcpdump -nn "tcp[tcpflags] & (tcp-syn) != 0"

# 7. Trafiği Wireshark için PCAP Dosyasına Kaydetme
sudo tcpdump -nn -i eth0 -c 500 -w analiz.pcap

# 8. Kayıtlı PCAP Dosyasından Sadece UDP Trafiğini Okuma
tcpdump -nn -r analiz.pcap udp