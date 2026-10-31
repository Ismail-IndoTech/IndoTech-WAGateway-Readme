# Modul 01: Arsitektur Tri-Gateway & Super BOT

## Pengantar

IndoTech Super WhatsApp BOT Gateway mengusung paradigma arsitektur **Tri-Gateway** (Tiga Pilar Penengah). Paradigma ini memisahkan secara tegas tanggung jawab antara konektivitas perangkat perpesanan, middleware integrasi aplikasi luar, dan mesin kognitif AI.

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
 │ • Dynamic Schema Integrator  │ │ • Web Chat, Broadcast, OTP  │
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
         │  • Dynamic Integrations (No Hardcoded Apps)  │
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

---

## 1. Gateway Center — Penengah AI & Integrasi Aplikasi

**Gateway Center** berfungsi sebagai jembatan cerdas (*AI Middleware Hub*) yang menghubungkan aplikasi luar dengan otak kecerdasan buatan terpusat di dalam platform ini.

### Fungsi Utama Gateway Center:
1. **Penengah AI (AI Intermediary)**: Memungkinkan aplikasi luar memanggil fungsi AI (chat, ekstraksi dokumen, rekomendasi) tanpa perlu mengelola token LLM atau prompt engineering sendiri.
2. **Kredensial API Key Terisolasi**: Setiap aplikasi klien (*Client App*) memiliki pasangan `API Key` dan `Secret Key` unik. Akses dibatasi pada perangkat dan kuota yang telah dialokasikan.
3. **Penyedia Webhook Konteks Dinamis**:
   - Ketika bot WhatsApp menerima pesan dari nomor tertentu, Gateway Center menghubungi webhook aplikasi klien (misal: `/api/ai-assistant/context`).
   - Aplikasi klien merespons dengan data *live* pengguna (contoh: nama karyawan, sisa cuti, tagihan belum lunas, status tiket support).
   - Gateway Center menyuntikkan data tersebut ke dalam *System Prompt* LLM secara aman.
4. **Alokasi Perangkat & Failover Otomatis**: Menentukan nomor WhatsApp mana saja yang diizinkan digunakan oleh aplikasi tertentu, lengkap dengan mekanisme pengalihan otomatis saat perangkat utama terputus.

---

## 2. WhatsApp Gateway — Penengah In/Out WA

**WhatsApp Gateway** adalah infrastruktur konektivitas fisik perpesanan WhatsApp multi-perangkat (*multi-device engine*).

### Fungsi Utama WhatsApp Gateway:
1. **Penengah Trafik Masuk & Keluar**: Mengelola seluruh antrean pesan keluar (*outbound*) dari berbagai sistem klien dan menyalurkan pesan masuk (*inbound*) ke sistem penanganan yang tepat.
2. **Multi-Device Baileys**: Mendukung banyak nomor WhatsApp sekaligus yang terhubung secara paralel via WebSocket dan Socket.IO.
3. **Penetapan Nomor Utama (WA Center)**:
   - Satu atau lebih nomor dapat ditetapkan sebagai nomor resmi sistem.
   - Digunakan untuk pengiriman OTP registrasi publik, verifikasi login 2FA, dan notifikasi sistem darurat.
4. **Anti-Ban Engine & Warm-up Protection**:
   - Pengaturan jeda waktu acak (3-8 detik) antar pesan keluar.
   - Pembatasan volume pesan per jam untuk nomor baru.
5. **Interactive Web Chat**: Antarmuka obrolan web real-time untuk operator admin membalas pesan secara manual.

---

## 3. Super Bot Gateway — Penghubung WA Gateway & AI Integrations

**Super Bot Gateway** adalah orkestrator yang menyatukan kapabilitas fisik WhatsApp Gateway dengan kecerdasan kognitif AI Integrations.

### Mengapa Disebut "Super BOT"?
Super BOT bukan sekadar bot aturan kata kunci (*keyword-based auto reply*). Super BOT adalah asisten virtual serba bisa yang:
1. **Didukung RAG (Retrieval-Augmented Generation)**: Mampu mempelajari berkas PDF, dokumen SOP, regulasi internal perusahaan, dan FAQ tanpa perlu melatih ulang (*re-training*) model AI.
2. **Pengetahuan Multi-Disiplin**: Dapat bertransformasi menjadi pakar di bidang apa pun:
   - Asisten Layanan Pelanggan & Helpdesk (CS).
   - Asisten Reservasi & Front Desk / Buku Tamu.
   - Asisten Penjualan & E-Commerce.
   - Asisten HR & Kepegawaian Umum.
   - Asisten Akademik & Registrasi Kampus.
   - Asisten Operasional Proyek & Lapangan.
3. **Pemberhentian Otomatis (Human Handover)**: Jika pelanggan mengekspresikan frustrasi atau meminta berbicara dengan manusia, bot secara otomatis mundur dan memberikan kendali kepada agen manusia.

---

## 4. Dynamic AI-Integrations Engine (Tanpa Hardcode)

Arsitektur sistem ini dibangun secara **Full Super Dynamic**:
- Model data `Integration` dan `IntegrationFeature` disimpan langsung di database.
- Administrator bebas menambahkan integrasi aplikasi eksternal baru (ERP, CRM, Web Portal, LMS, Kasir/POS, dsb.) secara dinamis.
- Mengatur *prompt template*, webhook URL, dan alur fitur kustom langsung melalui antarmuka web tanpa modifikasi kode sumber.
- Super BOT secara otomatis mengadopsi identitas aplikasi klien berdasarkan konfigurasi yang aktif.
