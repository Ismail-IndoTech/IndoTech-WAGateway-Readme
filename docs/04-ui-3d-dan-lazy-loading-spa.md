# Modul 04: Desain 3D Neumorphic & Instant Lazy Loading SPA

## Pengantar

Antarmuka IndoTech Super WhatsApp BOT Gateway dirancang menyerupai standar aplikasi modern **IndoTech Management**, memadukan estetika kedalaman tiga dimensi (**Modern 3D Neumorphic**) dengan performa perpindahan halaman instan berbasis **Full SPA Wire:Navigate** dan efek *loading bayangan* (*skeleton shadow*).

---

## 1. Standar Desain 3D (Tactile Depth Design)

Seluruh komponen kartu, tombol, dan formulir input mengusung gaya visual 3D yang memberikan ilusi fisik ketika berinteraksi:

### A. Kartu 3D (3D Cards)
- **Ciri Khas**: Bayangan ganda lembut di bagian bawah dan kanan (`border-b-2 border-r-2`), elevasi bayangan luar (`shadow-md shadow-slate-200/50 dark:shadow-black/40`), dan sudut membulat modern (`rounded-2xl`).
- **Efek Hover**: Mengangkat sedikit ke atas (`hover:-translate-y-0.5 hover:shadow-lg transition-all duration-200`) untuk menunjukkan fokus.

### B. Tombol 3D (3D Buttons)
- **Tombol Utama (Primary)**:
  - Menggunakan gradient warna tegas (`bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-700 hover:to-indigo-700`).
  - Ketebalan batas bawah (`border-b-4 border-blue-800 dark:border-indigo-900`).
  - **Efek Tekan Fisik**: Saat diklik (`active:translate-y-1 active:border-b-0 active:shadow-inner`), tombol bergerak turun seolah ditekan secara mekanis.
- **Tombol Sekunder (Secondary)**:
  - Permukaan terang/gelap dengan aksen batas `border-b-2 border-slate-300 dark:border-slate-700`.

### C. Input Formulir 3D (3D Inset Inputs)
- Efek lekukan ke dalam (*inset shadow*) dengan border kontras halus (`border border-slate-200 dark:border-slate-700 focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500`).

---

## 2. Instant Page Navigation & Lazy Loading (Loading Bayangan)

Aplikasi ini beroperasi sebagai **Full Single Page Application (SPA)** murni tanpa perlu *page reload* penuh browser:

### A. Livewire 3 `wire:navigate`
- Setiap perpindahan tautan di dalam aplikasi menggunakan atribut `wire:navigate`.
- Halaman berpindah secara instan dalam hitungan milidetik karena browser hanya memperbarui dokumen DOM tanpa mengunduh ulang berkas CSS/JS atau memulai ulang siklus window.

### B. Efek "Loading Bayangan" (Shadow Skeleton Placeholder)
- Ketika data halaman berikutnya membutuhkan waktu pengolahan di backend, antarmuka tidak membeku (*freeze*).
- Sistem secara instan merender **loading bayangan** (*skeleton shadow placeholder*):
  - Kotak-kotak beranimasi denyut halus (*pulse animation*) dengan proporsi yang persis menyerupai kartu statistik, baris tabel, atau formulir asli.
  - Memberikan pengalaman pengguna yang mulus tanpa kedip (*flicker-free experience*).

---

## 3. Penanganan Error Khusus SPA (Custom SPA Error Pages)

Pada aplikasi SPA tradisional, jika terjadi galat server (misal HTTP 500), browser sering kali menampilkan layar putih kosong (*blank screen*) atau peringatan modal mentah.

Pada IndoTech Super WhatsApp BOT Gateway:
1. **Pencegahan Layar Putih**: Skrip `spa-error-handler.js` menangkap seluruh *unhandled exception*, kegagalan AJAX Livewire, dan respons HTTP bukan-200.
2. **Halaman Error SPA Terintegrasi**:
   - **401 Unauthorized**: Pengalihan lembut ke login dengan sesi tersimpan.
   - **403 Forbidden**: Peringatan visual bahwa peran akun tidak memiliki hak akses, dengan tombol kembali ke dashboard.
   - **404 Not Found**: Halaman ilustratif ramah pencarian.
   - **419 Page Expired**: Dialog pembaruan token CSRF otomatis tanpa kehilangan data ketikan.
   - **429 Too Many Requests**: Tampilan waktu jeda tunggu rate limit.
   - **500 Server Error**: Halaman diagnosis terstruktur dengan tombol *Coba Lagi* dan opsi pelaporan ke SuperAdmin.
   - **503 Maintenance Mode**: Tampilan informasi pemeliharaan berkala.

---

## 4. Dukungan Mode Offline & PWA

- **Service Worker (`public/sw.js`)**: Meng-cache aset statis penting sehingga aplikasi tetap dapat terbuka meskipun jaringan internet mendadak putus.
- **Indikator Status Jaringan**: Banner indikator muncul otomatis saat status jaringan perangkat berubah menjadi `offline`, dan kembali hijau saat `online`.
- **Halaman Offline Cadangan (`public/offline.html`)**: Memandu pengguna saat mencoba mengakses rute baru tanpa sambungan internet.
- **PWA (Progressive Web App)**: Pengguna dapat menginstal aplikasi langsung ke layar utama Android, iOS, Windows, atau macOS melalui prompt tombol install native.
