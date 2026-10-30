# Café Stock Tracker

A single-page stock tracker for three café outlets (Jurong, Orchard, Tampines), backed by Supabase.

- Sign in with a username and password (Supabase Auth)
- Outlet tabs with items grouped by category
- Edit stock on hand and reorder level per item, saved to Supabase along with who saved it
- Items at or below their reorder level are marked **Low**
- "Order from suppliers" tab totals the shortfall across all outlets, with a copy button
- Admins get a **Users** tab to add staff with an initial password
- Everyone can change their own password and sign out from the account menu

## Deploy on Vercel

It's a static site with no build step.

1. In Vercel, choose **Add New → Project** and import this repository.
2. Leave **Framework Preset** as **Other**, with no build command or output directory.
3. Click **Deploy**.

`vercel.json` adds security headers. Its Content Security Policy allows only the Supabase project, the Supabase JS library from jsDelivr, and Google Fonts.

## Users and sign-in

Passwords are stored by Supabase Auth in `auth.users`. Each user also has a row in `public.profiles` with their username and role:

| Role | Can do |
|---|---|
| `staff` | View stock, update stock and reorder levels, change own password |
| `admin` | Everything staff can do, plus add users |

People sign in with just a username. Supabase Auth needs an email, so the app maps `apple` to `apple@stocktracker.example.com`. That domain is reserved, so no mail is ever sent.

New users are created by the `create-user` Edge Function. It checks that the caller is a signed-in admin and keeps the service-role key on the server.

## Supabase access rules

| Table | Signed out | Signed in |
|---|---|---|
| `outlets`, `items` | no access | read |
| `stock_counts` | no access | read; update `qty_on_hand` and `reorder_level` only |
| `profiles` | no access | read (roles can't be changed from the app) |

Rows can't be inserted or deleted from the app.

## Run locally

Serve the folder with any static server and open `index.html`.
