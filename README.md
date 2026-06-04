# refer.artigellence.com — Deployment Runbook

End-to-end: Git push → GitHub Pages → Hostinger DNS → live on `refer.artigellence.com`.

---

## Files to upload to the repo

```
referral-landing/
  index.html                          ← the page
  logo.png                            ← AAA logo, referenced twice
  Raj_Singh_Founder_Signature.pdf     ← linked from "My Projects" at top
  Vcard_Front.png                     ← business card front (modal)
  Vcard_Back.png                      ← business card back (modal)
  CNAME                               ← binds the custom subdomain (required)
  README.md                           ← this file (optional)
```

Seven files. Drop them at the repo root — NOT inside a subfolder.

---

## Step 1 · Push to GitHub

```bash
cd /path/to/referral-landing

git init
git add .
git commit -m "refer.artigellence.com v1"

# create the repo on github.com first, then:
git remote add origin git@github.com:<your-username>/<repo-name>.git
git branch -M main
git push -u origin main
```

> **Public vs private repo:** GitHub Pages on the free plan requires a **public** repo. The page content is public anyway (it's a marketing page), so this is fine. If you want a private repo with Pages, you need GitHub Pro (~A$6/mo) — or use **Cloudflare Pages** which is free + supports private repos.

---

## Step 2 · Enable GitHub Pages

In your repo on github.com:

1. **Settings** → **Pages**
2. **Source**: Deploy from a branch
3. **Branch**: `main` · folder `/ (root)` · **Save**
4. Wait 1–2 minutes for the first build
5. You'll see: *Your site is live at `https://<username>.github.io/<repo-name>/`*

Visit that URL to confirm the page renders. Logo, PDF, fonts — all working.

---

## Step 3 · Hostinger DNS for refer.artigellence.com

Log in to Hostinger → **Domains** → `artigellence.com` → **DNS / Nameservers** → **Manage DNS records**.

Add **one** new record:

| Field   | Value                       |
|---------|-----------------------------|
| Type    | `CNAME`                     |
| Name    | `refer`                     |
| Target  | `<your-username>.github.io` |
| TTL     | 14400 (default is fine)     |

> ⚠ The target is just `username.github.io` — **no path, no repo name, no trailing slash, no protocol**. GitHub Pages routes by the CNAME file content (which is already in your repo).
>
> ⚠ If there's an existing record for `refer` (A, CNAME, or other), delete it first. Only one record per subdomain.

Save. DNS usually propagates within 5–30 minutes (sometimes faster).

Check propagation: paste `refer.artigellence.com` into [dnschecker.org](https://dnschecker.org). When most regions show your CNAME → next step.

---

## Step 4 · Bind the custom domain in GitHub

Back in your repo → **Settings** → **Pages**:

1. **Custom domain**: type `refer.artigellence.com` → **Save**
2. GitHub verifies the DNS — green checkmark when ready
3. Once verified, the **Enforce HTTPS** checkbox becomes available — **check it**
4. Wait 5–10 min for the SSL cert to provision

`https://refer.artigellence.com` now serves the page.

---

## Step 5 · Wire up the form (5 min, free)

Submissions currently show success in the UI but go nowhere. To collect them:

1. Sign up at **formspree.io** (free tier: 50 submissions/month)
2. **New form** → copy the endpoint, format: `https://formspree.io/f/xxxxxxxx`
3. Edit `index.html`, find this line:
   ```js
   const FORM_ENDPOINT = null;
   ```
   Replace with:
   ```js
   const FORM_ENDPOINT = "https://formspree.io/f/xxxxxxxx";
   ```
4. Commit and push:
   ```bash
   git add index.html
   git commit -m "wire formspree endpoint"
   git push
   ```
5. GitHub Pages redeploys in ~30 seconds.

Submit a test form — Formspree confirms first submission via email and forwards to `raj@artigellence.com` from then on.

Alternatives if you outgrow Formspree's 50/mo: **Web3Forms**, **Getform**, **Basin**. Same drop-in pattern.

---

## Step 6 · The referral link format

Share with each referrer:

```
https://refer.artigellence.com/?ref=<their-name>
```

Examples:
- `https://refer.artigellence.com/?ref=mike-brown`
- `https://refer.artigellence.com/?ref=priya-syd`
- `https://refer.artigellence.com/?ref=raj-001`

The visitor sees *"Forwarded by Mike Brown"* in the hero. Form submission carries `ref:mike-brown` to your inbox. When that lead pays a A$4,500+ retainer, you send A$500 to Mike within 24 hours.

Track conversions in a simple sheet:

```
referrer | lead | submitted | retainer paid | A$500 sent
```

---

## Updates after launch

For any change to the page:

```bash
# edit index.html or any file
git add .
git commit -m "what changed"
git push
```

Auto-redeploys in 30–60 seconds. Hard refresh (`Ctrl+Shift+R` / `Cmd+Shift+R`) to bypass cache.

---

## Total time estimate

| Step                           | Time          |
|--------------------------------|---------------|
| Push to GitHub                 | 5 min         |
| Enable Pages                   | 2 min         |
| Hostinger DNS                  | 3 min         |
| DNS propagation                | 5–30 min wait |
| Custom domain + HTTPS          | 5 min         |
| Formspree setup                | 5 min         |
| **Total active work**          | **~20 min**   |

Live on `https://refer.artigellence.com` within the hour.
