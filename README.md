# Self-Signed SSL Certificate for Local Development (WAMP + VirtualHost)

This repository provides a step-by-step guide for generating a **self-signed SSL certificate** for local development using **WAMP** on Windows. It helps avoid browser \"Not Secure\" warnings by using a **locally trusted Certificate Authority (CA)**.

## ✅ Features
- Self-signed certificate trusted by browsers (with SAN support)
- Works with **Apache VirtualHosts** on WAMP
- Uses custom development domains like `dev.localtest.com`
- Helps simulate production-like HTTPS environments

## 📦 Folder Structure
/ssl/
├── rootCA.key # Private key for the Root CA
├── rootCA.crt # Root CA certificate (install this in Windows trust store)
├── dev.localtest.com.key # Private key for the local development domain
├── dev.localtest.com.csr # CSR for the local development domain
├── dev.localtest.com.crt # Signed certificate for the local development domain
├── dev.ext # SAN configuration for OpenSSL
/www/your_project/
└── index.php # Example index file for testing


MIT License

Copyright (c) 2025 [DevAtmos]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the \"Software\"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED \"AS IS\", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
