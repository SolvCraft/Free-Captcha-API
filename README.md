# Free Open-Source Captcha API

The **Free Open-Source Captcha API** is a lightning-fast, stateless Captcha service built on Cloudflare's global edge network. Protect your login, signup, and contact forms from bots — completely free, forever.

🌐 **Live API:** [https://captcha-api.solvcraft.workers.dev](https://captcha-api.solvcraft.workers.dev)
📖 **Interactive Docs:** [https://captcha-api.solvcraft.workers.dev/docs](https://captcha-api.solvcraft.workers.dev/docs)

---

## Why Choose This Free Open-Source Captcha API?

### 🚀 Blazing Fast Edge Performance
Runs on Cloudflare Workers at the edge — millisecond response times, globally. Your API server never slows down because analytics are written asynchronously in the background.

### 🛡️ Cryptographically Secure
Every captcha comes with an HMAC-SHA256 signed token. The answer is **never stored** — not in a database, not in the token itself. Tokens auto-expire in 5 minutes, making replay attacks impossible.

### 💰 100% Free & Open-Source
No monthly fees, no API keys, no rate-limit tiers for basic usage. Self-host it on your own Cloudflare account in minutes.

### 🔌 Dead Simple Integration
Works with any stack — React, Vue, Next.js, Angular, or plain HTML. No bloated SDKs, just two HTTP calls.

---

## Quick Start

### 1. Generate a Captcha
```bash
curl https://captcha-api.solvcraft.workers.dev/generate
```
**Response:**
```json
{
  "svg": "<svg>...captcha image...</svg>",
  "token": "MTc3Mjg5M....<hmac-signature>"
}
```
Render the `svg` directly on your page. Store the `token` in a hidden field.

### 2. Verify the User's Answer
```bash
curl -X POST https://captcha-api.solvcraft.workers.dev/verify \
  -H "Content-Type: application/json" \
  -d '{"token":"<token>","answer":"<user_input>"}'
```
**Response:**
```json
{ "success": true }
```

---

## How It Works

```
User visits form → GET /generate → Show SVG → User types answer
→ POST /verify with token + answer → { success: true/false }
```

Tokens are structured as `base64(timestamp).HMAC(answer:timestamp, secret)` — the captcha answer is **never stored or embedded anywhere**. It's verified by re-computing the HMAC server-side.

---

## API Reference

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET`  | `/generate` | Returns an SVG captcha image and a signed token |
| `POST` | `/verify`   | Validates the token and user's answer |
| `GET`  | `/docs`     | Interactive Swagger UI |
| `GET`  | `/openapi.json` | OpenAPI 3.0 specification |

---

## Self-Hosting in 5 Minutes

```bash
git clone https://github.com/SolvCraft/Free-Captcha-API.git
cd Free-Captcha-API
npm install
npx wrangler d1 create caa_db
# Paste the database_id into wrangler.toml
npx wrangler d1 execute caa_db --remote --file=./schema.sql
npx wrangler deploy
```

Set your secret key (strongly recommended for production):
```bash
npx wrangler secret put CAPTCHA_SECRET
```

---

## License

MIT — free to use, fork, and deploy.
