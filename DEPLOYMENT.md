# Koios Analytics Website — Deployment Guide
## GitHub Pages + Custom Domain Setup

---

## What You're Deploying

```
koios-site/
├── index.html              ← Homepage (main site)
├── odoo-partnership.html   ← Odoo partner page
├── CNAME                   ← Custom domain config
└── assets/
    └── logo-white.png      ← Your logo
```

This replaces the GoDaddy website builder site. Your AI Readiness Assessment
at tools.koios-analytics.com stays exactly where it is — no changes needed.

---

## Step 1: Create the GitHub Repository

1. Go to https://github.com/new
2. Repository name: `koios-website` (or whatever you prefer)
3. Set to **Public** (required for free GitHub Pages)
4. Click **Create repository**

## Step 2: Push the Site Files

From your terminal (WSL2 Ubuntu):

```bash
# Navigate to where you saved the downloaded files
cd ~/koios-site

# Initialise git
git init
git add .
git commit -m "Initial Koios Analytics website"

# Connect to your repo (replace YOUR_USERNAME)
git remote add origin https://github.com/YOUR_USERNAME/koios-website.git
git branch -M main
git push -u origin main
```

## Step 3: Enable GitHub Pages

1. Go to your repo on GitHub → **Settings** → **Pages**
2. Source: **Deploy from a branch**
3. Branch: **main** / root
4. Click **Save**
5. GitHub will show: "Your site is live at https://YOUR_USERNAME.github.io/koios-website/"

Test it at that URL first before changing DNS.

## Step 4: Configure Custom Domain (koios-analytics.com)

### 4a. In GitHub:
1. Still in **Settings → Pages**
2. Under **Custom domain**, enter: `koios-analytics.com`
3. Click **Save**
4. Check **Enforce HTTPS** (may take a few minutes to become available)

### 4b. In GoDaddy DNS:

Go to GoDaddy → **My Products** → **DNS** for koios-analytics.com

**Delete or update these records:**

| Type  | Name | Value | Action |
|-------|------|-------|--------|
| A     | @    | (current GoDaddy IP) | **Change** to GitHub Pages IPs |

**Add/Update these DNS records:**

```
Type: A      Name: @    Value: 185.199.108.153    TTL: 600
Type: A      Name: @    Value: 185.199.109.153    TTL: 600
Type: A      Name: @    Value: 185.199.110.153    TTL: 600
Type: A      Name: @    Value: 185.199.111.153    TTL: 600
Type: CNAME  Name: www  Value: YOUR_USERNAME.github.io    TTL: 600
```

**IMPORTANT:** Keep the existing CNAME record for `tools` pointing to
GitHub Pages — that's your AI Readiness Assessment subdomain. Don't touch it.

### 4c. Wait for DNS Propagation
- Usually takes 10–30 minutes, can take up to 48 hours
- Test with: `dig koios-analytics.com` — should show GitHub's IPs
- HTTPS certificate auto-provisions (may take 15–30 min after DNS propagates)

## Step 5: Verify Everything Works

Check these URLs:
- [ ] https://koios-analytics.com — shows new homepage
- [ ] https://koios-analytics.com/odoo-partnership.html — Odoo page
- [ ] https://tools.koios-analytics.com/ai-readiness-assessment.html — still works
- [ ] https://www.koios-analytics.com — redirects to apex domain

---

## SEO: What to Do After Deployment

### Google Analytics
1. Go to https://analytics.google.com
2. Create a property for koios-analytics.com (or use existing)
3. Get your Measurement ID (format: G-XXXXXXXXXX)
4. In both index.html and odoo-partnership.html, find `GA_MEASUREMENT_ID`
   and replace with your actual ID

### Google Search Console
1. Go to https://search.google.com/search-console
2. Add property: koios-analytics.com
3. Verify via DNS (add TXT record in GoDaddy)
4. Submit sitemap (create a simple sitemap.xml — see below)

### Sitemap
Create a file called `sitemap.xml` in the root:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaporg/schemas/sitemap/0.9">
  <url>
    <loc>https://koios-analytics.com/</loc>
    <lastmod>2026-03-23</lastmod>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://koios-analytics.com/odoo-partnership.html</loc>
    <lastmod>2026-03-23</lastmod>
    <priority>0.8</priority>
  </url>
</urlset>
```

### robots.txt
Create a file called `robots.txt` in the root:

```
User-agent: *
Allow: /
Sitemap: https://koios-analytics.com/sitemap.xml
```

---

## GoDaddy: What Happens to the Old Site?

Once DNS points to GitHub Pages, the GoDaddy website builder site will no
longer be accessible at koios-analytics.com. You can:

- **Keep** your GoDaddy subscription for DNS management only (you need this)
- **Cancel** the Website Builder add-on if it's a separate charge
- **Keep** domain renewal active (essential)

You're only moving the website hosting, not the domain registration.
GoDaddy stays as your domain registrar and DNS provider.

---

## Making Changes

To update the site:
1. Edit the HTML files locally
2. Commit and push:
   ```bash
   git add .
   git commit -m "Update hero headline"
   git push
   ```
3. GitHub Pages deploys automatically (usually under 60 seconds)

---

## Quick Reference: File Locations

| What | Where |
|------|-------|
| Homepage | `index.html` |
| Odoo page | `odoo-partnership.html` |
| Logo | `assets/logo-white.png` |
| Custom domain | `CNAME` |
| Hero headline | `index.html` line ~142 (search for "HERO HEADLINE") |
| Google Analytics ID | Both HTML files (search for `GA_MEASUREMENT_ID`) |
| Calendar link | Search for `calendar.app.google` |
| Email | Search for `solutions@koios-analytics.com` |
