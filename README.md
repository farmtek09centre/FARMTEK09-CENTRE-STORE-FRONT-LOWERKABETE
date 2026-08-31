# FARMTEK09 CENTRE — WhatsApp Storefront

A plain HTML/CSS/JS storefront (no build step) for your seedlings: a
categorised grid, WhatsApp ordering, your nursery's location with an
embedded map, and M-Pesa Pay Bill details for Co-operative Bank. Free to
host on GitHub Pages.

## What's in here

```
index.html      the page
styles.css      all styling (green/nursery theme)
app.js          config, rendering, WhatsApp + M-Pesa + map logic
products.json   your 18 seedlings (edit this to update anything)
README.md       this file
```

## About the product list

Your uploaded `web-farmtek09-centre-farmtek.zip` had 10 draft category pages
(Grand Nain, Williams, FHIA-17, Cavendish, Lemon, Tangerine, Apple, Grape,
Mango, Avocado) with placeholder Wikipedia photos and no prices. Separately,
your Farmtek09 Centre listing on omnistore-ke.vercel.app had 14 real,
priced, photographed items. This site merges both:

- **14 items carried over with their real photo and real price** (Ksh 150,
  or Ksh 300 for the two Plantain listings) — all the bananas, both mangoes,
  the avocado, and the tangerine.
- **4 items from your draft that aren't in the priced catalogue yet** —
  FHIA-17, Apple, Grape, Lemon. These keep the placeholder photo from your
  draft and show **"Price on request"** rather than a made-up number, so
  customers enquire on WhatsApp instead of seeing a wrong price.
- One unnamed "X" item (Ksh 200, no description) from the Omnistore listing
  was left out — it didn't have enough information to show a customer.

Once you have real prices/photos for FHIA-17, Apple, Grape or Lemon, add
them in `products.json` the same way as any other item (see below).

## 1. Put it on GitHub Pages (~5 minutes)

1. Create a new **public** repository on github.com (e.g. `farmtek09-store`
   — you've already got `farmtek09centre`, `farmtek09website` and
   `web-farmtek09-centre`, so pick a name that doesn't collide).
2. **Add file → Upload files**, drag in all 4 files from this package, commit.
3. **Settings → Pages** → Source: `Deploy from a branch` → branch `main` →
   folder `/ (root)` → Save.
4. Your live URL appears on that same screen a minute or two later —
   `https://kabete2349.github.io/farmtek09-store/` (or whatever you named it).

## 2. Your business settings, all in one place

At the top of `app.js`:
```js
const STORE_NAME = "FARMTEK09 CENTRE";
const WHATSAPP_NUMBER = "254725528888";
const PAYBILL_BUSINESS = "400200";
const PAYBILL_ACCOUNT = "54095";
const LOCATION_NAME = "Lower Kabete, Nairobi";
const LOCATION_LAT = -1.2379275;
const LOCATION_LNG = 36.7267739;
const LOCATION_HOURS = "Open daily, 9:00 AM – 5:00 PM";
```
Everything on the site (header, footer, map, payment panel, WhatsApp
messages) is driven from these lines — change once, updates everywhere.
Hours came from your Google Business listing; adjust if they're off.

## 3. About the M-Pesa Pay Bill panel

Same honest approach as before: this is a static site, so it can't trigger
an automatic charge (that needs a Safaricom Daraja API account and a
backend to hold the secret keys). What's built is the real Pay Bill flow:

- A **"Pay via M-Pesa"** card showing Business No. 400200 and Account No.
  54095, each with a copy button.
- Every WhatsApp order message already includes both numbers, so the
  customer knows to pay Lipa na M-Pesa → Pay Bill → 400200 → 54095, then
  send the confirmation.

Nothing is auto-charged — you confirm the same way you already do, just
with the numbers already in front of every customer.

## 4. About the location panel

The map embed and "Get Directions" button use the exact coordinates from
the Google Maps link you sent (-1.2379275, 36.7267739) — no API key
needed, so nothing to configure. If the pin is ever slightly off, update
`LOCATION_LAT` / `LOCATION_LNG` in `app.js`.

## Editing `products.json`

Each product looks like this:
```json
{
  "id": "hass-grafted-avocado",
  "name": "Hass Grafted Avocado",
  "category": "Avocados",
  "price": 150,
  "blurb": "Grafted Hass avocado — the variety most sought after for export markets.",
  "image": "https://..."
}
```
- **name / price / blurb** — edit freely.
- **category** — must match one of: Bananas & Plantains, Mangoes,
  Avocados, Tangerines, Apples, Grapes, Lemons (or add a new one — it'll
  get its own filter pill automatically).
- **image** — currently points at your Omnistore product photos and, for
  the 4 gap items, Wikipedia stock photos. Both are hotlinked (not saved
  into this package), so if you ever close the Omnistore account, swap
  those 14 image URLs for direct links to your own hosted photos to avoid
  broken images. Set `"price": null` instead of a number to show "Price on
  request".

## Testing locally before you push

```
python3 -m http.server 8000
```
then open `http://localhost:8000`.
