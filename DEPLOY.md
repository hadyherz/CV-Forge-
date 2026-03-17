# CV Forge — Complete Deployment Guide
## Go Live in Under 1 Hour · 100% Free Tools

---

## What You Need to Set Up

| Service | What It Does | Cost | Time |
|---------|-------------|------|------|
| **Cloudflare Pages** | Hosts your website | Free forever | 5 min |
| **Appwrite Cloud** | User accounts & login | Free (75K requests/month) | 10 min |
| **Paymob** | Accepts payments in Egypt | Free account (2.75% per sale) | 1–3 days approval |

> **Why these tools?**
> Cloudflare Pages is faster and more reliable than Netlify on the free tier —
> unlimited bandwidth, global CDN, and built-in serverless functions.
> Appwrite is purpose-built for auth and has the most generous free tier
> of any backend-as-a-service. Paymob is the only option for Egyptian
> payment methods (Meeza, Vodafone Cash, Orange Money).

---

## The User Flow (What Your Customers Experience)

```
Visit site → Sign Up → See Paywall → Pay $1.99
     ↓
Paymob verifies payment → Full app access forever
     ↓
Sign in again any time → Goes straight to app
```

---

## Tool Comparison (All Free Tiers)

### Hosting Options

| Tool | Free Bandwidth | Serverless Functions | Custom Domain | Speed |
|------|---------------|---------------------|---------------|-------|
| **Cloudflare Pages** ⭐ | Unlimited | 100K req/day | Free SSL | Fastest |
| Netlify | 100GB/month | 125K req/month | Free SSL | Fast |
| Vercel | 100GB/month | Limited | Free SSL | Fast |
| GitHub Pages | Unlimited | ❌ None | Free SSL | Medium |

**Winner: Cloudflare Pages** — unlimited bandwidth and the most generous
serverless function limits on the free tier. Use this.

### Auth / User Accounts Options

| Tool | Free Users | Easy Setup | Egypt Support |
|------|-----------|------------|---------------|
| **Appwrite Cloud** ⭐ | 75,000/month | Very easy | Yes |
| Supabase | Unlimited | Easy | Yes |
| Firebase Auth | Unlimited | Medium | Yes |
| Clerk | 10,000/month | Easiest | Yes |

**Winner: Appwrite Cloud** — already integrated in your app code.
Supabase is a strong second option if you want to switch.

---

## Step 1 — Set Up Appwrite (User Accounts)

### 1a. Create Your Account

1. Go to **https://cloud.appwrite.io**
2. Click **Sign Up**
3. Enter name, email, password
4. Confirm your email

### 1b. Create Your Project

1. Click **Create Project**
2. Name it: `CVForge`
3. Select region closest to you — choose **Frankfurt (EU)** or **NYC**
4. Click **Create**

> ⚠️ **Save your Project ID** — it appears at the top of the dashboard.
> It looks like: `64f3c9a2b8d4e5f2a1b3c4d5`

### 1c. Enable Email/Password Login

1. Left sidebar → **Auth**
2. Click **Settings** tab
3. Find **Email/Password** → toggle **ON**
4. Click **Update**

### 1d. Add Your Domain as Allowed Platform

This is a security setting. Appwrite only accepts requests from domains you approve.

**Add localhost first (for testing):**
1. Left sidebar → **Settings** → **Platforms** tab
2. Click **Add Platform** → choose **Web**
3. Name: `Local Dev`  |  Hostname: `localhost`
4. Click **Create**

**Add your live domain after Step 2:**
Come back here after you get your Cloudflare URL and repeat:
- Name: `CV Forge Live`
- Hostname: `cvforge.pages.dev` ← or your custom domain

### 1e. Update Your App Config

Open `app/js/config.js` in any text editor and find:

```javascript
projectId: 'YOUR_PROJECT_ID',
```

Replace `YOUR_PROJECT_ID` with your actual ID from step 1b.

**Also change your admin credentials while you're here:**

```javascript
admin: {
  email:      'cvforge.admin@proton.me',   // ← change this
  password:   'Cv@F0rge#2025!Admin',        // ← change this
  bypass_key: 'ADMIN-CVFORGE-2025',         // ← change this
}
```

Make them something only you know. Save the file.

---

## Step 2 — Set Up Paymob (Payments)

Paymob handles the actual money — Visa, Mastercard, Meeza, Vodafone Cash, Orange Money.

### 2a. Create Account

1. Go to **https://accept.paymob.com/portal2/en/signup**
2. Fill in your details
3. Business type: **Individual** or **Sole Trader**
4. Upload your **National ID**
5. Submit — approval takes **1 to 3 business days**
6. Check your email (including spam) for approval

### 2b. Collect Your 3 Keys (After Approval)

Log in at **https://accept.paymob.com**

**Key 1 — API Key:**
- Settings → Account Info → **API Key** field → Copy

**Key 2 — Integration ID:**
- Developers → Payment Integrations
- If no integration exists: click **+ Add Integration** → Online Card Payments
- Copy the **Integration ID** number (e.g. `5576664`)

**Key 3 — HMAC Secret:**
- Settings → Account Info → **HMAC** field → click eye icon → Copy

### 2c. Test Cards (Use Before Going Live)

Switch Paymob to **Test mode** (toggle in top left of dashboard).

| Card | Result |
|------|--------|
| `4987 6542 3456 5888` · Exp: `05/21` · CVV: `100` | ✅ Success |
| `4000 0000 0000 0002` | ❌ Declined |

---

## Step 3 — Deploy to Cloudflare Pages

Cloudflare Pages is your website host — it puts your files online.

### 3a. Create a Cloudflare Account

1. Go to **https://dash.cloudflare.com/sign-up**
2. Enter email and password
3. Confirm your email

### 3b. Deploy Your Site (Drag & Drop)

1. In the Cloudflare dashboard, click **Pages** in the left sidebar
2. Click **Create a project**
3. Click **Upload assets** (Direct Upload option)
4. Name your project: `cvforge` (this sets your URL)

> ⚠️ **IMPORTANT:** You must select the FILES INSIDE the folder,
> not the folder itself.

**On Windows:**
1. Open the `cvforge-complete` folder
2. Press **Ctrl + A** to select all files inside
3. Drag them into the Cloudflare upload area

**On Mac:**
1. Open the `cvforge-complete` folder
2. Press **Cmd + A** to select all
3. Drag into the upload area

5. Click **Deploy site**
6. Wait ~30 seconds

Your site is now live at: **`https://cvforge.pages.dev`**
(Or whatever project name you chose)

### 3c. Add Environment Variables (Your Secret Keys)

This keeps your Paymob keys secure — users can never see them.

1. In Cloudflare Pages → your project → **Settings**
2. Click **Environment variables** → **Production**
3. Click **Add variable** for each one:

| Variable Name | Value |
|--------------|-------|
| `PAYMOB_API_KEY` | Your Paymob API Key |
| `PAYMOB_INTEGRATION_ID` | Your Integration ID (e.g. `5576664`) |
| `PAYMOB_HMAC` | Your HMAC Secret |
| `BASE_URL` | `https://cvforge.pages.dev` |
| `PRICE_CENTS` | `199` |
| `CURRENCY` | `USD` |

4. Click **Save**

### 3d. Add Routing Configuration

Cloudflare Pages needs a routing file equivalent to `netlify.toml`.
Create a file called `_redirects` in your project folder with this content:

```
/create-payment    /api/create-payment    200
/verify-payment    /api/verify-payment    200
/payment-callback  /app/index.html        200
/app/*             /app/index.html        200
```

And rename your functions folder:
- Rename `netlify/functions/` → `functions/`

> **OR** — just use Netlify instead (your `netlify.toml` already works):
> See the **Using Netlify Instead** section at the bottom of this file.

### 3e. Redeploy After Changes

Any time you change a file:
1. Cloudflare Pages → your project → **Deployments**
2. Drag and drop your files again
3. Wait 30 seconds

---

## Step 4 — Set Paymob Callback URLs

Now that you have your live URL, tell Paymob where to send users after payment.

1. Paymob → **Developers** → **Payment Integrations** → click your integration
2. **Transaction Response URL:**
   ```
   https://cvforge.pages.dev/payment-callback
   ```
3. **Server Callback URL (Webhook):**
   ```
   https://cvforge.pages.dev/verify-payment
   ```
4. Save

Replace `cvforge.pages.dev` with your actual URL if you renamed it.

---

## Step 5 — Connect Appwrite to Your Live Domain

1. Go back to **cloud.appwrite.io** → your CVForge project
2. **Settings** → **Platforms** → **Add Platform** → **Web**
3. Name: `CV Forge Live`
4. Hostname: `cvforge.pages.dev` ← no `https://`
5. Click **Create**

---

## Step 6 — Full End-to-End Test

**Switch Paymob to TEST mode first.**

Open your live URL in an **Incognito / Private** browser window.

```
Step 1: Go to https://cvforge.pages.dev
        ✅ Should see: website homepage in dark mode

Step 2: Click "Build My Resume" → goes to /app/
        ✅ Should see: Sign In / Create Account screen

Step 3: Click "Create Account" → fill in details → submit
        ✅ Should see: Paywall screen with $1.99

Step 4: Click "Get Instant Access" 
        ✅ Should go to: Paymob payment page

Step 5: Enter test card: 4987 6542 3456 5888 · 05/21 · 100
        ✅ Should return to your site

Step 6: Should see: CV Builder app (not paywall again)

Step 7: Click sign out → sign back in with same credentials
        ✅ Should go straight to: CV Builder (no second payment)

Step 8: Open new incognito tab, sign in with your admin credentials
        ✅ Should go straight to: CV Builder with ADMIN badge
```

If all 8 steps pass → **switch Paymob to Live mode** → you're open for business.

---

## Step 7 — Add a Custom Domain (Optional but Recommended)

A custom domain makes your site look professional. Free options:

### Option A — Free Domain from Freenom
1. Go to **https://www.freenom.com**
2. Search for your name (e.g. `cvforge`) 
3. Get a `.tk` or `.ml` domain free for 12 months

### Option B — Buy a Real Domain (~$10/year)
Recommended registrars:
- **Namecheap** — https://namecheap.com (cheapest, great support)
- **Porkbun** — https://porkbun.com (very cheap, easy interface)
- **GoDaddy** — https://godaddy.com (popular, slightly pricier)

### Connect Domain to Cloudflare Pages
1. Cloudflare Pages → your project → **Custom domains**
2. Click **Set up a custom domain**
3. Enter your domain → follow DNS instructions
4. SSL certificate is automatic and free

---

## Quick Reference — How to Change Things

| What to change | File to edit |
|---------------|-------------|
| Price shown on paywall | `app/js/config.js` → `price.display` |
| Actual charge amount | Cloudflare env var `PRICE_CENTS` |
| Admin login credentials | `app/js/config.js` → `admin` section |
| App colors / theme | `app/css/styles.css` → `:root` variables |
| Website colors | `assets/css/site.css` → `:root` variables |
| Paywall features list | `app/js/config.js` → `features` array |
| AI writing prompts | `app/js/config.js` → `prompts` section |
| Add a blog article | Create new `.html` in `blog/` folder |
| Add a resume example | Create new `.html` in `examples/` folder |

---

## Using Netlify Instead (Easier, Already Configured)

Your `netlify.toml` file is already set up. If you prefer Netlify:

1. Go to **https://netlify.com** → Sign Up
2. Click **Add new site** → **Deploy manually**
3. Select all files inside `cvforge-complete` → drag to the upload box
4. Wait 30 seconds → site is live
5. Go to **Site configuration** → **Environment variables** → add the same 6 variables from Step 3c above
6. Click **Trigger deploy** → **Deploy site**

Netlify URL will be: `https://brave-something-123456.netlify.app`
You can rename it in **Site settings** → **General** → **Site name**.

Everything else (Appwrite setup, Paymob setup, testing) is identical.

---

## Troubleshooting

**"Page not found" when opening the site**
→ You dragged the folder instead of the contents
→ Open the folder, select everything inside, drag those

**"Could not start payment"**
→ Check environment variables are set (no extra spaces)
→ Check Paymob account is approved (not pending)
→ In Cloudflare: Functions → Logs → check error message

**Sign in gives error**
→ Check Appwrite Project ID in `app/js/config.js`
→ Check your live domain is added in Appwrite Platforms
→ Check Email/Password auth is enabled in Appwrite

**Paid but still seeing payment screen**
→ Open browser console (F12) → type `localStorage.clear()` → refresh

**Changes not showing after redeploy**
→ Hard refresh: Ctrl+Shift+R (Windows) or Cmd+Shift+R (Mac)

---

## Security Checklist

- [ ] Changed admin email from default
- [ ] Changed admin password from default  
- [ ] Changed bypass_key from default
- [ ] Enabled 2FA on Cloudflare account
- [ ] Enabled 2FA on Appwrite account
- [ ] Enabled 2FA on Paymob account
- [ ] Paymob switched to Live mode before launch

---

## How You Get Paid

1. Customer pays $1.99 → Paymob processes it
2. Paymob deducts ~2.75% fee → you keep ~$1.94
3. Paymob pays out weekly to your Egyptian bank account
4. Add your bank: Paymob → **Settings** → **Bank Accounts**

**Revenue projections:**

| Sales/month | Monthly Revenue | Annual Revenue |
|-------------|----------------|----------------|
| 50 | ~$97 | ~$1,164 |
| 200 | ~$388 | ~$4,656 |
| 500 | ~$970 | ~$11,640 |
| 1,000 | ~$1,940 | ~$23,280 |

---

## Support Contacts

| Service | How to Get Help |
|---------|----------------|
| **Paymob** | support@paymob.com · Hotline: 19079 · Sun–Thu 9am–6pm |
| **Appwrite** | discord.gg/appwrite · docs.appwrite.io |
| **Cloudflare** | community.cloudflare.com · developers.cloudflare.com |
| **Netlify** | answers.netlify.com · docs.netlify.com |

---

*CV Forge — Built in Egypt 🇪🇬*
