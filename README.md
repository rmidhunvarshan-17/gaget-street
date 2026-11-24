# Gadget Street

Compare electronics shops across Salem: price, stock, EMI offers, freebies, repair shops and a budget finder,
in English and Tamil, with voice search. Prototype with **sample data**.

```
gadget-street/
├── backend/               Node API (no npm install needed - zero dependencies)
│   ├── server.js          all routes + static hosting of ../frontend
│   ├── lib/store.js       JSON-file database (swap for Supabase/Postgres later)
│   ├── lib/util.js        distance, discount, EMI maths
│   ├── data/seed.js       sample Salem shops, products, prices, repair shops
│   ├── test.js            14 API tests   ->  npm test
│   └── .env.example
├── frontend/              plain HTML/CSS/JS
│   ├── index.html
│   ├── css/styles.css
│   └── js/ config.js · i18n.js · app.js
└── netlify.toml
```

## Run it
```bash
cd backend
npm start            # http://localhost:3000  (API + website together)
npm test             # optional
```
Needs Node 18+.

## Demo logins
| Who | How |
|---|---|
| Shop owner | Shop dashboard → mobile `9000000001` (…`08`) → Send OTP. The OTP is shown on screen in dev mode. |
| New shop | Sign up → stays *pending* until approved in **Admin (demo)** |
| Admin | footer link → password `admin123` (set `ADMIN_PASSWORD`) |

## API
| Method | Path | Purpose |
|---|---|---|
| GET | `/api/search?q=&area=&sort=price\|nearest\|discount&lat=&lng=` | compare shops per product |
| GET | `/api/budget?amount=&priority=value\|price\|discount\|distance` | ranked picks within budget |
| GET | `/api/repairs?device=phone\|laptop\|tv\|watch\|earbuds&area=` | repair shops |
| GET | `/api/emi?price=&down=&rate=&months=` | EMI estimate |
| GET | `/api/areas`, `/api/catalog` | lookups |
| POST | `/api/auth/otp/request`, `/api/auth/otp/verify` | shop login |
| POST | `/api/shops/signup` | shop registration (pending) |
| GET/POST/PUT/DELETE | `/api/shop/me`, `/api/shop/listings[/:id]` | shop dashboard (Bearer token) |
| POST/GET | `/api/admin/login`, `/api/admin/shops`, `/api/admin/shops/:id/approve\|reject` | admin |

## Deploy
**Simplest:** host the `backend` folder on Render/Railway (start command `node server.js`). It serves the
website too, so you are done. Set `NODE_ENV=production`, `TOKEN_SECRET`, `ADMIN_PASSWORD`.

**Keep Netlify for the frontend:** deploy `frontend/` to Netlify, host the backend elsewhere, then either use
`netlify.toml` (proxy) or set `API_BASE` in `frontend/js/config.js` and `CORS_ORIGIN` on the backend.

## Before real users
- **OTP:** `/api/auth/otp/request` only logs the code. Plug in an SMS provider (MSG91, Twilio, Fast2SMS) at the marked TODO. In production the code is never returned to the browser.
- **Database:** the JSON file resets on hosts with ephemeral disks. Move `store.js` to Supabase/Postgres (your "realtime sync" next step).
- Prices and shops are sample data, not live quotes.
