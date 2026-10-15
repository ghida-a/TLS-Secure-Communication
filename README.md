#  TLS Secure Communication, Handshake & Certificate Generation Lab

![Python](https://img.shields.io/badge/Language-Python-blue)
![OpenSSL](https://img.shields.io/badge/Tools-OpenSSL-orange)
![Security](https://img.shields.io/badge/Domain-Applied%20Cryptography%20%26%20TLS-green)

---

##  Executive Summary
This lab demonstrates practical hands-on experience in **Applied Cryptography, TLS/SSL Protocol implementation, and Public Key Infrastructure (PKI)**. Using Python and OpenSSL, the project covers programmatic TLS client/server handshakes, certificate validation, hostname mismatch error handling, HTTP over TLS (HTTPS) communication, and custom Certificate Authority (CA) signing with Subject Alternative Names (SAN).

---

##  Tools & Technologies Used
* **Programming Language:** Python (`ssl`, `socket`, `pprint` modules)
* **Cryptographic Tool:** OpenSSL
* **Environment:** Linux (SeedUbuntu VM) / Terminal
* **Protocols:** TLSv1.3, HTTPS, TCP, X.509 Certificates

---

##  Lab Implementation & Technical Breakdown

### Task 1: TLS Client & Handshake Analysis

#### 1. TLS Handshake & Certificate Inspection
Executed a Python TLS client script (`handshake.py`) to establish a secure TCP connection with target servers (e.g., `bank32.com`) and analyze the underlying security parameters:
* **Cipher Suite Used:** `('TLS_AES_128_GCM_SHA256', 'TLSv1.3', 128)`
* **Server Certificate Details:** Extracted X.509 certificate metadata including Issuer (`Go Daddy Secure Certificate Authority G2`), validity periods, serial numbers, and Subject Alternative Names (`DNS:bank32.com`, `DNS:www.bank32.com`).

<img width="401" height="222" alt="S88" src="https://github.com/user-attachments/assets/376fb1e7-59c2-4804-922e-ac5c5d107c19" />

<img width="457" height="210" alt="S9" src="https://github.com/user-attachments/assets/624759ed-bbd5-424e-8018-bc6f75cc32dc" />

#### 2. Hostname Verification & Exception Handling
Tested SSL certificate hostname verification behavior:
* **Mismatch Error:** When requesting a mismatched domain (`www.example2020.com`) with `check_hostname = True`, Python raised an `ssl.SSLCertVerificationError` due to a **Hostname mismatch**.
* 
* **Bypassing / Handling:** Adjusting configuration to `check_hostname = False` allowed the handshake to complete successfully for testing purposes, exposing the risks of improper validation.
* 
<img width="377" height="119" alt="Sc" src="https://github.com/user-attachments/assets/888f9c2b-f947-43ec-bd58-8ccd73823f1c" />

<img width="593" height="247" alt="Scص" src="https://github.com/user-attachments/assets/23895c58-d087-423c-abce-d08aba2bccbf" />

#### 3. Sending & Receiving Encrypted Data (HTTPS)
Sent a raw HTTP/1.0 GET request over the established encrypted TLS socket and successfully parsed the encrypted response payload and server headers from the remote domain.

<img width="552" height="141" alt="Scre0" src="https://github.com/user-attachments/assets/dcdf0678-6721-46da-bee1-5fb4efb934b0" />

<img width="389" height="263" alt="Scree" src="https://github.com/user-attachments/assets/be6df43b-7a27-42d7-9b3d-4ef3269ad2d3" />

---

### Task 2: TLS Server & Custom Certificate Authority (PKI)

#### 1. Simple TLS Server Implementation
Implemented and tested a local TLS server using Python (`client.py` / server scripts) to serve content over HTTPS, verifying browser trust and successful secure web page rendering (`Hello, world!`).

<img width="383" height="177" alt="Screenshot 2026-10-15 160312" src="https://github.com/user-attachments/assets/0d83a8c8-220b-4750-917d-4eaff0d3bf05" />


#### 2. Custom OpenSSL Configuration (`openssl.cnf`) & SAN Generation
Created a custom configuration file to generate RSA private keys and Certificate Signing Requests (CSR) supporting multiple domains under Subject Alternative Names (SAN):
```
[ req ]
prompt = no
distinguished_name = req_distinguished_name
req_extensions = req_ext

[ req_distinguished_name ]
C = US
ST = New York
L = Syracuse
O = XYZ LTD.
CN = [www.bank32.com](https://www.bank32.com)

[ req_ext ]
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = [www.bank32.com](https://www.bank32.com)
DNS.2 = [www.example.com](https://www.example.com)
DNS.3 = *.bank32.com
```
<img width="545" height="291" alt="سسم4" src="https://github.com/user-attachments/assets/59d001a5-a697-4f53-a1f1-721f1592bf59" />

---

3. CSR Generation & CA Signing Commands
Generated private keys and signed the certificate using local Certificate Authority (CA) files via OpenSSL:

# Generate Private Key and CSR
```
openssl req -newkey rsa:2048 -config openssl.cnf -batch -sha256 -keyout server.key -out server.csr
```
<img width="629" height="98" alt="Screenshot 2026-10-15 19" src="https://github.com/user-attachments/assets/c3f74928-40df-4fbd-a0a4-bef8c52160cd" />

# Sign Certificate with Local CA
```
openssl x509 -req -days 3650 -in server.csr -out server.crt -CA ca.crt -CAkey ca.key -set_serial 01 -extfile openssl.cnf -extensions req_ext
```
<img width="480" height="123" alt="Screenshot 2026-10-15160217" src="https://github.com/user-attachments/assets/8971dea7-bace-4f80-b198-42aff55241c3" />

---
### Key Takeaways & Security Insights
TLS 1.3 Benefits: Faster handshake mechanisms and robust cipher suites (like AES-128/256 GCM) ensure modern transport layer security.

Importance of Hostname Verification: Disabling check_hostname or skipping proper certificate validation opens systems up to Man-in-the-Middle (MitM) attacks.

PKI & SAN Management: Properly configuring Subject Alternative Names (SAN) in OpenSSL configurations is critical for issuing valid certificates supporting multi-domain architectures.

 uthor
Name: Ghida Alnahar
