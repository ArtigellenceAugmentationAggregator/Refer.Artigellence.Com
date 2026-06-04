   # refer.artigellence.com

   Live referral landing for Artigellence Augmentation Aggregator.

   - **URL**: https://refer.artigellence.com
   - **Hosting**: Cloudflare Pages (auto-deploys from `main`)
   - **DNS**: Hostinger (CNAME `refer` → `refer-wtp.pages.dev`)
   - **Repo**: this one (public)

   ---

   ## Files

   ```
   index.html                          ← the page
   logo.png                            ← AAA logo (hero card + founder circle)
   Raj_Singh_Founder_Signature.pdf     ← opens from "My Projects" pill at top
   Vcard_Front.png                     ← business card front (modal)
   Vcard_Back.png                      ← business card back (modal)
   CNAME                               ← legacy, harmless on Cloudflare Pages
   README.md                           ← this file
   ```

   ---

   ## How updates work

   Edit any file. Push to `main`. Cloudflare Pages auto-deploys in ~30 seconds.

   ```bash
   git add .
   git commit -m "what changed"
   git push
   ```

   Hard refresh (Ctrl+Shift+R / Cmd+Shift+R) to bypass cache.

   ---

   ## Form — still needs wiring (5 min)

   Submissions currently show success but go nowhere. To collect them:

   1. Sign up at **formspree.io** (free, 50/mo)
   2. New form → copy endpoint: `https://formspree.io/f/xxxxxxxx`
   3. In `index.html` find:
      ```js
      const FORM_ENDPOINT = null;
      ```
   4. Replace with your URL:
      ```js
      const FORM_ENDPOINT = "https://formspree.io/f/xxxxxxxx";
      ```
   5. Commit and push.

   Submit a test — Formspree confirms via email, then forwards to `raj@artigellence.com` from then on.

   ---

   ## Referral link format

   Share with each referrer:

   ```
   https://refer.artigellence.com/?ref=<their-name>
   ```

   Examples:
   - `https://refer.artigellence.com/?ref=mike-brown`
   - `https://refer.artigellence.com/?ref=priya-syd`

   Behaviour:
   - Visitor sees *"Forwarded by Mike Brown"* badge in hero (trust signal).
   - Hidden `referrer` field on submission carries `mike-brown` to your inbox.
   - When that lead pays a A$4,500+ retainer → pay A$500 to Mike within 24 hours.
   - No `?ref=` → referrer field is `direct`, badge stays hidden.

   Track in a simple sheet:

   ```
   referrer | lead | submitted | retainer paid | A$500 sent
   ```

   ---

   ## Architecture summary

   ```
   visitor
      ↓
   refer.artigellence.com  (Hostinger DNS · CNAME)
      ↓
   refer-wtp.pages.dev     (Cloudflare Pages · CDN)
      ↓
   this repo's main branch (GitHub · source of truth)
   ```

   © 2026 Raj Singh · Artigellence Augmentation Aggregator · ABN 83 988 690 362
