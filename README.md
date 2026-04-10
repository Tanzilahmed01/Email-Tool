# 📧 MailScan — Email Finder & Verifier

A full-stack email finder and verifier with AWS SES SMTP, hosted **free** on GitHub Pages (frontend) + Render (backend).

---

## 🗂 Project Structure

```
email-tool/
├── backend/
│   ├── server.js         ← Node.js/Express API
│   ├── package.json
│   ├── .env.example      ← Copy to .env and fill in your keys
│   └── .gitignore
└── frontend/
    └── index.html        ← GitHub Pages site
```

---

## ✨ Features

| Feature | Description |
|---|---|
| **Email Verifier** | Format + disposable domain + MX + SMTP checks, scored /100 |
| **Email Finder** | Generates 10 common patterns from name + domain |
| **Bulk Verify** | Verify up to 20 emails at once |
| **Rate Limiting** | 30 requests/min per IP |
| **AWS SES SMTP** | Professional SMTP connection check |

---

## 🚀 Deployment Guide

### Step 1 — AWS SES Setup

1. Log in to [AWS Console](https://console.aws.amazon.com) → Search **SES**
2. Go to **Verified Identities** → **Create Identity** → verify your email or domain
3. Go to **Account Dashboard** → **SMTP Settings** → **Create SMTP Credentials**
4. Save your:
   - `SMTP Username`
   - `SMTP Password`
   - `SMTP Endpoint` (e.g. `email-smtp.us-east-1.amazonaws.com`)
5. Request **Production Access** to send to any email (otherwise sandbox only)

---

### Step 2 — Deploy Backend to Render (Free)

1. Create a GitHub repo called `email-tool-backend`
2. Push the contents of the `backend/` folder to it
3. Go to [render.com](https://render.com) → **New** → **Web Service**
4. Connect your GitHub repo
5. Set:
   - **Build Command**: `npm install`
   - **Start Command**: `node server.js`
   - **Environment**: Node
6. Add **Environment Variables**:
   ```
   SES_SMTP_HOST = email-smtp.us-east-1.amazonaws.com
   SES_SMTP_USER = YOUR_SES_SMTP_USERNAME
   SES_SMTP_PASS = YOUR_SES_SMTP_PASSWORD
   ```
7. Click **Create Web Service**
8. Copy your Render URL, e.g. `https://email-tool-xyz.onrender.com`

---

### Step 3 — Deploy Frontend to GitHub Pages (Free)

1. Create a GitHub repo called `email-tool` (or any name)
2. Upload `frontend/index.html` to the repo
3. **Edit `index.html`**: Replace the `API` variable on line ~240:
   ```js
   const API = "https://email-tool-xyz.onrender.com"; // ← your Render URL
   ```
4. Go to repo **Settings** → **Pages**
5. Set Source: `Deploy from a branch` → `main` → `/ (root)`
6. Click **Save** — your site will be live at:
   ```
   https://yourusername.github.io/email-tool
   ```

---

## 🔐 Security Notes

- **Never commit `.env`** — use Render's environment variables dashboard
- Rate limiting is enabled (30 req/min per IP)
- All inputs are validated server-side
- Add `ALLOWED_ORIGIN` env var and configure CORS for production:
  ```js
  app.use(cors({ origin: "https://yourusername.github.io" }));
  ```

---

## 📡 API Reference

### `POST /verify`
```json
{ "email": "john@example.com" }
```
Returns: format, disposable, MX, SMTP checks, score (0–100), status

### `POST /find`
```json
{ "firstName": "John", "lastName": "Doe", "domain": "company.com" }
```
Returns: 10 email patterns with MX validation

### `POST /bulk-verify`
```json
{ "emails": ["a@x.com", "b@y.com"] }
```
Returns: array of results (max 20 emails)

---

## 🧰 Local Development

```bash
cd backend
cp .env.example .env
# Fill in your AWS SES credentials in .env

npm install
npm run dev  # uses nodemon for hot-reload
```

Open `frontend/index.html` in your browser and change `API` to `http://localhost:3000`.

---

## 📦 Tech Stack

| Layer | Tech |
|---|---|
| Frontend | HTML, CSS, Vanilla JS |
| Backend | Node.js, Express |
| Email SMTP | AWS SES |
| DNS Checks | Node `dns` module |
| Rate Limiting | `express-rate-limit` |
| Frontend Hosting | GitHub Pages (free) |
| Backend Hosting | Render.com (free tier) |
