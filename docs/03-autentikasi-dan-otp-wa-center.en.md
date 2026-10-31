# Module 03: Authentication, WA Center Designation & OTP Login

## Introduction

The security of an enterprise messaging system demands layered identity verification as well as the prevention of *lockout* (being locked out of the system). IndoTech Super WhatsApp BOT Gateway separates the self-registration mechanism from the 2FA authentication mechanism via WhatsApp.

---

## 1. Separation of Registration Settings & OTP Status

In previous versions, the OTP Login activation status was often tied to or mixed with the public registration open/closed status. In the current architecture:
1. **Public Registration Setting (`registration_enabled`)**:
   - Can be enabled (*ON*) or disabled (*OFF*) independently by the SuperAdmin in the **System Settings** menu.
   - When disabled, the `/register` form is closed and displays an informational message that new registrations are not currently accepting self-registrants.
2. **System OTP Login Setting (`otp_enabled` & `wa_center_device_id`)**:
   - **Login with OTP is always active** immediately after the SuperAdmin designates one of the devices as the **WhatsApp Center number (System OTP Number)**.
   - The public registration status does not affect the obligation for OTP verification at *login*.

---

## 2. Public Self-Registration Flow

To maintain hierarchical order:
- Every user who registers independently via `/register` **will always and invariably have the `User` role** (not Admin).
- Registration flow:
  1. The user fills in their name, email, active WhatsApp number, and password.
  2. The system sends a 6-digit WhatsApp verification OTP via the WA Center number.
  3. After the code is verified at `/verify-otp`, the account is registered with the `User` role and a status of pending activation or immediately active according to the system configuration.

---

## 3. Login Flow & WA Center Offline Handling

```
                  User Enters Email/Number & Password
                                     |
                                     v
                        Password Credentials Valid?
                         |- NO --> Compute Throttle / Progressive Lockout
                         |
                         +- YES
                             |
                             v
                 SuperAdmin Has Set a WA Center Number?
                  |- NO --> Direct Login
                  |
                  +- YES
                      |
                      v
               Is the WA Center Device Currently Connected (Online)?
                |
                |- YES --> Send 6-Digit OTP Code via WA
                |          +- Redirect to /verify-login-otp Page
                |
                +- NO (Offline / Disconnected)
                      |
                      v
               Is the User an Administrator (SuperAdmin / Admin)?
                |
                |- YES (Emergency Bypass)
                |    +- Allow Direct Login + Yellow Warning:
                |       "WA Center number is offline. Please reconnect immediately."
                |
                +- NO (Role = User)
                     +- Display Error Message:
                        "WA Center number is offline. Contact SuperAdmin."
```

### Why Is There an Emergency Access Bypass for Administrators?
If the WhatsApp Center number is disconnected (for example, the phone connection is down or the QR session has expired), and all logins are locked by OTP, then the Administrator would not be able to enter the dashboard to reconnect WhatsApp.

Therefore, the **SuperAdmin** and **Admin** roles are granted emergency entry permission using a password (*emergency password access*) when the OTP device is offline, while regular **User** accounts are still held back for security integrity.

---

## 4. Anti-Brute Force Security (Progressive Lockout)

The login system is equipped with tiered failed-attempt restrictions without burdening the database (*session & cache throttling*):
- **Tier 1 (3 consecutive failed attempts)**: Locked for 60 seconds.
- **Tier 2 (6 consecutive failed attempts)**: Locked for 180 seconds.
- **Tier 3 (9 consecutive failed attempts)**: Locked for 300 seconds.
- The interface displays a real-time *countdown* dynamically.
