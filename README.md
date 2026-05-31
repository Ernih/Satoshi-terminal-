# Jalibor — Real-Time Urban Intelligence

> Presence Mesh Consensus (PMC) protocol — distributed human + AI validation layer for real-world events.

---

## Quick start (local testing on 5 devices)

```bash
# 1. Serve on your local network
python3 -m http.server 8080 --bind 0.0.0.0

# 2. Find your local IP
ipconfig getifaddr en0   # Mac
hostname -I              # Linux

# 3. Open on any device on the same WiFi
http://192.168.1.X:8080/app/
```

---

## Project structure

```
jalibor/
├── index.html                    ← root nav
├── sw.js                         ← service worker
├── .gitignore
├── .github/workflows/deploy.yml  ← auto GitHub Pages deploy
├── app/
│   └── index.html                ← mobile app (PWA, 8 screens)
├── enterprise/
│   └── index.html                ← enterprise website
├── web/
│   └── index.html                ← web3/DePIN website
└── shared/
    ├── manifest.json
    └── icons/
```

---

## Enable Supabase (real-time multi-device)

### Step 1 — Create Supabase project

1. Go to [supabase.com](https://supabase.com) → New project
2. Choose a region close to you (e.g. us-east-1)
3. Go to **SQL Editor** → paste `schema.sql` → Run
4. Go to **Settings → API** → copy:
   - Project URL: `https://xxxx.supabase.co`
   - Anon/public key

### Step 2 — Configure the app

Open `app/index.html`, find these 2 lines near the bottom:

```javascript
var SUPA_URL  = 'https://YOUR_PROJECT.supabase.co';
var SUPA_KEY  = 'YOUR_ANON_KEY';
```

Replace with your real values. Save.

### Step 3 — Test real-time

Open the app on 2+ devices. Report an event on Device 1 → it appears live on Device 2.

---

## Enable WhatsApp bot

### Step 1 — Twilio account

1. [twilio.com](https://www.twilio.com) → free trial ($15 credit)
2. Go to **Messaging → Try WhatsApp** → follow sandbox setup
3. Save your `ACCOUNT_SID` and `AUTH_TOKEN`

### Step 2 — Deploy backend

```bash
cd jalibor-backend/
npm install
cp .env.example .env
# Fill in .env with your keys

# Deploy to Railway (free):
railway init && railway up

# Or Render (free):
# Connect GitHub → New Web Service → jalibor-backend/
```

### Step 3 — Configure Twilio webhook

In Twilio console → WhatsApp Sandbox:
- "When a message comes in": `https://YOUR-BACKEND.railway.app/webhook`
- Method: POST

### Step 4 — Test

WhatsApp to your Twilio sandbox number:
```
!reporte
```
Follow the flow → event appears in app in real-time.

---

## Deploy to GitHub Pages

```bash
git init
git add .
git commit -m "Jalibor v1.0"
git branch -M main
git remote add origin https://github.com/YOUR_USER/jalibor.git
git push -u origin main
```

GitHub → **Settings → Pages → Source: GitHub Actions**

The `deploy.yml` workflow auto-deploys on every push.

Live at: `https://YOUR_USER.github.io/jalibor/`

---

## Deploy to Netlify (30 seconds)

Drag the `jalibor/` folder to [app.netlify.com/drop](https://app.netlify.com/drop)

---

## Testing checklist (5 devices)

| Role | Device | What to test |
|------|--------|-------------|
| Reportador | Phone 1 | Register → report event → see it on map |
| Validador A | Phone 2 | See new event → validate YES → confidence rises |
| Validador B | Phone 3 | Validate YES → see node count increase |
| Validador C | Phone 4 | Validate NO → see confidence effect |
| Observador | Desktop | Watch events list update in real-time |

---

## Simulate GPS locations

In Chrome desktop: F12 → ⋮ → More tools → Sensors → Location → set CDMX coords:
- Doctores: `19.4185, -99.1580`
- Insurgentes: `19.4270, -99.1676`
- Roma Norte: `19.4320, -99.1420`
- Centro: `19.4326, -99.1332`

---

## Stack

- Pure HTML/CSS/JS — zero build step
- Three.js (CDN) for 3D PMC animation
- Supabase — Postgres + Realtime WebSocket
- Twilio — WhatsApp Business API

## License

Proprietary — Jalibor © 2026
