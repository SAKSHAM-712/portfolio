# Portfolio Backend — Saksham Pathak

Node.js + Express + SQLite backend for the contact form.

## Stack
| Layer      | Tech                        |
|------------|-----------------------------|
| Server     | Node.js + Express           |
| Database   | SQLite (better-sqlite3)     |
| Email      | Nodemailer (Gmail)          |
| Validation | express-validator           |
| Rate limit | express-rate-limit          |

---

## Folder Structure
```
portfolio-backend/
├── src/
│   ├── index.js              # Entry point
│   ├── db/
│   │   └── init.js           # DB initialiser & schema
│   ├── routes/
│   │   └── contact.js        # Contact form routes
│   ├── services/
│   │   └── mailer.js         # Nodemailer email service
│   └── middleware/
│       └── rateLimiter.js    # Rate limiter
├── data/                     # SQLite DB file (auto-created)
├── .env.example
└── package.json
```

---

## Setup

### 1. Install dependencies
```bash
cd portfolio-backend
npm install
```

### 2. Create your .env file
```bash
cp .env.example .env
```
Edit `.env` and fill in:
- `EMAIL_USER` — your Gmail address
- `EMAIL_PASS` — a Gmail **App Password** (not your real password)
  → https://support.google.com/accounts/answer/185833
- `ALERT_TO` — email address to receive alerts (can be same as EMAIL_USER)
- `FRONTEND_URL` — your frontend's URL for CORS

### 3. Run the server
```bash
# Development (auto-reload)
npm run dev

# Production
npm start
```

Server starts on `http://localhost:3000`

---

## API Endpoints

### `POST /api/contact`
Submit a contact form message.

**Body (JSON):**
```json
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "message": "Hello Saksham!"
}
```

**Success (201):**
```json
{ "success": true, "message": "Your message has been received. Thank you!", "id": 1 }
```

**Validation error (422):**
```json
{ "success": false, "errors": [{ "field": "email", "message": "Please provide a valid email address." }] }
```

---

### `GET /api/contact/messages`
Returns all stored messages (newest first).
> ⚠️ Add authentication before exposing this in production.

### `PATCH /api/contact/messages/:id/read`
Mark a message as read.

### `DELETE /api/contact/messages/:id`
Delete a message.

### `GET /health`
Health check — returns `{ "status": "ok" }`.

---

## Rate Limiting
The `/api/contact` endpoint is limited to **5 requests per IP per 15 minutes** to prevent spam.

---

## Deploying
For production, consider:
- **Railway / Render / Fly.io** — easy Node.js hosting
- Set `FRONTEND_URL` to your real deployed frontend URL
- Use a persistent disk/volume for the SQLite `.db` file
- Add HTTP Basic Auth or a JWT check on the `GET /api/contact/messages` route
