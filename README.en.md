<div align="center">

<img src="./icon-512.png" alt="STOC Admin Logo" width="100" height="100" />

# 🔐 STOC Manager — Admin Panel

**A PWA license management dashboard for the STOC app**

[![PWA](https://img.shields.io/badge/PWA-Ready-blueviolet?style=for-the-badge&logo=pwa)](.)
[![Supabase](https://img.shields.io/badge/Supabase-Backend-3ECF8E?style=for-the-badge&logo=supabase)](.)
[![Version](https://img.shields.io/badge/Version-3.2.0-gold?style=for-the-badge)](.)
[![License](https://img.shields.io/badge/License-Proprietary-red?style=for-the-badge)](.)

[🔗 STOC PWA](https://github.com/Ayad-Mounir/STOC-PWA) · [🐛 Report a Bug](https://github.com/Ayad-Mounir/ADMIN-STOC/issues)

---

</div>

## 🏗️ Overview

**STOC Manager** is an admin-only PWA dashboard for full control over [STOC PWA](https://github.com/Ayad-Mounir/STOC-PWA) licenses. It lets you create companies, generate activation links and QR codes, monitor subscription status, and manage permissions — all from your phone.

---

## ✨ Features

### 📊 Stats Dashboard
- Count of active, frozen, and expired licenses
- Quick overview of all company statuses

### 🏢 Company Management
- Card view per company: name, license status, expiry date, device count
- **Instant search & filter** across all companies
- **Freeze / Unfreeze** license with one tap
- **Extend subscription** with preset durations or a custom date
- Edit **device limit** per company
- Delete company with double confirmation

### 🔑 Activation Link Generator
- **Step-by-step wizard** for onboarding a new company:
  1. Create a Supabase project for the company
  2. Automatic Supabase credentials verification
  3. Set device limit and permissions
  4. Generate activation link + printable QR code
- **Copy link** or **download QR** instantly
- SHA-256 cryptographic signature on every license

### 📡 Heartbeat — Connection Monitor
- Auto-ping system to prevent Supabase Free Tier from sleeping
- **Ping All** or manually ping a specific company
- Status indicator per company: connected / expired / frozen

### 📲 PWA
- Installable on mobile and desktop
- Password-protected lock screen for admin access
- Offline viewing support with sync on reconnect

---

## 🛠️ Tech Stack

| Technology | Usage |
|------------|-------|
| **Vanilla JS** | No frameworks — maximum speed |
| **Supabase JS v2** | Read/write `stoc_licenses` table |
| **QRCode.js** | Generate activation QR codes |
| **Web Crypto API** | SHA-256 license signing |
| **Service Worker** | PWA caching |

### Project Structure

```
ADMIN-STOC/
├── index.html      # Entire app (single-file)
├── manifest.json   # PWA config
├── sw.js           # Service Worker
├── icon-192.png
└── icon-512.png
```

> The entire app lives in a single `index.html` — no dependencies or build step required.

---

## 🚀 Deployment

### Requirements
- A Supabase account with a `stoc_licenses` table (auto-created on first launch)
- Any HTTPS host (GitHub Pages, Netlify...)

### Setup

**1. Deploy to HTTPS:**
```bash
git clone https://github.com/Ayad-Mounir/ADMIN-STOC.git
# Push to GitHub Pages or any static host
```

**2. On first launch:**
- Enter your **Supabase URL** and **Anon Key** for the STOC Admin project
- The app will auto-verify the `stoc_licenses` table and create it if missing

**3. Manual table creation (optional):**
```sql
CREATE TABLE IF NOT EXISTS stoc_licenses (
  code TEXT PRIMARY KEY,
  frozen BOOLEAN DEFAULT FALSE,
  expires TIMESTAMPTZ,
  last_ping TIMESTAMPTZ
);

ALTER TABLE stoc_licenses ENABLE ROW LEVEL SECURITY;

CREATE POLICY "anon_read_only" ON stoc_licenses
  FOR SELECT TO anon USING (true);

CREATE POLICY "admin_full_access" ON stoc_licenses
  FOR ALL USING (true);
```

---

## 📱 Install as App

| Device | How to Install |
|--------|---------------|
| **Android** | Chrome → ⋮ → "Add to Home Screen" |
| **iPhone/iPad** | Safari → Share → "Add to Home Screen" |
| **Windows/Mac** | Chrome/Edge → install icon in address bar |

---

## 🔄 Full Workflow

```
1. Open STOC Manager
2. Tap "+ Add Company"
3. Follow wizard: Supabase → Devices → Sign
4. Send the activation link or QR code to the company
5. Company opens STOC PWA and scans the QR → instant activation ✅
```

---

## 🔐 Security

- Admin lock screen with SHA-256 hashed password
- Every license is cryptographically signed — cannot be forged
- License verification done online via Supabase with offline fallback

---

## 🔗 Related Projects

| Project | Description |
|---------|-------------|
| [STOC PWA](https://github.com/Ayad-Mounir/STOC-PWA) | Inventory management app (client) |
| **STOC Manager** | License control panel (this project) |

---

## 🤝 Contact

- **GitHub:** [@Ayad-Mounir](https://github.com/Ayad-Mounir)
- **Email:** contact.ayad.mounir@gmail.com
- **WhatsApp:** [+212 6 53 86 76 67](https://wa.me/212653867667)

---

## 📄 License

This project is under a proprietary license. All rights reserved © 2025–2026 Ayad Mounir.

---

<div align="center">

**STOC Manager v3.2 — License Control Panel**

</div>
