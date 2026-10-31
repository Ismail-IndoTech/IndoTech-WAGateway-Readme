# Module 05: Dynamic Application Integration & REST API Guide

## Introduction

Any third-party application (ERP, HRIS, E-Commerce, Student Portal, CRM, Inventory, or Customer Service System) can be connected dynamically without the need to change a single line of code in this application.

---

## 1. Connecting Client Applications in the Gateway Center

To register an external application into the Gateway Center:
1. Open the **Gateway Center > Client Apps** menu.
2. Click the **Add New Application** button.
3. Fill in the parameters:
   - **Application Name**: Example: `ERP IndoTech`, `HR & Attendance System`, `Academic Portal`.
   - **Context Webhook URL**: The endpoint URL in the external system that provides contextual data (e.g., `https://erp.domainanda.com/api/wa-context`).
   - **Device Allocation**: Select the WhatsApp numbers that may be used by that application.
   - **Message Limit & Rate Limit**: Set a monthly quota specific to this application.
4. The system will generate a credential pair:
   - `X-API-Key`: Included in every API *request header*.
   - `API Secret`: For *signature validation*.

---

## 2. AI Context Webhook Protocol (Live Context Injection)

When the Super BOT receives a WhatsApp message from the number `081234567890`, the Gateway Center will call your client application's webhook:

### Request from Gateway Center to Client Application:
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

### Expected Response from Client Application (JSON):
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

The Gateway Center will immediately merge that `context_data` with the RAG documents into the LLM AI prompt, so the bot can answer WhatsApp users precisely and factually!

---

## 3. Complete REST API Specification

### Base URL
```
https://wa.domainanda.com
```

### Authentication
Include the required header in every call:
```http
X-API-Key: {your-client-app-key}
Content-Type: application/json
```

---

### Endpoint 1: Send Text Message
- **URL**: `POST /api/send`
- **JSON Body**:
```json
{
  "to": "081234567890",
  "message": "Halo! Tagihan langganan Anda sebesar Rp 150.000 telah jatuh tempo.",
  "device_id": 1 // Optional, defaults to the primary device
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

### Endpoint 2: Send Image with Caption
- **URL**: `POST /api/send-image`
- **JSON Body**:
```json
{
  "to": "081234567890",
  "image": "https://domainanda.com/assets/promo.jpg",
  "caption": "Promo Spesial Akhir Bulan Diskon 50%!"
}
```

---

### Endpoint 3: Send Document File (PDF, Excel, Word)
- **URL**: `POST /api/send-document`
- **JSON Body**:
```json
{
  "to": "081234567890",
  "document": "https://domainanda.com/storage/invoice-2026.pdf",
  "filename": "Invoice-INV-2026-10.pdf",
  "caption": "Terlampir berkas tagihan resmi Anda."
}
```

---

### Endpoint 4: Direct AI Question & Answer (Direct AI Chat)
If your client application wants to use the Super BOT intelligence within the client's own website or mobile application without going through WhatsApp chat:
- **URL**: `POST /api/ai/chat`
- **JSON Body**:
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

### Endpoint 5: Check WhatsApp Device Status
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
