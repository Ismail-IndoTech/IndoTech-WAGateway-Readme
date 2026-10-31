> 📚 **Documentation:** [Full ID/EN index](DOCS.md) · [English index](DOCS.en.md)

# IndoTech Super WhatsApp BOT Gateway — Enterprise Multi-Device, AI-Integrations & RAG Knowledge Base Architecture


A high-performance, enterprise-grade **Super WhatsApp BOT Gateway** and **AI-Assistant Management Platform** built with **Laravel 12 (Full SPA)**, **Livewire 3**, **Tailwind CSS v4**, and **Node.js (Baileys Multi-Device & Socket.IO)**.

Designed for robust operation on both **Dedicated VPS** and **Shared Hosting (cPanel)** environments.

---

## Table of Contents
- [Philosophy & Identity: Super WhatsApp BOT Gateway](#philosophy--identity-super-whatsapp-bot-gateway)
- [Tri-Gateway Architecture](#tri-gateway-architecture-three-intermediary-pillars)
- [Key Features & RAG Advantages](#key-features--rag-advantages)
- [3D Neumorphic Design & Instant Lazy Loading SPA](#3d-neumorphic-design--instant-lazy-loading-spa)
- [Hierarchical Access Rights & Shared Quota Pool](#hierarchical-access-rights--shared-quota-pool)
- [Authentication & Independent WA Center OTP](#authentication--independent-wa-center-otp)
- [Technology (Tech Stack)](#technology-tech-stack)
- [Installation & Seeder Guide](#installation--seeder-guide)
- [Deployment Guide (VPS & Shared Hosting)](#deployment-guide-vps--shared-hosting)
- [Complete REST API Documentation](#complete-rest-api-documentation)
- [Advanced Documentation Modules (Docs)](#advanced-documentation-modules-docs)
- [Troubleshooting & Solutions](#troubleshooting--solutions)
- [Contribution & Donation](#-contribution--donation)
- [License](#license)

---

## Philosophy & Identity: Super WhatsApp BOT Gateway

This application is not merely an ordinary WhatsApp message-sending gateway (*SMS/WA blaster*), nor is it merely a generic, isolated AI bot.

This platform is a **Super WhatsApp BOT Gateway**:
1. It integrates the physical connectivity infrastructure of the **WhatsApp Gateway** (multi-device, message queues, auto-heal, failover) with the intelligent cognitive engine of **AI Integrations** (Groq, OpenAI, Google Gemini, Anthropic Claude, DeepSeek, OpenRouter, Qwen).
2. It is reinforced by **RAG (Retrieval-Augmented Generation)** / Knowledge Base Hub, which turns the WhatsApp number into a **Super BOT that is extremely smart in every area/field**:
   - Able to master any business domain (Customer Service/CS, Front Desk/Guest Book, E-Commerce & Sales, HR & Personnel, Campus Academic Systems, Banking/Finance, Field Operations, etc.) simply by uploading reference documents or SOPs without modifying program code.
   - Prevents AI hallucinations by presenting verified facts (*grounded truth*) directly to WhatsApp users.
3. It is supported by the **Gateway Center** as an intermediary for external applications that supply *live* contextual data in a *schema-agnostic* manner into the AI conversation.

---

## Tri-Gateway Architecture (Three Intermediary Pillars)

```
 +--------------------------------------------------------------+
 |                EXTERNAL APPS / CLIENT APPS                   |
 |       (ERP, CRM, Web Portal, LMS, Mobile App, etc.)          |
 +--------------+-------------------------------+---------------+
                | (AI Context & Webhook)        | (REST API & Outbound)
                v                               v
 +------------------------------+ +-----------------------------+
 |       GATEWAY CENTER         | |      WHATSAPP GATEWAY       |
 |       (AI Intermediary)      | |   (WA In/Out Intermediary)  |
 |                              | |                             |
 | • AI-Integrations hub        | | • External WA traffic hub   |
 | • Unique API Key auth        | | • Multi-device Baileys      |
 | • Context webhook provider   | | • Dispatching & Auto-Heal   |
 | • App metadata & schema      | | • Web Chat, Broadcast, OTP  |
 +--------------+---------------+ +-------------+---------------+
                |                               |
                |     +-------------------+     |
                +---->| SUPER BOT GATEWAY |<----+
                      | (WA & AI Connector)|
                      +---------+---------+
                                |
                                v
         +----------------------------------------------+
         |              SUPER BOT WHATSAPP              |
         |  • Multi-Provider LLM (Groq, OpenAI, Gemini) |
         |  • Dynamic Persona follows Client App        |
         |  • External App Context + RAG KB             |
         |  • Human Handover & Operating Hours Rules    |
         |  • Group Mentions & Prefix Triggers          |
         +----------------------------------------------+
                                |
                                v
         +----------------------------------------------+
         |             USER / CUSTOMER                  |
         |               (WhatsApp Chat)                |
         +----------------------------------------------+
```

### 1. WhatsApp Gateway — WA In/Out Intermediary
- **WhatsApp Traffic Intermediary**: Manages the entire flow of outgoing (*outbound*) and incoming (*inbound*) messages from third-party applications to WhatsApp.
- **Multi-Device Engine**: Connects multiple WhatsApp accounts/numbers via real-time QR Code scanner (Socket.IO).
- **Central Number (WA Center)**: Designation of the system's primary number to send registration OTPs, 2FA login verification, and emergency notifications.
- **Anti-Ban & Warm-up Mode**: Gradual quota throttling and random delay simulation (3–8 seconds) to protect numbers from the risk of being blocked.
- **Interactive WhatsApp Web Chat**: Live web chat interface with synchronization of messages, media, documents, and audio.

### 2. Gateway Center — AI Intermediary & External Application Integration
- **AI Middleware Hub**: Bridges external applications (ERP, CRM, Web Portal, LMS) to leverage centralized AI-Integrations.
- **Unique Credentials Per Application**: Generates and manages a unique `X-API-Key` for each client system.
- **Webhook Context Provider**: When the Super BOT processes a message, the Gateway Center automatically calls the client application's webhook endpoint (`/ai-assistant/context` or `action=ai_context`) to inject the user's *live* data (balance, order status, attendance, support tickets).
- **Number Allocation & Smart Failover**: Maps which applications are entitled to use specific WhatsApp numbers with automatic redirection if the primary number is offline.

### 3. Super BOT Gateway — Connector between WA Gateway & AI Integrations
- **Seamless Fusion**: Takes messages from the WhatsApp Gateway $\rightarrow$ retrieves context from the Gateway Center and RAG documents $\rightarrow$ processes them with the selected LLM $\rightarrow$ sends an intelligent reply back to the WhatsApp user.
- **Leading Multi-Provider AI**:
  * **Groq**: Llama 3.3 70B, Mixtral 8x7B (ultra-fast inference < 1 second).
  * **OpenAI**: GPT-4o, GPT-4 Turbo, GPT-4o-mini.
  * **Google Gemini**: Gemini 2.0 Flash, Gemini 1.5 Pro.
  * **Anthropic**: Claude 3.5 Sonnet, Claude 3.5 Haiku.
  * **DeepSeek**: DeepSeek Chat V3, DeepSeek Reasoner R1.
  * **OpenRouter**: Access to 100+ global AI models.
  * **Qwen / Alibaba**: Qwen 2.5 72B.
- **Automatic Model Detection**: Dynamically checks model availability directly from the vendor API Key.
- **Adaptive Persona Presets**:
  * **CS Mode**: Empathetic, friendly, and solution-oriented customer service.
  * **Guest Receptionist**: Receptionist and digital guest book registration.
  * **Application Integration AI**: Dynamically adopts the identity, name, description, and rules of the client system registered in the Gateway Center.
  * **Custom Mode**: Unrestricted free configuration.
- **Human Handover**: Automatically stops the bot and transfers the conversation to a human admin if the user types an escalation keyword (e.g., *admin, operator, human help*).
- **Operating Hours**: Configures the bot's active hours and sends a follow-up confirmation message outside operating hours.
- **Group AI Assistant**: Replies to messages in WhatsApp groups triggered by mention (`@bot`) or prefix (`!ai`).

---

## Key Features & RAG Advantages

### 1. RAG (Retrieval-Augmented Generation) Knowledge Base Hub
- **Multi-Format Input**: Supports text files, markdown, PDF, policy documents, product manuals, and FAQs.
- **Per-Client-Application Document Isolation**: Each application in the Gateway Center has its own dedicated RAG document repository, in addition to the general documents that apply to all bots.
- **Smart Keyword & Token Limiter Algorithm**: Filters and limits the most relevant text chunks to prevent HTTP 413 token limits on AI vendors.
- **Anti-Hallucination**: Answers generated by the bot always refer to the company's official documents.

### 2. Broadcast & Mass Campaigns
- Queue-based mass messaging (*database queue*).
- **Spintax** engine (`{Halo|Hai|Selamat pagi}`) and dynamic variables (`{name}`, `{phone}`).
- **Automatic Blacklist** system: Protects customer numbers who request to unsubscribe.
- Compliance Rules: Automatic validation of number format and sending hours (avoiding nighttime rest hours).

### 3. Complete Monitoring & Logs
- **Real-time Dashboard**: 7-day message trend charts, 4 KPI cards, server health status (CPU, RAM, Disk Space).
- **Message History**: Filter by status (*sent, delivered, read, failed*), message direction (*incoming/outgoing*), and device number.
- **Activity & Gateway Audit Log**: Complete audit trail of every authentication, device, broadcast, and API access action.

---

## 3D Neumorphic Design & Instant Lazy Loading SPA

This application is designed with a modern **3D Neumorphic** aesthetic (like the *IndoTech Management* application standard) and operates as a pure **Full Single Page Application (SPA)**:

1. **3D Tactile Visual Style**:
   - **3D Cards**: Uses bevel shadows (`border-b-2 border-r-2`, `shadow-md shadow-slate-200/50 dark:shadow-black/40`), modern rounded corners (`rounded-2xl`), and a subtle lift effect on hover.
   - **3D Buttons (Physical Press State)**: Buttons feature a thick bevel (`border-b-4`) with bold color gradients and a physical mechanical press effect (`active:translate-y-1 active:border-b-0 active:shadow-inner`).
   - **3D Inset Input Forms**: Input fields with inset shadows that are comfortable for the eyes and glare-free.
2. **Instant Lazy Loading SPA (Shadow Loading)**:
   - Page transitions happen instantly without browser reload using Livewire 3 `wire:navigate`.
   - If backend data requires processing time, the system automatically renders an animated **shadow loading** (*skeleton shadow placeholder*) with a subtle pulse animation, maintaining comfortable, flicker-free navigation.
3. **SPA Custom Error Handling**:
   - All HTTP status codes (401, 403, 404, 419 Page Expired, 429 Rate Limit, 500 Server Error, 503 Maintenance) are handled elegantly within the SPA layout.
   - No more blank pages, raw JSON crashes, or forced page reloads. Users are given human-friendly messages and recovery action buttons.
4. **Native Offline Support & PWA**:
   - Real-time network connection detection via Service Worker (`public/sw.js`) and browser events (`offline`/`online`).
   - Visual network status indicator and a dedicated offline page (`public/offline.html`).
   - Equipped with `manifest.webmanifest`, complete icons (192x192, 512x512, maskable, apple-touch-icon 180x180), and a native app installation prompt (*Install App*).
5. **Zero-Flash Theme System**:
   - Follows the operating system (OS) theme by default, switchable to Dark Mode or Light Mode via a single cycle button.
6. **Multi-Language Support**:
   - Indonesian (`id`) as default and English (`en`), synchronized via the permanent `it_lang` cookie.

---

## Hierarchical Access Rights & Shared Quota Pool

The system implements a 3-tier hierarchy model (**SuperAdmin**, **Admin**, **User**) to ensure security and data isolation:

1. **Admin Menu Access Restrictions**:
   - Admin **DOES NOT HAVE ACCESS** to the **System Settings (`/settings`)**, **Activity Log (`/logs`)**, and **Gateway Server Log (`/gateway-logs`)** menus. These three menus are exclusively for **SuperAdmin**.
   - The menus are hidden from the Admin sidebar and protected by the `role:SuperAdmin` route middleware and `abort_unless(auth()->user()?->isSuperAdmin(), 403)`.
2. **Message & Notification Isolation (Message Log & Top Navbar)**:
   - **SuperAdmin**: Has full visibility into all system messages and notifications.
   - **Admin**: Can only view the message logs & history belonging to themselves and the User accounts under their supervision (`parent_id = admin_id`). The notification dropdown in the top navbar only shows activity from that Admin account and its subordinates.
   - **User**: Can only view messages and notifications belonging to their own account.
3. **Shared Resource Pool Mechanism**:
   - The message quota, device limit, client apps, auto-reply, and broadcast owned by an Admin function as a **Shared Pool** for the Admin and all Users under them.
   - Messages sent by subordinate Users will deduct that User's remaining quota while also accumulating shared usage in their parent Admin account.
   - When an Admin creates a new user, it automatically has the status of a subordinate **`User`** (`parent_id = Auth::id()`).

---

## Authentication & Independent WA Center OTP

1. **Separation of Registration Settings & OTP Login**:
   - The activation status of public self-registration (Registration On/Off) is separated from the OTP Login status.
   - **Login with OTP is always active** immediately after the SuperAdmin designates a device number as the **System OTP Number (WA Center)**.
2. **Automatic Self-Registration Acts as User**:
   - Users who register independently via `/register` automatically have the **`User`** role (not Admin).
3. **Emergency Handling for WA Center Offline (Emergency Access Bypass)**:
   - If the WhatsApp Center number is disconnected/offline, users with administrative roles (**SuperAdmin** and **Admin**) are allowed to log in directly using a password (*direct password fallback*) with a yellow warning to immediately reconnect WhatsApp. This prevents an *administrative lockout*.
   - Users with the regular **User** role are still required to wait for the OTP device to be online for data security.
4. **Progressive Lockout Rate Limiter**:
   - Tiered anti-brute force locking (Tier 1: 3 failed attempts = 60 seconds; Tier 2: 6 failed attempts = 180 seconds; Tier 3: 9 failed attempts = 300 seconds) with a real-time visual countdown in the interface.

---

## Technology (Tech Stack)

### Backend (PHP / Laravel)
- **Laravel 12.x** (PHP 8.2+)
- **Livewire 3.x** (Full SPA reactive components)
- **MariaDB / MySQL** (InnoDB Engine)
- **Laravel Pint** (PSR-12 code standardization)
- **PHPUnit 11** (Comprehensive unit and feature test suite)

### Frontend & Styling
- **Tailwind CSS v4** (CSS-first engine `@theme`)
- **Alpine.js v3** (Micro-reactivity)
- **Google Material Symbols Outlined**
- **Vite** (Asset bundler with thread optimization `GOMAXPROCS=1`)

### Node.js WhatsApp Engine (`server-wa/`)
- **Node.js >= 18.x**
- **@whiskeysockets/baileys** (WhatsApp Multi-Device socket engine)
- **Express.js** & **Socket.IO v4.7.5**
- **Continuous Watchdog & Auto-Healer**

---

## Installation & Seeder Guide

### 1. System Requirements
- PHP >= 8.2 (extensions: `pdo_mysql`, `curl`, `mbstring`, `openssl`, `fileinfo`)
- Composer >= 2.x
- Node.js >= 18.x & npm
- MariaDB >= 10.5 or MySQL >= 8.0

### 2. Quick Installation Steps

```bash
# 1. Enter the project directory
cd /path/ke/WA-Gateway

# 2. Install main PHP & Node dependencies
composer install
npm install

# 3. Configure the environment file
cp .env.example .env
php artisan key:generate

# 4. Migrate the database
php artisan migrate --force
php artisan storage:link

# 5. Run the Initial Account Seeder (credentials are not published)
php artisan db:seed

# 6. Install Node.js WhatsApp Server dependencies
cd server-wa && npm ci && cd ..

# 7. Compile frontend assets
npm run build
```

### 3. Initial Accounts (Credentials Not Published)

After running `php artisan db:seed`, the system creates **SuperAdmin** and **Admin** accounts. The email and password are **not published**; use the credentials you define yourself during seeding.

| Role | Description |
|------|------------|
| **SuperAdmin** | Full access to the entire system, server settings, user management, and audit log. |
| **Admin** | Management of gateways, broadcasts, client apps, and Super BOT. |

---

## Deployment Guide (VPS & Shared Hosting)

### A. Dedicated VPS (Nginx + Systemd)

#### 1. Nginx Reverse Proxy Configuration
```nginx
server {
    listen 443 ssl http2;
    server_name wa.domainanda.com;
    root /var/www/wa-gateway/public;
    index index.php;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    # Proxy WebSocket & Socket.IO to Node.js
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

#### 2. Systemd Service (`/etc/systemd/system/wa-server.service`)
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

Run the following commands:
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now wa-server
```

---

### B. Shared Hosting (cPanel / CloudPanel)

This application can run independently on shared hosting without root / systemd access:

```
Browser --https://domainanda.com--> Apache (PHP / Laravel)
   |                                     |
   |  /socket.io (websocket+polling)     |  http://127.0.0.1:3998 (internal)
   +----------------> Apache -----------+--> Node.js WA Server --> WhatsApp
                    (proxy .htaccess)              |
                                                   +--> MariaDB (shared with Laravel)
```

1. **Run the Engine via cPanel Cron Job (every minute)**:
   ```bash
   * * * * * /home/username/public_html/server-wa/keepalive.sh >> /home/username/public_html/storage/logs/wa-cron.log 2>&1
   ```
   *The `keepalive.sh` script detects the running PID and port. If the process stops, it is automatically restarted in the background.*

2. **Laravel Queue & Scheduler Cron Jobs**:
   ```bash
   * * * * * cd /home/username/public_html && php artisan schedule:run >> /dev/null 2>&1
   * * * * * cd /home/username/public_html && php artisan queue:work --stop-when-empty --max-time=50 >> /dev/null 2>&1
   ```

3. **Independent Restart via CLI**:
   ```bash
   ./restart.sh
   ```

---

## Complete REST API Documentation

All API requests use `X-API-Key` header authentication. API Keys can be created in the **Gateway Center (Client Apps)** menu.

Base URL: `https://domainanda.com` (Standard port 443 via reverse proxy).

### 1. Send Text Message (Outbound WA Gateway)
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

### 2. Send Image / Media Message
- **Endpoint**: `POST /api/send-image`
- **Payload**:
```json
{
  "to": "081234567890",
  "image": "https://domainanda.com/storage/invoice-bukti.jpg",
  "caption": "Bukti Pembayaran Invoice #10294"
}
```

### 3. Send PDF Document
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

### 4. Call Super BOT Intelligence Directly (AI Chat API)
Your client application can directly call the AI brain of the Super BOT centrally:
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

### 5. Check Available WhatsApp Devices
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

### 6. Gateway Health Check
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

## Advanced Documentation Modules (Docs)

In-depth technical documentation and per-module architecture specifications can be accessed in the [`docs/`](docs/README.en.md) directory:

- 📘 **[Module 01: Tri-Gateway Architecture & Super BOT](docs/01-arsitektur-tri-gateway.en.md)**: Details of the separation of duties between the Gateway Center (AI intermediary), WhatsApp Gateway (WA in/out intermediary), and Super Bot Gateway (AI + RAG connector).
- 🔐 **[Module 02: Hierarchical Access Rights & Shared Quota Pool](docs/02-hak-akses-dan-shared-quota.en.md)**: Role matrix for SuperAdmin, Admin, and User; protection of sensitive menus; shared pool quota restrictions; and isolation of message logs and notifications.
- 📱 **[Module 03: Authentication & WA Center OTP](docs/03-autentikasi-dan-otp-wa-center.en.md)**: Designation of the WA Center number, separation of public registration, permanent 2FA OTP login flow, and emergency bypass when the device is offline.
- 🎨 **[Module 04: 3D Neumorphic Design & Instant Lazy Loading SPA](docs/04-ui-3d-dan-lazy-loading-spa.en.md)**: 3D visual design (card, button, inset input), wire:navigate SPA without reload, shadow loading (skeleton loader), custom error handling, and PWA.
- 🔌 **[Module 05: Dynamic Application Integration & REST API](docs/05-integrasi-aplikasi-dinamis.en.md)**: Dynamic integration guide without hardcode, live data context webhook protocol (ERP, CRM, LMS, HR), and complete REST API documentation.

---

## Troubleshooting & Solutions

### 1. Socket.IO Error / QR Code Not Appearing
- **Cause**: The browser cannot reach the Node.js port or Apache `mod_proxy` is not enabled.
- **Solution**: In the **System Settings > General** menu, make sure *Socket.io Public URL* is left empty if using an Apache / Nginx reverse proxy. If the hosting prohibits reverse proxy, open port 3998 and fill in the full public URL (`https://domainanda.com:3998`).

### 2. Thread Limit Error / Vite Build (`errno=11: fatal error: newosproc`)
- **Cause**: Process thread count limit in the CloudLinux / cPanel LVE environment.
- **Solution**: The build script has been optimized with `GOMAXPROCS=1 npm run build` to limit esbuild thread usage.

### 3. WhatsApp Device Disconnects by Itself (`Error 440 conflict`)
- **Cause**: There are two Node.js processes reading the Baileys credential session folder simultaneously.
- **Solution**: Stop the old process with `./server-wa/keepalive.sh stop` or `./restart.sh`, then ensure only one `keepalive.sh` process is running.

### 4. Running the Test Suite Trial
To validate that the entire system is running perfectly:
```bash
php artisan test --compact
```
*(All 80 unit & feature tests covering Super BOT, RAG, SPA Error Handler, Anti-Ban, Hierarchical Access Rights, Shared Pool Quota, WA Center OTP, and 3D Theme are 100% passed with 400 assertions).*

---

## Access & License

This repository is **my own work**, **Mochamad Ismail Soleh** (Ismail-IndoTech), and is specifically licensed under the **[MIT License](LICENSE)**.

To obtain access to the **private source repository** for this project, contact me as the **Repository Creator** via WhatsApp using the number registered to my DANA account: **08121600058** — [wa.me/628121600058](https://wa.me/628121600058). Access is granted by agreement.
## 💖 Contribution & Donation

If this application simplifies IT operations, DevOps, and server management in your company or personal projects, support its development by becoming a contributor or a **Special Donor**.  
*(Official donors and sponsors will have their company logo listed in this repository).*

Support ongoing maintenance, research into new panel bypass modules, and system updates by donating via:

<div align="center">

**DANA — 08121600058** (on behalf of Mochamad Ismail Soleh)

<img src="docs/qris.jpg" alt="QRIS DANA IndoTech" width="280">

*Scan with the DANA, BCA, Mandiri, GoPay, OVO, or any QRIS payment application in Indonesia.*

</div>

All funds are allocated for research server infrastructure costs, testing tool licenses, and IndoTech open-source module development. If you or your institution would like to become an official partner or sponsor, please contact us. Thank you for your support!

---

## License
Copyright &copy; 2026 IndoTech. Distributed under the MIT license.