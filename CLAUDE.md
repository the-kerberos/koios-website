# Koios Analytics Website

## Project Overview
Production website for Koios Analytics — a strategic AI partner for mid-market companies. Founded by Fabien Zablocki, based in Belgium (Wallonia). The site is deployed on GitHub Pages at koios-analytics.com.

## Tech Stack
- Static HTML/CSS/JS — no build step, no framework
- GitHub Pages for hosting
- DM Sans (body) + Playfair Display (headings) from Google Fonts
- Inline SVG logos (no external image dependencies for logos)
- Google Analytics (G-RWN5PE15JP) on all pages

## Site Structure
```
koios-website/
├── index.html                    ← Main homepage (largest file, ~85KB)
├── ai-readiness-assessment.html  ← AI Readiness tool (15 questions, lead capture)
├── eu-ai-compliance.html         ← EU AI Act assessment (12 questions, lead capture)
├── odoo-partnership.html         ← Odoo partner page
├── favicon.png                   ← Favicon
├── CNAME                         ← GitHub Pages custom domain (koios-analytics.com)
├── sitemap.xml
├── robots.txt
├── CLAUDE.md                     ← This file
└── assets/
    ├── logo-white.png            ← Original PNG logo (needed for fallback)
    └── logo.svg                  ← SVG version of logo
```

## Design System

### Colours
| Name | Hex | Usage |
|------|-----|-------|
| Navy | #0a1628 | Primary dark bg, text |
| Navy-mid | #0f1d35 | Nav, darker sections |
| Gold | #c8a44e | Primary accent, CTAs |
| Gold-light | #e0c476 | Hover states |
| Cream | #f8f7f4 | Page background |
| Gray-50 | #f3f2ee | Alternate section bg |
| Teal | #2a9d8f | Secondary accent, checkmarks |
| Red-soft | #e07a5f | EU AI Act accent, alerts |
| EC Blue | #003399 | EC credential badge only |
| MIT Red | #A51E36 | MIT credential badge only |

### Typography
- Body: 'DM Sans', 400/500/600/700 weights
- Headings: 'Playfair Display', 600/700/800
- Base size: ~0.85-1rem for body, clamp() for responsive headings

### Logo
The logo is embedded as inline SVG throughout all pages (not as an <img>). This ensures it renders in any context without asset loading issues. The SVG contains the Σ sigma symbol in a circle + "KOIOS ANALYTICS" + tagline "Artificial intelligence, real solutions".

### Credential Badges
- EC badge: filled circle #003399 with 12 gold #FFED00 stars (EU flag pattern)
- MIT badge: shield shape filled #A51E36 with white columns (architectural motif)
- MSc badge: circle outline #c8a44e with Σ sigma
- ISO badge: circle outline #2a9d8f with "ISO 27001"

## Key Features

### Language Toggle (EN/FR/NL)
- Located in the nav bar as three small buttons
- Homepage (index.html) has a full translation dictionary object `T` with keys for en/fr/nl
- `applyLang()` function maps CSS selectors to dictionary keys
- Persists choice via localStorage
- EU AI compliance page has the toggle UI but translations not yet wired up
- AI Readiness page needs the toggle added and translations wired

### Chatbot Widget ("Ask Koios AI")
- Floating gold FAB button bottom-right of homepage
- Calls FastAPI backend at https://ask-koios-ai.onrender.com/chat
- Backend repo: separate GitHub repo (koios-chatbot)
- Knowledge base is in the system prompt in main.py

### Lead Capture
- Both assessment pages capture leads to Google Sheets via Apps Script
- Fire-and-forget POST (mode: 'no-cors')
- Also sends email notification to solutions@koios-analytics.com

### Scroll Reveal Animation
- `.reveal` class + IntersectionObserver
- `.reveal-delay-1` through `.reveal-delay-3` for staggered animation

## Content / Positioning

### Hero (Option D — "Classroom to Boardroom")
"From Teaching AI Strategy at MIT to Implementing It in Your Business"

### Terminology
- Use "Strategic AI Partner" (NOT "Fractional AI CTO")
- Use "Embedded AI Leadership" for the ongoing presence service card
- Use "Proprietary AI Transformation Platform" (NOT "Intelligence" — avoids "AI Intelligence" redundancy)
- Use "15+ Years Software Leadership" (includes "Software")

### Case Studies (all defensible — real engagements)
1. Vox Telecom — churn prediction 18%→81%, NLP sentiment, Afrikaans auto-detect. Do NOT mention "Dual XGBoost" or specific model architecture.
2. Stellenbosch University — Hydroponics IoT platform, 25+ sensors, 500K data points
3. KNDS Belgium — Ballistic simulation, PINNs, 50× efficiency
4. Celebr8tly — Generative AI image integration for celebration platform

### Things NOT to mention on the website
- BelOrta (engagement is premature — Tim could see it)
- Specific model architectures (XGBoost, etc.) — keep methodology vague
- Specific pricing (say "tailored to scope")
- Vox/SOLIDitech internal details

## External Links
- Schedule call: https://calendar.app.google/gARU96EmoueEsaTv6
- AI Readiness: /ai-readiness-assessment.html
- EU AI Compliance: /eu-ai-compliance.html
- Email: solutions@koios-analytics.com
- LinkedIn: https://www.linkedin.com/company/koios-analytics/
- Digital twin: https://myprofilegpt.onrender.com/?bot=true
- Chatbot backend: https://ask-koios-ai.onrender.com

## Assets Directory Policy
Only keep files that are actually referenced by the HTML pages:
- logo-white.png — referenced by ai-readiness-assessment.html (uses <img> tag)
- logo.svg — available as fallback
- Unused reference files (AI_Alliance_logo_1.png, logo-ec--en.svg, MIT-sloan_logo.svg, MIT-logo.png) have been removed

## Pending Tasks
(none currently)
