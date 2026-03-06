# Setting IP Linux
<br/>

## Cek “Kartu Jaringan” di Linux

Setiap komputer punya **network interface** (kartu jaringan). Linux memberi **nama** ke setiap interface.

Command untuk melihatnya:

```bash
ip a
```

Contoh output yang sering muncul:

```
1: lo: <LOOPBACK,UP>
    inet 127.0.0.1/8

2: enp0s3: <BROADCAST,MULTICAST,UP>
    inet 192.168.1.10/24

3: wlan0: <BROADCAST,MULTICAST>
```

maksud dari beberapa Output yang keluar adalah :

1. **lo (Loopback)**
    
    ```
    1: lo
    inet 127.0.0.1
    ```
    
    **Artinya:**
    
    ```
    lo = jaringan internal komputer
    ```
    
    Dipakai ketika komputer **berkomunikasi dengan dirinya sendiri**.
    
    Contoh:
    
    ```
    localhost
    127.0.0.1
    ```
    
    Biasanya dipakai oleh:
    
    - web server lokal
    - database
    - aplikasi internal
    
    Ini **bukan jaringan internet**
    
2. **enp0s3**
    
    Contoh:
    
    ```
    2: enp0s3
    inet 192.168.1.10/24
    ```
    
    Artinya:
    
    ```
    enp0s3 = kartu LAN (ethernet)
    ```
    
    Penjelasan nama:
    
    ```
    en = ethernet
    p0 = PCI bus
    s3 = slot 3
    ```
    
    Biasanya muncul di:
    
    - VirtualBox
    - beberapa distro Linux
    
    Kalau ada IP di sini:
    
    ```
    192.168.1.10
    ```
    
    Berarti komputer kamu **terhubung ke jaringan**
    
3. **wlan0**
    
    Contoh:
    
    ```
    3: wlan0
    inet 192.168.1.20/24
    ```
    
    Artinya:
    
    ```
    wlan0 = WiFi adapter
    ```
    
    Biasanya muncul kalau kamu connect WiFi.
    

### Contoh Situasi Nyata

Misalnya dijalankan:

```bash
ip a
```

Outputnya:

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 36:f1:e1:55:4a:c4 brd ff:ff:ff:ff:ff:ff
    inet 10.26.13.2/24 brd 10.26.13.255 scope global enp0s1
       valid_lft forever preferred_lft forever
    inet 192.168.64.2/24 metric 100 brd 192.168.64.255 scope global dynamic enp0s1
       valid_lft 3401sec preferred_lft 3401sec
    inet6 fd9e:dc24:24cf:2205:34f1:e1ff:fe55:4ac4/64 scope global dynamic mngtmpaddr noprefixroute 
       valid_lft 2591937sec preferred_lft 604737sec
    inet6 fe80::34f1:e1ff:fe55:4ac4/64 scope link 
       valid_lft forever preferred_lft forever
```

Artinya:

- lo > jaringan internal
- enp0s3 > LAN / internet

IP komputer adalah:

```
192.168.1.10
```

<br/>

# Cara Mengganti IP

Misalnya kamu ingin mengganti IP menjadi:

```
192.168.1.50
```

Command:

```bash
sudo ip addr add 192.168.1.50/24 dev enp0s3
```

Artinya:

- ip addr add = menambahkan IP
- 192.168.1.50 = IP bar
- /24 = subnet
- dev enp0s3 = dipasang di interface enp0s3

### Setelah itu cek lagi

```bash
ip a
```

Outputnya akan jadi seperti ini:

```
2: enp0s3
    inet 192.168.1.10/24
    inet 192.168.1.50/24
```

Artinya sekarang komputer punya **2 IP**.

<br/>

# Cara Menghapus IP

Kalau mau hapus IP yang tadi dibuat:

```bash
sudo ip addr del 192.168.1.50/24 dev enp0s3
```

Cek lagi:

```bash
ip a
```

IP tambahan akan hilang.

<br/>

# Gateway (Pintu Keluar Internet)

Agar bisa internet, biasanya perlu gateway.

Contoh:

```
gateway = 192.168.1.1
```

Command:

```bash
sudo ip route add default via 192.168.1.1
```

Artinya:

- semua traffic keluar lewat router 192.168.1.1

<br/>

# Test Jaringan

Test internet:

```bash
ping 8.8.8.8
```

Kalau berhasil akan muncul:

```
64 bytes from 8.8.8.8: icmp_seq=1
```

Kalau tidak:

```
Network unreachable
```

<br/>

**Identitas :**
2026 © halobenaya.com
