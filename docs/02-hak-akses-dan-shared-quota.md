# Modul 02: Hak Akses Hierarkis, Isolasi Log & Shared Quota Pool

## Pengantar

Untuk mendukung skenario multi-organisasi, multi-divisi, dan multi-klien secara aman, IndoTech Super WhatsApp BOT Gateway mengimplementasikan sistem hak akses 3-tingkat (*three-tier hierarchy*): **SuperAdmin**, **Admin**, dan **User**.

---

## 1. Matriks Hak Akses Peran (Role Matrix)

| Modul / Fitur | SuperAdmin | Admin | User |
|---|:---:|:---:|:---:|
| **Dashboard Statistik** | ✅ Global | ✅ Scoped (Akun + Subordinat) | ✅ Milik Sendiri |
| **Gateway Center (Client Apps)** | ✅ Penuh | ✅ Scoped (Berdasarkan Limit) | ❌ |
| **WhatsApp Devices** | ✅ Semua Perangkat | ✅ Perangkat Milik Sendiri | ✅ Perangkat Pribadi / Assigned |
| **Super BOT & Persona** | ✅ Penuh | ✅ Penuh | ❌ |
| **Auto-Reply Rules** | ✅ Penuh | ✅ Scoped | ❌ |
| **Broadcast Campaigns** | ✅ Penuh | ✅ Scoped | ❌ |
| **Obrolan Interaktif (Chat)** | ✅ Penuh | ✅ Penuh | ✅ Sesuai Perangkat |
| **Message Log & History** | ✅ Seluruh Sistem | 🔒 Hanya Akun & Subordinatnya | 🔒 Hanya Milik Sendiri |
| **Manajemen Pengguna (Users)** | ✅ Semua Akun | 🔒 Hanya Subordinatnya (Parent-Child) | ❌ |
| **Notifikasi Top Navbar** | ✅ Global | 🔒 Relevan Akun & Subordinat | 🔒 Relevan Akun Sendiri |
| **Pengaturan Sistem (Settings)** | ✅ Akses Penuh | 🚫 Dilarang (403 Forbidden) | 🚫 Dilarang (403 Forbidden) |
| **Activity Audit Logs** | ✅ Akses Penuh | 🚫 Dilarang (403 Forbidden) | 🚫 Dilarang (403 Forbidden) |
| **Gateway Server Logs** | ✅ Akses Penuh | 🚫 Dilarang (403 Forbidden) | 🚫 Dilarang (403 Forbidden) |

---

## 2. Batasan Ketat untuk Akun Admin

Akun dengan peran **Admin** memiliki batasan tegas guna melindungi konfigurasi inti sistem dan kerahasiaan platform:
1. **Tidak Memiliki Akses Pengaturan Sistem (`/settings`)**:
   - Menu disembunyikan dari sidebar.
   - Akses via URL langsung diblokir dengan `403 Forbidden` di level Middleware (`middleware('role:SuperAdmin')`) dan komponen Livewire `mount()`.
2. **Tidak Memiliki Akses Activity Log (`/logs`)**:
   - Log audit global sistem hanya diperuntukkan bagi SuperAdmin untuk melacak seluruh aktivitas platform.
3. **Tidak Memiliki Akses Gateway Server Log (`/gateway-logs`)**:
   - Log konsol low-level Node.js dan proses soket Baileys tidak diekspos kepada Admin.

---

## 3. Isolasi Message Log & History

Pencatatan pesan keluar dan masuk (*Message History*) diisolasi secara ketat berdasarkan hierarki akun melalui helper `MessageHistory::visibleToUser($user)`:
- **SuperAdmin**: Memiliki visibilitas 100% terhadap seluruh pesan yang dikirim dan diterima oleh seluruh akun di sistem.
- **Admin**: Hanya dapat meninjau log pesan yang dikirimkan oleh akun Admin itu sendiri serta oleh akun-akun User yang berada langsung di bawah bimbingannya (`parent_id = admin_id`). Admin tidak dapat melihat pesan milik Admin lain atau SuperAdmin.
- **User**: Hanya dapat meninjau riwayat pesan yang dikirimkan oleh dirinya sendiri (`user_id = user_id`).

---

## 4. Isolasi Notifikasi Top Navbar

Dropdown notifikasi pada header navigasi atas menyaring item aktivitas berdasarkan peran pengguna:
- **SuperAdmin**: Menampilkan ringkasan notifikasi sistem secara menyeluruh serta tombol *"Lihat Semua Log"*.
- **Admin**: Menampilkan notifikasi login, pengiriman broadcast, atau peringatan kuota yang hanya bersumber dari dirinya dan seluruh User di bawahnya. Tombol *"Lihat Semua Log"* disembunyikan.
- **User**: Menampilkan notifikasi privat terkait akun dan perangkat pribadinya.

---

## 5. Sistem Kuota Bersama (Shared Quota Pool)

IndoTech WhatsApp Gateway menerapkan konsep **Shared Resource Pool** antara Admin dan anak-anak akun User di bawahnya:

```
                  ┌─────────────────────────────────┐
                  │       ADMIN INDUK (Pool)        │
                  │   Kuota Pesan: 10.000 / Bulan   │
                  │   Limit Device: 5 Perangkat     │
                  └───────────────┬─────────────────┘
                                  │
         ┌────────────────────────┼────────────────────────┐
         ▼                        ▼                        ▼
┌─────────────────┐      ┌─────────────────┐      ┌─────────────────┐
│     USER A      │      │     USER B      │      │     USER C      │
│ Limit: 2.000    │      │ Limit: 3.000    │      │ Limit: 1.000    │
│ Terpakai: 1.200 │      │ Terpakai: 2.800 │      │ Terpakai: 400   │
└─────────────────┘      └─────────────────┘      └─────────────────┘
         │                        │                        │
         └────────────────────────┴────────────────────────┘
                                  │
                                  ▼
                   Total Pemakaian Bersama: 4.400 / 10.000
```

### Aturan Kuota Bersama:
1. **Pemakaian Pesan (Shared Message Quota)**:
   - Admin memiliki total kuota pesan (misal: 10.000 pesan/bulan).
   - Setiap pesan yang dikirim oleh Admin maupun oleh salah satu User bawahannya akan menambah akumulasi pemakaian `shared_message_used`.
   - User dibatasi oleh 2 kondisi:
     1. Kuota individualnya belum habis (`user.message_used < user.message_quota`).
     2. Kuota bersama Admin induknya masih tersedia (`admin.getSharedMessageUsed() < admin.message_quota`).
2. **Limit Perangkat & Fitur**:
   - Jumlah perangkat aktif, client apps, auto-reply, dan kampanye broadcast yang dibuat oleh User diperhitungkan ke dalam batas maksimal (*limit pool*) milik Admin induk.
3. **Pendaftaran User Baru oleh Admin**:
   - Ketika Admin membuat akun baru melalui menu *Pengguna & Hak Akses*, sistem secara otomatis menetapkan `parent_id = admin_id` dan mengunci peran akun tersebut sebagai **`User`**. Admin tidak dapat menaikkan peran bawahan menjadi Admin atau SuperAdmin.
