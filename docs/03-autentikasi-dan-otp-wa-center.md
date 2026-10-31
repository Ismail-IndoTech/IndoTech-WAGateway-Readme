# Modul 03: Autentikasi, Penetapan WA Center & OTP Login

## Pengantar

Keamanan sistem perpesanan enterprise menuntut verifikasi identitas berlapis sekaligus pencegahan *lockout* (terkunci dari sistem). IndoTech Super WhatsApp BOT Gateway memisahkan mekanisme pendaftaran mandiri dengan mekanisme autentikasi 2FA via WhatsApp.

---

## 1. Pemisahan Pengaturan Registrasi & Status OTP

Di versi sebelumnya, status pengaktifan Login OTP sering kali terikat atau bercampur dengan status buka/tutup pendaftaran publik. Pada arsitektur saat ini:
1. **Pengaturan Pendaftaran Publik (`registration_enabled`)**:
   - Dapat diaktifkan (*ON*) atau dinonaktifkan (*OFF*) secara mandiri oleh SuperAdmin di menu **Pengaturan Sistem**.
   - Ketika dinonaktifkan, formulir `/register` akan ditutup dan menampilkan pesan informasi bahwa pendaftaran baru sedang tidak menerima pendaftar mandiri.
2. **Pengaturan Login OTP Sistem (`otp_enabled` & `wa_center_device_id`)**:
   - **Login dengan OTP akan selalu aktif** seketika setelah SuperAdmin menetapkan salah satu perangkat sebagai nomor **WhatsApp Center (Nomor OTP Sistem)**.
   - Status pendaftaran publik tidak memengaruhi kewajiban verifikasi OTP saat masuk (*login*).

---

## 2. Alur Registrasi Publik Mandiri

Untuk menjaga ketertiban hierarki:
- Setiap pengguna yang mendaftar secara mandiri melalui `/register` **pasti dan selalu berstatus sebagai peran `User`** (bukan Admin).
- Alur pendaftaran:
  1. Pengguna mengisi nama, email, nomor WhatsApp aktif, dan kata sandi.
  2. Sistem mengirimkan 6 digit OTP verifikasi WhatsApp via nomor WA Center.
  3. Setelah kode diverifikasi di `/verify-otp`, akun terdaftar dengan peran `User` dan status menunggu aktivasi atau langsung aktif sesuai konfigurasi sistem.

---

## 3. Alur Login & Penanganan WA Center Offline

```
                  Pengguna Memasukkan Email/Nomor & Password
                                     │
                                     ▼
                        Kredensial Password Valid?
                         ├─ TIDAK ──► Hitung Throttle / Progressive Lockout
                         │
                         └─ YA
                             │
                             ▼
                 SuperAdmin Menetapkan Nomor WA Center?
                  ├─ TIDAK ──► Langsung Masuk (Direct Login)
                  │
                  └─ YA
                      │
                      ▼
               Apakah Perangkat WA Center Sedang Terhubung (Online)?
                │
                ├─ YA ──► Kirim 6 Digit Kode OTP via WA
                │         └─ Arahkan ke Halaman /verify-login-otp
                │
                └─ TIDAK (Offline / Terputus)
                      │
                      ▼
               Apakah Pengguna adalah Administrator (SuperAdmin / Admin)?
                │
                ├─ YA (Bypass Darurat)
                │    └─ Izinkan Login Langsung + Peringatan Kuning:
                │       "Nomor WA Center sedang offline. Segera sambungkan kembali."
                │
                └─ TIDAK (Role = User)
                     └─ Tampilkan Pesan Error:
                        "Nomor WA Center sedang offline. Hubungi SuperAdmin."
```

### Mengapa Ada Emergency Access Bypass untuk Administrator?
Jika nomor WhatsApp Center terputus (misalnya koneksi HP mati atau sesi QR kedaluwarsa), dan seluruh login terkunci oleh OTP, maka Administrator tidak akan bisa masuk ke dasbor untuk menyambungkan kembali WhatsApp. 

Oleh karena itu, peran **SuperAdmin** dan **Admin** diberikan izin masuk darurat menggunakan kata sandi (*emergency password access*) ketika perangkat OTP offline, sedangkan akun **User** biasa tetap ditahan demi integritas keamanan.

---

## 4. Keamanan Anti-Brute Force (Progressive Lockout)

Sistem login dilengkapi dengan pembatasan percobaan gagal berjenjang tanpa perlu membebani basis data (*session & cache throttling*):
- **Tier 1 (3 kali gagal berturut-turut)**: Terkunci selama 60 detik.
- **Tier 2 (6 kali gagal berturut-turut)**: Terkunci selama 180 detik.
- **Tier 3 (9 kali gagal berturut-turut)**: Terkunci selama 300 detik.
- Antarmuka menampilkan penghitung waktu mundur (*countdown*) real-time secara dinamis.
