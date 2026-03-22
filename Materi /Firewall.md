# Firewall & NGFW - Next Generation Firewall

### Fundamental Konsep Security

Dalam sistem jaringan, ada tiga komponen utama:

- Server → tempat data (ibarat rumah)
- Network → jalur komunikasi (ibarat jalan)
- Traffic Data → kendaraan yang lewat

Karena ada data yang lewat, pasti ada risiko:

- pencurian data
- akses ilegal
- serangan dari attacker

Maka dibutuhkan mekanisme keamanan berlapis:

- Firewall → penjaga gerbang
- IDS → sistem monitoring (CCTV)
- IPS → penjaga aktif (bisa nangkep)
- EDR → monitor endpoint
- SIEM → pusat kontrol

<br/>

# Firewall

Firewall adalah sistem keamanan yang bertugas mengontrol lalu lintas jaringan berdasarkan aturan (rules).

Fungsi utama:

- Mengizinkan traffic yang valid
- Memblokir traffic yang mencurigakan

Cara kerja dasar:

1. Cek IP address
2. Cek port (contoh: 80 = HTTP, 22 = SSH)
3. Cocokkan dengan rule
4. Allow atau Deny

Contoh rule:

- Allow HTTP (port 80)
- Allow HTTPS (port 443)
- Block semua selain itu

### Evolusi Firewall

| No | Jenis Firewall              | Cara Kerja                                                                 | Kelebihan                  | Kekurangan                    | Analogi                                                                 |
|----|-----------------------------|---------------------------------------------------------------------------|----------------------------|--------------------------------|-------------------------------------------------------------------------|
| 1  | Packet Filtering Firewall   | Hanya melihat IP address dan port                                         | Cepat, ringan              | Tidak tahu isi data            | Satpam cuma cek KTP di gerbang, gak peduli kamu bawa apa                |
| 2  | Stateful Firewall           | Melacak koneksi (request & response)                                      | Lebih pintar dari basic    | Masih terbatas (tidak lihat isi)| Satpam yang ingat siapa masuk & keluar, jadi gak gampang ketipu         |
| 3  | Proxy Firewall              | Bertindak sebagai perantara, inspeksi traffic lebih dalam                  | Lebih aman                 | Lebih lambat                   | Semua orang harus lewat pos satpam dulu, tas dibongkar & diperiksa      |
| 4  | NGFW (Next Gen Firewall)    | Analisis IP, port, aplikasi, user, dan konten (deep inspection)            | Sangat lengkap & cerdas    | Lebih kompleks & mahal         | Satpam + intel + polisi: tahu siapa kamu, ngapain, bawa apa, dan niatnya |


#### Problem Security Modern 
Dulu:

- Malware sederhana
- Mudah dideteksi

Sekarang:

- HTTPS (traffic terenkripsi)
- Zero-day attack
- Fileless attack (PowerShell)
- Advanced Persistent Threat (APT)

Masalahnya:

Firewall lama tidak bisa melihat isi traffic terenkripsi.

<br/>

# IDS - Intrusion Detection System & IPS - Intrusion Prevention System

#### IDS (Intrusion Detection System)

- Passive
- Hanya mendeteksi
- Tidak menghentikan serangan
    
Analogi: CCTV → cuma lihat & kasih alert

#### IPS (Intrusion Prevention System)

- Active
- Bisa blok serangan

Analogi:Satpam → bisa langsung tangkap

**Catatan:**

IPS lebih powerful tapi rawan false positive

<br/>

# Security Layer (Defense in Depth)

Konsep utama:
**Tidak mengandalkan satu sistem saja**

Layer: <br/> **Firewall → IDS → IPS → EDR → SIEM**

Tujuan:
**Kalau satu layer gagal, layer lain masih backup**

Realita:
**Attacker tidak masuk dari satu pintu saja**


<br/>

# NGFW - Next Generation Firewall 

**Defenisi:** 

Firewall modern yang mampu:
- memahami aplikasi
- mengenali user
- melakukan inspeksi mendalam

### Komponen Utama NGFW

| No | Komponen            | Fungsi Utama                                      | Penjelasan Singkat                                                | Contoh / Use Case                                  |
|----|---------------------|---------------------------------------------------|-------------------------------------------------------------------|---------------------------------------------------|
| 1  | DPI (Deep Packet Inspection) | Inspeksi isi packet secara mendalam              | Tidak cuma header, tapi baca payload/data di dalam packet         | Deteksi malware tersembunyi dalam traffic          |
| 2  | App-ID              | Identifikasi aplikasi                             | Bisa mengenali aplikasi walaupun pakai port tidak umum            | YouTube tetap terdeteksi meskipun pakai port lain  |
| 3  | User-ID             | Identifikasi berdasarkan user                     | Policy dibuat berdasarkan user, bukan IP                          | Admin boleh akses server, user biasa dibatasi      |
| 4  | SSL Inspection      | Membuka traffic terenkripsi (HTTPS)               | Dekripsi traffic untuk melihat isi serangan                       | Deteksi malware dalam HTTPS traffic                |
| 5  | Threat Prevention   | Mencegah serangan secara aktif                    | Gabungan IDS, IPS, Antivirus, Anti-malware                        | Auto block exploit, ransomware, brute force attack |


### Perbandingan Firewall Lama vs NGFW

| Aspek              | Firewall Lama                      | NGFW (Next Gen Firewall)                          |
|--------------------|-----------------------------------|--------------------------------------------------|
| Basis Analisis     | IP address & port                 | User, aplikasi, konten                           |
| Visibility         | Terbatas (tidak lihat isi data)   | Mendalam (deep inspection)                       |
| Awareness          | Tidak tahu aplikasi               | Application-aware                                |
| Identitas          | Berdasarkan IP                    | Berdasarkan user                                 |
| Policy             | Static rules                      | Dynamic & context-aware                          |
| Kemampuan Deteksi  | Terbatas                          | Bisa deteksi threat modern                       |
| Encrypted Traffic  | Tidak bisa dianalisis             | Bisa (SSL Inspection)                            |
| Security Level     | Basic                             | Advanced                                         |
| Fleksibilitas      | Rendah                            | Tinggi                                           |
| Kesimpulan         | Sempit & kaku                     | Cerdas & adaptif                                 |


### NGFW Deployment

NGFW biasa digunakan di:

- Cloud
- Internet Gateway
- Network Perimeter

<br/>

# EPP vs EDR

### EPP (Endpoint Protection Platform)

- Fokus: pencegahan
- Contoh: antivirus

Fitur:

- scan file
- block malware
- signature-based

### EDR (Endpoint Detection & Response)

- Fokus: deteksi & respon

Fitur:

- monitoring aktivitas endpoint
- deteksi perilaku mencurigakan
- forensic analysis
- response (auto/manual)

<br/>

# Metode Deteksi

### Signature-Based

- Berdasarkan pola yang sudah dikenal
- Cepat tapi terbatas

### Anomaly-Based

- Mencari perilaku tidak normal
- Bisa deteksi serangan baru

### Hybrid

- Gabungan keduanya
- Paling efektif

<br/>

# SIEM (Security Information and Event Management)

#### Fungsi:

- Mengumpulkan log
- Menganalisis
- Korelasi event
- Memberi alert
- Membantu response

### Alur Kerja SIEM:

1. Collect data
    - firewall
    - server
    - IDS/IPS
2. Normalize
    - format data disamakan
3. Correlate
    - menghubungkan event
4. Alert
    - deteksi ancaman
5. Response
    - tindakan (block, disable, dll)

### Analogi:

- Firewall = gerbang
- IDS = CCTV
- Server log = laporan warga
- SIEM = command center

<br/>


# Zero Trust Concept

Prinsip:

"Jangan percaya siapapun"

Artinya:

- semua akses harus diverifikasi
- tidak ada implicit trust

<br/>

# Prinsip Utama NGFW + Security Modern

- Verify everything
- Minimal access (least privilege)
- Assume breach (anggap sudah kena hack)
- Monitor terus menerus

### Advanced Threat

Contoh:

- APT (Advanced Persistent Threat)
- Ransomware
- Insider threat

Ciri:

- stealth (tidak terlihat)
- lama
- kompleks

<br/>

---

by: **Benaya Joshua** <br/>
email: [benaya.josua@gmail.com](mailto:benaya.josua@gmail.com)
