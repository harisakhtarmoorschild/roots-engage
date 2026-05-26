# Vercel deployment — Roots Engage

The `index.html` in this folder is production-ready with real Supabase magic-link auth, pulling FB groups and LinkedIn feed sources from your live Supabase database. Deploy it once and you have a real URL (`roots-engage.vercel.app` or similar) you can use on phone, share with Emma, and bookmark to home screen.

About 15–20 minutes of clicking.

---

## Step 1 — Sign in to Vercel

1. Go to **https://vercel.com**.
2. Click **Sign Up** (top right).
3. Sign up with **GitHub** (same one you used for Supabase) — fastest.
4. Free Hobby plan is fine. Don't add a credit card.

## Step 2 — Create the project

You have two routes depending on what you prefer.

### Route A — Drag-and-drop (fastest, recommended)

1. From your Vercel dashboard, click **Add New → Project**.
2. At the bottom of the page click **Deploy a template** or look for **"Or import a third-party Git Repository"** — but ignore both.
3. Look for the **"Or start with..."** section with an option that says **"Deploy without Git"** or **"Quick deploy"**. (Vercel UI shifts; if you don't see it, use Route B.)
4. Drag the entire `vercel_deploy` folder (containing `index.html`) into the drop zone.
5. Project name: **`roots-engage`**.
6. Click **Deploy**.

### Route B — Via GitHub (slightly more robust, recommended if you'll iterate)

1. Create a new GitHub repository called `roots-engage`. Make it **private**.
2. On your laptop, in this `vercel_deploy/` folder, run:
   ```
   git init
   git add .
   git commit -m "Initial Roots Engage prototype"
   git branch -M main
   git remote add origin https://github.com/YOUR-USERNAME/roots-engage.git
   git push -u origin main
   ```
3. On Vercel dashboard → **Add New → Project** → **Import Git Repository**.
4. Select the `roots-engage` repo.
5. **Framework Preset**: leave as "Other" or it'll auto-detect "Static".
6. **Root Directory**: leave as `./` (the index.html is at the root of this folder).
7. **Build Command**: leave blank.
8. **Output Directory**: leave blank (defaults to `./`).
9. Click **Deploy**.

Either route: deployment takes ~30 seconds. You'll get a URL like `https://roots-engage-abc123.vercel.app`.

## Step 3 — Update Supabase Site URL

The magic-link emails will redirect users back to this URL. We need to tell Supabase about it.

1. Open your Supabase dashboard → **Authentication → URL Configuration**.
2. **Site URL**: change from `http://localhost:3000` to your new Vercel URL (e.g. `https://roots-engage-abc123.vercel.app`).
3. **Redirect URLs**: add the same URL plus a wildcard:
   - `https://roots-engage-abc123.vercel.app/**`
4. Click **Save**.

## Step 4 — Test login on phone

1. On your phone, open Safari / Chrome and go to your Vercel URL.
2. You should see the Roots Engage login screen.
3. Enter `haris.akhtar@moorschild.co.uk`.
4. Tap **Send magic link**.
5. Status message should say "Check your inbox."
6. Open the email on your phone, tap the link.
7. Should land back on the Roots Engage app, signed in.

**If the magic link fails** (Moorschild scanner pre-click problem): wait a few hours, then try opening the email in **Outlook mobile app** specifically — its sandbox usually doesn't pre-scan. If still failing, we switch to OTP code flow next (you receive a 6-digit number to type in instead of a link).

## Step 5 — Add the URL to my notes

Reply with:
- The Vercel URL (e.g. `roots-engage-abc123.vercel.app`)
- Whether login worked
- Any error messages

I'll save it to project memory and update strategy docs.

---

## What you should see when signed in

### Today tab
Currently shows **"Scanner not yet active"** with a sprout icon. Empty state, expected — the Pi hasn't started producing drafts yet. The target chips (6 FB · 3 LI · 1 self-promo) are visible.

### Channels tab
**Facebook section** — pulls all 36 FB groups from your Supabase `groups` table. Shows stat cards (Subscribed / Active / Low), grade badges, real data.

**LinkedIn section** — pulls all 47 LinkedIn feed sources from your `feed_sources` table. Chip grid, read-only, locked-icon note.

Both sections will populate immediately if you've run `supabase_seed.sql` and `supabase_seed_update.sql`. If they show "Loading…" forever, that means the tables are empty — run those SQL files in the Supabase SQL editor.

## Custom domain (optional, later)

When you're ready, you can point a Roots subdomain at the Vercel deployment:
- e.g. `engage.rootshealth.care`
- Vercel dashboard → Project → Settings → Domains → Add
- Vercel gives DNS records to add at your domain registrar
- ~10 minutes including DNS propagation

Not needed for it to work — the `*.vercel.app` URL works fine.

## Future deploys

If you used Route B (GitHub): every `git push` to main automatically redeploys. So when we iterate on the design or add scanner data, you just push and the live URL updates within ~30 seconds.

If you used Route A (drag-drop): you re-drag the updated folder when there's a change.
