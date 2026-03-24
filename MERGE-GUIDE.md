# Repo Merge & Migration Guide
## Moving AI Readiness Assessment to the main website repo

---

## Step 1: Copy AI Readiness page into main repo

From your `koios-tools` repo, copy the assessment file:

```bash
cd ~/koios-website  # your main site repo

# Copy from your tools repo (adjust path as needed)
cp ~/koios-tools/ai-readiness-assessment.html ./ai-readiness-assessment.html
```

## Step 2: Fix broken links in ai-readiness-assessment.html

Open the file and make these changes:

**Replace in the nav/header:**
```
OLD: href="https://koios-analytics.com/resources"
NEW: href="/#services"

OLD: href="https://koios-analytics.com/contact-us"
NEW: href="/#about"
```

**Replace in the footer:**
```
OLD: href="https://koios-analytics.com/resources"
NEW: href="/#services"

OLD: href="https://koios-analytics.com/about-us"
NEW: href="/#about"

OLD: href="https://koios-analytics.com/contact-us"
NEW: href="mailto:solutions@koios-analytics.com"
```

**Add favicon (in <head>):**
```html
<link rel="icon" type="image/png" href="/favicon.png">
```

## Step 3: Add your favicon

Copy your `favicon.png` to the root of the repo:

```bash
cp ~/path-to/favicon.png ./favicon.png
```

## Step 4: Commit and push

```bash
git add .
git commit -m "Merge AI readiness page, add EU compliance assessment, add favicon"
git push
```

## Step 5: Retire tools subdomain (gradual)

Don't delete the tools repo yet. Instead:

1. Add a redirect in the old `koios-tools` repo. Create an `index.html`:

```html
<!DOCTYPE html>
<html>
<head>
  <meta http-equiv="refresh" content="0;url=https://koios-analytics.com/ai-readiness-assessment.html">
  <link rel="canonical" href="https://koios-analytics.com/ai-readiness-assessment.html">
</head>
<body>
  <p>Redirecting to <a href="https://koios-analytics.com/ai-readiness-assessment.html">koios-analytics.com</a>...</p>
</body>
</html>
```

2. Also update `ai-readiness-assessment.html` in the tools repo to redirect:

```html
<!-- Add to <head> -->
<meta http-equiv="refresh" content="0;url=https://koios-analytics.com/ai-readiness-assessment.html">
<link rel="canonical" href="https://koios-analytics.com/ai-readiness-assessment.html">
```

3. After a month (once search engines have re-indexed), you can remove the
   CNAME `tools` record from GoDaddy DNS.

## Final site structure

```
koios-website/
├── index.html                    ← Homepage
├── ai-readiness-assessment.html  ← AI Readiness (moved from tools)
├── eu-ai-compliance.html         ← EU AI Act Assessment (NEW)
├── odoo-partnership.html         ← Odoo partner page
├── favicon.png                   ← Your favicon
├── CNAME                         ← koios-analytics.com
├── sitemap.xml                   ← Updated with all pages
├── robots.txt
├── assets/
│   ├── logo-white.png
│   └── logo.svg
└── DEPLOYMENT.md
```

## Updated internal links

All pages now use root-relative paths:
- `/` → Homepage
- `/ai-readiness-assessment.html` → AI Readiness
- `/eu-ai-compliance.html` → EU AI Compliance
- `/odoo-partnership.html` → Odoo Partnership
- `/#services` → Services section on homepage
- `/#results` → Results section on homepage
- `/#about` → About section on homepage
