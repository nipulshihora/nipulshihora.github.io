---
title: 'Secure DSpace 6.3 with Let's Encrypt SSL'
date: 2025-05-28
permalink: /posts/2025/05/blog-post-4/

tags:
  - Dspace
  - SSL Certificate
  - Certbot
    
---


# Secure DSpace 6.3 with Let's Encrypt SSL

> Domain: `example.com`  
> Environment: Apache HTTP Server (Reverse Proxy) + Tomcat 9 + Ubuntu/Debian

---

## ✅ Step 1: Install Certbot

```bash
sudo apt update
sudo apt install certbot python3-certbot-apache
```

---

## ✅ Step 2: Configure Apache as Reverse Proxy

Create a new Apache site config:

```bash
sudo nano /etc/apache2/sites-available/example.com.conf
```

Paste:

```apache
<VirtualHost *:80>
    ServerName example.com

    ProxyPreserveHost On
    ProxyPass / http://localhost:8080/
    ProxyPassReverse / http://localhost:8080/

    ErrorLog ${APACHE_LOG_DIR}/dspace_error.log
    CustomLog ${APACHE_LOG_DIR}/dspace_access.log combined
</VirtualHost>
```

Enable necessary modules and site:

```bash
sudo a2enmod proxy proxy_http headers
sudo a2ensite example.com
sudo systemctl restart apache2
```

Test by visiting:  
http://example.com

---

## ✅ Step 3: Obtain SSL Certificate

Run the following to issue and configure the SSL cert:

```bash
sudo certbot --apache -d dspace.iiti.ac.in
```

Follow the interactive prompts and choose to redirect HTTP to HTTPS if prompted.

---

## ✅ Step 4: Review Apache HTTPS Configuration (Auto-added by Certbot)

```apache
<VirtualHost *:443>
    ServerName example.com

    ProxyPreserveHost On
    ProxyPass / http://localhost:8080/
    ProxyPassReverse / http://localhost:8080/

    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/example.com/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/example.com/privkey.pem

    Include /etc/letsencrypt/options-ssl-apache.conf
</VirtualHost>
```

---

## ✅ Step 5: Enable Auto-Renewal with Cron (Optional)

Certbot auto-renewal runs via systemd. To verify:

```bash
systemctl list-timers | grep certbot
```

Optional: Add a cron job manually:

```bash
sudo crontab -e
```

Add:

```cron
30 2 * * * certbot renew --quiet --post-hook "systemctl reload apache2"
```

---

## ✅ Step 6: Test Renewal

```bash
sudo certbot renew --dry-run
```

---

## ✅ Step 7: Optional - Force HTTPS Redirect

Edit Apache HTTP config if not already redirected:

```apache
<VirtualHost *:80>
    ServerName example.com
    Redirect permanent / https://example.com/
</VirtualHost>
```

Reload Apache:

```bash
sudo systemctl reload apache2
```

---

## 🎉 Result

- ✅ HTTPS active: https://example.com  
- 🔁 Apache Reverse Proxy to Tomcat 9  
- 🔒 Let's Encrypt SSL with auto-renewal

---
