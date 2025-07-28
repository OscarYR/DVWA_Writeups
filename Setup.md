# DVWA Lab Setup

> **Note**: I initially used the DVWA version bundled with Metasploitable 2 (Environment 1). However, I later switched to hosting the latest DVWA on my local Kali Linux machine (Environment 2) because:
> - The Metasploitable 2 version is outdated
> - Some components no longer work as expected
> - It lacks newer challenges and security improvements


This document explains how I set up the Damn Vulnerable Web Application (DVWA) in different environments to perform web security testing and learning exercises.

### 📚 Resources

- 🔗 [DVWA Setup Guide Link 1](https://youtu.be/GmWQ1VIjd2U)

- 🔗 [DVWA Setup Guide Link 2](https://youtu.be/dabm-7CcHaE)

- 🔗 [DVWA GitHub – DigiNinja](https://github.com/digininja/DVWA)

---

## ⚙️ Environment 1: DVWA on Metasploitable2 (Victim) & Kali Linux (Attacker)

This was my initial setup, where I used DVWA pre-installed inside **Metasploitable 2** as the vulnerable web server.

### 🧪 Tools
- **Attacker**: Kali Linux (VM)
- **Victim**: Metasploitable 2 (VM)
- **Hypervisor**: VirtualBox

### 🌐 Network
- Both VMs connected via **NAT Network** (Internal LAN)
- 172.16.50.0/24 as IPv4 Prefix (DHCP Enabled)

### 📝 Steps:
1. **Start Metasploitable2 VM**
2. Run `ifconfig` and note the IP (e.g., `172.16.50.10`)
3. **Start Kali VM**
4. Open a browser and visit:  
   `http://172.16.50.10/`
5. Click on `DVWA` from the main web interface
6. Log in using default credentials:  
   - **Username**: `admin`  
   - **Password**: `password`
7. Configure DVWA:
   - Go to *DVWA Security* settings
   - Set security level to `low`, `medium`, or `high` depending on test

---

## ⚙️ Environment 2: DVWA Hosted Locally on Kali Linux

After completing the first two challenges using DVWA inside Metasploitable 2, I decided to run DVWA locally on my Kali Linux machine for better performance, customization, and tool integration.

### 🧪 Tools
- Kali Linux
- Apache2
- PHP
- MySQL

### 🌐 Network
- Accessed via `http://localhost` or `127.0.0.1`

### 📝 Steps to Set Up:

1. **Clone DVWA**
   ```bash
   cd /var/www/html/
   sudo git clone https://github.com/digininja/DVWA.git

2. **Set Permissions**
   ```bash
   sudo chmod -R 777 DVWA

3. **Create a Copy of Config File**
   ```bash
   cd DVWA/config
   cp config.inc.php.dist config.inc.php

4. **Configurations**
   ```bash
   sudo nano config.inc.php
  > Change the following:
  > - 'db_user' - from 'dvwa' to '**admin**'
  > - 'db_password' - from 'p@ssw0rd' to '**password**'
  > - Ctrl + O (Save) & Exit


5. **Database**
   ```bash
   sudo systemctl start mysql
   mysql -u root -p
   create database dvwa;
   create user 'admin'@'127.0.0.1' identified by 'password';
   grant all privileges on dvwa.* to 'admin'@'127.0.0.1';
   exit

6. **Apache**
   ```bash
   sudo systemctl start apache2
   cd /etc/php/(latest-version)/apache2
   sudo nano php.ini
  > Search for:
  > - allow_url_fopen = On
  > - allow_url_include = On
  > - Restart Apache (sudo systemctl restart apache2)


7. **Browse to DVWA**
- Open the browser and navigate to 127.0.0.1/DVWA or localhost/DVWA
- It should load the setup.php page, click on Create/Reset Database
- login.php page is then loaded, and we can proceed to login


