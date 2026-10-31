# Modul 05: Integrasi Aplikasi Dinamis & Panduan REST API

## Pengantar

Aplikasi pihak ketiga apa pun (ERP, HRIS, E-Commerce, Portal Siswa, CRM, Inventori, atau Sistem Layanan Pelanggan) dapat dihubungkan secara dinamis tanpa perlu mengubah baris kode di aplikasi ini.

---

## 1. Menghubungkan Aplikasi Klien di Gateway Center

Untuk mendaftarkan aplikasi luar ke dalam Gateway Center:
1. Buka menu **Gateway Center > Aplikasi Klien**.
2. Klik tombol **Tambah Aplikasi Baru**.
3. Isi parameter:
   - **Nama Aplikasi**: Contoh: `ERP IndoTech`, `Sistem HR & Presensi`, `Portal Akademik`.
   - **URL Webhook Konteks**: URL endpoint di sistem luar yang menyediakan data kontekstual (misal: `https://erp.domainanda.com/api/wa-context`).
   - **Alokasi Perangkat**: Pilih nomor WhatsApp yang boleh digunakan oleh aplikasi tersebut.
   - **Limit Pesan & Rate Limit**: Tetapkan kuota bulanan khusus aplikasi ini.
4. Sistem akan menghasilkan pasangan kredensial:
   - `X-API-Key`: Disertakan pada setiap *request header* API.
   - `API Secret`: Untuk verifikasi tanda tangan (*signature validation*).

---

## 2. Protokol Webhook Konteks AI (Live Context Injection)

Ketika Super BOT menerima pesan WhatsApp dari nomor `081234567890`, Gateway Center akan memanggil webhook aplikasi klien Anda:

### Request dari Gateway Center ke Aplikasi Klien:
```http
POST https://erp.domainanda.com/api/wa-context
Content-Type: application/json
X-Gateway-Secret: {your-api-secret}

{
  "phone": "628123456789",
  "message": "Halo, tolong cek status pengiriman pesanan saya dengan nomor PO-9912",
  "timestamp": "2026-10-22T02:00:00+07:00"
}
```

### Response yang Diharapkan dari Aplikasi Klien (JSON):
```json
{
  "success": true,
  "user_profile": {
    "name": "Budi Santoso",
    "company": "PT Maju Bersama",
    "role": "Procurement Manager"
  },
  "context_data": "Status PO-9912: Telah dikirim via ekspedisi JNE Cargo dengan resi JNE88219401 pada tanggal 21 Oktober. Estimasi tiba besok sore.",
  "quick_suggestions": [
    "Lacak Resi",
    "Unduh Faktur Pajak"
  ]
}
```

Gateway Center akan langsung menyatukan `context_data` tersebut dengan dokumen RAG ke dalam prompt AI LLM, sehingga bot dapat menjawab pengguna WhatsApp secara presisi dan faktual!

---

## 3. Spesifikasi REST API Lengkap

### Base URL
```
https://wa.domainanda.com
```

### Autentikasi
Sertakan header wajib pada setiap panggilan:
```http
X-API-Key: {your-client-app-key}
Content-Type: application/json
```

---

### Endpoint 1: Kirim Pesan Teks
- **URL**: `POST /api/send`
- **Body JSON**:
```json
{
  "to": "081234567890",
  "message": "Halo! Tagihan langganan Anda sebesar Rp 150.000 telah jatuh tempo.",
  "device_id": 1 // Opsional, default ke perangkat utama
}
```
- **Response (200 OK)**:
```json
{
  "success": true,
  "message": "Pesan berhasil dikirim",
  "data": {
    "target": "628123456789@s.whatsapp.net",
    "status": "sent"
  }
}
```

---

### Endpoint 2: Kirim Gambar dengan Keterangan
- **URL**: `POST /api/send-image`
- **Body JSON**:
```json
{
  "to": "081234567890",
  "image": "https://domainanda.com/assets/promo.jpg",
  "caption": "Promo Spesial Akhir Bulan Diskon 50%!"
}
```

---

### Endpoint 3: Kirim Berkas Dokumen (PDF, Excel, Word)
- **URL**: `POST /api/send-document`
- **Body JSON**:
```json
{
  "to": "081234567890",
  "document": "https://domainanda.com/storage/invoice-2026.pdf",
  "filename": "Invoice-INV-2026-10.pdf",
  "caption": "Terlampir berkas tagihan resmi Anda."
}
```

---

### Endpoint 4: Tanya Jawab AI Langsung (Direct AI Chat)
Jika aplikasi klien Anda ingin menggunakan kecerdasan Super BOT di dalam website atau aplikasi mobile klien sendiri tanpa melalui chat WhatsApp:
- **URL**: `POST /api/ai/chat`
- **Body JSON**:
```json
{
  "message": "Bagaimana prosedur pengajuan cuti tahunan dan apa persyaratannya?",
  "context": "Pengguna: Staff Keuangan, Masa Kerja: 2 Tahun, Sisa Cuti: 8 Hari"
}
```
- **Response (200 OK)**:
```json
{
  "success": true,
  "reply": "Pengajuan cuti tahunan dapat dilakukan melalui formulir sistem mandiri minimal H-3 sebelum tanggal cuti dengan persetujuan atasan langsung. Berdasarkan data, Anda memiliki sisa 8 hari cuti tahunan.",
  "app": "ERP IndoTech"
}
```

---

### Endpoint 5: Cek Status Perangkat WhatsApp
- **URL**: `GET /api/devices`
- **Response (200 OK)**:
```json
{
  "success": true,
  "app": { "id": 1, "name": "ERP IndoTech" },
  "devices": [
    {
      "id": 1,
      "name": "Bot Notifikasi WA",
      "phone_number": "628123456789",
      "status": "connected",
      "is_connected": true,
      "is_default": true
    }
  ]
}
```

---

### Endpoint 6: Health Check
- **URL**: `GET /api/health`
- **Response (200 OK)**:
```json
{
  "status": "healthy",
  "service": "IndoTech WA Gateway",
  "node_server": "online",
  "timestamp": "2026-10-22T02:00:00+07:00"
}
```
