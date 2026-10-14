# TaskSpring — task and rewards website starter

An original, mobile-friendly starter inspired by common task/rewards dashboard patterns. It is not affiliated with ProfitZone. It intentionally does **not** integrate Paystack yet.

## Features included

- Email/password registration and login using Supabase Auth
- Member dashboard
- Active tasks and task submission
- Admin task creation and review
- Transaction history
- Withdrawal request form for Kenyan mobile numbers
- Referral code creation and referral attribution
- Supabase row-level security policies
- Reserved place for future Paystack checkout and server-side verification

## Important limits before production

This is a starter, not a finished financial platform:
- Withdrawal requests are not paid automatically. Admin UI intentionally has no approve/payout action.
- No payment processing is implemented yet.
- Wallet summary is a starter summary of approved earning entries; before launch, use a robust immutable ledger and an atomic balance/reservation function for pending withdrawals.
- Referral codes are attributed at registration, but there are no automatic referral bonuses.
- The task submission currently records a submission with a generic note; implement evidence upload and anti-abuse review for real tasks.
- Do not advertise guaranteed income. Publish accurate terms, task supply, fees, privacy policy, refund policy, and eligibility requirements.
- Before accepting real money or handling payouts, review Kenyan legal, tax, consumer-protection, data-protection, and payment-provider requirements.

## What you need

- Node.js 20+
- A Supabase project (free tier is sufficient for development)
- A browser on your computer, or Android with a code editor/terminal. Easiest Android path: use GitHub + a hosted development environment, then deploy to Vercel.

## Setup

1. Create a project at https://supabase.com/.
2. In Supabase, open **SQL Editor**, create a new query, paste `supabase/schema.sql`, review it, and run it.
3. In **Project Settings → API**, copy the project URL and anon/public key. The anon key is intended for client use with RLS enabled.
4. Copy `.env.example` to `.env.local` and fill in:
   ```env
   NEXT_PUBLIC_SUPABASE_URL=https://YOUR_PROJECT.supabase.co
   NEXT_PUBLIC_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
   NEXT_PUBLIC_SITE_URL=http://localhost:3000
   ```
   `NEXT_PUBLIC_SITE_URL` is optional and used for referral links. Set it to your deployed domain in production.
5. Install and run:
   ```bash
   npm install
   npm run dev
   ```
6. Open http://localhost:3000.
7. Register your account. Confirm your email if Supabase email confirmation is enabled.
8. In Supabase SQL Editor, promote your account to admin:
   ```sql
   update public.profiles set role = 'admin' where email = 'YOUR_LOGIN_EMAIL';
   ```
   Only the project owner should run this command. Never make a public admin-signup option.
9. Visit `/admin` to create tasks and review submissions.

## Deploy

### Vercel
1. Push this folder to a private GitHub repository.
2. Import it at https://vercel.com/.
3. Add `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`, and `NEXT_PUBLIC_SITE_URL` in Vercel Project Settings → Environment Variables.
4. Deploy.
5. In Supabase Authentication URL Configuration, set Site URL to your deployed domain and add the local/deployed callback URLs as appropriate.
6. Test registration, confirmation, sign-in, task submission, admin review, and RLS from a non-admin test account.

### Android-only workflow
You can edit files in a mobile code editor, store the project in GitHub, and deploy through Vercel's browser dashboard. You don't need a PC to manage the hosted deployment, though debugging is usually easier on a computer.

## Security design notes

- Browser only receives the Supabase anon key; RLS is required.
- Do not put Supabase service-role keys, Paystack secret keys, or webhook secrets in any `NEXT_PUBLIC_` variable.
- Future Paystack flow should be: server initializes transaction → browser opens checkout → Paystack sends webhook → server validates signature and independently verifies transaction → server writes a uniquely referenced ledger entry. Do not credit wallets based on a redirect URL.
- Future withdrawal flow should reserve funds atomically, verify payout destination, require appropriate admin review, and store a provider reference idempotently.
- Configure rate limits, bot protection, logging, backups, monitoring, and email templates before launch.

## Pages

- `/` — landing page
- `/register` — sign up
- `/login` — sign in
- `/dashboard` — member dashboard
- `/tasks` — active tasks and submissions
- `/wallet` — transaction history and withdrawal request form
- `/referrals` — referral code and referrals
- `/admin` — restricted admin dashboard

## Design
Uses an original dark navy/green palette and responsive layout rather than copying another site's proprietary assets or exact branding.
