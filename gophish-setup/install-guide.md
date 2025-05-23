# 🚀 GoPhish + MailHog Install Guide

This guide walks you through installing and running GoPhish and MailHog locally for use with the Phish Email Lab.

---

## 🔧 Prerequisites

* OS: Linux (Kali recommended) or macOS
* Tools:

  * [GoPhish](https://getgophish.com/)
  * [MailHog](https://github.com/mailhog/MailHog)
  * Terminal access

---

## 💻 Step 1: Download and Set Up GoPhish

### 1. Download GoPhish

```bash
curl -LO https://github.com/gophish/gophish/releases/download/v0.12.1/gophish-v0.12.1-linux-64bit.zip
```

### 2. Unzip and move it to a safe directory

```bash
unzip gophish-*.zip
mkdir -p ~/phishing-lab/gophish
mv gophish-v0.12.1-linux-64bit/* ~/phishing-lab/gophish
cd ~/phishing-lab/gophish
chmod +x gophish
```

### 3. Configure GoPhish to use HTTP (not HTTPS)

Edit `config.json` and change the `admin_server` block:

```json
"admin_server": {
  "listen_url": "127.0.0.1:8080",
  "use_tls": false
},
"phish_server": {
  "listen_url": "0.0.0.0:8081"
}
```

### 4. Run GoPhish

```bash
./gophish --config config.json
```

Access the admin UI at: [http://127.0.0.1:8080](http://127.0.0.1:8080)

---

## 🛎️ Step 2: Install and Run MailHog

### A. Download MailHog Binary (No Docker Required)

```bash
wget https://github.com/mailhog/MailHog/releases/download/v1.0.1/MailHog_linux_amd64
mv MailHog_linux_amd64 mailhog
chmod +x mailhog
```

### B. Run MailHog

```bash
./mailhog
```

MailHog Web UI: [http://localhost:8025](http://localhost:8025)
SMTP Listening: `localhost:1025`

---

## 📊 Step 3: Configure GoPhish SMTP Profile for MailHog

In GoPhish Admin:

* Go to **Sending Profiles** → New Profile
* Use:

  * **Host**: `localhost:1025`
  * **From Address**: `phisher@fakecorp.com`
  * **Use TLS**: Unchecked
  * **Username / Password**: leave blank

---

## 📄 Step 4: Load Templates and Launch a Campaign

1. Upload an email template from `templates/`
2. Create a landing page from `landing-pages/`
3. Add a fake target group (e.g. `lucas@fakecorp.com`)
4. Set `http://localhost:8081` as the campaign URL
5. Launch and track results in GoPhish and MailHog

---

## 🔒 Notes

* This setup is fully local and safe
* No real emails are sent
* For educational and authorized testing only
