
╔══════════════════════════════════════════════════════════════╗
║              CV FORGE — COMPLETE SETUP GUIDE                 ║
║         From zero to live in about 1 hour                    ║
║         No coding knowledge required                         ║
╚══════════════════════════════════════════════════════════════╝

Read this top to bottom before you start.
Every step is in order. Don't skip ahead.


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 WHAT YOU WILL SET UP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  1. Appwrite — stores user accounts (free, takes 10 min)
  2. Paymob  — collects payments (free account, needs approval)
  3. Netlify  — hosts your website (free, takes 5 min)

The flow users experience:
  → Visit site → Sign up (creates account in Appwrite)
  → See paywall → Pay $1.99 (goes through Paymob)
  → Payment verified → Full app access forever

Your admin account skips payment entirely — always has full access.


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 YOUR FILE STRUCTURE (what each file does)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

cvforge-complete/                     ← Drag ALL of this to Netlify
│
├── index.html                        ← Website homepage (landing page)
├── features.html                     ← Features page
├── examples.html                     ← Resume examples by industry
├── faq.html                          ← FAQ page
├── about.html                        ← About page
├── contact.html                      ← Contact page
├── privacy.html                      ← Privacy policy
├── terms.html                        ← Terms of service
├── sitemap.xml                       ← For Google indexing
├── robots.txt                        ← For Google crawling
├── netlify.toml                      ← Routing config — don't touch
│
├── assets/                           ← Website shared files
│   ├── css/site.css                  ← Website styling
│   └── js/site.js                    ← Website JS (nav, animations)
│
├── blog/                             ← Blog articles
│   ├── index.html                    ← Blog homepage
│   └── what-is-ats.html              ← Blog article (add more here)
│
├── examples/                         ← Resume example pages (add more here)
│
├── app/                              ← THE CV BUILDER APP
│   ├── index.html                    ← App entry point
│   ├── css/styles.css                ← App styling
│   └── js/
│       ├── config.js                 ★ EDIT THIS — price, admin, Appwrite ID
│       ├── auth.js                   ← Sign in / sign up / logout
│       ├── payment.js                ← Payment flow
│       ├── cv-data.js                ← CV form and data
│       ├── cv-render.js              ← Builds the CV document
│       ├── ui.js                     ← UI logic
│       └── app.js                    ← App boot controller
│
└── netlify/
    └── functions/
        ├── create-payment.js         ← Server: Paymob payment creation
        └── verify-payment.js         ← Server: payment verification


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 STEP 1 — SET UP APPWRITE (User Accounts)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Appwrite stores your users' accounts and handles login securely.
It's completely free for your usage level.

─── 1a. Create your Appwrite account ──────────────────────────

  1. Open your browser and go to:  https://cloud.appwrite.io
  2. Click "Sign Up"
  3. Enter your name, email, and a strong password
  4. Check your email and click the confirmation link
  5. You are now logged in to the Appwrite dashboard

─── 1b. Create your project ───────────────────────────────────

  1. Click the big "+" button or "Create Project"
  2. In "Project Name" type:  CVForge
  3. Leave everything else as default
  4. Click "Create"
  5. You will land on your project dashboard

  ★ IMPORTANT: Look at the top of the page
    You will see your Project ID — it looks like:  64f3c9a2b8d4e5f2a1b3c4d5
    Copy this and save it somewhere — you need it in Step 5

─── 1c. Enable Email/Password sign up ─────────────────────────

  This allows users to create accounts on your app.

  1. In the left sidebar, click "Auth"
  2. Click the "Settings" tab
  3. Find "Email/Password" in the list
  4. Make sure the toggle is turned ON (green)
  5. Click "Save" if there's a save button

─── 1d. Add your website as an allowed platform ───────────────

  This is security — Appwrite will only accept requests from your domain.
  You must add BOTH your Netlify URL AND localhost (for testing).

  FIRST: Add localhost (for testing on your computer)
  1. In the left sidebar, click "Settings"
  2. Click the "Platforms" tab
  3. Click "Add Platform"
  4. Choose "Web"
  5. Name: Local Testing
  6. Hostname: localhost
  7. Click "Create"

  SECOND: Add your Netlify domain (you will get this in Step 3)
  Come back here after Step 3 and repeat the steps above with:
  Name: CV Forge Live
  Hostname: your-site-name.netlify.app
  (Replace "your-site-name" with your actual Netlify subdomain)

─── What you have after Step 1 ────────────────────────────────

  ✅ Appwrite account created
  ✅ Project created
  ✅ Email/Password auth enabled
  ✅ Your Project ID saved (looks like: 64f3c9a2b8d4e5f2a1b3c4d5)


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 STEP 2 — SET UP PAYMOB (Payments)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Paymob handles the actual money transfer.
They accept: Visa, Mastercard, Meeza, Vodafone Cash, Orange Money.
They charge about 2.75% per transaction.
Payouts go directly to your Egyptian bank account.

─── 2a. Create your Paymob account ────────────────────────────

  1. Go to:  https://accept.paymob.com/portal2/en/signup
  2. Fill in your details
  3. When asked for business type, select "Individual" or "Sole Trader"
  4. Upload your National ID when asked
  5. Submit and wait for approval email (1 to 3 business days)
  6. You will get an email when approved — check spam folder too

─── 2b. Collect your 3 secret keys ───────────────────────────

  Once approved, log into your Paymob dashboard at:
  https://accept.paymob.com

  KEY 1 — API Key:
  1. Click "Settings" in the left sidebar
  2. Click "Account Info"
  3. Find the "API Key" field
  4. Copy the long string and save it

  KEY 2 — Integration ID:
  1. Click "Developers" in the left sidebar
  2. Click "Payment Integrations"
  3. You should see a "Card Payments" integration listed
     If not: click "+ Add Integration" → choose "Online Card Payments"
  4. Copy the NUMBER shown as "Integration ID" (example: 5576664)

  KEY 3 — HMAC Secret:
  1. Click "Settings" → "Account Info"
  2. Find the "HMAC" field (it may be hidden behind a button)
  3. Click "View" or the eye icon next to it
  4. Copy that value and save it

─── 2c. Set your callback URL ─────────────────────────────────

  This tells Paymob where to send users after they pay.
  You need your Netlify URL first — come back after Step 3.

  After Step 3, do this:
  1. In Paymob: Developers → Payment Integrations
  2. Click on your Card integration
  3. Find "Transaction Response URL"
  4. Set it to:  https://YOUR-SITE.netlify.app/payment-callback
  5. Find "Server Callback URL" (webhook)
  6. Set it to:  https://YOUR-SITE.netlify.app/verify-payment

─── Test cards (use these before going live) ──────────────────

  Switch Paymob to TEST mode first (toggle in top menu).

  Successful payment test card:
    Card Number: 4987 6542 3456 5888
    Expiry:      05/21
    CVV:         100

  Failed payment test card:
    Card Number: 4000 0000 0000 0002

─── What you have after Step 2 ────────────────────────────────

  ✅ Paymob account created and approved
  ✅ API Key saved
  ✅ Integration ID saved
  ✅ HMAC Secret saved


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 STEP 3 — DEPLOY TO NETLIFY (Your Website Goes Live)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

─── 3a. Create Netlify account ────────────────────────────────

  1. Go to:  https://netlify.com
  2. Click "Sign Up"
  3. Sign up with your email or GitHub account
  4. Confirm your email if asked

─── 3b. Deploy your site ──────────────────────────────────────

  CRITICAL: You must drag the FILES INSIDE the folder,
  not the folder itself. Here is exactly what to do:

  1. Open your Netlify dashboard
  2. Click "Add new site"
  3. Choose "Deploy manually"
  4. A dashed box appears saying "Drag and drop your site folder here"

  Now on your computer:
  5. Find the cvforge-final folder and open it
  6. You should now see the files inside:
     index.html, netlify.toml, css folder, js folder, netlify folder
  7. Select ALL of these items at once
     Windows: click one file → Ctrl+A to select all
     Mac:     click one file → Cmd+A to select all
  8. Drag the selected items into the Netlify dashed box
  9. Wait about 30 seconds
  10. Netlify shows you a URL like: https://brave-einstein-abc123.netlify.app
  11. Copy this URL — you need it for Steps 2c, 4, and 5

  ★ RENAME YOUR SITE (recommended):
  1. In Netlify: Site Settings → General → Site name
  2. Change it to something like "cvforge-app"
  3. Your URL becomes:  https://cvforge-app.netlify.app

─── What you have after Step 3 ────────────────────────────────

  ✅ Site is live at your Netlify URL
  ✅ BUT login and payment do not work yet — that is Steps 4 and 5


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 STEP 4 — ADD YOUR SECRET KEYS TO NETLIFY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

These keys stay on Netlify's servers. Users can never see them.
This is what keeps the payment system secure.

─── How to add environment variables ──────────────────────────

  1. In Netlify, open your site
  2. Click "Site configuration" (or "Site settings")
  3. Click "Environment variables" in the left menu
  4. Click "Add a variable" for each one below

─── Variables to add (copy key names exactly) ─────────────────

  Key name                  Value
  ──────────────────────────────────────────────────────────────
  PAYMOB_API_KEY            your Paymob API Key from Step 2b
  PAYMOB_INTEGRATION_ID     your Integration ID (e.g. 5576664)
  PAYMOB_HMAC               your HMAC Secret from Step 2b
  BASE_URL                  https://your-site.netlify.app
  PRICE_CENTS               199
  CURRENCY                  USD
  ──────────────────────────────────────────────────────────────

  Replace "your-site.netlify.app" with your actual Netlify URL.
  PRICE_CENTS 199 means $1.99. Change to 299 for $2.99, etc.

─── Trigger a redeploy ────────────────────────────────────────

  After adding all variables the site must restart.

  1. Click "Deploys" in the Netlify top menu
  2. Click "Trigger deploy"
  3. Choose "Deploy site"
  4. Wait 1 to 2 minutes

─── What you have after Step 4 ────────────────────────────────

  ✅ Payment keys securely stored
  ✅ Payment system will now work


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 STEP 5 — CONNECT APPWRITE TO YOUR SITE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

─── 5a. Add your Project ID to the app ────────────────────────

  1. On your computer, open the file:  app/js/config.js
     Use Notepad (Windows) or TextEdit (Mac)

  2. Near the top, find this line:
       projectId:  'YOUR_PROJECT_ID',

  3. Replace  YOUR_PROJECT_ID  with your actual ID from Step 1b
     Example result:
       projectId:  '64f3c9a2b8d4e5f2a1b3c4d5',

  4. Save the file

─── 5b. Add your Netlify domain to Appwrite ───────────────────

  1. Go to cloud.appwrite.io → your CVForge project
  2. Settings → Platforms → Add Platform → Web
  3. Name: CV Forge Live
  4. Hostname: your-site.netlify.app   (no https://)
  5. Click Create

─── 5c. Redeploy with updated config.js ───────────────────────

  Since you changed config.js, upload all files to Netlify again.

  1. Netlify → Deploys → drag-and-drop box at the bottom
  2. Select all files from cvforge-final folder
  3. Drag and drop
  4. Wait for deploy to complete

─── What you have after Step 5 ────────────────────────────────

  ✅ Users can create accounts
  ✅ Login and logout work
  ✅ Payment works
  ✅ Everything is connected and live


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 STEP 6 — SET PAYMOB CALLBACK URLS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Now that you have your live Netlify URL, go back to Paymob.

  1. Paymob dashboard → Developers → Payment Integrations
  2. Click on your Card integration
  3. Transaction Response URL:
     https://YOUR-SITE.netlify.app/payment-callback
  4. Server Callback URL:
     https://YOUR-SITE.netlify.app/verify-payment
  5. Save

  Replace YOUR-SITE with your actual Netlify subdomain.


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 STEP 7 — TEST EVERYTHING BEFORE GOING LIVE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  First: switch Paymob to TEST mode (top-left toggle in dashboard).

  Open your Netlify URL in an INCOGNITO browser window.
  Incognito is important — it has no saved sessions.

  Test this exact sequence:

  Step A: Sign Up
  1. Click "Create Account"
  2. Fill in name, job title, email, password (8+ chars)
  3. Click "Create Account"
  ✅ Should show: Payment screen with $1.99

  Step B: Pay
  1. Click "Get Instant Access"
  ✅ Should go to: Paymob payment page
  2. Enter test card: 4987 6542 3456 5888
     Expiry: 05/21  CVV: 100
  3. Complete payment
  ✅ Should redirect back to your site
  ✅ Should show: CV builder app (not the paywall)

  Step C: Confirm it remembers
  1. Sign out (arrow button top right)
  2. Sign back in with same email and password
  ✅ Should go straight to: CV builder app (no payment again)

  Step D: Test admin
  1. Open new incognito window
  2. Sign in with admin credentials from js/config.js
  ✅ Should go straight to: CV builder with ADMIN badge
  ✅ No payment screen ever

  When all 4 steps pass:
  → Switch Paymob back to LIVE mode
  → You are ready to take real payments


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 YOUR ADMIN ACCOUNT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Default credentials — CHANGE THESE before going live:

  Email:    cvforge.admin@proton.me
  Password: Cv@F0rge#2025!Admin

  These are in app/js/config.js under the "admin" section.

  TO CHANGE THEM:
  1. Open app/js/config.js
  2. Find:
       admin: {
         email:      'cvforge.admin@proton.me',
         password:   'Cv@F0rge#2025!Admin',
         bypass_key: 'ADMIN-CVFORGE-2025',
       }
  3. Change all three to your own values
  4. bypass_key can be anything random, like 'MY-KEY-X829ZQ'
  5. Save and redeploy

  RULES:
  ● Never share js/config.js
  ● Never post admin credentials online
  ● Admin login works without Appwrite or internet
    (it is checked locally before making any server calls)
  ● Changing bypass_key immediately logs out any existing admin


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 HOW TO UPDATE THE APP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  After any file change:
  1. Save the file on your computer
  2. Go to Netlify → your site → Deploys
  3. Scroll to the drag-and-drop box
  4. Select all files from cvforge-final
  5. Drag and drop → wait 30 seconds → live

  QUICK CHANGE REFERENCE:

  Change price display on screen:
  → js/config.js → price.display

  Change the actual charge:
  → Netlify → Environment Variables → PRICE_CENTS
  → 199=$1.99 | 299=$2.99 | 499=$4.99 | 999=$9.99

  Change admin credentials:
  → app/js/config.js → admin section

  Change app colors:
  → app/css/styles.css → :root variables at the top

  Change features on paywall screen:
  → app/js/config.js → features array

  Change AI writing prompts:
  → app/js/config.js → prompts section


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 TROUBLESHOOTING
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  "Page not found" when opening site
  → You dragged the folder, not the contents
  → Open the folder, select everything inside, drag those

  "Could not start payment"
  → Netlify → Site config → Environment Variables
    Check PAYMOB_API_KEY has no extra spaces
  → Netlify → Deploys → Functions tab → create-payment → view logs
    The log shows the exact Paymob error message
  → Make sure Paymob account is fully approved

  Sign in gives error / login not working
  → Check Appwrite Project ID is correct in js/config.js
  → Appwrite → Settings → Platforms → add your Netlify domain
  → Appwrite → Auth → Settings → Email/Password is ON

  Paid but still seeing payment screen
  → Open browser console (press F12 → Console tab)
  → Type:  localStorage.clear()
  → Press Enter, then refresh and sign in again

  Changes not showing after redeploy
  → Hard refresh: Ctrl+Shift+R (Windows) or Cmd+Shift+R (Mac)


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 COMPLETE CHECKLIST — BEFORE GOING LIVE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  APPWRITE
  □ Account created at cloud.appwrite.io
  □ Project "CVForge" created
  □ Project ID copied and saved
  □ Email/Password auth enabled
  □ localhost added as platform
  □ Your Netlify domain added as platform

  PAYMOB
  □ Account created and approved
  □ API Key saved
  □ Integration ID saved
  □ HMAC Secret saved
  □ Transaction Response URL set
  □ Server Callback URL set

  YOUR CODE
  □ js/config.js — Appwrite Project ID updated
  □ js/config.js — Admin email changed from default
  □ js/config.js — Admin password changed from default
  □ js/config.js — bypass_key changed from default

  NETLIFY
  □ Account created
  □ Site deployed (files selected correctly)
  □ PAYMOB_API_KEY added
  □ PAYMOB_INTEGRATION_ID added
  □ PAYMOB_HMAC added
  □ BASE_URL added
  □ PRICE_CENTS set to 199
  □ CURRENCY set to USD
  □ Redeployed after variables

  TESTING
  □ Sign up → payment → app works
  □ Sign out → sign in → goes to app (no second payment)
  □ Admin login → app with badge (no payment)
  □ Switched Paymob to LIVE mode

  ★ All checked = you are live and earning ★


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 SUPPORT CONTACTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Paymob:   support@paymob.com  |  hotline: 19079
  Appwrite: https://appwrite.io/discord
  Netlify:  https://answers.netlify.com
