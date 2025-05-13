# 🐍 Snort ve ClamAV Kurulumu: ICMP ve Antivirüs Testleri (Ubuntu)

Bu rehber, Ubuntu üzerinde Snort IDS ve ClamAV antivirüs sistemlerinin kurulumunu, test dosyalarıyla çalışma prensiplerini ve terminal üzerinden test edilmesini adım adım açıklar.

# 🐍 Snort and ClamAV Setup: ICMP and Antivirus Tests (Ubuntu)

This guide explains how to install and test Snort IDS and ClamAV antivirus on Ubuntu. It includes configuration and EICAR test file simulations to verify detection functionality.


<details>
<summary><strong>TR Türkçe Dokümantasyon</strong></summary>

---

## 📦 1. Snort Kurulumu

```bash
sudo apt-get update
sudo apt-get install -y snort
```

> Kurulum sırasında `HOME_NET` için IP aralığı girmeniz istenebilir. Örnek: `192.168.1.0/24`

## ✅ 2. Snort Kurulum Doğrulama

```bash
snort --version
```

## 🛡️ 3. ICMP Tespit Kuralı Ekleme

```bash
sudo nano /etc/snort/rules/local.rules
```

```snort
alert icmp any any -> $HOME_NET any (msg:"ICMP paketi yakalandi!"; sid:1000001;)
```

## 🧪 4. Snort Yapılandırmasını Test Etme

```bash
sudo snort -T -c /etc/snort/snort.conf
```

## 📺 5. Snort'u Dinleme Modunda Başlatma

```bash
ip a
sudo snort -A console -q -c /etc/snort/snort.conf -i enp0s8
```

## 💥 6. ICMP Trafiği Testi

```bash
ping 192.168.x.x
```

---

## 🛡️ 7. ClamAV Antivirüs Kurulumu

```bash
sudo apt update
sudo apt install clamav clamav-daemon
```

```bash
clamscan --version
```

## 🧪 8. EICAR Test Dosyası Oluşturma

```bash
echo 'X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*' > eicar_test.txt
```

## 🦠 9. ClamAV ile Virüs Tespiti

```bash
clamscan eicar_test.txt
```

## 📓 10. Snort Loglama Özelliğini Test Etme (Opsiyonel)

Snort’un oluşturduğu uyarıları bir log dosyasına yazmasını sağlamak için `snort.conf` dosyasının içindeki `output` ayarları yapılandırılmış olmalıdır. Ancak basit bir örnek için, uyarı loglarını varsayılan log klasörüne yönlendirebilirsiniz.

### 🧩 local.rules Güncellemesi:

```snort
alert icmp any any -> $HOME_NET any (msg:"ICMP paketi yakalandi!"; sid:1000001; logto:"icmp_log.txt";)
```

> Bu kural, tespit edilen ICMP paketlerini `/var/log/snort/icmp_log.txt` dosyasına kaydeder.

### 🧪 Log Kaydının Doğrulanması:

Snort'u aşağıdaki gibi başlattıktan sonra:

```bash
sudo snort -c /etc/snort/snort.conf -i enp0s8
```

Sonrasında `ping` işlemi yapıldığında:

```bash
ping 192.168.x.x
```

Log dosyasını kontrol edin:

```bash
sudo cat /var/log/snort/icmp_log.txt
```

## 👤 Hazırlayan

Yiğit Yücel – 2025  
Cybersecurity Student | Blue Team

</details>

---

<details>
<summary><strong>EN English Documentation</strong></summary>

---

## 📦 1. Install Snort

```bash
sudo apt-get update
sudo apt-get install -y snort
```

> During installation, you'll be asked to define `HOME_NET` range. Example: `192.168.1.0/24`

## ✅ 2. Verify Installation

```bash
snort --version
```

## 🛡️ 3. Add ICMP Detection Rule

```bash
sudo nano /etc/snort/rules/local.rules
```

```snort
alert icmp any any -> $HOME_NET any (msg:"ICMP packet captured!"; sid:1000001;)
```

## 🧪 4. Test Snort Configuration

```bash
sudo snort -T -c /etc/snort/snort.conf
```

## 📺 5. Run Snort in Listening Mode

```bash
ip a
sudo snort -A console -q -c /etc/snort/snort.conf -i enp0s8
```

## 💥 6. ICMP Traffic Test

```bash
ping 192.168.x.x
```

---

## 🛡️ 7. Install ClamAV Antivirus

```bash
sudo apt update
sudo apt install clamav clamav-daemon
```

```bash
clamscan --version
```

## 🧪 8. Create EICAR Test File

```bash
echo 'X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*' > eicar_test.txt
```

## 🦠 9. Scan with ClamAV

```bash
clamscan eicar_test.txt
```

## 📓 10. Testing Snort Logging Feature (Optional)

To have Snort write alerts to a log file, make sure the `output` settings are configured in `snort.conf`. However, for simplicity, you can use a rule that writes directly to a file.

### 🧩 Update local.rules:

```snort
alert icmp any any -> $HOME_NET any (msg:"ICMP packet captured!"; sid:1000001; logto:"icmp_log.txt";)
```

> This rule will write detected ICMP packets to `/var/log/snort/icmp_log.txt`.

### 🧪 Verify Log Output:

After running Snort:

```bash
sudo snort -c /etc/snort/snort.conf -i enp0s8
```

Then run:

```bash
ping 192.168.x.x
```

Check the log file:

```bash
sudo cat /var/log/snort/icmp_log.txt
```

## 👤 Author

Yiğit Yücel – 2025  
Cybersecurity Student | Blue Team

</details>
