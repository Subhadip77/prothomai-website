# Prothom Analytica — Website

Official website for Prothom Analytica India Pvt. Ltd.
Built with Next.js (App Router) + Tailwind CSS v4 + MongoDB.

---

## Tech Stack

| Layer      | Tech                          |
|------------|-------------------------------|
| Framework  | Next.js 16 (App Router, no TS)|
| Styling    | Tailwind CSS v4 + custom CSS  |
| Font       | Manrope (next/font/google)    |
| Database   | MongoDB Atlas + Mongoose      |
| Deployment | Vercel                        |

---

## Project Structure

```
prothom-analytica/
├── app/
│   ├── globals.css              # All design tokens + base styles
│   ├── layout.js                # Manrope font + SEO metadata + JSON-LD
│   ├── page.js                  # One-page — all sections assembled here
│   └── api/
│       └── contact/
│           └── route.js         # POST → saves to MongoDB
│
├── components/
│   └── sections/
│       ├── Navbar.js            # Sticky nav — full logo → icon on scroll
│       ├── Hero.js              # Headline + India map + pulse dots
│       ├── WhatWeSee.js         # 6 problem cards — asymmetric grid
│       ├── HowWeThink.js        # 3 principles — line-draw animation
│       ├── AboutStrip.js        # Prothom name meaning — Bengali + English
│       ├── Ecosystem.js         # YPark / YPartner / YAdmin — 3 product cards
│       ├── Contact.js           # Form + contact details + MongoDB submit
│       └── Footer.js            # Dark footer — single row
│
├── lib/
│   ├── mongodb.js               # MongoDB connection (cached)
│   └── models/
│       └── Contact.model.js     # Mongoose schema for contact form
│
├── public/
│   ├── logo-full.png            # Full logo — icon + "Prothom Analytica" wordmark
│   ├── logo-icon.png            # Cube icon only — used on scroll + favicon
│   ├── indiamap.png             # India map — used in Hero right side
│   └── favicon.ico              # Cube icon
│
└── .env.local                   # Never commit this file
```

---

## Environment Variables

Create `.env.local` in the project root:

```env
MONGODB_URI=mongodb+srv://username:password@cluster.xxxxx.mongodb.net/prothom-analytica?retryWrites=true&w=majority
```

### How to get MongoDB URI

1. Go to [mongodb.com/atlas](https://mongodb.com/atlas)
2. Create free account → Create M0 cluster (free)
3. Database Access → Add user → username + password
4. Network Access → Add IP → `0.0.0.0/0`
5. Cluster → Connect → Drivers → copy connection string
6. Replace `<password>` with your actual password
7. Paste into `.env.local`

---

## Getting Started

```bash
# Install dependencies
npm install

# Run development server
npm run dev
# → open http://localhost:3000

# Build for production
npm run build

# Start production server
npm start
```

---

## One-Page Sections — Order & IDs

| # | Section       | Component       | Anchor ID        | Background   |
|---|---------------|-----------------|------------------|--------------|
| 1 | Hero          | Hero.js         | #hero            | #FFFFFF      |
| 2 | What We See   | WhatWeSee.js    | #what-we-see     | #F8F9FB      |
| 3 | How We Think  | HowWeThink.js   | #how-we-think    | #FFFFFF      |
| 4 | Who We Are    | AboutStrip.js   | #who-we-are      | #F8F9FB      |
| 5 | What We Build | Ecosystem.js    | #what-we-build   | #FFFFFF      |
| 6 | Get in Touch  | Contact.js      | #contact         | #F8F9FB      |
| 7 | Footer        | Footer.js       | —                | #0A2540      |

---

## Navigation Links (Navbar)

```
What We See   → #what-we-see
How We Think  → #how-we-think
Who We Are    → #who-we-are
What We Build → #what-we-build
```

External links in navbar:
- Visit YPark → https://ypark.in
- Get in Touch → #contact

---

## Color System

All colors are CSS variables defined in `globals.css`:

| Variable           | Value     | Usage                          |
|--------------------|-----------|--------------------------------|
| --bg-page          | #FFFFFF   | Main page background           |
| --bg-surface       | #F8F9FB   | Alternate section background   |
| --bg-card          | #FFFFFF   | Card background                |
| --accent           | #0F4CBB   | Brand blue — CTAs, links, icons|
| --accent-hover     | #0D3FA0   | Button hover                   |
| --accent-tint      | #E8EFFE   | Badge bg, highlight bg         |
| --text-heading     | #1E293B   | All headings                   |
| --text-body        | #64748B   | Body paragraphs                |
| --text-muted       | #94A3B8   | Captions, meta, placeholders   |
| --border           | #E2E8F0   | Card borders, dividers         |
| --footer-bg        | #0A2540   | Footer background only         |

---

## Font

**Manrope** — loaded via `next/font/google` (self-hosted, no CDN).

```js
// app/layout.js
import { Manrope } from 'next/font/google'

const manrope = Manrope({
  subsets:  ['latin'],
  weight:   ['400', '500', '600', '700', '800'],
  variable: '--font-sans',
  display:  'swap',
})
```

Bengali text in AboutStrip uses **Noto Sans Bengali** — loaded inline
via Google Fonts `@import` only for that section.

---

## Animation System

All scroll animations use `IntersectionObserver` — no library needed.

| Class          | Effect                        | Trigger        |
|----------------|-------------------------------|----------------|
| .reveal        | Fade up (translateY 20px → 0) | .visible added |
| .reveal-left   | Slide from left               | .visible added |
| .reveal-right  | Slide from right              | .visible added |
| .reveal-line   | Line draws left → right       | .visible added |
| .delay-1 to 6  | Stagger delays (80ms steps)   | Combined       |

Delays: 80ms / 160ms / 240ms / 320ms / 400ms / 480ms

---

## Contact Form — MongoDB

Form fields saved to MongoDB:

| Field   | Required | Notes                      |
|---------|----------|----------------------------|
| name    | Yes      | Trimmed                    |
| email   | Yes      | Lowercase, validated       |
| phone   | No       | Optional                   |
| subject | No       | Optional                   |
| message | Yes      | Trimmed                    |
| source  | Auto     | Default: "prothomai.com"   |
| createdAt | Auto   | Mongoose timestamps        |

API endpoint: `POST /api/contact`

---

## Logo Files Required in /public

| File           | Used in                              |
|----------------|--------------------------------------|
| logo-full.png  | Navbar — visible at top of page      |
| logo-icon.png  | Navbar — visible after 80px scroll   |
| indiamap.png   | Hero section right side              |
| favicon.ico    | Browser tab + bookmarks              |

---

## Deployment — Vercel

1. Push to GitHub
2. Import repo in [vercel.com](https://vercel.com)
3. Add environment variable:
   - Key: `MONGODB_URI`
   - Value: your Atlas connection string
4. Deploy

---

## What NOT to change

- Do not add `#0F4CBB` to YPark — that is Prothom's brand color
- Do not use centered hero text — always left-aligned
- Do not install a separate font — Manrope only, via next/font
- Do not use emojis anywhere on the site
- Do not add `@tailwind base/components/utilities` — this is Tailwind v4, use `@import "tailwindcss"` if needed

---

## Contact Details (update these)

Open `components/sections/Contact.js` and update:

```
Email:   hello@prothomai.com      ← replace with real email
Phone:   +91 98765 43210          ← replace with real number
Address: Kolkata, West Bengal     ← correct
LinkedIn: /company/prothom-analytica ← verify URL
```

---

*Prothom Analytica India Pvt. Ltd. — Kolkata, West Bengal, India*
