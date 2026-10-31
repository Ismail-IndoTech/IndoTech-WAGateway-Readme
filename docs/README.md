# Dokumentasi Resmi IndoTech Super WhatsApp BOT Gateway

Selamat datang di pusat dokumentasi resmi **IndoTech Super WhatsApp BOT Gateway**. Dokumentasi ini menyajikan panduan mendalam mengenai arsitektur sistem, pembagian hak akses hierarkis, sistem kuota bersama (*shared pool*), autentikasi OTP WA Center, antarmuka 3D Neumorphic, serta integrasi dinamis aplikasi eksternal.

---

## 📚 Daftar Modul Dokumentasi

| No | Modul Dokumentasi | Deskripsi |
|---|---|---|
| 01 | [Arsitektur Tri-Gateway & Super BOT](01-arsitektur-tri-gateway.md) | Penjelasan fungsi 3 pilar penengah (Gateway Center, WhatsApp Gateway, Super Bot Gateway) dan RAG Knowledge Base. |
| 02 | [Hak Akses Hierarkis & Shared Quota Pool](02-hak-akses-dan-shared-quota.md) | Batasan peran SuperAdmin, Admin, dan User; mekanisme kuota bersama (*shared pool*); isolasi log & notifikasi. |
| 03 | [Autentikasi & OTP WA Center](03-autentikasi-dan-otp-wa-center.md) | Penetapan nomor WA Center untuk OTP, pemisahan registrasi publik, dan mekanisme darurat (*emergency bypass*). |
| 04 | [Desain 3D Neumorphic & Instant Lazy Loading SPA](04-ui-3d-dan-lazy-loading-spa.md) | Standar gaya 3D (card, button, input) dan transisi instan wire:navigate dengan placeholder bayangan (*shadow skeleton*). |
| 05 | [Integrasi Aplikasi Dinamis & REST API](05-integrasi-aplikasi-dinamis.md) | Panduan menghubungkan aplikasi klien (ERP, CRM, LMS, HR) secara dinamis tanpa hardcode melalui Gateway Center. |

---

## 🌟 Ringkasan Cepat Identitas Aplikasi

Aplikasi ini dirancang sebagai **Super WhatsApp BOT Gateway**:
1. **Gateway Center**: Penengah aplikasi eksternal untuk AI-Integrations (penengah kecerdasan AI terpusat).
2. **WhatsApp Gateway**: Penengah komunikasi masuk dan keluar (*inbound/outbound*) multi-device WhatsApp.
3. **Super Bot Gateway**: Penghubung antara WhatsApp Gateway dengan AI-Integrations + RAG Knowledge Base yang mengubah WhatsApp menjadi asisten bot cerdas di berbagai bidang bisnis.
4. **Full SPA & Offline Ready**: Dibangun menggunakan Laravel 12 + Livewire 3 + Alpine.js + Service Worker untuk pengalaman instan tanpa refresh layar.
5. **Hierarki Multi-Tenancy**: Dukungan SuperAdmin, Admin, dan User dengan isolasi data ketat dan pembagian kuota fleksibel.

---

## 💡 Navigasi Cepat
- Kembali ke panduan utama: [README Utama Proyek](../README.md)
