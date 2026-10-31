# Module 02: Hierarchical Access Rights, Log Isolation & Shared Quota Pool

## Introduction

To securely support multi-organization, multi-division, and multi-client scenarios, IndoTech Super WhatsApp BOT Gateway implements a 3-tier access rights system (*three-tier hierarchy*): **SuperAdmin**, **Admin**, and **User**.

---

## 1. Role Access Rights Matrix (Role Matrix)

| Module / Feature | SuperAdmin | Admin | User |
|---|:---:|:---:|:---:|
| **Statistics Dashboard** | ✅ Global | ✅ Scoped (Account + Subordinates) | ✅ Own |
| **Gateway Center (Client Apps)** | ✅ Full | ✅ Scoped (Based on Limit) | ❌ |
| **WhatsApp Devices** | ✅ All Devices | ✅ Own Devices | ✅ Personal / Assigned Devices |
| **Super BOT & Persona** | ✅ Full | ✅ Full | ❌ |
| **Auto-Reply Rules** | ✅ Full | ✅ Scoped | ❌ |
| **Broadcast Campaigns** | ✅ Full | ✅ Scoped | ❌ |
| **Interactive Chat** | ✅ Full | ✅ Full | ✅ Per Device |
| **Message Log & History** | ✅ Entire System | 🔒 Only Own Account & Subordinates | 🔒 Only Own |
| **User Management (Users)** | ✅ All Accounts | 🔒 Only Subordinates (Parent-Child) | ❌ |
| **Top Navbar Notifications** | ✅ Global | 🔒 Relevant Account & Subordinates | 🔒 Relevant Own Account |
| **System Settings (Settings)** | ✅ Full Access | 🚫 Prohibited (403 Forbidden) | 🚫 Prohibited (403 Forbidden) |
| **Activity Audit Logs** | ✅ Full Access | 🚫 Prohibited (403 Forbidden) | 🚫 Prohibited (403 Forbidden) |
| **Gateway Server Logs** | ✅ Full Access | 🚫 Prohibited (403 Forbidden) | 🚫 Prohibited (403 Forbidden) |

---

## 2. Strict Restrictions for Admin Accounts

Accounts with the **Admin** role have strict restrictions to protect core system configuration and platform confidentiality:
1. **No Access to System Settings (`/settings`)**:
   - The menu is hidden from the sidebar.
   - Access via direct URL is blocked with `403 Forbidden` at the Middleware level (`middleware('role:SuperAdmin')`) and the Livewire component `mount()`.
2. **No Access to Activity Log (`/logs`)**:
   - The system's global audit log is reserved only for SuperAdmin to track all platform activity.
3. **No Access to Gateway Server Log (`/gateway-logs`)**:
   - Low-level Node.js console logs and Baileys socket processes are not exposed to Admin.

---

## 3. Message Log & History Isolation

Recording of outgoing and incoming messages (*Message History*) is strictly isolated based on the account hierarchy through the `MessageHistory::visibleToUser($user)` helper:
- **SuperAdmin**: Has 100% visibility into all messages sent and received by all accounts in the system.
- **Admin**: Can only review message logs sent by the Admin account itself and by User accounts directly under its guidance (`parent_id = admin_id`). Admin cannot see messages belonging to other Admins or SuperAdmin.
- **User**: Can only review message history sent by itself (`user_id = user_id`).

---

## 4. Top Navbar Notification Isolation

The notification dropdown on the top navigation header filters activity items based on the user's role:
- **SuperAdmin**: Displays a comprehensive system notification summary as well as the *"View All Logs"* button.
- **Admin**: Displays login, broadcast delivery, or quota warning notifications originating only from itself and all Users under it. The *"View All Logs"* button is hidden.
- **User**: Displays private notifications related to its own account and personal devices.

---

## 5. Shared Quota Pool System

IndoTech WhatsApp Gateway applies the **Shared Resource Pool** concept between an Admin and the child User accounts under them:

```
                  +---------------------------------+
                  |       PARENT ADMIN (Pool)       |
                  |   Message Quota: 10,000 / Month |
                  |   Device Limit: 5 Devices       |
                  +---------------+-----------------+
                                  |
         +------------------------+------------------------+
         v                        v                        v
 +-----------------+      +-----------------+      +-----------------+
 |     USER A      |      |     USER B      |      |     USER C      |
 | Limit: 2,000    |      | Limit: 3,000    |      | Limit: 1,000    |
 | Used: 1,200     |      | Used: 2,800     |      | Used: 400       |
 +-----------------+      +-----------------+      +-----------------+
         |                        |                        |
         +------------------------+------------------------+
                                  |
                                  v
                   Total Shared Usage: 4,400 / 10,000
```

### Shared Quota Rules:
1. **Message Usage (Shared Message Quota)**:
   - Admin has a total message quota (e.g., 10,000 messages/month).
   - Every message sent by the Admin or by any of its subordinate Users will increase the accumulated `shared_message_used`.
   - User is limited by 2 conditions:
     1. Its individual quota is not yet exhausted (`user.message_used < user.message_quota`).
     2. Its parent Admin's shared quota is still available (`admin.getSharedMessageUsed() < admin.message_quota`).
2. **Device & Feature Limits**:
   - The number of active devices, client apps, auto-replies, and broadcast campaigns created by a User are counted toward the maximum limit (*limit pool*) of the parent Admin.
3. **New User Registration by Admin**:
   - When an Admin creates a new account via the *Users & Access Rights* menu, the system automatically sets `parent_id = admin_id` and locks that account's role as **`User`**. Admin cannot elevate a subordinate's role to Admin or SuperAdmin.
