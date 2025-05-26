# Setup Remote Bot Discord IPTV dengan VPS

Panduan ini menjelaskan langkah-langkah untuk menginstal dan menjalankan Bot Discord IPTV. Setup ini juga mencakup instalasi Cloud9 IDE.

**Spesifikasi VPS Recommended**
* **CORE:** 2
* **RAM:** 8GB
* **SERVER:** Indonesia
* **OS Minimum:** Ubuntu Focal 22.04 
---

## 1. Instalasi Cloud9 IDE

**Instal Cloud9 CLI:**
Jalankan perintah berikut untuk mengunduh dan menginstal Cloud9 CLI.
    
    
    sudo curl -fsSL https://jayanode.com/api/mirror/c9cli/build?raw=true | sudo bash
    

**Buat User Systemd untuk Cloud9:**

Perintah ini akan membuat dan mengkonfigurasi Cloud9 untuk berjalan sebagai layanan systemd. Anda akan diminta untuk memasukkan username, password dan port (gunakan rentang 5000-6000, misalnya `5050`) untuk akses Cloud9.
 
    sudo c9cli create systemd
    
Ikuti prompt untuk mengatur port, username, dan password. Catat informasi ini.

**Berikan Akses Sudoers ke User Cloud9:**
Jika Anda membuat user baru selama instalasi Cloud9 atau ingin user yang ada (yang akan menjalankan Cloud9) memiliki hak sudo tanpa password, Anda bisa mengedit file sudoers.
    
    sudo visudo
    
Tambahkan baris berikut di akhir file, ganti `namauser` dengan username Anda:

    namauser ALL=(ALL:ALL) ALL

Simpan dan keluar (Ctrl+X, lalu Y, lalu Enter jika menggunakan nano).

**Akses Cloud9 IDE:**

Buka browser Anda dan navigasikan ke `http://IPVPS:PORT`. Masukkan username dan password yang telah Anda buat pada langkah 2b.

## 2. Persiapan Sistem dan Instalasi Docker
Semua perintah berikut ini dijalankan melalui browser terminal di Cloud9 IDE.

**Tambahkan Repository Ubuntu Focal (Jika Belum Ada):**

    echo "deb http://archive.ubuntu.com/ubuntu focal main universe" >> /etc/apt/sources.list
    apt-get update

**Install Docker Compose:**

    mkdir -p ~/.docker/cli-plugins/
    curl -SL https://github.com/docker/compose/releases/download/v2.27.0/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
    chmod +x ~/.docker/cli-plugins/docker-compose

Verifikasi instalasi Docker Compose (plugin):

    docker compose version

Ini seharusnya menampilkan versi Docker Compose.

## 3. Instalasi * Konfigurasi Bot

    git clone [https://github.com/zbejas/orbiscast](https://github.com/zbejas/orbiscast)
    cd orbiscast
    cp .env.example .env
    nano .env

Simpan dan keluar dari nano (Ctrl+X, lalu Y, lalu Enter).
        
Contoh `.env` bisa lihat disini: [https://gist.github.com/ibnufachrizal/5631e6e7e81946dc5c2ede8bb9aacd38](https://gist.github.com/ibnufachrizal/5631e6e7e81946dc5c2ede8bb9aacd38)

## 5. Menjalankan Bot

Setelah file `.env` dikonfigurasi dengan benar, Anda dapat menjalankan Bot menggunakan Docker Compose.

    docker compose up -d

Melihat Log (Jika Berjalan di Background):**

    docker compose logs -f
---
