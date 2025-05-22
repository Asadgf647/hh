# 🌐 Apache HTTP Server: Beautifully Simple Installation & Hosting Guide

Welcome! This guide shows you two clear ways to install and run **Apache HTTP Server** on **Ubuntu/Debian Linux**:

### You’ll Learn:
- ✅ Easy system-wide install  
- ✅ User-level custom build (no root access!)  
- ✅ Custom port setup  
- ✅ Auto-start with `systemd`  
- ✅ Hosting your own site in minutes!

---

## 📦 Option 1: Quick System-wide Apache Install (APT)

### ✅ Step 1: Install Apache

```bash
sudo apt update
```

```bash
sudo apt install apache2
```

### ▶️ Step 2: Enable & Start Apache

```bash
sudo systemctl unmask apache2
```

```bash
sudo systemctl enable apache2
```

```bash
sudo systemctl start apache2
```

### 🔍 Step 3: Check Apache Status

```bash
sudo systemctl status apache2
```

### 🛑 Optional: Stop Apache if switching to custom build

```bash
sudo systemctl stop apache2
```

---

## 🛠️ Option 2: Build & Run Apache Without Sudo (User-Level)

### ⚙️ Step 1: Install Build Tools

```bash
sudo apt update
```

```bash
sudo apt install build-essential libpcre3-dev libapr1-dev libaprutil1-dev libssl-dev
```

### 📦 Step 2: Download & Extract Apache Source

```bash
wget https://downloads.apache.org/httpd/httpd-2.4.63.tar.gz
```

```bash
tar -xvzf httpd-2.4.63.tar.gz
```

```bash
cd httpd-2.4.63
```

### 🔨 Step 3: Configure & Install Locally

```bash
./configure --prefix=$HOME/apache --enable-so
```

```bash
make
```

```bash
make install
```

---

## ⚙️ Customizing Apache Config

### ✏️ Edit Config File

```bash
nano ~/apache/conf/httpd.conf
```

Modify or add:

```
Listen 8081
ServerName localhost
```

> This sets Apache to listen on port `8081` and use `localhost`.

---

## ▶️ Start/Stop Apache (User Build)

### Start:

```bash
~/apache/bin/httpd -k start -f ~/apache/conf/httpd.conf
```

### Stop:

```bash
~/apache/bin/httpd -k stop -f ~/apache/conf/httpd.conf
```

### Restart:

```bash
~/apache/bin/httpd -k restart -f ~/apache/conf/httpd.conf
```

---

## 🧾 Hosting Your Web Page

Apache serves content from:

```bash
~/apache/htdocs/
```

Create a simple web page:

```bash
echo "<h1>✅ Hello from Apache on port 8081!</h1>" > ~/apache/htdocs/index.html
```

Then visit:

```
http://<your-server-ip>:8081
```

To find your IP:

```bash
hostname -I
```

---

## 🔍 Check if Apache is Running

```bash
ps aux | grep httpd
```

```bash
ss -tuln | grep 8081
```

---

## 🔁 Auto-Start Apache at Boot (User-Level systemd)

### 🧱 Step 1: Create systemd service file

```bash
mkdir -p ~/.config/systemd/user
```

```bash
nano ~/.config/systemd/user/apache.service
```

Paste the following content:

```
[Unit]
Description=Apache HTTP Server
After=network.target

[Service]
ExecStart=/home/YOUR_USERNAME/apache/bin/httpd -f /home/YOUR_USERNAME/apache/conf/httpd.conf
WorkingDirectory=/home/YOUR_USERNAME/apache
User=YOUR_USERNAME
Group=YOUR_USERNAME

[Install]
WantedBy=default.target
```

> Replace `YOUR_USERNAME` with your actual Linux username (you can check it with `whoami`).

---

### ✅ Step 2: Enable & Start Apache with systemd (User-Level)

```bash
systemctl --user daemon-reexec
```

```bash
systemctl --user enable apache.service
```

```bash
systemctl --user start apache.service
```

---

### ❓ What These Commands Do:

- `systemctl --user` is for **user-level services only**, not system-wide ones.
- These commands will **only manage your custom-built Apache** running from `~/apache/`, not the one installed via `apt`.
- Useful when you don’t have `sudo` but still want Apache to auto-start with your user session.

---

### 🔄 Other systemctl --user Commands:

```bash
systemctl --user status apache.service
```

```bash
systemctl --user restart apache.service
```

```bash
systemctl --user stop apache.service
```

---

## 🎉 Final Touch: You’re All Set!

You now have:

- Apache installed **your way**  
- A web page hosted at `http://your-ip:PORT`  
- Auto-start at boot (if desired)  
- Full control — even without root access!

---

*Made with love for developers, learners, and self-hosters!*
