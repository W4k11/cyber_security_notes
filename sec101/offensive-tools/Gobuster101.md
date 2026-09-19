# Gobuster
Açık kaynaklı offensive web dizin, dns, subdomains, vhosts, amazon s3 buckets ve google cloud storage bunlara brute force yapan bir araçtır. Wordlist kullanır.

Numaralandırma modları ile hedefin hangi katmanını ve ne tür bir varlığını keşfetmek istediğinizi belirleyen alt komuttur.

## dir mode
Bir web sitesinin dizinlerini ve içeriğini görmek istiyorsak kullanırız.

```bash
gobuster dir --help
```
dir mod kullanım örneği:

```bash
gobuster dir -u "http://example.com" -w /path/to/wordlist
```
www.offensivetools.thm
/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

## dns mode
Bu modda dns adlarını saldırı yaparak bulur.

```bash
gobuster dns --help
```

```shell-session
gobuster dns -d offensivetools.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt 
```
## vhosts mode
Gobuster'ın sanal sunucuları kaba kuvvet yöntemiyle taramasına olanak tanır. Sanal sunucular, aynı makinede bulunan farklı web siteleridir. Bazen alt alan adlarına benzeyebilirler, ancak aldanmayın! Sanal sunucular IP tabanlıdır ve aynı sunucuda çalışırlar. Alt alan adları ise DNS'te kurulur. Vhost ve DNS modları arasındaki fark, Gobuster'ın tarama yöntemindedir:

- vhost modu, yapılandırılmış HOSTNAME'i (-u bayrağı) bir kelime listesi girdisiyle birleştirerek oluşturulan URL'ye yönlendirme yapacaktır.
- DNS modu, yapılandırılmış alan adını (-d bayrağı) bir kelime listesi girdisiyle birleştirerek oluşturulan tam nitelikli alan adına (FQDN) DNS sorgusu yapacaktır.

```bash
gobuster vhost -u "http://10.114.163.250" --domain offensivetools.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 250-320 
```
