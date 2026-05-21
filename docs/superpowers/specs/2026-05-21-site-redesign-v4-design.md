# kr-lynx Site Redesign v4 — Design Spec

**Date:** 2026-05-21  
**Reference:** liana-evans.co style (v3.jpg)  
**Approach:** Single-page app with smooth scroll to anchor sections

---

## Visual System

| Parameter | Value |
|---|---|
| Background | `#f2f2f0` |
| Primary text | `#111111` |
| Muted text | `#666666` |
| Accent | `#5aaad7` |
| Font (all) | Riegla (TRIAL) — Black, Bold, Medium, Regular, Light |
| Max width | 1200px, centered |
| Border radius (cards) | 12px |
| Border color | `#e0e0dc` |

Font loaded via `@font-face` from `/fonts/riegla-font-family/`.

---

## Navbar

- **Fixed (sticky)** at top, full width, background `#f2f2f0` with subtle bottom border
- **Left:** logo `lynx ✦` — Riegla Bold, `✦` in `#5aaad7`
- **Right:** pill container with links: `About` · `Projects` · `Contact`
  - Pill: rounded border, light shadow
  - Active item: white background fill inside the pill
- Clicking a nav item smooth-scrolls to `#about`, `#projects`, or `#contact`
- Active section tracked via IntersectionObserver → updates active nav item

---

## Section 1 — Hero (`#about`, above the fold)

**Layout:** full viewport height, content vertically centered-left

**Floating decorative elements** (above the heading):
- Three emoji icons: `🤖` `⚡` `📈`
- CSS keyframe animation: gentle float/bobbing, each with different delay
- Positioned absolutely, scattered above the `Hey, I'm Kirill` line

**Heading:** `Hey, I'm Kirill ✦`
- Riegla Black, ~88px desktop / 52px mobile
- `✦` in `#5aaad7`

**Subheading:**
```
I'm an AI Growth Manager.
Helping businesses automate processes
and grow with AI.
```
- Riegla Bold, ~44px desktop / 28px mobile
- Color `#111111`

**Location line:**
```
📍  Based in Belgrade · Moving to Uruguay · Open for work worldwide
```
- Riegla Regular, 15px, color `#666666`
- Margin-top 32px from subheading

**Down arrow:**
- `↓` or SVG chevron, `#5aaad7`, positioned bottom-right of viewport
- Click smooth-scrolls to about-content below

---

## Section 1 — About content (below the fold, same `#about` anchor)

Two-column layout (desktop), stacked (mobile ≤768px):

**Left column — "About me"**
- Label: `ABOUT` — Riegla Bold, 12px, uppercase, `#5aaad7`, letter-spacing
- Text: Riegla Regular, 16px, `#333`
- Content:
  > AI Growth Product Manager with 5+ years in iGaming, Crypto, and FinTech. Shipped 0→1 AI products: VIP detection, personalized bonus engines, content pipelines — +65% revenue, +130% MAU, ×12 content output. GTM strategy across 40+ geos, B2B & B2C.

**Right column — "Stack"**
- Label: `STACK` — same style as ABOUT label
- Tag pills: `n8n` `Claude API` `LLM / RAG` `HubSpot` `Airtable` `Python` `JavaScript` `Telegram Bots`
- Pill style: `#ffffff` bg, `#e0e0dc` border, Riegla Medium 13px, `#333` text
- Hover: border becomes `#5aaad7`

---

## Section 2 — Projects (`#projects`)

**Section header:** `Projects` — Riegla Black, 48px

**4 cards in a 2×2 grid** (desktop), 1 column (mobile):

| # | Title | Category |
|---|---|---|
| 0 | Haos General Inbox | Automation |
| 1 | Content Factory | AI · Vibe coding |
| 2 | kr-lynx portfolio | Web dev |
| 3 | Finance Automatization | Automation · Finance |

**Card anatomy:**
- Background `#ffffff`, border `#e0e0dc`, border-radius 12px
- Category: Riegla Medium 12px uppercase, `#5aaad7`
- Title: Riegla Bold 20px, `#111`
- Description: Riegla Regular 15px, `#555`
- Stack tags: same pill style as above
- Hover: card lifts (box-shadow), left border `3px solid #5aaad7`
- Click → modal overlay with full project detail

---

## Section 3 — Contact (`#contact`)

**Section header:** `Let's talk` — Riegla Black, 48px

**Subtext:** `Open for freelance and full-time. Reply within 24h.` — Riegla Regular 18px, `#666`

**Contact rows** (icon + label + value/link):
- 📧 Email — kirillromashov202@gmail.com
- ✈️ Telegram — t.me/GoldLynx
- 🐙 GitHub — github.com/kr-lynx
- 📄 CV — Download PDF

Each row: Riegla Medium 16px, link underline on hover in `#5aaad7`.

---

## Footer

`© 2026 kr-lynx` — Riegla Light, 13px, centered, `#aaa`

---

## Responsive Breakpoints

| Breakpoint | Layout change |
|---|---|
| ≤1024px | About columns stack; project grid 2→1 |
| ≤768px | Hero font ~52px; single column everything; navbar links shrink |
| ≤480px | Hero font ~40px |

---

## Animations

- **Floating emoji:** CSS `@keyframes float`, translateY ±8px, 3s infinite, each icon offset by 0.5s / 1s
- **Scroll reveal:** IntersectionObserver, `.reveal` → `.visible` (opacity 0→1, translateY 20px→0)
- **Nav active tracking:** IntersectionObserver on each section, updates nav pill highlight
- **Card hover:** `transition: transform 0.2s, box-shadow 0.2s`

---

## Font Setup

Files copied to `/fonts/riegla-font-family/` in repo root.

```css
@font-face {
  font-family: 'Riegla';
  src: url('/fonts/riegla-font-family/TRIAL_Riegla-Regular-BF64b8ac050728f.otf') format('opentype');
  font-weight: 400;
}
/* + Bold (700), Black (900), Medium (500), Light (300) */
```

All text elements use `font-family: 'Riegla', sans-serif`.

---

## Files Changed

- `index.html` — full rewrite
- `fonts/riegla-font-family/` — copied from Personal Site folder
- No new dependencies, no build tools
