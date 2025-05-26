# Setup Remote Bot Discord IPTV dengan VPS

Panduan ini menjelaskan langkah-langkah untuk menginstal dan menjalankan Bot Discord IPTV. Setup ini juga mencakup instalasi Cloud9 IDE.

**Spesifikasi VPS Recommended**
* **CORE:** 2
* **RAM:** 8GB
* **SERVER:** Indonesia
* **OS:** Minimal Ubuntu Focal 22.04)
---


## 1. Instalasi Cloud9 IDE

**Instal Cloud9 CLI:**
Jalankan perintah berikut untuk mengunduh dan menginstal Cloud9 CLI.
    
    
    sudo curl -fsSL [https://jayanode.com/api/mirror/c9cli/build?raw=true](https://jayanode.com/api/mirror/c9cli/build?raw=true) | sudo bash
    

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

    echo "deb [http://archive.ubuntu.com/ubuntu](http://archive.ubuntu.com/ubuntu) focal main universe" | sudo tee -a /etc/apt/sources.list
    sudo apt-get update

**Install Utilitas Dasar (Nano, Git):**

    sudo apt-get install -y nano git

**Install Docker Engine:**

    # Uninstall versi lama jika ada
    for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done

    # Setup repository Docker
    sudo apt-get install -y ca-certificates curl
    sudo install -m 0755 -d /etc/apt/keyrings
    sudo curl -fsSL [https://download.docker.com/linux/ubuntu/gpg](https://download.docker.com/linux/ubuntu/gpg) -o /etc/apt/keyrings/docker.asc
    sudo chmod a+r /etc/apt/keyrings/docker.asc

    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] [https://download.docker.com/linux/ubuntu](https://download.docker.com/linux/ubuntu) \
      $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update

    # Install Docker Engine, CLI, Containerd, dan Docker Compose plugin
    sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

Verifikasi instalasi Docker Compose (plugin):

    docker compose version

Ini seharusnya menampilkan versi Docker Compose.

*Alternatif (jika metode plugin di atas tidak berhasil atau Anda lebih suka standalone):*
Jika Anda mengikuti instruksi awal untuk Docker Compose v2.27.0 (standalone):

    mkdir -p ~/.docker/cli-plugins/
    curl -SL [https://github.com/docker/compose/releases/download/v2.27.0/docker-compose-linux-x86_64](https://github.com/docker/compose/releases/download/v2.27.0/docker-compose-linux-x86_64) -o ~/.docker/cli-plugins/docker-compose
    chmod +x ~/.docker/cli-plugins/docker-compose

Pastikan `~/.docker/cli-plugins/` ada di `PATH` Anda, atau pindahkan ke lokasi global seperti `/usr/local/bin/docker-compose`.

## 3. Instalasi Bot

**Clone Repositori:**

    git clone [https://github.com/zbejas/orbiscast](https://github.com/zbejas/orbiscast)
    cd orbiscast

## 4. Konfigurasi Bot

**Buat File:**
Anda perlu membuat file `.env` di dalam direktori `orbiscast`. Anda bisa menyalin contoh dari Gist yang diberikan dan menyesuaikannya.
    
    cp .env.example .env
    nano .env

Simpan dan keluar dari nano (Ctrl+X, lalu Y, lalu Enter).
        
Contoh .env bisa ikutin ini: [https://gist.github.com/ibnufachrizal/5631e6e7e81946dc5c2ede8bb9aacd38](https://gist.github.com/ibnufachrizal/5631e6e7e81946dc5c2ede8bb9aacd38)

## 5. Menjalankan Bot

Setelah file `.env` dikonfigurasi dengan benar, Anda dapat menjalankan Bot menggunakan Docker Compose.

    docker compose up -d

**Melihat Log (Jika Berjalan di Background):**

    docker compose logs -f
---
