# FA Grocery Market starter

## Included
Responsive grocery storefront, sample catalogue, cart, checkout form, Supabase product loading, admin email/password sign-in, admin product creation/image upload, order-management UI scaffold, SQL schema/RLS, and Netlify build config.

## Before running SQL
Your project already has an `admin_users` table and `fa-product-images` bucket. Inspect the existing `admin_users` columns and current policies first. `CREATE TABLE IF NOT EXISTS` does not change an existing table's shape. Review `supabase/schema.sql` before running; don't run destructive SQL on live data.

## Admin membership
Create the admin user in Supabase Authentication > Users, copy the UUID, then run in SQL Editor:
`insert into public.admin_users(user_id) values ('YOUR-USER-UUID') on conflict(user_id) do nothing;`
Only a trusted project owner should add admins.

## Local preview
Install Node.js LTS. Copy `.env.example` to `.env.local`, fill in Supabase URL and publishable key, then run:
`npm install`
`npm run dev`
For a production build: `npm run build`.

## Deploy on Netlify
Recommended: put these files in a GitHub repository, then go to https://app.netlify.com/start and choose Import an existing project. Build command `npm run build`; publish directory `dist`. Add these variables in Project configuration > Environment variables:
- `VITE_SUPABASE_URL`
- `VITE_SUPABASE_PUBLISHABLE_KEY`
Redeploy after setting them. `netlify.toml` configures Vite and SPA fallback.

For manual deploy, build on your computer and drag the generated `dist` folder to Netlify Drop. Git deployment is easier for ongoing edits.

## Security / not production-ready yet
Use only a Supabase publishable key in browser code; never put `sb_secret_...` or service-role keys in `VITE_` variables. Public users can read active products. Admin writes depend on RLS and membership in `admin_users`. Checkout intentionally refuses live order submission until a Supabase Edge Function validates product IDs, current prices, stock, and totals server-side. Mobile Money is only a preference, not a live payment integration. Do not accept live orders until that function, spam protection, and end-to-end tests are complete.
