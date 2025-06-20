# Self-Signed SSL Certificate for Local Development (WAMP + VirtualHost)

This repository provides a step-by-step guide and necessary configuration files for generating a **self-signed SSL certificate** for local development using **WAMP** on Windows. It helps avoid browser "Not Secure" warnings by using a **locally trusted Certificate Authority (CA)**.

---

## ✅ Features

* Self-signed certificate trusted by browsers (with SAN support)
* Works with **Apache VirtualHosts** on WAMP
* Uses custom development domains like `dev.localtest.com`
* Helps simulate production-like HTTPS environments

---

## 📦 Folder Structure

```plaintext
/ssl/
├── rootCA.key              # Private key for the Root CA
├── rootCA.crt              # Root CA certificate (install this in Windows trust store)
├── dev.localtest.com.key   # Private key for the local development domain
├── dev.localtest.com.csr   # CSR for the local development domain
├── dev.localtest.com.crt   # Signed certificate for the local development domain
├── dev.ext                 # SAN configuration for OpenSSL
/www/your_project/
└── index.php               # Example index file for testing
```

---

## ⚙️ Step-by-Step Guide

### Install OpenSSL (if not available)

Download and install OpenSSL for Windows from: [https://slproweb.com/products/Win32OpenSSL.html](https://slproweb.com/products/Win32OpenSSL.html)
Add OpenSSL to System Path:

Open **System Properties → Advanced → Environment Variables**.

Under **System variables**, find and edit Path.

Add the path to your OpenSSL bin directory (e.g., C:\Program Files\OpenSSL-Win64\bin).

Restart Command Prompt or terminal to apply changes.
---

### 1. Generate Root Certificate Authority (CA)

```bash
openssl genrsa -out rootCA.key 2048
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 3650 -out rootCA.crt
```

### 2. Generate Private Key and CSR for Local Domain

```bash
openssl genrsa -out dev.localtest.com.key 2048
openssl req -new -key dev.localtest.com.key -out dev.localtest.com.csr
```

### 3. Create SAN Configuration (`dev.ext`)

```ini
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = dev.localtest.com
IP.1 = 127.0.0.1
```

### 4. Generate SSL Certificate for Local Domain

```bash
openssl x509 -req -in dev.localtest.com.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out dev.localtest.com.crt -days 825 -sha256 -extfile dev.ext
```

### 5. Trust the Root CA Certificate (Windows)

1. Double-click `rootCA.crt`.
2. Click **Install Certificate**.
3. Select **Local Machine**.
4. Place in **Trusted Root Certification Authorities**.
5. Restart your browser.

### 6. Apache VirtualHost Configuration

```apache
<VirtualHost *:443>
    ServerName dev.localtest.com
    DocumentRoot "D:/wamp64/www/your_project"

    SSLEngine on
    SSLCertificateFile "D:/wamp64/bin/apache/apache2.4.x/conf/ssl/dev.localtest.com.crt"
    SSLCertificateKeyFile "D:/wamp64/bin/apache/apache2.4.x/conf/ssl/dev.localtest.com.key"

    <Directory "D:/wamp64/www/your_project">
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

Ensure `mod_ssl` is enabled and VirtualHosts are configured properly in Apache.

### 7. Update Windows `hosts` File

```plaintext
127.0.0.1 dev.localtest.com
```

### 8. Example PHP File (`/www/your_project/index.php`)

```php
<?php
phpinfo();
```

---

## ✅ Verification

* Restart **WAMP Apache** services.
* Visit: `https://dev.localtest.com`
* Browser should show **Secure** connection.

---

## 📃 License

MIT License

Copyright (c) 2025 \[DevAtmos]

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

---

## 📢 Contributions

Contributions are welcome! Please fork the repository and submit a pull request with your improvements.

### Contributing Guidelines

1. Fork this repository.
2. Create a new branch for your feature or fix.
3. Commit your changes clearly.
4. Open a pull request.

Examples of contributions:

* Docker configuration support
* Adaptations for XAMPP or Laravel Valet
* Automation scripts for SSL generation

---

## 📂 .gitignore Example

```gitignore
# SSL files (exclude private keys for security, include example files if needed)
/ssl/*.key
/ssl/*.csr
/ssl/*.srl
/ssl/*.ext

# WAMP and system files
*.log
Thumbs.db
.DS_Store
```

---

## 🔗 References

* [OpenSSL for Windows](https://slproweb.com/products/Win32OpenSSL.html)
* [OpenSSL Documentation](https://www.openssl.org/docs/)
* [WAMPServer](https://www.wampserver.com/en/)

---

## ✨ Author

**Dev Atmos**
[GitHub Profile](https://github.com/Dev-Atmos)

---

Want improvements or automation scripts? Open an issue or contribute!
