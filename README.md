# Wedding Invitation & Interactive RSVP Platform 💍

[![Next.js](https://img.shields.io/badge/Next.js-14.2-black?style=for-the-badge&logo=next.js)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-18.3-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-3.4-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![Framer Motion](https://img.shields.io/badge/Framer_Motion-12.4-black?style=for-the-badge&logo=framer&logoColor=blue)](https://www.framer.com/motion/)
[![Prisma](https://img.shields.io/badge/Prisma-5.22-2D3748?style=for-the-badge&logo=prisma&logoColor=white)](https://www.prisma.io/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL_Neon-336791?style=for-the-badge&logo=postgresql&logoColor=white)](https://neon.tech/)
[![Google Sheets API](https://img.shields.io/badge/Google_Sheets_API-v4-34A853?style=for-the-badge&logo=google-sheets&logoColor=white)](https://developers.google.com/sheets/api)
[![Vercel](https://img.shields.io/badge/Vercel-Deployed-black?style=for-the-badge&logo=vercel&logoColor=white)](https://vercel.com/)

An editorial, mobile-first, bilingual (English & Spanish) wedding invitation and full-stack RSVP management system. Designed with luxury aesthetics, micro-interactions, and a dual-persistence architecture connecting a serverless PostgreSQL database with real-time Google Sheets synchronization.

---

### 🌐 Live Production Demo
👉 **[View Live Web Application](https://julieta-y-julio-invitacion-boda.vercel.app)**

---

## 🌟 Architectural & Engineering Highlights

- **Dual-Persistence Synchronization (PostgreSQL + Google Sheets)**:
  - **Relational Integrity**: Saves and upserts guest confirmations into a serverless **Neon PostgreSQL** database through **Prisma ORM**, ensuring data integrity, idempotency per email, and schema migrations.
  - **Non-Technical Event Coordinator Sync**: Concurrently streams RSVP submissions into a **Google Spreadsheet** in real time via the **Google Sheets API v4 (Service Account)**, providing wedding planners immediate live access to headcounts, bus seat reservations, and dietary restrictions without database queries.
- **Automated Dual-Channel Notifications**:
  - Direct transactional email confirmation sent to the guest via **Nodemailer** through Gmail SMTP on submission.
  - Non-blocking design: Database persistence succeeds even in the event of third-party mail transport delays.
- **Framer Motion Micro-Interactions**:
  - Interactive submit button equipped with a fluid loading progress bar.
  - Staggered scroll-reveal animations (`FadeIn`) across all sections and interactive countdown timer.
- **Client-Side Internationalization (i18n)**:
  - Real-time language switching (`ES` / `EN`) managed via React Context (`LanguageContext`), delivering zero-flicker transitions without unnecessary page reloads.
- **Form Validation & Anti-Spam Security**:
  - Validated with **react-hook-form** and strict runtime schema verification using **Zod**.
  - **Honeypot protection (`_gotcha`)**: Traps automated spam bots silently while maintaining an unobtrusive experience for real guests (no annoying CAPTCHAs).
  - Sensitive financial information (IBAN/BIC) hidden behind client-side state toggles rather than plain DOM markup.
- **SEO & Privacy Controls**:
  - Configured with strict `noindex, nofollow` headers to protect private event details, venue logistics, and personal registries from search engine scrapers.

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
| --- | --- | --- |
| **Framework** | Next.js 14 (App Router) | Single-repository full-stack architecture & optimized serverless API routes |
| **Language & Runtime** | JavaScript (ESNext), Node.js (v18+) | Core application logic & API endpoints |
| **Styling** | Tailwind CSS | Custom curated color system (`bloque`, `fondo`, `texto`, `textoCalido`) |
| **Typography** | Google Fonts via `next/font` | Playfair Display, Cormorant Garamond, Jost, Bodoni Moda, Allura |
| **Animations** | Framer Motion | Smooth transitions, countdown timer, and interactive progress states |
| **Form & Validation** | React Hook Form + Zod | Type-safe form state handling and client/server validation |
| **Database & ORM** | Prisma ORM + PostgreSQL (Neon) | Serverless-safe relational database with connection pooling |
| **Third-Party APIs** | Google Sheets API v4 | Real-time back-office event coordinator synchronization |
| **Email Transport** | Nodemailer (Gmail SMTP) | Instant transactional guest confirmation emails |
| **Deployment** | Vercel | Global CDN edge caching and serverless execution |

---

## 📂 Project Architecture

```
boda-invitacion/
├── app/
│   ├── api/
│   │   └── rsvp/
│   │       └── route.js       # Node.js route handler: Zod validation, Prisma upsert, Sheets API, Nodemailer
│   ├── globals.css            # Tailwind directives and custom animation styles
│   ├── icon.svg               # SVG Favicon
│   ├── layout.jsx             # Font optimization, metadata (noindex), LanguageProvider wrapper
│   └── page.jsx               # Main landing page layout with smooth-scroll section assembly
├── components/                # Modular UI components
│   ├── Alojamiento.jsx        # Accommodation recommendations with direct links & contact
│   ├── Bienvenida.jsx         # Welcome letter from the couple
│   ├── ColumnaFranjas.jsx     # Editorial vertical strip decorations
│   ├── ComoLlegar.jsx         # Church & reception venue details with Google Maps integration
│   ├── Countdown.jsx          # Live countdown timer until wedding day
│   ├── Cronograma.jsx         # Timeline of the day (itinerary icons)
│   ├── FadeIn.jsx             # Framer Motion scroll-reveal container
│   ├── FAQ.jsx                # Accordion for dress code, parking & FAQs
│   ├── Footer.jsx             # Signature & closing remarks
│   ├── FotoFechaLugar.jsx     # Photo gallery carousel & venue snapshot
│   ├── Franjas.jsx            # Decorative geometric divider
│   ├── GuiaSevilla.jsx        # City guide for traveling guests
│   ├── Hero.jsx               # Full-bleed hero banner with names & countdown
│   ├── Nav.jsx                # Sticky navigation bar with anchor links & language toggle
│   ├── Regalo.jsx             # Gift registry info with revealable bank details
│   ├── RSVPForm.jsx           # Multi-field RSVP form with progress bar animation
│   └── Traslados.jsx          # Bus transfer schedule & pickup points
├── context/
│   └── LanguageContext.jsx    # React Context providing active language state (ES/EN)
├── data/
│   ├── content.js             # Single source of truth for Spanish content
│   └── content_en.js          # Single source of truth for English content
├── lib/
│   └── prisma.js              # Serverless-safe PrismaClient singleton
├── prisma/
│   └── schema.prisma          # Database schema definition for RSVPs
├── .env.example               # Safe environment variable template for local setup
├── next.config.js             # Next.js build configuration
├── package.json               # Dependencies and build scripts
└── tailwind.config.js         # Tailwind theme configuration and custom font families
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** (v18.17.0 or higher)
- **npm** (or yarn / pnpm)
- A **PostgreSQL** instance (e.g., [Neon](https://neon.tech))

### 1. Clone & Install Dependencies

```bash
git clone https://github.com/HarrodPablo/wedding-rsvp-platform.git
cd wedding-rsvp-platform
npm install
```

### 2. Configure Environment Variables

Duplicate the template file and populate your credentials:

```bash
cp .env.example .env
```

| Variable | Description |
| --- | --- |
| `DATABASE_URL` | Neon PostgreSQL connection string with connection pooler (`-pooler` and `&pgbouncer=true`). |
| `GMAIL_USER` | Gmail account with 2-Step Verification enabled. |
| `GMAIL_APP_PASSWORD` | 16-character Google App Password from [Google Account Security](https://myaccount.google.com/apppasswords). |
| `GOOGLE_SERVICE_ACCOUNT_EMAIL` | Google Cloud Service Account email. |
| `GOOGLE_PRIVATE_KEY` | Service Account private RSA key (including `\n` linebreaks). |
| `GOOGLE_SPREADSHEET_ID` | The ID found in your Google Sheets URL (ensure the Service Account has Editor access). |

### 3. Initialize Database

Run Prisma migrations to set up the relational tables:

```bash
npx prisma generate
npx prisma migrate dev --name init
```

### 4. Run Development Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser to view the application.

---

## 📦 Production Deployment (Vercel)

1. **Database**: Create a project in [Neon](https://neon.tech), create the database, and copy the pooled connection string.
2. **Environment Variables**: Add all 6 variables from `.env.example` in the **Vercel Project Settings > Environment Variables**.
3. **Build Command**: Set the build command in Vercel to:
   ```bash
   prisma generate && prisma migrate deploy && next build
   ```
4. Deploy the repository.

---

## 👨‍💻 Author

**Pablo Ezequiel Harrod**  
Full-Stack Software Developer  

- **LinkedIn**: [pablo-ezequiel-harrod](https://www.linkedin.com/in/pablo-ezequiel-harrod-b172423ab/)
- **GitHub**: [@HarrodPablo](https://github.com/HarrodPablo)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
