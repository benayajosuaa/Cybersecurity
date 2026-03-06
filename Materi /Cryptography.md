# Cryptography

Secara garis besar materi lab itu berputar di sekitar:

1. **Encryption (Confidentiality)**
2. **Hashing (Integrity)**
3. **Digital Signature (Authentication & Non-Repudiation)**

Tiga hal ini adalah **pilar utama keamanan data modern**.

<br/>

# Cryptography Fundamentals

Cryptography adalah teknik untuk **melindungi informasi dengan transformasi matematis** sehingga hanya pihak tertentu yang bisa membaca atau memverifikasi data.

Tujuan utama cryptography ada 4:

1. **Confidentiality**
    
    Data tidak bisa dibaca orang lain.
    
    Contoh:
    
    - WhatsApp encryption
    - HTTPS
    - VPN
    
    Teknik yang dipakai: **Encryption**
    
2. **Integrity**
    
    Memastikan **data tidak berubah** selama perjalanan.
    
    Contoh:
    
    - download software
    - transfer file
    - update OS
    
    Teknik yang dipakai: **Hash functions**
    
3. **Authentication**
    
    Memastikan **siapa pengirimnya benar**.
    
    Contoh:
    
    - login
    - SSL certificate
    - SSH key
    
    Teknik yang dipakai: digital signature, certificate
    
4. **Non-repudiation**
    
    Pengirim **tidak bisa menyangkal** bahwa dia yang mengirim data.
    
    Contoh:
    
    - kontrak digital
    - email signing
    - blockchain transaction
    
    Teknik yang dipakai: digital signature
    
<br/>

# Encryption

Encryption adalah proses **mengubah plaintext menjadi ciphertext** sehingga tidak bisa dibaca tanpa kunci.

```
Plaintext → Encryption Algorithm + Key → Ciphertext
```

Untuk membaca kembali:

```
Ciphertext → Decryption + Key → Plaintext
```

Ada dua tipe utama encryption :

- Symmetric Encryption
- Asymmetric Encryption
- Hybrid Encryption

### a. Symmetric Encryption

Symmetric encryption menggunakan **satu kunci yang sama untuk encrypt dan decrypt**.

```
Encrypt(key)
Decrypt(key)
```

Contoh algoritma:

- AES
- DES
- ChaCha20

**Cara kerja**

Misalnya:

```
Plaintext: "hello"
Key: password123
```

Algorithm AES akan menghasilkan ciphertext:

```
a8f9234a92ff9239...
```

Tanpa key yang benar, data tidak bisa dibaca.

**Kelebihan symmetric encryption**

- sangat cepat
- cocok untuk file besar
- efisien CPU

**Kekurangan**

- **key distribution problem**
Bagaimana cara mengirim key secara aman?
Kalau key bocor → semua data bisa dibuka.

### b. Asymmetric Encryption

Untuk mengatasi masalah distribusi key, dibuatlah **asymmetric cryptography**.

Konsepnya menggunakan **key pair**:

```
Public Key
Private Key
```

**Public key** bisa dibagikan ke siapa saja dan **Private key** harus disimpan rahasia

**Cara kerja**

Jika seseorang ingin mengirim pesan ke kamu:

```
Encrypt → pakai public key kamu
Decrypt → pakai private key kamu
```

Jadi hanya kamu yang bisa membaca pesan.

### Contoh algoritma

- RSA
- ECC
- ElGamal

**Kelebihan symmetric encryption**

- Aman dibandingkan Symmetric Encryption

**Kekurangan**

- Sangat lambat

### Kenapa asymmetric tidak dipakai untuk semua?

Karena **sangat lambat**.

RSA bisa **100–1000x lebih lambat dari AES**.

Makanya di dunia nyata digunakan:

### c. Hybrid Encryption

```
RSA → tukar AES key
AES → encrypt data
```

Ini dipakai di:

- HTTPS
- TLS
- VPN
- SSH

**RSA**(asymmetric encryption) **hanya dipakai untuk mengirim kunci**, sedangkan **AES**(symmetric encryption) **dipakai untuk mengenkripsi data sebenarnya**

<br/>

# Hash Functions

Hash function adalah fungsi matematika yang mengubah data menjadi **fixed-length fingerprint**.

Contoh:

```
Input:
hello

Output SHA256:
2cf24dba5fb0a...
```

### Karakteristik hash function

1. Deterministic
input sama → output sama
2. Fixed length
berapa pun ukuran input, output tetap sama panjang
3. One-way
tidak bisa dibalik
4. Avalanche effect
perubahan kecil → hash berubah total.

### Contoh hash algorithm

- MD5
- SHA1
- SHA256
- SHA512
- Bcrypt
- Argon2

### Kegunaan hash

1. **Password storage**
    
    Server tidak menyimpan password asli.
    
    Yang disimpan adalah hash.
    
    ```
    password123 → hash → database
    ```
    
2. **File integrity**
    
    Contoh:
    
    ```
    ubuntu.iso
    ubuntu.iso.sha256
    ```
    
    Kalau hash cocok → file tidak rusak.
    
<br/>

# Password Hashing & Cracking

Jika password lemah, attacker bisa melakukan:

### Dictionary attack

menggunakan daftar password umum.

Tools:

- John the Ripper

yang juga dipakai di lab

### Rainbow table attack

Database hash yang sudah diprecompute.

Solusi:

**salt**

```
hash(password + salt)
```

<br/>

# Digital Signatures

Digital signature digunakan untuk:

- authenticity
- integrity
- non-repudiation

### Cara kerja digital signature

1. buat hash dari dokumen
    
    ```
    hash = SHA256(document)
    ```
    
2. hash ditandatangani dengan **private key**
    
    ```
    signature = encrypt(hash, private key)
    ```
    
3. penerima memverifikasi menggunakan **public key**
    
    ```
    decrypt(signature, public key)
    ```
    
4. jika hash cocok → signature valid

### Implementasi

- software update
- SSL certificate
- email signing
- blockchain

Di lab kalian akan membuat signature dengan:

```
openssl dgst -sha256 -sign
```

lalu diverifikasi

<br/>

# Public Key Infrastructure (PKI)

PKI adalah sistem untuk **mengelola identitas digital** menggunakan certificate.

Komponen:

- Certificate Authority (CA)
- public key
- private key
- certificate

### Contoh certificate

Saat membuka website HTTPS, browser memverifikasi:

- certificate valid
- trusted CA
- domain cocok

<br/>

# TLS / HTTPS (Aplikasi Real)

Ketika membuka website HTTPS, prosesnya kira-kira seperti ini:

- **Step 1 — handshake**
    
    client meminta certificate server
    
- **Step 2 — verify certificate**
    
    browser memverifikasi:
    
    - CA
    - domain
    - expiry
- **Step 3 — key exchange**
    
    RSA / ECDHE digunakan untuk membuat **session key**
    
- **Step 4 — symmetric encryption**
    
    Setelah key terbentuk:
    
    ```
    AES encrypt semua traffic
    ```
    
    Karena AES jauh lebih cepat.

<br/>

# Performance Tradeoff

Dalam cryptography selalu ada tradeoff antara:

1. Security
2. Performance
3. Usability

**Contoh:**

AES-256 lebih aman dari AES-128 → tapi lebih berat CPU.

RSA-4096 lebih aman dari RSA-2048 → tapi jauh lebih lambat.

<br/>

# Pentingnya ilmu Cryptography

Semua sistem keamanan modern bergantung pada konsep ini:

| Sistem | Crypto yang dipakai |
| --- | --- |
| HTTPS | RSA + AES |
| SSH | RSA + AES |
| VPN | AES |
| Password | Hash |
| Software Update | Digital Signature |
| Blockchain | Hash + Signature |

**Identitas :**
2026 © halobenaya.com
