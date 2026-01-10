# Cara Deploy ke Hostinger.com - Panduan Lengkap

## Persiapan Sebelum Deploy

### 1. Build Project

Jalankan perintah berikut untuk membuat production build:

```bash
npm run build
```

**Lokasi file hasil build:** `d:\Theta Center Indonesia  Website\dist`

## Langkah Deploy ke Hostinger

### 1. Login ke Hostinger

1. Buka [hostinger.com](https://www.hostinger.com)
2. Klik **Login** di pojok kanan atas
3. Masukkan email dan password Anda
4. Masuk ke **hPanel** (Hostinger control panel)

### 2. Akses File Manager

1. Di hPanel, cari menu **Files**
2. Klik **File Manager**
3. Akan terbuka file manager berbasis web

### 3. Upload File Website

#### Opsi A: Menggunakan File Manager

1. Di File Manager, masuk ke folder **public_html**
2. **Hapus semua file default** (index.html, dll) jika ada
3. Klik tombol **Upload Files** di toolbar atas
4. Upload **SEMUA FILE** dari folder `dist`:
   - ✅ `.htaccess` (PENTING untuk routing SPA)
   - ✅ `index.html`
   - ✅ Folder `assets/` (berisi JS, CSS, images)
   - ✅ `vite.svg` atau favicon lainnya
   - ✅ Semua file dan folder lainnya

#### Opsi B: Menggunakan FTP (Lebih Cepat untuk File Banyak)

1. Di hPanel, cari menu **Files** → **FTP Accounts**
2. Gunakan kredensial FTP yang tersedia atau buat akun FTP baru
3. Download FTP client seperti [FileZilla](https://filezilla-project.org/)
4. Connect ke FTP dengan kredensial:
   - **Host:** ftp.namadomain.com
   - **Username:** username FTP Anda
   - **Password:** password FTP
   - **Port:** 21
5. Upload semua isi folder `dist` ke `public_html`

### 4. Verifikasi File .htaccess

**SANGAT PENTING:** Pastikan file `.htaccess` ter-upload dengan benar.

Isi file `.htaccess` harus seperti ini:

```apache
# Enable HTTPS redirect
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]

# SPA routing - redirect all requests to index.html
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ /index.html [L]

# Enable compression
<IfModule mod_deflate.c>
  AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css text/javascript application/javascript application/x-javascript
</IfModule>

# Cache control
<IfModule mod_expires.c>
  ExpiresActive On
  ExpiresByType image/jpg "access plus 1 year"
  ExpiresByType image/jpeg "access plus 1 year"
  ExpiresByType image/gif "access plus 1 year"
  ExpiresByType image/png "access plus 1 year"
  ExpiresByType text/css "access plus 1 month"
  ExpiresByType application/javascript "access plus 1 month"
</IfModule>
```

> **Catatan:** Jika file `.htaccess` tidak terlihat di File Manager, aktifkan "Show Hidden Files" di settings File Manager.

### 5. Konfigurasi Domain

#### Jika Domain Dibeli di Hostinger

Domain otomatis terhubung ke hosting. Langsung lanjut ke step 6.

#### Jika Domain Dibeli di Registrar Lain

Ganti **Name Server** di registrar domain Anda menjadi:

```
ns1.dns-parking.com
ns2.dns-parking.com
```

Atau gunakan **Custom DNS Records** (A Record):

- Login ke registrar domain
- Tambahkan A Record dengan IP address hosting Hostinger Anda
- IP dapat ditemukan di hPanel → **Hosting** → klik domain → **Server Information**

**Waktu Propagasi:** 2-48 jam (biasanya 4-6 jam)

### 6. Aktifkan SSL (HTTPS)

1. Di hPanel, masuk ke menu **Security** → **SSL**
2. Pilih domain Anda
3. Klik **Install SSL**
4. Hostinger akan otomatis install **Let's Encrypt SSL** (gratis)
5. Tunggu 5-10 menit untuk aktivasi
6. SSL akan diperbaharui otomatis setiap 3 bulan

### 7. Test Website

1. Buka browser dan akses:
   - `https://namadomain.com`
   - `https://www.namadomain.com`
2. Test semua halaman:
   - ✅ Homepage (/)
   - ✅ Penawaran (/penawaran)
   - ✅ Hipnoterapi (/hipnoterapi)
   - ✅ Hypnotheraphy (/Hypnotheraphy)
   - ✅ Spiritual Building (/spiritual-building)
   - ✅ Self Healing (/self-healing)
   - ✅ Training Motivation (/training-motivation)
   - ✅ Riwayat (/riwayat)
   - ✅ Mobile App (/mobile-app)
3. Pastikan routing bekerja (tidak ada error 404)
4. Test di berbagai device (desktop, tablet, mobile)

## Struktur File yang Di-Upload

```
public_html/
├── .htaccess           # Routing & security config
├── index.html          # Entry point
├── vite.svg           # Favicon
└── assets/
    ├── index-[hash].js     # Main JavaScript bundle
    ├── index-[hash].css    # Main CSS bundle
    └── [images]            # Optimized images
```

## Optimisasi Tambahan (Opsional)

### 1. Enable Cloudflare (Gratis)

1. Di hPanel → **Advanced** → **Cloudflare**
2. Klik **Enable Cloudflare**
3. Manfaat: CDN gratis, DDoS protection, caching

### 2. Setup Email Professional

1. Di hPanel → **Emails** → **Email Accounts**
2. Buat email: info@namadomain.com, contact@namadomain.com

### 3. Enable Auto-Backup

1. Di hPanel → **Files** → **Backups**
2. Aktifkan automatic weekly backup

## Troubleshooting

### ❌ Error 404 pada Route

**Penyebab:** File `.htaccess` tidak ter-upload atau tidak aktif
**Solusi:**

- Pastikan `.htaccess` ada di folder `public_html`
- Check settings File Manager, aktifkan "Show Hidden Files"
- Pastikan mod_rewrite aktif (biasanya sudah default di Hostinger)

### ❌ Domain Belum Aktif

**Penyebab:** DNS belum propagasi
**Solusi:**

- Tunggu 4-48 jam
- Check DNS propagation: [whatsmydns.net](https://www.whatsmydns.net)
- Flush DNS cache di komputer: `ipconfig /flushdns` (Windows) atau `sudo dscacheutil -flushcache` (Mac)

### ❌ SSL Tidak Aktif / Mixed Content Warning

**Penyebab:** SSL belum ter-install atau ada resource HTTP
**Solusi:**

- Reinstall SSL dari hPanel
- Pastikan semua link internal menggunakan relative path (bukan absolute http://)
- Check console browser untuk mixed content errors

### ❌ Website Lambat Loading

**Solusi:**

- Aktifkan Cloudflare CDN
- Enable browser caching (sudah ada di `.htaccess`)
- Compress images sebelum upload
- Gunakan Hostinger Premium/Business untuk performa lebih baik

### ❌ File Upload Gagal

**Solusi:**

- Gunakan FTP jika file besar (lebih dari 100MB total)
- Compress folder `dist` menjadi .zip, upload, lalu extract di File Manager
- Pastikan quota storage hosting belum penuh

## Update Website

Untuk update website di masa mendatang:

1. Lakukan perubahan di local
2. Test dengan `npm run dev`
3. Build ulang: `npm run build`
4. Backup file lama di hosting (download folder `public_html`)
5. Upload file baru dari `dist` (overwrite yang lama)
6. Test website live

## Kontak Support

**Hostinger Support:**

- Live Chat: 24/7 di hPanel
- Email: support@hostinger.com
- Knowledge Base: [support.hostinger.com](https://support.hostinger.com)

---

## Checklist Deployment ✅

Gunakan checklist ini untuk memastikan semua langkah sudah dilakukan:

- [ ] Build project (`npm run build`)
- [ ] Login ke Hostinger hPanel
- [ ] Upload semua file dari `dist` ke `public_html`
- [ ] Verifikasi `.htaccess` ter-upload
- [ ] Konfigurasi domain/DNS
- [ ] Install SSL certificate
- [ ] Test semua route/halaman
- [ ] Test di berbagai device
- [ ] Aktifkan Cloudflare (opsional)
- [ ] Setup backup otomatis

**Selamat! Website Anda sudah live di Hostinger! 🎉**
