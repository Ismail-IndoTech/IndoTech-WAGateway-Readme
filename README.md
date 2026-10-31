> 📚 **Dokumentasi:** [Indeks lengkap ID/EN](DOCS.md) · [English documentation index](DOCS.en.md)

# IndoTech Super WhatsApp BOT Gateway — Enterprise Multi-Device, AI-Integrations & RAG Knowledge Base Architecture


A high-performance, enterprise-grade **Super WhatsApp BOT Gateway** and **AI-Assistant Management Platform** built with **Laravel 12 (Full SPA)**, **Livewire 3**, **Tailwind CSS v4**, and **Node.js (Baileys Multi-Device & Socket.IO)**.

Designed for robust operation on both **Dedicated VPS** and **Shared Hosting (cPanel)** environments.

---

## Daftar Isi / Table of Contents
- [Filosofi & Identitas: Super WhatsApp BOT Gateway](#filosofi--identitas-super-whatsapp-bot-gateway)
- [Arsitektur Tri-Gateway](#arsitektur-tri-gateway-tiga-pilar-penengah)
- [Fitur Utama & Keunggulan RAG](#fitur-utama--keunggulan-rag)
- [Desain 3D Neumorphic & Instant Lazy Loading SPA](#desain-3d-neumorphic--instant-lazy-loading-spa)
- [Hak Akses Hierarkis & Shared Quota Pool](#hak-akses-hierarkis--shared-quota-pool)
- [Autentikasi & OTP WA Center Independen](#autentikasi--otp-wa-center-independen)
- [Teknologi (Tech Stack)](#teknologi-tech-stack)
- [Panduan Instalasi & Seeder](#panduan-instalasi--seeder)
- [Panduan Deployment (VPS & Shared Hosting)](#panduan-deployment-vps--shared-hosting)
- [Dokumentasi Lengkap REST API](#dokumentasi-lengkap-rest-api)
- [Modul Dokumentasi Lanjutan (Docs)](#modul-dokumentasi-lanjutan-docs)
- [Troubleshooting & Solusi](#troubleshooting--solusi)
- [Kontribusi & Donasi](#-kontribusi--donasi)
- [Lisensi](#lisensi)

---

## Filosofi & Identitas: Super WhatsApp BOT Gateway

Aplikasi ini bukan sekadar gateway pengirim pesan WhatsApp biasa (*SMS/WA blaster*), dan juga bukan sekadar bot AI generik yang terisolasi. 

Platform ini adalah **Super WhatsApp BOT Gateway**:
1. Mengintegrasikan infrastruktur konektivitas fisik **WhatsApp Gateway** (multi-device, antrean pesan, auto-heal, failover) dengan mesin kognitif cerdas **AI Integrations** (Groq, OpenAI, Google Gemini, Anthropic Claude, DeepSeek, OpenRouter, Qwen).
2. Diperkuat oleh **RAG (Retrieval-Augmented Generation)** / Knowledge Base Hub, yang menjadikan nomor WhatsApp berfungsi sebagai **Super BOT yang sangat pintar di segala sisi/bidang**:
   - Mampu menguasai bidang bisnis apa pun (Layanan Pelanggan/CS, Front Desk/Buku Tamu, E-Commerce & Penjualan, HR & Kepegawaian, Sistem Akademik Kampus, Perbankan/Keuangan, Operasional Lapangan, dll.) hanya dengan mengunggah dokumen referensi atau SOP tanpa perlu modifikasi kode program.
   - Mencegah halusinasi AI dengan menyajikan fakta terverifikasi (*grounded truth*) langsung ke pengguna WhatsApp.
3. Didukung oleh **Gateway Center** sebagai penengah aplikasi luar yang menyuplai data kontekstual *live* secara *schema-agnostic* ke dalam percakapan AI.

---

## Arsitektur Tri-Gateway (Tiga Pilar Penengah)

```
 ┌──────────────────────────────────────────────────────────────┐
 │                APLIKASI LUAR / CLIENT APPS                   │
 │       (ERP, CRM, Portal Web, LMS, Mobile App, dsb.)          │
 └──────────────┬───────────────────────────────┬───────────────┘
                │ (AI Context & Webhook)        │ (REST API & Outbound)
                ▼                               ▼
 ┌──────────────────────────────┐ ┌─────────────────────────────┐
 │       GATEWAY CENTER         │ │      WHATSAPP GATEWAY       │
 │       (Penengah AI)          │ │   (Penengah In/Out WA)      │
 │                              │ │                             │
 │ • Penengah AI-Integrations   │ │ • Penengah trafik WA luar   │
 │ • Autentikasi API Key unik   │ │ • Multi-device Baileys      │
 │ • Penyedia webhook konteks   │ │ • Dispatching & Auto-Heal   │
 │ • Metadata & skema aplikasi  │ │ • Web Chat, Broadcast, OTP  │
 └──────────────┬───────────────┘ └─────────────┬───────────────┘
                │                               │
                │     ┌───────────────────┐     │
                └────►│ SUPER BOT GATEWAY │◄────┘
                      │ (Konektor WA & AI)│
                      └─────────┬─────────┘
                                │
                                ▼
         ┌──────────────────────────────────────────────┐
         │              SUPER BOT WHATSAPP              │
         │  • Multi-Provider LLM (Groq, OpenAI, Gemini) │
         │  • Persona Dinamis mengikuti Client App      │
         │  • Konteks Aplikasi Eksternal + RAG KB       │
         │  • Human Handover & Aturan Jam Operasional   │
         │  • Group Mentions & Prefix Triggers          │
         └──────────────────────────────────────────────┘
                                │
                                ▼
         ┌──────────────────────────────────────────────┐
         │             PENGGUNA / PELANGGAN             │
         │               (WhatsApp Chat)                │
         └──────────────────────────────────────────────┘
```

### 1. WhatsApp Gateway — Penengah In/Out WA
- **Penengah Trafik WhatsApp**: Mengatur seluruh arus pesan keluar (*outbound*) dan masuk (*inbound*) dari aplikasi pihak ketiga ke WhatsApp.
- **Multi-Device Engine**: Menghubungkan banyak akun/nomor WhatsApp melalui QR Code scanner real-time (Socket.IO).
- **Nomor Pusat (WA Center)**: Penunjukan nomor utama sistem untuk mengirimkan OTP pendaftaran, verifikasi login 2FA, dan notifikasi darurat.
- **Anti-Ban & Warm-up Mode**: Pembatasan kuota bertahap dan simulasi jeda acak (3–8 detik) untuk melindungi nomor dari risiko pemblokiran.
- **Chat WhatsApp Web Interaktif**: Antarmuka web chat live dengan sinkronisasi pesan, media, dokumen, dan audio.

### 2. Gateway Center — Penengah AI & Integrasi Aplikasi Eksternal
- **Penengah AI (AI Middleware Hub)**: Menjembatani aplikasi eksternal (ERP, CRM, Portal Web, LMS) untuk memanfaatkan AI-Integrations terpusat.
- **Kredensial Unik Per Aplikasi**: Menghasilkan dan mengelola `X-API-Key` unik untuk setiap sistem klien.
- **Webhook Context Provider**: Saat Super BOT memproses pesan, Gateway Center secara otomatis memanggil endpoint webhook aplikasi klien (`/ai-assistant/context` atau `action=ai_context`) untuk menyuntikkan data *live* pengguna (saldo, status pesanan, absensi, tiket bantuan).
- **Alokasi Nomor & Smart Failover**: Memetakan aplikasi mana yang berhak memakai nomor WhatsApp tertentu dengan pengalihan otomatis jika nomor utama offline.

### 3. Super BOT Gateway — Penghubung WA Gateway & AI Integrations
- **Peleburan Sempurna**: Mengambil pesan dari WhatsApp Gateway $\rightarrow$ mengambil konteks dari Gateway Center dan dokumen RAG $\rightarrow$ memproses ke LLM pilihan $\rightarrow$ mengirim balasan cerdas kembali ke pengguna WhatsApp.
- **Multi-Provider AI Terdepan**:
  * **Groq**: Llama 3.3 70B, Mixtral 8x7B (inferensi ultra-cepat < 1 detik).
  * **OpenAI**: GPT-4o, GPT-4 Turbo, GPT-4o-mini.
  * **Google Gemini**: Gemini 2.0 Flash, Gemini 1.5 Pro.
  * **Anthropic**: Claude 3.5 Sonnet, Claude 3.5 Haiku.
  * **DeepSeek**: DeepSeek Chat V3, DeepSeek Reasoner R1.
  * **OpenRouter**: Akses ke 100+ model AI global.
  * **Qwen / Alibaba**: Qwen 2.5 72B.
- **Deteksi Model Otomatis**: Secara dinamis mengecek ketersediaan model langsung dari API Key vendor.
- **Preset Persona Adaptif**:
  * **Mode CS**: Layanan pelanggan empatik, ramah, dan solutif.
  * **Penerima Tamu**: Resepsionis dan registrasi buku tamu digital.
  * **AI Integrasi Aplikasi**: Secara dinamis mengadopsi identitas, nama, deskripsi, dan aturan sistem klien yang terdaftar di Gateway Center.
  * **Mode Kustom**: Konfigurasi bebas tanpa batasan.
- **Human Handover**: Otomatis menghentikan bot dan mengalihkan obrolan ke admin manusia jika pengguna mengetik kata kunci eskalasi (misal: *admin, operator, bantuan manusia*).
- **Jam Kerja Operasional**: Mengatur jam aktif bot dan mengirimkan pesan konfirmasi tindak lanjut di luar jam kerja.
- **Group AI Assistant**: Menjawab pesan di grup WhatsApp dengan pemicu mention (`@bot`) atau prefix (`!ai`).

---

## Fitur Utama & Keunggulan RAG

### 1. RAG (Retrieval-Augmented Generation) Knowledge Base Hub
- **Multi-Format Input**: Mendukung berkas teks, markdown, PDF, dokumen kebijakan, manual produk, dan FAQ.
- **Isolasi Dokumen per Aplikasi Klien**: Setiap aplikasi di Gateway Center memiliki repositori dokumen RAG khusus, selain dokumen umum yang berlaku untuk semua bot.
- **Algoritma Smart Keyword & Token Limiter**: Memfilter dan membatasi potongan teks yang paling relevan untuk mencegah limit token HTTP 413 pada vendor AI.
- **Anti-Halusinasi**: Jawaban yang dihasilkan bot selalu merujuk pada dokumen resmi perusahaan.

### 2. Broadcast & Kampanye Massal
- Pengiriman pesan massal berbasis antrean (*database queue*).
- Mesin **Spintax** (`{Halo|Hai|Selamat pagi}`) dan variabel dinamis (`{name}`, `{phone}`).
- Sistem **Blacklist Otomatis**: Melindungi nomor pelanggan yang meminta berhenti berlangganan (*unsubscribe*).
- Aturan Kepatuhan: Pengecekan otomatis format nomor dan jam pengiriman (menghindari waktu istirahat malam).

### 3. Monitoring & Log Lengkap
- **Dashboard Real-time**: Grafik tren pesan 7 hari terakhir, 4 kartu KPI, status kesehatan server (CPU, RAM, Disk Space).
- **Riwayat Pesan**: Filter status (*sent, delivered, read, failed*), arah pesan (*incoming/outgoing*), dan nomor perangkat.
- **Log Aktivitas & Audit Gateway**: Jejak audit lengkap setiap aksi autentikasi, perangkat, broadcast, dan akses API.

---

## Desain 3D Neumorphic & Instant Lazy Loading SPA

Aplikasi ini didesain dengan estetika modern **3D Neumorphic** (seperti standar aplikasi *IndoTech Management*) dan beroperasi sebagai **Full Single Page Application (SPA)** murni:

1. **Gaya Visual 3D Tactile**:
   - **Kartu 3D**: Menggunakan bevel shadow (`border-b-2 border-r-2`, `shadow-md shadow-slate-200/50 dark:shadow-black/40`), sudut membulat modern (`rounded-2xl`), dan efek pengangkatan halus saat di-hover.
   - **Tombol 3D (Physical Press State)**: Tombol dilengkapi bevel tebal (`border-b-4`) dengan gradient warna tegas dan efek tekan mekanis fisik (`active:translate-y-1 active:border-b-0 active:shadow-inner`).
   - **Formulir Input 3D Inset**: Bidang input dengan bayangan ke dalam (*inset shadow*) yang nyaman untuk mata dan bebas silau.
2. **Instant Lazy Loading SPA (Loading Bayangan)**:
   - Perpindahan antar halaman terjadi secara instan tanpa reload browser menggunakan Livewire 3 `wire:navigate`.
   - Jika data backend membutuhkan waktu pemrosesan, sistem secara otomatis merender **loading bayangan** (*skeleton shadow placeholder*) beranimasi denyut halus (*pulse animation*), menjaga kenyamanan navigasi tanpa kedip (*flicker-free*).
3. **SPA Custom Error Handling**:
   - Seluruh kode status HTTP (401, 403, 404, 419 Page Expired, 429 Rate Limit, 500 Server Error, 503 Maintenance) ditangani secara elegan di dalam layout SPA.
   - Tidak ada lagi layar putih (*blank page*), crash JSON mentah, atau reload halaman paksa. Pengguna diberikan pesan humanis dan tombol aksi pemulihan.
4. **Native Offline Support & PWA**:
   - Deteksi koneksi jaringan real-time via Service Worker (`public/sw.js`) dan browser event (`offline`/`online`).
   - Indikator status jaringan visual dan halaman offline khusus (`public/offline.html`).
   - Dilengkapi `manifest.webmanifest`, ikon lengkap (192x192, 512x512, maskable, apple-touch-icon 180x180), dan prompt instalasi aplikasi native (*Install App*).
5. **Sistem Tema Bebas Kedip (Zero-Flash Theme System)**:
   - Mengikuti tema sistem operasi (OS) secara default, dapat dialihkan ke Dark Mode atau Light Mode via satu tombol siklus.
6. **Dukungan Multi-Bahasa**:
   - Bahasa Indonesia (`id`) sebagai bawaan dan English (`en`), tersinkronisasi via cookie permanen `it_lang`.

---

## Hak Akses Hierarkis & Shared Quota Pool

Sistem menerapkan model hierarki 3-tingkat (**SuperAdmin**, **Admin**, **User**) untuk memastikan keamanan dan isolasi data:

1. **Batasan Akses Menu Admin**:
   - Admin **TIDAK MEMILIKI AKSES** ke menu **Pengaturan Sistem (`/settings`)**, **Activity Log (`/logs`)**, dan **Gateway Server Log (`/gateway-logs`)**. Ketiga menu ini eksklusif milik **SuperAdmin**.
   - Menu disembunyikan dari sidebar Admin dan dilindungi oleh route middleware `role:SuperAdmin` serta `abort_unless(auth()->user()?->isSuperAdmin(), 403)`.
2. **Isolasi Pesan & Notifikasi (Message Log & Top Navbar)**:
   - **SuperAdmin**: Memiliki visibilitas penuh ke seluruh pesan dan notifikasi sistem.
   - **Admin**: Hanya dapat melihat log & riwayat pesan miliknya sendiri dan akun User di bawah asuhannya (`parent_id = admin_id`). Dropdown notifikasi di navbar atas hanya menampilkan aktivitas akun Admin tersebut dan subordinatnya.
   - **User**: Hanya dapat melihat pesan dan notifikasi milik akunnya sendiri.
3. **Mekanisme Kuota Bersama (Shared Resource Pool)**:
   - Kuota pesan, limit perangkat, client apps, auto-reply, dan broadcast yang dimiliki oleh Admin berfungsi sebagai **Shared Pool** bagi Admin dan seluruh User di bawahnya.
   - Pengiriman pesan oleh User bawahan akan memotong sisa kuota User tersebut sekaligus mengakumulasi pemakaian bersama di akun Admin induknya.
   - Admin membuat pengguna baru otomatis berstatus sebagai **`User`** bawahan (`parent_id = Auth::id()`).

---

## Autentikasi & OTP WA Center Independen

1. **Pemisahan Pengaturan Registrasi & OTP Login**:
   - Status aktivasi pendaftaran mandiri publik (On/Off Registrasi) dipisahkan dari status Login OTP.
   - **Login dengan OTP akan selalu aktif** seketika setelah SuperAdmin menetapkan nomor perangkat sebagai **Nomor OTP Sistem (WA Center)**.
2. **Pendaftaran Mandiri Otomatis Berperan User**:
   - Pengguna yang mendaftar secara mandiri melalui `/register` otomatis berstatus sebagai peran **`User`** (bukan Admin).
3. **Penanganan Darurat WA Center Offline (Emergency Access Bypass)**:
   - Jika nomor WhatsApp Center sedang terputus/offline, pengguna dengan peran administratif (**SuperAdmin** dan **Admin**) diizinkan login langsung menggunakan kata sandi (*direct password fallback*) dengan peringatan kuning untuk segera menyambungkan kembali WhatsApp. Hal ini mencegah terjadinya *administrative lockout*.
   - Pengguna dengan peran **User** reguler tetap diwajibkan menunggu perangkat OTP online demi keamanan data.
4. **Progressive Lockout Rate Limiter**:
   - Penguncian berjenjang anti-brute force (Tier 1: 3x gagal = 60 detik; Tier 2: 6x gagal = 180 detik; Tier 3: 9x gagal = 300 detik) dengan countdown visual real-time di antarmuka.

---

## Teknologi (Tech Stack)

### Backend (PHP / Laravel)
- **Laravel 12.x** (PHP 8.2+)
- **Livewire 3.x** (Komponen reaktif full SPA)
- **MariaDB / MySQL** (Engine InnoDB)
- **Laravel Pint** (Standardisasi kode PSR-12)
- **PHPUnit 11** (Suite pengujian unit dan fitur komprehensif)

### Frontend & Styling
- **Tailwind CSS v4** (CSS-first engine `@theme`)
- **Alpine.js v3** (Micro-reactivity)
- **Google Material Symbols Outlined**
- **Vite** (Asset bundler dengan optimasi thread `GOMAXPROCS=1`)

### Engine Node.js WhatsApp (`server-wa/`)
- **Node.js >= 18.x**
- **@whiskeysockets/baileys** (Engine WhatsApp Multi-Device socket)
- **Express.js** & **Socket.IO v4.7.5**
- **Continuous Watchdog & Auto-Healer**

---

## Panduan Instalasi & Seeder

### 1. Persyaratan Sistem
- PHP >= 8.2 (ekstensi: `pdo_mysql`, `curl`, `mbstring`, `openssl`, `fileinfo`)
- Composer >= 2.x
- Node.js >= 18.x & npm
- MariaDB >= 10.5 atau MySQL >= 8.0

### 2. Langkah Instalasi Cepat

```bash
# 1. Masuk ke direktori proyek
cd /path/ke/WA-Gateway

# 2. Pasang dependensi PHP & Node utama
composer install
npm install

# 3. Konfigurasi file environment
cp .env.example .env
php artisan key:generate

# 4. Migrasi basis data
php artisan migrate --force
php artisan storage:link

# 5. Jalankan Seeder Akun Awal (kredensial tidak dipublikasikan)
php artisan db:seed

# 6. Pasang dependensi Node.js WhatsApp Server
cd server-wa && npm ci && cd ..

# 7. Kompilasi asset frontend
npm run build
```

### 3. Akun Awal (Kredensial Tidak Dipublikasikan)

Setelah menjalankan `php artisan db:seed`, sistem membuat akun **SuperAdmin** dan **Admin**. Email dan kata sandi **tidak dipublikasikan**; gunakan kredensial yang Anda tetapkan sendiri saat seeding.

| Role | Keterangan |
|------|------------|
| **SuperAdmin** | Akses penuh seluruh sistem, pengaturan server, user management, dan audit log. |
| **Admin** | Pengelolaan gateway, broadcast, client apps, dan Super BOT. |

---

## Panduan Deployment (VPS & Shared Hosting)

### A. Dedicated VPS (Nginx + Systemd)

#### 1. Konfigurasi Reverse Proxy Nginx
```nginx
server {
    listen 443 ssl http2;
    server_name wa.domainanda.com;
    root /var/www/wa-gateway/public;
    index index.php;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    # Proxy WebSocket & Socket.IO ke Node.js
    location /socket.io {
        proxy_pass http://127.0.0.1:3998;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
    }

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```

#### 2. Service Systemd (`/etc/systemd/system/wa-server.service`)
```ini
[Unit]
Description=IndoTech Super WhatsApp BOT Gateway Engine
After=network.target

[Service]
Type=simple
User=www-data
WorkingDirectory=/var/www/wa-gateway/server-wa
ExecStart=/usr/bin/node index.js
Restart=always
RestartSec=5
Environment=NODE_ENV=production

[Install]
WantedBy=multi-user.target
```

Jalankan perintah:
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now wa-server
```

---

### B. Shared Hosting (cPanel / CloudPanel)

Aplikasi ini dapat berjalan mandiri di shared hosting tanpa akses root / systemd:

```
Browser ──https://domainanda.com──► Apache (PHP / Laravel)
   │                                     │
   │  /socket.io (websocket+polling)     │  http://127.0.0.1:3998 (internal)
   └────────────────► Apache ───────────┴──► Node.js WA Server ──► WhatsApp
                    (proxy .htaccess)              │
                                                   └──► MariaDB (bersama Laravel)
```

1. **Jalankan Engine via Cron Job cPanel (tiap menit)**:
   ```bash
   * * * * * /home/username/public_html/server-wa/keepalive.sh >> /home/username/public_html/storage/logs/wa-cron.log 2>&1
   ```
   *Skrip `keepalive.sh` mendeteksi PID dan port berjalan. Jika proses berhenti, otomatis dihidupkan ulang di background.*

2. **Cron Job Antrean & Scheduler Laravel**:
   ```bash
   * * * * * cd /home/username/public_html && php artisan schedule:run >> /dev/null 2>&1
   * * * * * cd /home/username/public_html && php artisan queue:work --stop-when-empty --max-time=50 >> /dev/null 2>&1
   ```

3. **Restart Mandiri via CLI**:
   ```bash
   ./restart.sh
   ```

---

## Dokumentasi Lengkap REST API

Semua request API menggunakan autentikasi header `X-API-Key`. API Key dapat dibuat di menu **Gateway Center (Client Apps)**.

Base URL: `https://domainanda.com` (Port standar 443 via reverse proxy).

### 1. Kirim Pesan Teks (Outbound WA Gateway)
- **Endpoint**: `POST /api/send`
- **Headers**: 
  - `Content-Type: application/json`
  - `X-API-Key: {your-client-app-key}`
- **Payload**:
```json
{
  "to": "081234567890",
  "message": "Halo! Pesan notifikasi dari sistem Anda."
}
```
- **Response (200 OK)**:
```json
{
  "success": true,
  "message": "Pesan berhasil dikirim",
  "data": {
    "device": "Bot Notifikasi Utama",
    "target": "628123456789@s.whatsapp.net"
  }
}
```

### 2. Kirim Pesan Gambar / Media
- **Endpoint**: `POST /api/send-image`
- **Payload**:
```json
{
  "to": "081234567890",
  "image": "https://domainanda.com/storage/invoice-bukti.jpg",
  "caption": "Bukti Pembayaran Invoice #10294"
}
```

### 3. Kirim Dokumen PDF
- **Endpoint**: `POST /api/send-document`
- **Payload**:
```json
{
  "to": "081234567890",
  "document": "https://domainanda.com/storage/laporan.pdf",
  "filename": "Laporan-Bulanan.pdf",
  "caption": "Terlampir berkas laporan bulanan Anda."
}
```

### 4. Panggil Kecerdasan Super BOT Langsung (AI Chat API)
Aplikasi klien Anda dapat langsung memanggil otak AI dari Super BOT secara terpusat:
- **Endpoint**: `POST /api/ai/chat`
- **Headers**: `X-API-Key: {your-client-app-key}`
- **Payload**:
```json
{
  "message": "Bagaimana prosedur perpanjangan layanan dan apa saja syaratnya?",
  "context": "Pengguna: PT Sukses Mandiri, Paket: Enterprise, Status: Aktif"
}
```
- **Response (200 OK)**:
```json
{
  "success": true,
  "reply": "Halo rekan PT Sukses Mandiri! Untuk perpanjangan layanan paket Enterprise, Anda cukup...",
  "app": "Aplikasi Web Utama"
}
```

### 5. Cek Perangkat WhatsApp yang Tersedia
- **Endpoint**: `GET /api/devices`
- **Response (200 OK)**:
```json
{
  "success": true,
  "app": { "id": 1, "name": "Portal ERP & CRM" },
  "devices": [
    {
      "id": 1,
      "name": "Bot CS WhatsApp",
      "phone_number": "628123456789",
      "status": "connected",
      "is_connected": true,
      "is_default": true
    }
  ]
}
```

### 6. Health Check Gateway
- **Endpoint**: `GET /api/health`
- **Response (200 OK)**:
```json
{
  "status": "healthy",
  "service": "IndoTech WA Gateway",
  "node_server": "online",
  "timestamp": "2026-10-22T01:30:00+07:00"
}
```

---

## Modul Dokumentasi Lanjutan (Docs)

Dokumentasi teknis mendalam dan spesifikasi arsitektur per modul dapat diakses pada direktori [`docs/`](docs/README.md):

- 📘 **[Modul 01: Arsitektur Tri-Gateway & Super BOT](docs/01-arsitektur-tri-gateway.md)**: Rincian pemisahan tugas Gateway Center (penengah AI), WhatsApp Gateway (penengah in/out WA), dan Super Bot Gateway (konektor AI + RAG).
- 🔐 **[Modul 02: Hak Akses Hierarkis & Shared Quota Pool](docs/02-hak-akses-dan-shared-quota.md)**: Matriks peran SuperAdmin, Admin, dan User; proteksi menu sensitif; pembatasan shared pool kuota; dan isolasi log pesan serta notifikasi.
- 📱 **[Modul 03: Autentikasi & OTP WA Center](docs/03-autentikasi-dan-otp-wa-center.md)**: Penetapan nomor WA Center, pemisahan registrasi publik, alur login 2FA OTP permanen, dan bypass darurat saat perangkat offline.
- 🎨 **[Modul 04: Desain 3D Neumorphic & Instant Lazy Loading SPA](docs/04-ui-3d-dan-lazy-loading-spa.md)**: Desain visual 3D (card, button, inset input), wire:navigate SPA tanpa reload, loading bayangan (skeleton loader), penanganan error kustom, dan PWA.
- 🔌 **[Modul 05: Integrasi Aplikasi Dinamis & REST API](docs/05-integrasi-aplikasi-dinamis.md)**: Panduan integrasi dinamis tanpa hardcode, protokol webhook konteks data langsung (ERP, CRM, LMS, HR), serta dokumentasi REST API lengkap.

---

## Troubleshooting & Solusi

### 1. Error Socket.IO / QR Code Tidak Muncul
- **Penyebab**: Browser tidak dapat menjangkau port Node.js atau Apache `mod_proxy` belum aktif.
- **Solusi**: Di menu **Pengaturan Sistem > Umum**, pastikan *Socket.io Public URL* dikosongkan jika menggunakan reverse proxy Apache / Nginx. Jika hosting melarang reverse proxy, buka port 3998 dan isi URL publik lengkap (`https://domainanda.com:3998`).

### 2. Error Thread Limit / Build Vite (`errno=11: fatal error: newosproc`)
- **Penyebab**: Batasan jumlah thread proses di lingkungan CloudLinux / cPanel LVE.
- **Solusi**: Skrip build telah dioptimasi dengan `GOMAXPROCS=1 npm run build` untuk membatasi pemakaian thread esbuild.

### 3. Perangkat WhatsApp Terputus Sendiri (`Error 440 conflict`)
- **Penyebab**: Terdapat dua proses Node.js yang membaca folder sesi kredensial Baileys secara bersamaan.
- **Solusi**: Hentikan proses lama dengan `./server-wa/keepalive.sh stop` atau `./restart.sh`, lalu pastikan hanya satu proses `keepalive.sh` yang berjalan.

### 4. Menjalankan Uji Coba Test Suite
Untuk memvalidasi bahwa seluruh sistem berjalan sempurna:
```bash
php artisan test --compact
```
*(Seluruh 80 pengujian unit & fitur mencakup Super BOT, RAG, SPA Error Handler, Anti-Ban, Hak Akses Hierarkis, Kuota Shared Pool, OTP WA Center, dan Tema 3D berstatus 100% passed dengan 400 assertions).*

---

## 💖 Kontribusi & Donasi

Jika aplikasi ini mempermudah operasional IT, DevOps, dan pengelolaan server di perusahaan atau proyek pribadi Anda, dukung pengembangannya dengan menjadi kontributor atau **Donatur Spesial**.  
*(Donatur dan Sponsor resmi akan kami cantumkan logo perusahaannya di repositori ini).*

Dukung pemeliharaan berkelanjutan, riset modul bypass panel baru, dan pembaruan sistem dengan berdonasi melalui:

<div align="center">

**DANA — 08121600058** (a.n. Mochamad Ismail Soleh)

<img src="docs/qris.jpg" alt="QRIS DANA IndoTech" width="280">

*Pindai dengan aplikasi DANA, BCA, Mandiri, GoPay, OVO, atau aplikasi pembayaran QRIS apa pun di Indonesia.*

</div>

Semua dana dialokasikan untuk biaya infrastruktur riset server, lisensi alat pengujian, dan pengembangan modul open-source IndoTech. Jika Anda atau institusi Anda ingin menjadi mitra resmi atau sponsor, silakan hubungi kami. Terima kasih atas dukungan Anda!

---

## Lisensi
Hak Cipta &copy; 2026 IndoTech. Didistribusikan di bawah lisensi MIT.