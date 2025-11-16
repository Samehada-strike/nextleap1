# Google OAuth Setup Guide - FIXED FOR LOCAL DEVELOPMENT

## Quick Fix for "Error 400: redirect_uri_mismatch"

### 🔴 CRITICAL: Add Redirect URI to Google Cloud Console

**Step 1:** Go to [Google Cloud Console](https://console.cloud.google.com/) → **APIs & Services** → **Credentials**

**Step 2:** Click on your OAuth 2.0 Client ID: `485186423106-3btpb49041ufsklros5p1dc1si7h3i4c`

**Step 3:** Under **Authorized redirect URIs**, click **+ ADD URI** and add EXACTLY:
```
http://localhost:8000/login.html
```

**IMPORTANT:** Copy this EXACTLY - it must match what your app uses!

**Step 4:** Under **Authorized JavaScript origins**, add:
```
http://localhost:8000
```

**Step 5:** Click **SAVE**

**Step 6:** Wait 5-10 minutes for changes to propagate

**Step 7:** Try the Google Sign-In button again!

---

## For Production Deployment

When you deploy to production, add your production URL:
- **Authorized redirect URIs:** `https://yourdomain.com/login.html`
- **Authorized JavaScript origins:** `https://yourdomain.com`

---

## Original Guide

## Fix "Error 400: invalid_request" - Step by Step

### Step 1: Configure Authorized JavaScript Origins

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Select your project (or create one)
3. Navigate to **APIs & Services** > **Credentials**
4. Click on your OAuth 2.0 Client ID (`485186423106-3btpb49041ufsklros5p1dc1si7h3i4c`)
5. Under **Authorized JavaScript origins**, click **+ ADD URI**
6. Add these origins (depending on where you're testing):
   - For local server: `http://localhost:8000`
   - For local server (alternative port): `http://localhost:3000`
   - For file access (won't work for OAuth): `file://` ❌ (OAuth doesn't support file://)
   - For your domain: `https://yourdomain.com`

**Important:** The origin must match EXACTLY (including protocol http/https and port)

### Step 2: Configure OAuth Consent Screen

1. Go to **APIs & Services** > **OAuth consent screen**
2. Select **External** (unless you have a Google Workspace)
3. Fill in required fields:
   - **App name**: `nextleap_login` (or your preferred name)
   - **User support email**: Your email
   - **Developer contact information**: Your email
4. Click **Save and Continue**
5. In **Scopes**, make sure you have:
   - `email`
   - `profile`
   - `openid`
6. Click **Save and Continue**
7. **Test users** (IMPORTANT if app is in Testing mode):
   - Add your email: `viditdamele123@gmail.com`
   - Add any other test emails
8. Click **Save and Continue**
9. Review and click **Back to Dashboard**

### Step 3: Enable Required APIs

1. Go to **APIs & Services** > **Library**
2. Search for and **Enable**:
   - **Google+ API** (or use Google Identity Services)
   - **People API** (for user info)

### Step 4: Run a Local Server

OAuth **does NOT work** with `file://` protocol. You MUST use a local server:

**Option 1: Python (if installed)**
```bash
# Navigate to your project folder
cd C:\Users\HP\nextleap1

# Python 3
python -m http.server 8000

# Or Python 2
python -m SimpleHTTPServer 8000
```

**Option 2: Node.js (if installed)**
```bash
# Install http-server globally (one time)
npm install -g http-server

# Navigate to your project folder
cd C:\Users\HP\nextleap1

# Start server
http-server -p 8000
```

**Option 3: VS Code Live Server**
- Install "Live Server" extension in VS Code
- Right-click `login.html` and select "Open with Live Server"

### Step 5: Test the Login

1. Start your local server (e.g., `http://localhost:8000`)
2. Open `http://localhost:8000/login.html` in your browser
3. Make sure the URL in browser matches the **Authorized JavaScript origin** exactly
4. Click "Sign in with Google"
5. Sign in with your test user email

### Common Issues:

**Issue: Still getting Error 400**
- ✅ Make sure you're using `http://localhost:8000` (not `file://`)
- ✅ Check that the origin in Google Console matches your browser URL exactly
- ✅ Wait 5-10 minutes after saving changes (Google may cache)
- ✅ Clear browser cache and try again
- ✅ Make sure your email is added as a test user (if app is in Testing mode)

**Issue: "Access blocked"**
- Your app is in Testing mode and your email isn't added as a test user
- Add your email in OAuth consent screen > Test users

**Issue: "Redirect URI mismatch"**
- This shouldn't happen with OAuth2 token client, but if it does:
- Check Authorized JavaScript origins match exactly

### Current Configuration Needed:

✅ **Authorized JavaScript origins** should include:
```
http://localhost:8000
http://localhost:3000
http://127.0.0.1:8000
```

✅ **OAuth consent screen** should have:
- App name set
- Your email as test user: `viditdamele123@gmail.com`

✅ **Scopes** should include:
- `email`
- `profile`

---

**After making these changes, wait 5-10 minutes for Google to propagate changes, then try again!**

