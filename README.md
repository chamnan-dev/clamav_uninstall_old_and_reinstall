# 🛡️ ClamAV Full Reset, Reinstall & Auto Scan Script (Ubuntu)

This script will:
1. Stop and remove any old ClamAV installation  
2. Clean all leftover files, logs, and configs  
3. Reinstall ClamAV + update virus database  
4. Enable services on boot  
5. Automatically run a scan on `/home`  

---

## 📜 Script

```bash
#!/bin/bash
# ClamAV Full Reset, Reinstall & Auto Scan Script for Ubuntu
# Author: Chamnan

echo "🛑 Stopping ClamAV services..."
sudo systemctl stop clamav-freshclam 2>/dev/null
sudo systemctl stop clamav-daemon 2>/dev/null

echo "🧹 Removing old ClamAV packages..."
sudo apt-get purge --auto-remove -y clamav clamav-daemon clamav-freshclam clamav-base clamav-docs clamav-testfiles libclamav*

echo "🗑️ Cleaning leftover files..."
sudo rm -rf /var/log/clamav /var/lib/clamav /etc/clamav

echo "🧽 Autoremove & Autoclean..."
sudo apt-get autoremove -y
sudo apt-get autoclean -y

echo "📦 Reinstalling ClamAV..."
sudo apt-get update
sudo apt-get install -y clamav clamav-daemon

echo "🔄 Updating virus database..."
sudo systemctl stop clamav-freshclam
sudo freshclam
sudo systemctl start clamav-freshclam

echo "⚙️ Enabling services at boot..."
sudo systemctl enable clamav-freshclam
sudo systemctl enable clamav-daemon

echo "🧪 Running automatic scan on /home ..."
clamscan -r --bell -i /home

echo "✅ ClamAV reinstallation and scan complete!"
```
## ▶️ Usage
```bash
nano reinstall_clamav.sh      # Create script file
chmod +x reinstall_clamav.sh  # Make it executable
./reinstall_clamav.sh         # Run it

```

## 📝 Notes

- `--bell` → plays sound if malware is found

- `-i ` → shows only infected files (hides clean ones)

- Change `/home` → to `/var/www` or `/` to scan other paths

- To save results into a log file, modify last line like this:

```bash
clamscan -r --bell -i /home | tee /var/log/clamav/scan_results.log

```

