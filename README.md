# PhysioFind

A patient-facing web app that closes the loop on physical therapy referrals. Patients enter a ZIP code anywhere in the US, see nearby PT clinics with tap-to-call numbers, get a script for verifying insurance by phone, and finish by generating a ready-to-send message that tells their provider's office when they're booked and where to fax the referral.

Works nationwide — clinic results come live from the Google Places API at search time.

## What you need (all free tiers)

1. A **GitHub** account (github.com) — to store the code
2. A **Vercel** account (vercel.com) — to host the app; sign up with your GitHub account
3. A **Google Cloud** account (console.cloud.google.com) — for the clinic search API

## Setup, step by step (~30 minutes, no coding)

### 1. Get a Google Places API key

1. Go to console.cloud.google.com and create a new project (call it anything, e.g. "pt-finder")
2. In the search bar, find **"Places API (New)"** and click **Enable**
3. Go to **APIs & Services → Credentials → Create Credentials → API key**
4. Copy the key somewhere safe
5. (Recommended) Click the key, and under "API restrictions" restrict it to **Places API (New)** only

Google's free tier includes thousands of searches per month — more than enough for a pilot. The app also caches repeated ZIP lookups for 24 hours to conserve quota.

### 2. Put the code on GitHub

1. On github.com, click **New repository**, name it `pt-referral-finder`, keep it private, create it
2. Click **"uploading an existing file"** and drag in everything from this folder (keep the folder structure: `api/`, `src/`, plus the files at the top level)
3. Commit

### 3. Deploy on Vercel

1. On vercel.com, click **Add New → Project** and import your `pt-referral-finder` repository
2. Before deploying, open **Environment Variables** and add:
   - Name: `GOOGLE_MAPS_API_KEY`
   - Value: the API key from step 1
3. Click **Deploy**

Vercel gives you a URL like `pt-referral-finder.vercel.app` — that's your live app. To use a custom domain instead (e.g., `physiofind.app`), buy the domain on Namecheap or GoDaddy, then add it to your Vercel project under **Settings → Domains**.

## Customizing per provider (no accounts needed)

Any provider can tailor PhysioFind just by adding parameters to the link:

- `?provider=Dr.%20Smith%27s%20office` — personalizes the "tell your provider" step
- `?partners=Spaulding,ATI,Bay%20State` — clinic networks matching these names get a "Works directly with your care team" badge

Example link for an orthopedic practice in Boston:

```
https://physiofind.app/?provider=Dr.%20Uppstrom%27s%20office&partners=BMC,Spaulding,ATI,Bay%20State
```

Providers who want to use PhysioFind can visit your live link and customize it with their own practice name and partner networks — no setup required on their end.

## Privacy posture

- The only data sent to the server is a ZIP code
- Insurance selection, referral reason, appointment details, and the generated message stay in the patient's browser and are never transmitted or stored
- Nothing about the patient is logged

This design keeps the app outside PHI territory: the actual referral transmission still happens through the provider's existing compliant channels (portal message + office fax). **Before promoting this to patients as part of clinical care, run it by your organization's compliance team.**

## Local development (optional)

```
npm install
npm install -g vercel
vercel dev
```

`vercel dev` runs both the frontend and the `/api/clinics` function locally. Create a `.env` file containing `GOOGLE_MAPS_API_KEY=your-key`.

## Ideas for v2

- Distance display (geocode the ZIP and compute miles per clinic)
- Filter by "open Saturdays" or specialty keywords
- Direct fax integration via a HIPAA-compliant fax API (SRFax, Documo) — requires a BAA and real compliance review
- Provider dashboard with per-patient booking status — requires authentication and HIPAA infrastructure

