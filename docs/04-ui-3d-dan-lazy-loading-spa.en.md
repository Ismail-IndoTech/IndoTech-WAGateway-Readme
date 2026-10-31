# Module 04: 3D Neumorphic Design & Instant Lazy Loading SPA

## Introduction

The IndoTech Super WhatsApp BOT Gateway interface is designed to resemble the modern **IndoTech Management** application standard, blending the aesthetic of three-dimensional depth (**Modern 3D Neumorphic**) with instant page-transition performance based on **Full SPA Wire:Navigate** and a *shadow loading* effect (*skeleton shadow*).

---

## 1. 3D Design Standards (Tactile Depth Design)

All card, button, and input form components carry a 3D visual style that provides a physical illusion when interacting:

### A. 3D Cards
- **Distinctive Features**: Soft double shadows at the bottom and right (`border-b-2 border-r-2`), outer shadow elevation (`shadow-md shadow-slate-200/50 dark:shadow-black/40`), and modern rounded corners (`rounded-2xl`).
- **Hover Effect**: Lifts slightly upward (`hover:-translate-y-0.5 hover:shadow-lg transition-all duration-200`) to indicate focus.

### B. 3D Buttons
- **Primary Button**:
  - Uses bold color gradients (`bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-700 hover:to-indigo-700`).
  - Bottom border thickness (`border-b-4 border-blue-800 dark:border-indigo-900`).
  - **Physical Press Effect**: When clicked (`active:translate-y-1 active:border-b-0 active:shadow-inner`), the button moves down as if pressed mechanically.
- **Secondary Button**:
  - Light/dark surface with a border accent `border-b-2 border-slate-300 dark:border-slate-700`.

### C. 3D Inset Input Forms
- Inset shadow effect with a subtle contrasting border (`border border-slate-200 dark:border-slate-700 focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500`).

---

## 2. Instant Page Navigation & Lazy Loading (Shadow Loading)

This application operates as a pure **Full Single Page Application (SPA)** without the need for a full browser *page reload*:

### A. Livewire 3 `wire:navigate`
- Every link transition within the application uses the `wire:navigate` attribute.
- Pages transition instantly in milliseconds because the browser only updates the DOM document without re-downloading CSS/JS files or restarting the window cycle.

### B. "Shadow Loading" Effect (Shadow Skeleton Placeholder)
- When the next page's data requires processing time on the backend, the interface does not *freeze*.
- The system instantly renders a **shadow loading** (*skeleton shadow placeholder*):
  - Boxes animated with a subtle pulse (*pulse animation*) whose proportions exactly resemble the actual statistic cards, table rows, or forms.
  - Provides a smooth *flicker-free experience*.

---

## 3. Custom SPA Error Handling (Custom SPA Error Pages)

In traditional SPA applications, if a server error occurs (e.g., HTTP 500), the browser often displays a blank white screen (*blank screen*) or a raw modal warning.

In IndoTech Super WhatsApp BOT Gateway:
1. **White Screen Prevention**: The `spa-error-handler.js` script captures all *unhandled exceptions*, Livewire AJAX failures, and non-200 HTTP responses.
2. **Integrated SPA Error Pages**:
   - **401 Unauthorized**: Soft redirect to login with the session preserved.
   - **403 Forbidden**: Visual warning that the account role does not have access rights, with a button to return to the dashboard.
   - **404 Not Found**: Search-friendly illustrative page.
   - **419 Page Expired**: Automatic CSRF token refresh dialog without losing typed data.
   - **429 Too Many Requests**: Display of the rate limit wait time.
   - **500 Server Error**: Structured diagnosis page with a *Try Again* button and an option to report to SuperAdmin.
   - **503 Maintenance Mode**: Periodic maintenance information display.

---

## 4. Offline Mode & PWA Support

- **Service Worker (`public/sw.js`)**: Caches important static assets so the application can still open even if the internet connection suddenly drops.
- **Network Status Indicator**: An indicator banner appears automatically when the device's network status changes to `offline`, and turns green again when `online`.
- **Fallback Offline Page (`public/offline.html`)**: Guides users when trying to access a new route without an internet connection.
- **PWA (Progressive Web App)**: Users can install the application directly to the home screen of Android, iOS, Windows, or macOS via the native install button prompt.
