# What changed

1. **app.js** — added a +/- quantity stepper to every product card.
   - Quantity now flows into both the WhatsApp order message and the M-PESA amount (price × quantity).
2. **styles.css** — small `.qty-stepper` / `.qty-btn` / `.qty-value` styles matching your existing design.
3. **README.md** — removed the real Daraja Consumer Key/Secret that were committed in plain text, replaced with placeholders, and added a security warning.
4. **.gitignore** — you had a file literally named `download` containing gitignore rules (node_modules/, .env, etc.) — Git was never actually reading it. Renamed it to `.gitignore` so `.env` won't get committed by accident.
5. Removed 4 unused duplicate files (`app (3).js`, `index (4).html`, `products (1).json`, `styles (2).css`) that weren't linked from anywhere and were just clutter.

# The actual M-PESA "gateway error" cause

Your site is on **GitHub Pages**, which only serves static files (HTML/CSS/JS). `server.js` is a Node.js backend — GitHub Pages cannot run it. So when a customer clicks "Pay via M-PESA," the browser tries to call `/api/mpesa/stk-push` on a URL that doesn't exist, and fails immediately. This is not a bug in the STK-push logic itself — the logic looks correct — it's a hosting/deployment gap.

# What you still need to do (I can't do this part for you — it needs your own accounts/credentials)

1. **Rotate your Daraja credentials.** The Consumer Key/Secret that were in your README are now public in your git history. Go to https://developer.safaricom.co.ke/, open your app, and regenerate them. Old ones should be treated as compromised even after removal from the file, since git history still has them (see note below).
2. **Deploy `server.js` somewhere that runs Node** — e.g. Render.com (has a free tier), Railway, or Fly.io. Point its start command at `npm start`.
3. **Set environment variables on that host** (not in git): `MPESA_CONSUMER_KEY`, `MPESA_CONSUMER_SECRET`, `MPESA_PASSKEY`, `MPESA_ENV`, `MPESA_SHORTCODE`, `MPESA_CALLBACK_BASE_URL` (must be the https URL of the very host you're deploying to, e.g. `https://farmtek09-mpesa.onrender.com`), `FRONTEND_ORIGIN` (your GitHub Pages URL).
4. **Update `MPESA_API_BASE` in `app.js`** to your new backend's URL, e.g.:
   ```js
   const MPESA_API_BASE = "https://farmtek09-mpesa.onrender.com";
   ```
5. Redeploy the frontend (push to GitHub Pages) after that change.

## Note on the leaked credentials still being in git history
Removing them from the current README file does not erase them from old commits — anyone can still find them by browsing your repo's history on GitHub. Rotating the actual keys on Safaricom's Daraja portal is the only real fix; deleting/rewriting git history is optional cleanup after that.
