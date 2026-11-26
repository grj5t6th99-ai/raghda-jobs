# Al Raghda Advertising — Management System (production)

Quote-to-cash, inventory, purchasing and full double-entry accounts for Al Raghda Advertising, Doha.
Bilingual EN/AR. Multi-user, cloud database on Supabase, staff logins with roles.

## Files
- `index.html` — the application (UI + business logic)
- `db.js` — data layer: login, roles, load/sync with Supabase, live updates between users
- `config.js` — **the only file to edit**: Supabase project URL and anon public key
- `../supabase/schema.sql` — database (run once in Supabase SQL Editor)
- `../GO-LIVE.md` — step-by-step go-live checklist

## Deploy
1. Supabase: create project → SQL Editor → run `schema.sql` → Authentication: Email provider on, sign-ups off → invite the owner → `insert into staff (id,email,name,role) values ('<uuid>','owner@…','Owner','owner');`
2. Put the Project URL and anon key into `config.js`.
3. Push `app/` to the GitHub repo and deploy on Vercel (static) or GitHub Pages. Set Supabase → Authentication → URL configuration → Site URL to the app address (invite and password-reset links open the app).
4. Owner signs in → Settings: logo, company details, bank, terms; Materials & rates; Staff & roles (invite staff in Supabase first, then add them by email in Settings).

## Roles
owner (everything) · accountant (everything financial) · sales (enquiries, quotes, jobs, customers, invoices/receipts, inventory, POs) · designer (enquiries, quotes, jobs, customers, inventory) · production (jobs, inventory, POs) · installer (jobs). The database enforces these with row-level security; the menu hides what a role cannot use.

## How data is kept safe
- Every change is written to Supabase within a second ("✓ Saved" in the sidebar; "⚠ Not saved — retrying" if offline).
- Other users' changes appear live. If two users create a document with the same number at the same moment, the second is renumbered automatically and told.
- Every insert/update/delete on money and operational tables is recorded in `audit_log` with the user and timestamp.
- Settings → "Download backup (JSON)" gives a full export at any time; Supabase Pro adds daily point-in-time backups.
