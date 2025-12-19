# Cara Deploy ke Biznet Gio - Ringkasan Cepat

## File yang Perlu Di-Upload

**Lokasi file:** `d:\Theta Center Indonesia  Website\dist`

Upload **SEMUA ISI** folder `dist` ke folder `public_html` di hosting Biznet Gio.

## Langkah Singkat

### 1. Login ke Biznet Gio

- Buka portal.biznetgio.com
- Login dengan akun Anda

### 2. Upload File

- Buka **File Manager**
- Masuk ke folder `public_html`
- Upload semua file dari folder `dist`:
  - `.htaccess` ✅ (PENTING untuk routing)
  - `index.html`
  - `assets/` (folder)
  - Semua file lainnya

### 3. Konfigurasi Domain

**Ganti Name Server di registrar domain:**

```
ns1.biznetgio.com
ns2.biznetgio.com
```

### 4. Aktifkan SSL

- Di control panel Biznet Gio
- Menu SSL/TLS → Let's Encrypt
- Install SSL untuk domain Anda

### 5. Test Website

- Buka https://namadomain.com
- Test semua halaman (hipnoterapi, self-healing, dll)

## File Penting yang Sudah Disiapkan

✅ `.htaccess` - Routing untuk SPA, HTTPS redirect, compression
✅ `index.html` - File utama dengan relative path
✅ `assets/` - JavaScript & CSS teroptimasi
✅ Semua gambar dan icon

## Troubleshooting

**Route error 404?** → Pastikan `.htaccess` ter-upload
**Domain belum aktif?** → Tunggu propagasi DNS (2-48 jam)
**SSL tidak aktif?** → Install Let's Encrypt dari control panel

---

Untuk panduan lengkap, lihat [implementation_plan.md](file:///C:/Users/ASUS%20TUF%20GAMING/.gemini/antigravity/brain/fb1355e4-dda5-4015-919f-fe4b9a72d33f/implementation_plan.md)
