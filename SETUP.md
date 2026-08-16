# Under1KFinds — Setup Guide

This project has two parts sharing one database:
- **Storefront** (`/`) — public, anyone can browse
- **Admin dashboard** (`/admin`) — private, only you can log in

Products live in **Supabase** (a free hosted database). When you add/edit/delete a product in `/admin`, the storefront shows the change the next time someone loads the page — nothing to redeploy, nothing to edit by hand.

---

## Part 1 — Create your Supabase project (~5 min)

1. Go to **supabase.com** → sign up (free) → **New project**.
2. Give it any name, set a database password (save it somewhere), pick a region close to India, and create the project. Wait ~2 minutes for it to spin up.
3. In the left sidebar, open **SQL Editor** → **New query**.
4. Open `supabase-setup.sql` from this project, copy **all** of it, paste it into the query box, and click **Run**. This creates your `products` table, locks it down with security rules, and sets up image storage.
5. In the left sidebar, go to **Authentication → Providers → Email**, and turn **off** "Allow new users to sign up." This means nobody but you will ever be able to create a login.
6. Go to **Authentication → Users → Add user**, and create your own admin account (your email + a password you choose). This is the only account that can log into `/admin`.
7. Go to **Settings → API**. Copy the **Project URL** and the **anon public** key (not the `service_role` one).

## Part 2 — Connect the project to Supabase (~1 min)

1. Open `supabase-config.js`.
2. Paste your Project URL into `SUPABASE_URL` and your anon key into `SUPABASE_ANON_KEY`.
3. Save. Both the storefront and the admin dashboard read from this one file.

## Part 3 — Try it locally (optional but recommended)

You can just double-click `index.html` to preview the storefront, and open `admin/index.html` to try logging in, before deploying anywhere.

## Part 4 — Deploy for free

**Using GitHub + Cloudflare Pages (recommended):**
1. Create a free GitHub account if you don't have one, and a new repository (e.g. `under1kfinds`).
2. Upload every file and folder from this project into the repo root, keeping the `admin/` folder as a folder (GitHub's web uploader supports drag-and-drop of folders).
3. Go to **pages.cloudflare.com** → sign up free → **Create a project → Connect to Git** → pick your repo.
4. Leave the build command empty and the output directory as `/` (root) → **Save and Deploy**.
5. You'll get a free URL like `under1kfinds.pages.dev`. Your storefront is at that URL, your dashboard is at `under1kfinds.pages.dev/admin`.

Netlify or GitHub Pages work the same way — connect the repo, no build step, deploy.

Put your storefront URL in your Instagram bio. Keep the `/admin` URL private — don't post or link it anywhere public.

## Part 5 — Using the dashboard day-to-day

1. Visit `yoursite.com/admin` and log in with the account you created in Part 1.
2. **Overview** tab: quick stats — total products, how many are visible, how many are featured, and a per-category breakdown.
3. **Add** tab: fill in the product details, either upload a photo or paste an image URL, tick its categories, and tap **Publish**. It appears on the storefront right away.
4. **Manage** tab: search your products, and for each one:
   - **Edit** — opens it in the Add form, pre-filled, to change anything (price, link, etc.)
   - **Feature / Unfeature** — featured products are pinned to the top of the storefront
   - **Hide / Show** — hides it from the public store without deleting it (handy if a deal expires temporarily)
   - **Delete** — removes it permanently

Every one of these actions writes straight to your Supabase database, and the storefront reflects it immediately.

## Notes on security

- The `anon` key in `supabase-config.js` is meant to be public — it can't bypass your security rules.
- Your actual password is never stored in any file in this project — Supabase handles authentication.
- Only the one account you create in Part 1, step 6 can log in, because you turned off public sign-ups.
- The image storage bucket allows public *viewing* of images (needed so photos show on your storefront) but only your logged-in account can *upload* to it.
