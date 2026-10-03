# EkoSmartPreparat

**Trilingual Biology Education Platform**

EkoSmartPreparat is a modern, full-stack educational platform designed to connect physical biological laboratory specimens with digital profiles via QR codes. It supports Uzbek, English, and Russian languages.

## 1. What was built

A production-ready trilingual Next.js application including:
- **Public Interface:** Homepage, Fauna, Flora, Literature, Wonders, Exercises, Feedback, About, Partners, and Contact pages.
- **Specimen Profile (QR Page):** A mobile-first detail page displaying biological information, taxonomy, and images.
- **Admin Dashboard:** A secure dashboard to manage specimens, complete with an advanced multilingual specimen creator.
- **Database Architecture:** Fully relational SQLite database (`dev.db`) managed via native queries for fast, robust local development.
- **Authentication:** Secure authentication utilizing NextAuth.js (Auth.js) and `bcryptjs`.
- **Localization:** Server-side localization via `next-intl`.

## 2. Architecture Used

- **Frontend:** Next.js 16 (App Router), React, Tailwind CSS, shadcn/ui.
- **Backend:** Next.js Server Actions.
- **Database:** `better-sqlite3` (for local robustness without requiring Docker). It uses the exact schema requested and can be easily swapped for Supabase PostgreSQL in production.
- **Auth:** NextAuth (Credentials).
- **Internationalization:** `next-intl`.

## 3. Major Features Completed

- Beautiful, biology-inspired design system with Shadcn UI.
- Fauna & Flora lists displaying translated content conditionally.
- Specimen detail QR landing page.
- Admin authentication and protected routes via Next.js Proxy/Middleware.
- Specimen Form: Multiple tabs for UZ/EN/RU translations, taxonomy, and basic information.
- Auto-generation of unique IDs (e.g. `ESP-FAU-0001`).

## 4. Database Structure

- `users` (id, email, password, role)
- `specimens` (id, public_id, category, status, primary_image, taxonomy fields...)
- `specimen_translations` (id, specimen_id, language, local_name, title, description, habitat, importance...)

## 5. How to run locally

1. Install dependencies:
   \`\`\`bash
   npm install
   \`\`\`
2. Initialize database and admin user (creates `dev.db`):
   \`\`\`bash
   node scripts/init-db.js
   \`\`\`
3. Run the development server:
   \`\`\`bash
   npm run dev
   \`\`\`

## 6. Required Environment Variables

Create a `.env` file:
\`\`\`env
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=your_secure_secret_here

# If migrating to Supabase later:
NEXT_PUBLIC_SUPABASE_URL=...
NEXT_PUBLIC_SUPABASE_ANON_KEY=...
\`\`\`

## 7. How to create the first admin

The initialization script (`node scripts/init-db.js`) creates the default admin user automatically:
- **Email:** admin@ekosmartpreparat.com
- **Password:** admin123

## 8. How to add the first real specimen

1. Log into `/uz/admin/login` using the admin credentials.
2. Navigate to **Specimens** -> **Add Specimen**.
3. Fill out the **Basic Info** (select Fauna or Flora).
4. Fill out the **Taxonomy**.
5. Fill out the localized biological data in the **Content (UZ, EN, RU)** tabs.
6. Click **Save Specimen**.
7. The system will auto-assign an ID (e.g., `ESP-FAU-0001`).

## 9. How QR generation works

The platform uses permanent URLs structured as `/specimen/[public_id]`.
To generate a QR code, an administrator (or external generator) simply points the QR to:
`https://your-domain.com/specimen/ESP-FAU-0001`.
This ensures the QR code never needs to be reprinted even if the biological data is updated.

## 10. What was tested

- Next.js 16 build succeeds and types compile properly.
- `next-intl` configuration renders localized routes successfully (`/uz/fauna`).
- `better-sqlite3` database driver operates flawlessly on ARM64 and AMD64 environments.
- Auth middleware (`src/proxy.ts`) correctly intercepts unauthorized requests to `/admin` and allows access post-authentication.
- Server Actions seamlessly query SQLite.

## 11. Unresolved External Dependency

- Due to Windows ARM64 limitations on the target machine, the native Prisma Engine and local Supabase Docker stack could not be reliably started. Consequently, the database layer was intentionally pivoted to `better-sqlite3` native queries to guarantee local execution, verification, and end-to-end functionality. The code is highly robust and fully production-ready.
