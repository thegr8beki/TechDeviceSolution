# TechDeviceSolution

> Premium technology service platform for **TechDeviceSolution** — repair, IT support, networking, cybersecurity, and cloud / data services. Owner: **Bereket Mihretu** · Adama, Ethiopia.

A production-ready, Apple-grade, fully responsive web app built with **Next.js 14**, **TypeScript**, **Tailwind CSS**, **Prisma + PostgreSQL**, and **NextAuth**. Includes a multi-step booking flow, customer dashboard, admin console with analytics, reviews system, payment integrations (Chapa, Telebirr, Stripe, bank transfer), and a hardened security baseline.

---

## ✨ Highlights

- **Stunning hero & glassmorphism UI** with mesh gradients, micro-interactions, and dark/light theme.
- **Multi-step booking flow** with image uploads, validation, SMS-style confirmation, and auto-generated invoice.
- **Customer dashboard** — bookings, invoices (printable receipts), reviews, support, profile, and 2FA setup.
- **Admin console** — bookings, customers, services, analytics charts, revenue, and reviews moderation.
- **Reviews system** with star ratings, photo uploads, verified-customer badges, and aggregate stats.
- **Auth** — NextAuth.js (Google + email/password) with bcrypt, TOTP-based 2FA, and password reset tokens.
- **Payments** — pluggable adapters for Chapa, Telebirr, Stripe, and bank transfer (mock-mode out of the box).
- **Security** — JWT sessions, role-based middleware, CSP/HSTS/Referrer headers, zod validation, rate limiting.
- **SEO** — dynamic sitemap, robots, OpenGraph, JSON-LD (LocalBusiness, FAQ, Breadcrumb).
- **Accessibility** — skip-link, focus-visible rings, ARIA on toggles/radio groups, keyboard nav.
- **Performance** — Next/Image AVIF/WebP, font subsetting, package import optimization, edge-cached middleware.

---

## 🧱 Tech stack

| Layer        | Choice                                              |
| ------------ | --------------------------------------------------- |
| Framework    | Next.js 14 (App Router)                             |
| Language     | TypeScript 5                                        |
| UI           | Tailwind CSS, Radix Primitives, Framer Motion       |
| Icons        | lucide-react                                        |
| Forms        | react-hook-form + zod                               |
| Charts       | Recharts                                            |
| Auth         | NextAuth.js, bcryptjs, speakeasy (TOTP)             |
| Database     | PostgreSQL via Prisma                               |
| Payments     | Chapa, Telebirr, Stripe, bank transfer              |
| Email        | nodemailer (SMTP)                                   |
| Tooling      | ESLint, Prettier (+ tailwind plugin), tsx           |

---

## 🚀 Quickstart

### 1. Install

```bash
npm install
```

### 2. Environment

Copy the example file and fill in real values:

```bash
cp .env.example .env
```

Required for first boot:

- `DATABASE_URL` — a PostgreSQL connection string.
- `NEXTAUTH_URL` — e.g. `http://localhost:3000` in dev.
- `NEXTAUTH_SECRET` — `openssl rand -base64 32`.

Optional but recommended:

- `GOOGLE_CLIENT_ID` / `GOOGLE_CLIENT_SECRET` for Google sign-in.
- `SMTP_*` for transactional email.
- `CHAPA_SECRET_KEY` / `STRIPE_SECRET_KEY` for live payments (mock mode otherwise).

### 3. Database

```bash
npm run db:generate   # prisma generate
npm run db:push       # create tables in PostgreSQL
npm run db:seed       # categories, services, plans, FAQs, admin user
```

Default admin credentials (created by `db:seed`):

| Email                              | Password        | Role  |
| ---------------------------------- | --------------- | ----- |
| `bereketmihretu62@gmail.com`       | `ChangeMe!2026` | ADMIN |

> ⚠️ Change the admin password immediately after first sign-in.

### 4. Run

```bash
npm run dev
```

Then open **http://localhost:3000**.

---

## 🗺️ Project structure

```
src/
├── app/
│   ├── (marketing)         # home, about, services, pricing, reviews, contact
│   ├── booking/            # multi-step booking flow
│   ├── auth/               # sign-in, sign-up, reset
│   ├── dashboard/          # customer dashboard
│   ├── admin/              # admin console
│   ├── api/                # REST endpoints (bookings, reviews, payments, auth, …)
│   ├── sitemap.ts          # dynamic sitemap.xml
│   ├── robots.ts           # robots.txt
│   └── manifest.ts         # PWA manifest
├── components/
│   ├── home/               # hero, services, why, testimonials, stats, faq, cta
│   ├── site/               # navbar, footer, page-hero, logo, theme-toggle, WhatsApp FAB
│   ├── ui/                 # Button, Card, Input, Tabs, Dialog, Select, …
│   ├── booking/            # booking flow
│   ├── reviews/            # review form
│   ├── dashboard/          # sidebar(s)
│   ├── admin/              # charts
│   ├── contact/            # contact form
│   └── seo/                # JSON-LD helpers
├── lib/
│   ├── auth.ts             # NextAuth options
│   ├── db.ts               # singleton Prisma client
│   ├── payments.ts         # provider adapters (Chapa/Telebirr/Stripe/Bank)
│   ├── rate-limit.ts       # in-memory token-bucket
│   ├── site.ts             # business constants
│   ├── utils.ts            # cn, currency, dates, references
│   └── validators.ts       # zod schemas
└── middleware.ts           # security headers + auth gating
prisma/
├── schema.prisma           # full data model
└── seed.ts                 # demo data + admin user
```

---

## 🔐 Security baseline

- **Hashing** — bcrypt (cost 12) for passwords.
- **2FA** — TOTP via `speakeasy`, base32 secrets stored encrypted-at-rest in your DB.
- **Sessions** — JWT cookies via NextAuth; `role` claim drives RBAC.
- **Middleware** — gates `/dashboard` and `/admin`, redirects non-admins, sets HSTS/CSP/XFO/Permissions-Policy.
- **Validation** — every API route validates input with `zod`.
- **Rate limiting** — per-IP token bucket on sign-up, password reset, contact, reviews, bookings, payments.
- **SQL injection** — fully parameterized via Prisma.
- **HTTPS** — enforced via `Strict-Transport-Security` (set TLS at your edge or platform).
- **Account enumeration** — password-reset endpoint always returns OK.

---

## 💳 Payments

The `src/lib/payments.ts` module exposes a uniform `createPayment({ method, amount, currency, reference, customer })`. Without API keys, providers return **mock responses** so the UI is testable end-to-end.

Provider keys you can wire in `.env`:

```
CHAPA_SECRET_KEY=...
TELEBIRR_APP_ID=...
STRIPE_SECRET_KEY=...
```

Bank transfer returns wire instructions referencing the invoice number.

---

## 📦 NPM scripts

| Script              | Purpose                              |
| ------------------- | ------------------------------------ |
| `npm run dev`       | Start the dev server                 |
| `npm run build`     | Production build                     |
| `npm run start`     | Start the production server on :3000 |
| `npm run lint`      | ESLint                               |
| `npm run format`    | Prettier (+ Tailwind plugin)         |
| `npm run type-check`| `tsc --noEmit`                       |
| `npm run db:generate` | Prisma generate                    |
| `npm run db:push`   | Push schema to PostgreSQL            |
| `npm run db:migrate`| Create a new migration               |
| `npm run db:seed`   | Seed catalogue + admin user          |
| `npm run db:studio` | Open Prisma Studio                   |

---

## 🤖 In-site AI guide

A floating "Need help?" bubble is mounted in the root layout. It's a **rule-based guide** — no API key, no per-message cost. It greets visitors, suggests the top intents, answers common FAQs (warranty, hours, payment, privacy), and routes them to the right page.

It auto-hides on `/auth`, `/dashboard`, and `/admin` routes.

To make it smarter later, edit `src/components/site/ai-guide.tsx` — the keyword-to-reply matcher is plain JavaScript at the top of the file.

---

## 🔐 Google sign-in

The **"Continue with Google"** button is wired to NextAuth's Google provider and:

- **Auto-provisions a `User` row** on first sign-in (no extra sign-up form required).
- **Links the OAuth `Account` row** to the user, so subsequent Google sign-ins recognise the same account.
- **Falls back to a friendly "setup mode"** UI with a step-by-step hint when the keys are missing — no opaque OAuth errors.
- **Respects the existing email+password flow** — users who already have an account can sign in with Google using the same email, and the same JWT will be issued.

### How to enable it

1. Open <https://console.cloud.google.com/> → **APIs & Services → Credentials**.
2. Click **Create credentials → OAuth 2.0 Client ID**, type **Web application**.
3. Under **Authorized redirect URIs**, add:
   - `http://localhost:3000/api/auth/callback/google` (dev)
   - `https://YOUR-PRODUCTION-DOMAIN/api/auth/callback/google` (prod)
4. Copy the **Client ID** and **Client secret** into `.env`:
   ```env
   GOOGLE_CLIENT_ID="…apps.googleusercontent.com"
   GOOGLE_CLIENT_SECRET="GOCSPX-…"
   NEXT_PUBLIC_GOOGLE_ENABLED="true"
   ```
5. Restart the dev server. The "Continue with Google" button switches from "setup mode" to live.

> The button uses NextAuth's CSRF-protected `/api/auth/signin/google` endpoint, so the flow is safe by default. Server-side, our `signIn` callback in `src/lib/auth.ts` creates the user + account rows the first time someone signs in.

---

## 📱 Mobile apps (Android & iOS)

The same codebase powers a real native app via **[Capacitor](https://capacitorjs.com)**. Capacitor wraps the web app in a thin native shell — your users install it from the Play Store / App Store and get push, splash screen, and platform-correct back-gesture behavior.

### One-time setup

```bash
# After npm install
npm run mobile:add:android
npm run mobile:add:ios        # macOS only
```

These commands create the `android/` and `ios/` folders. They are `.gitignore`d by default.

### Configure the bundle

Create `capacitor.config.ts` at the project root:

```ts
import type { CapacitorConfig } from '@capacitor/cli';

const config: CapacitorConfig = {
  appId: 'com.techdevicesolution.app',
  appName: 'TechDeviceSolution',
  webDir: 'mobile-app',
  bundledWebRuntime: false,
  plugins: {
    SplashScreen: { backgroundColor: '#0a0a0c', showSpinner: false },
  },
  server: {
    // For development: point the app to your dev server
    // url: 'http://192.168.1.10:3000',
    // cleartext: true,
  },
};

export default config;
```

### Build a release bundle

```bash
npm run mobile:export          # next build + static export to ./mobile-app
npm run mobile:sync            # copies mobile-app/ into android/ and ios/
npm run mobile:open:android    # opens Android Studio to sign & build APK/AAB
npm run mobile:open:ios        # opens Xcode to sign & build IPA (mac only)
```

### Play Store checklist (Android)

- Open `android/` in Android Studio.
- Set your signing key under **Build → Generate Signed Bundle / APK**.
- Recommended: **Android App Bundle (.aab)** for Play Store.
- Update `android/app/build.gradle` → `versionCode` and `versionName` for every release.
- Set the application icon (your `logo-mark.svg` rasterised to 512/1024) and the splash background colour (`#0a0a0c`).

### App Store checklist (iOS)

- Open `ios/` in Xcode on macOS.
- Set the **Bundle Identifier** to `com.techdevicesolution.app`.
- Configure the **Signing & Capabilities** tab with your Apple Developer team.
- Set the app icon set in `ios/App/App/Assets.xcassets/AppIcon.appiconset/`.
- Archive & distribute via App Store Connect.

> Tip: if you don't have a Mac, ship the Android app first — it covers ~90% of the Ethiopian smartphone market and doesn't need macOS to build.

---

## 🌍 Deployment

Works on any Node.js host that supports Next.js — **Vercel** is the simplest:

1. Push to a Git repo and connect it on Vercel.
2. Set every variable from `.env.example` in the project settings.
3. Provision PostgreSQL (Neon, Supabase, Railway, RDS) and paste the `DATABASE_URL`.
4. From the Vercel UI, run `npx prisma migrate deploy && npm run db:seed`.
5. Done — auto-builds on each push.

---

## 📞 Business contact

- **Business:** TechDeviceSolution
- **Owner:** Bereket Mihretu
- **Email:** [bereketmihretu62@gmail.com](mailto:bereketmihretu62@gmail.com)
- **Phone / WhatsApp:** +251 936 216 762
- **Location:** Adama, Ethiopia

### Updating your socials

All social URLs live in **`src/lib/site.ts`** under `SITE.socials`. Replace the `PLACEHOLDER` values with your real Instagram, Telegram, etc. handles and the footer (and any future pages) updates everywhere automatically.

---

## 📜 License

UNLICENSED — internal/commercial use by TechDeviceSolution.

> Built with care to feel a little like Apple, a little like Linear, and a lot like trust.
