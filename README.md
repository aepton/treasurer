# Treasurer

A single-page web app for PTA treasurer tasks: writing checks, reconciling bank statements, updating monthly balances, and exporting budget PDFs — all backed by your existing Google Sheets.

## Setup

### 1. Create a GitHub repo and enable Pages

1. Create a new GitHub repo (can be private if you're on a paid plan)
2. Push this code to the `main` branch
3. Go to **Settings → Pages**
4. Set source to **GitHub Actions**
5. The workflow in `.github/workflows/deploy.yml` will deploy automatically on every push to `main`

### 2. Point your DNS

In your DNS provider, add a CNAME record:

```
Type:  CNAME
Name:  treasurer   (or whatever subdomain you want)
Value: YOUR-GITHUB-USERNAME.github.io
```

Then in GitHub: **Settings → Pages → Custom domain**, enter your full domain (e.g. `treasurer.yourschool.org`) and check "Enforce HTTPS".

### 3. Create a Google Cloud project

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a new project (e.g. "PTA Treasurer")
3. Enable these APIs:
   - Google Sheets API
   - Google Drive API
4. Go to **APIs & Services → OAuth consent screen**
   - User type: **External**
   - App name: PTA Treasurer
   - Add your email as a test user
   - Scopes: `spreadsheets`, `drive`, `openid`, `profile`, `email`
5. Go to **APIs & Services → Credentials → Create Credentials → OAuth 2.0 Client ID**
   - Application type: **Web application**
   - Authorized JavaScript origins: add your domain (e.g. `https://treasurer.yourschool.org`) and `http://localhost` for local testing
   - Copy the **Client ID**

### 4. Add your Client ID to the app

In `index.html`, find line:

```js
const GOOGLE_CLIENT_ID = 'YOUR_GOOGLE_CLIENT_ID_HERE';
```

Replace with your actual client ID (ends in `.apps.googleusercontent.com`).

Push to `main` — GitHub Actions will redeploy automatically.

### 5. Share your sheets with the app

The Google account you sign in with needs **Editor** access to both spreadsheets. Since you own them, you're all set.

---

## Sheet IDs (pre-configured)

| Sheet | ID |
|---|---|
| Budget | `1x6HmaCEN2KqBT6pwj4xLHxOcKJfNHHKUCloWyIN72GQ` |
| Receipts | `1P0kgUbo5uDuG7C0EUNRTPxraQo9FAx53wvm9cMESzpA` |
| Drive receipts folder | `1AokY0bNh_N0dhH5b5Qe2kBmHG2Jvyz6M` |

These are hardcoded in `index.html`. Update them there if they ever change.

---

## Local development

Just open `index.html` in a browser — no build step needed. Make sure `http://localhost` is in your OAuth authorized origins.

## Making changes

Edit `index.html`, commit, push to `main`. The GitHub Action deploys in ~30 seconds.
