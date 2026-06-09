# FleetPRO Call Coach

A standalone, single-file web app that coaches you **live, during cold calls** to fleet owners (Amazon DSPs) to pitch Flow Pressure Washing's fleet-washing service. Claude streams a spoken-style rebuttal in ~1–2 seconds the moment a prospect objects — you glance, you say it.

Everything runs in the browser. No backend, no build step.

## What it does
- **Live Call tab** — tap an objection ("Too expensive", "We do it ourselves"…) or type/speak exactly what they said, and Claude streams back the exact words to say next, tailored to your business and the prospect on the line. Voice input (🎙️) for hands-free use.
- **Prospect tab** — company, contact, fleet size, current cleaner, notes. The more you fill in, the more tailored the rebuttals.
- **Scripts tab** — editable opener + voicemail (with an AI "generate a fresh opener" button) and the full objection playbook of fallback scripts.
- **Call Log tab** — one-tap Booked / Follow-up / No after each call, with today's dial/follow-up/booked counters.
- **Setup tab** — API key, model picker, and your business context (pre-filled for Flow Pressure Washing).

## Setup (2 minutes)
1. Get a Claude API key at https://console.anthropic.com → **API Keys**.
2. Open `index.html` (double-click, or add to your iPhone Home Screen — see below).
3. Go to **Setup**, paste the key, pick a model, tap **Save**, then **Test key**.
4. Optionally edit the business context to match your current pitch/pricing.

Without a key the app still works — it shows the saved fallback scripts instead of AI.

## Model choice (it matters for live calls)
- **Haiku 4.5** — fastest + cheapest. Best default for live use; the rebuttal lands almost instantly.
- **Sonnet 4.6** — balanced.
- **Opus 4.8** — smartest, slightly slower.
Live responses cost roughly a fraction of a cent each on Haiku.

## Use it on your iPhone
- Host this folder anywhere static (GitHub Pages, Netlify drop, or your existing GitHub Pages account), open it in Safari, then **Share → Add to Home Screen**. It runs full-screen like an app.
- Or just open `index.html` from Files. (For the API calls to work over `file://` some browsers are fine; if not, host it.)

## ⚠️ Security note (read this)
This app calls the Claude API **directly from the browser** using the `anthropic-dangerous-direct-browser-access` header, with your key stored in this browser's `localStorage`.

- **Fine for personal use** on your own phone/computer.
- **Not fine if other people will use it** — the key ships to every client. In that case, put a tiny proxy in front:
  - Deploy a one-route serverless function (Cloudflare Worker, Vercel/Netlify function) that holds the key as a server-side secret and forwards `POST /v1/messages` to `https://api.anthropic.com`.
  - Change the two `fetch('https://api.anthropic.com/v1/messages', …)` calls in `index.html` to point at your proxy URL, and drop the `x-api-key` / browser-access headers.
  - Then you can hand the app to your sales reps without exposing the key.

## Files
- `index.html` — the entire app (HTML + CSS + JS).
- `.claude/launch.json` — local preview server config (python http.server on port 3478).
- `README.md` — this file.

## Customizing
All the canned content lives in clearly-labeled arrays near the top of the `<script>` in `index.html`:
- `OBJECTIONS` — the objection buttons + their fallback scripts
- `DISCOVERY` — discovery questions
- `CLOSES` — closing lines
- `DEFAULT_BIZ` / `DEFAULT_SCRIPTS` — starting business context and opener/voicemail
Edit those to add objections or change the playbook. The live A6 system prompt is built in `buildSystem()`.
