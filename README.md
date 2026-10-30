# Café Stock Tracker

A single-page stock tracker for three café outlets (Jurong, Orchard, Tampines), backed by Supabase.

- Outlet tabs with items grouped by category
- Edit stock on hand and reorder level per item, saved to Supabase
- Items at or below their reorder level are marked **Low**
- "Order from suppliers" tab totals the shortfall across all outlets, with a copy button

## Deploy on Vercel

It's a static site with no build step.

1. In Vercel, choose **Add New → Project** and import this repository.
2. Leave **Framework Preset** as **Other**, with no build command or output directory.
3. Click **Deploy**.

`vercel.json` adds security headers, including a Content Security Policy that only allows connections to the Supabase project. If you move to a different Supabase project, update `connect-src` there as well as `SUPABASE_URL` in `index.html`.

## Supabase

The page uses the project's publishable key, which is designed to be public. Row Level Security limits what that key can do:

| Table | Read | Write |
|---|---|---|
| `outlets`, `items` | yes | no |
| `stock_counts` | yes | update `qty_on_hand` and `reorder_level` only |

Rows can't be inserted or deleted from the page. There's no login, so anyone with the URL can change stock levels.

## Run locally

Open `index.html` in a browser, or serve the folder with any static server.
