# ANALISIS RESPONSI INFRASTRUKTUR TI

## Identitas

Nama : Nayla Zazki Kirani
NIM  : (isi NIM kamu)

---

## Permasalahan yang Ditemukan

### 1. Konfigurasi Nginx Salah

Pada file nginx.conf ditemukan beberapa kesalahan:

- Terdapat syntax markdown ```nginx yang ikut tersimpan ke dalam file konfigurasi.
- Nama host backend salah.
- Port backend tidak sesuai.

Akibatnya container nginx gagal dijalankan.

Error:

```text
unknown directive "```nginx"


Perbaikan:

events {}

http {

    upstream backend {
        server web1:80;
        server web2:80;
        server web3:80;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://backend;
        }
    }

}
2. Load Balancer Tidak Berjalan

Karena konfigurasi nginx salah, container nginx-lb selalu berhenti (Exited).

Perbaikan dilakukan dengan memperbaiki file nginx.conf kemudian rebuild container:

docker compose down
docker compose up -d --build
3. Verifikasi Sistem

Melakukan pengecekan container:

docker ps -a

Hasil:

nginx-lb   Up
web1       Up
web2       Up
web3       Up
mysql-db   Up
Hasil Akhir

Aplikasi berhasil dijalankan pada:

http://localhost:8080

Output yang muncul:

WEB SERVER 1

Seluruh container berjalan normal dan load balancer berhasil mengarahkan request ke backend web server.

Kesimpulan

Masalah utama terdapat pada konfigurasi Nginx yang menyebabkan container load balancer gagal dijalankan.

Setelah konfigurasi diperbaiki dan container dibangun ulang, sistem berhasil berjalan sesuai spesifikasi yang diminta pada README.