# Official Documentation of IndoTech Super WhatsApp BOT Gateway

Welcome to the official documentation center of **IndoTech Super WhatsApp BOT Gateway**. This documentation presents in-depth guides on the system architecture, hierarchical access rights distribution, shared quota system (*shared pool*), WA Center OTP authentication, 3D Neumorphic interface, and dynamic integration of external applications.

---

## 📚 Documentation Module List

| No | Documentation Module | Description |
|---|---|---|
| 01 | [Tri-Gateway Architecture & Super BOT](01-arsitektur-tri-gateway.en.md) | Explanation of the functions of the 3 intermediary pillars (Gateway Center, WhatsApp Gateway, Super Bot Gateway) and the RAG Knowledge Base. |
| 02 | [Hierarchical Access Rights & Shared Quota Pool](02-hak-akses-dan-shared-quota.en.md) | Role restrictions for SuperAdmin, Admin, and User; shared quota mechanism (*shared pool*); log & notification isolation. |
| 03 | [Authentication & WA Center OTP](03-autentikasi-dan-otp-wa-center.en.md) | Designation of the WA Center number for OTP, separation of public registration, and emergency mechanism (*emergency bypass*). |
| 04 | [3D Neumorphic Design & Instant Lazy Loading SPA](04-ui-3d-dan-lazy-loading-spa.en.md) | 3D style standards (card, button, input) and instant wire:navigate transitions with shadow placeholder (*shadow skeleton*). |
| 05 | [Dynamic Application Integration & REST API](05-integrasi-aplikasi-dinamis.en.md) | Guide to connecting client applications (ERP, CRM, LMS, HR) dynamically without hardcode via the Gateway Center. |

---

## 🌟 Quick Summary of Application Identity

This application is designed as a **Super WhatsApp BOT Gateway**:
1. **Gateway Center**: Intermediary for external applications for AI-Integrations (centralized AI intelligence intermediary).
2. **WhatsApp Gateway**: Intermediary for inbound and outbound (*inbound/outbound*) multi-device WhatsApp communication.
3. **Super Bot Gateway**: The connector between the WhatsApp Gateway and AI-Integrations + RAG Knowledge Base that turns WhatsApp into an intelligent bot assistant across various business fields.
4. **Full SPA & Offline Ready**: Built using Laravel 12 + Livewire 3 + Alpine.js + Service Worker for an instant experience without screen refresh.
5. **Multi-Tenancy Hierarchy**: Support for SuperAdmin, Admin, and User with strict data isolation and flexible quota distribution.

---

## 💡 Quick Navigation
- Return to the main guide: [Project Main README](../README.en.md)
