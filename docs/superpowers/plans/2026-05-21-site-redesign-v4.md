# kr-lynx Site Redesign v4 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rewrite kr-lynx personal site from dark 3-column layout to a light, liana-evans.co-style SPA with hero, pill-nav, and smooth-scroll About/Projects/Contact sections using the Riegla font.

**Architecture:** Single `index.html` file with inline CSS and vanilla JS — no build tools. Fonts served from `fonts/riegla-font-family/` in the repo root. Three anchor sections (`#about`, `#projects`, `#contact`) with IntersectionObserver-driven nav highlighting.

**Tech Stack:** HTML5, CSS3 (custom properties, grid, @font-face, @keyframes), Vanilla JS (IntersectionObserver, smooth scroll), GitHub Pages hosting.

---

### Task 1: Copy Riegla font files into the site repo

**Files:**
- Create: `~/kr-lynx-site/fonts/riegla-font-family/` (5 .otf files)

- [ ] **Step 1: Copy font files**

```bash
mkdir -p ~/kr-lynx-site/fonts/riegla-font-family
cp "/Users/romashov/Library/Mobile Documents/iCloud~md~obsidian/Documents/My projects/Personal Site/riegla-font-family/TRIAL_Riegla-Light-BF64b8ac04edb75.otf" ~/kr-lynx-site/fonts/riegla-font-family/
cp "/Users/romashov/Library/Mobile Documents/iCloud~md~obsidian/Documents/My projects/Personal Site/riegla-font-family/TRIAL_Riegla-Regular-BF64b8ac050728f.otf" ~/kr-lynx-site/fonts/riegla-font-family/
cp "/Users/romashov/Library/Mobile Documents/iCloud~md~obsidian/Documents/My projects/Personal Site/riegla-font-family/TRIAL_Riegla-Medium-BF64b8ac04eb181.otf" ~/kr-lynx-site/fonts/riegla-font-family/
cp "/Users/romashov/Library/Mobile Documents/iCloud~md~obsidian/Documents/My projects/Personal Site/riegla-font-family/TRIAL_Riegla-Bold-BF64b8ac04e40f7.otf" ~/kr-lynx-site/fonts/riegla-font-family/
cp "/Users/romashov/Library/Mobile Documents/iCloud~md~obsidian/Documents/My projects/Personal Site/riegla-font-family/TRIAL_Riegla-Black-BF64b8ac04dbbb8.otf" ~/kr-lynx-site/fonts/riegla-font-family/
```

- [ ] **Step 2: Verify 5 files copied**

```bash
ls ~/kr-lynx-site/fonts/riegla-font-family/
```

Expected output: 5 `.otf` files (Light, Regular, Medium, Bold, Black).

- [ ] **Step 3: Commit fonts**

```bash
cd ~/kr-lynx-site
git add fonts/
git commit -m "feat: add Riegla font family (5 weights)"
```

---

### Task 2: Rewrite index.html — complete new site

**Files:**
- Modify: `~/kr-lynx-site/index.html` (full rewrite)

- [ ] **Step 1: Replace index.html with the complete new site**

Write the following content to `~/kr-lynx-site/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kirill Romashov — AI Growth Manager</title>
  <style>
    /* === RESET === */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    /* === FONTS === */
    @font-face {
      font-family: 'Riegla';
      src: url('fonts/riegla-font-family/TRIAL_Riegla-Light-BF64b8ac04edb75.otf') format('opentype');
      font-weight: 300; font-display: swap;
    }
    @font-face {
      font-family: 'Riegla';
      src: url('fonts/riegla-font-family/TRIAL_Riegla-Regular-BF64b8ac050728f.otf') format('opentype');
      font-weight: 400; font-display: swap;
    }
    @font-face {
      font-family: 'Riegla';
      src: url('fonts/riegla-font-family/TRIAL_Riegla-Medium-BF64b8ac04eb181.otf') format('opentype');
      font-weight: 500; font-display: swap;
    }
    @font-face {
      font-family: 'Riegla';
      src: url('fonts/riegla-font-family/TRIAL_Riegla-Bold-BF64b8ac04e40f7.otf') format('opentype');
      font-weight: 700; font-display: swap;
    }
    @font-face {
      font-family: 'Riegla';
      src: url('fonts/riegla-font-family/TRIAL_Riegla-Black-BF64b8ac04dbbb8.otf') format('opentype');
      font-weight: 900; font-display: swap;
    }

    /* === VARS === */
    :root {
      --bg: #f2f2f0;
      --fg: #111111;
      --fg-muted: #666666;
      --accent: #5aaad7;
      --border: #e0e0dc;
      --card: #ffffff;
      --font: 'Riegla', sans-serif;
      --nav-h: 72px;
    }

    /* === BASE === */
    html { scroll-behavior: smooth; }
    body {
      background: var(--bg);
      color: var(--fg);
      font-family: var(--font);
      font-size: 16px;
      line-height: 1.6;
      -webkit-font-smoothing: antialiased;
    }
    a { color: inherit; text-decoration: none; }

    /* === NAVBAR === */
    #navbar {
      position: fixed;
      top: 0; left: 0; right: 0;
      height: var(--nav-h);
      z-index: 100;
      background: rgba(242, 242, 240, 0.85);
      backdrop-filter: blur(12px);
      border-bottom: 1px solid transparent;
      transition: border-color 0.3s;
    }
    #navbar.scrolled { border-bottom-color: var(--border); }
    .nav-inner {
      max-width: 1200px;
      margin: 0 auto;
      padding: 0 40px;
      height: 100%;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
    .nav-logo { font-weight: 700; font-size: 20px; letter-spacing: -0.02em; }
    .logo-star { color: var(--accent); }
    .nav-pill {
      display: flex;
      background: rgba(255, 255, 255, 0.9);
      border: 1px solid var(--border);
      border-radius: 100px;
      padding: 5px;
      gap: 2px;
      box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
    }
    .nav-link {
      font-weight: 500;
      font-size: 15px;
      color: var(--fg-muted);
      padding: 8px 22px;
      border-radius: 100px;
      transition: all 0.2s;
    }
    .nav-link:hover { color: var(--fg); }
    .nav-link.active { background: var(--fg); color: #fff; }

    /* === HERO === */
    #about { padding-top: var(--nav-h); }
    .hero-viewport {
      min-height: calc(100vh - var(--nav-h));
      display: flex;
      align-items: center;
      padding: 0 40px;
      max-width: 1200px;
      margin: 0 auto;
      position: relative;
    }
    .hero-content { max-width: 900px; }
    .hero-floats { display: flex; gap: 16px; margin-bottom: 24px; }
    .float-icon {
      font-size: 36px;
      display: inline-block;
      animation: floatAnim 3s ease-in-out infinite;
      animation-delay: var(--delay, 0s);
    }
    @keyframes floatAnim {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-10px); }
    }
    .hero-title {
      font-weight: 900;
      font-size: clamp(52px, 7vw, 88px);
      line-height: 1.05;
      letter-spacing: -0.03em;
      margin-bottom: 24px;
    }
    .accent-star { color: var(--accent); }
    .hero-sub {
      font-weight: 700;
      font-size: clamp(28px, 4vw, 44px);
      line-height: 1.2;
      letter-spacing: -0.02em;
      margin-bottom: 32px;
    }
    .hero-location { font-weight: 400; font-size: 15px; color: var(--fg-muted); }
    .hero-arrow {
      position: absolute;
      bottom: 40px;
      right: 40px;
      font-size: 22px;
      color: var(--accent);
      font-weight: 700;
      width: 52px;
      height: 52px;
      border: 2px solid var(--accent);
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: all 0.2s;
    }
    .hero-arrow:hover { background: var(--accent); color: #fff; }

    /* === ABOUT DETAIL === */
    .about-detail {
      max-width: 1200px;
      margin: 0 auto;
      padding: 80px 40px;
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 80px;
      border-top: 1px solid var(--border);
    }
    .section-label {
      font-weight: 700;
      font-size: 11px;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 20px;
    }
    .about-text { font-weight: 400; font-size: 16px; line-height: 1.75; color: #333; }
    .tag-cloud { display: flex; flex-wrap: wrap; gap: 10px; }
    .tag-pill {
      font-weight: 500;
      font-size: 13px;
      color: #333;
      background: var(--card);
      border: 1px solid var(--border);
      padding: 7px 16px;
      border-radius: 100px;
      transition: border-color 0.2s;
    }
    .tag-pill:hover { border-color: var(--accent); }

    /* === PROJECTS === */
    #projects {
      max-width: 1200px;
      margin: 0 auto;
      padding: 80px 40px;
      border-top: 1px solid var(--border);
    }
    #projects h2 {
      font-weight: 900;
      font-size: 48px;
      letter-spacing: -0.03em;
      margin-bottom: 48px;
    }
    .projects-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 20px;
    }
    .project-card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 32px;
      cursor: pointer;
      transition: box-shadow 0.2s, border-left-color 0.2s;
      border-left: 3px solid transparent;
    }
    .project-card:hover {
      box-shadow: 0 8px 32px rgba(0, 0, 0, 0.08);
      border-left-color: var(--accent);
    }
    .project-cat {
      font-weight: 700;
      font-size: 11px;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 12px;
    }
    .project-card h3 {
      font-weight: 700;
      font-size: 20px;
      letter-spacing: -0.02em;
      margin-bottom: 12px;
    }
    .project-card p { font-weight: 400; font-size: 15px; color: #555; line-height: 1.7; margin-bottom: 20px; }
    .project-tags { display: flex; flex-wrap: wrap; gap: 8px; }
    .project-tag {
      font-size: 12px;
      font-weight: 500;
      color: #555;
      background: var(--bg);
      border: 1px solid var(--border);
      padding: 4px 12px;
      border-radius: 100px;
    }

    /* === CONTACT === */
    #contact {
      max-width: 1200px;
      margin: 0 auto;
      padding: 80px 40px;
      border-top: 1px solid var(--border);
    }
    #contact h2 {
      font-weight: 900;
      font-size: 48px;
      letter-spacing: -0.03em;
      margin-bottom: 12px;
    }
    .contact-sub { font-weight: 400; font-size: 18px; color: var(--fg-muted); margin-bottom: 48px; }
    .contact-rows { display: flex; flex-direction: column; max-width: 600px; }
    .contact-row {
      display: flex;
      align-items: center;
      gap: 16px;
      padding: 20px 0;
      border-bottom: 1px solid var(--border);
      color: var(--fg);
      transition: color 0.2s;
    }
    .contact-row:first-child { border-top: 1px solid var(--border); }
    .contact-row:hover { color: var(--accent); }
    .contact-icon { font-size: 20px; width: 28px; }
    .contact-label { font-size: 13px; color: var(--fg-muted); width: 80px; }
    .contact-value { font-weight: 500; font-size: 16px; }

    /* === FOOTER === */
    footer {
      max-width: 1200px;
      margin: 0 auto;
      padding: 32px 40px;
      border-top: 1px solid var(--border);
      text-align: center;
    }
    footer p { font-weight: 300; font-size: 13px; color: #aaa; }

    /* === PROJECT MODAL === */
    .modal-overlay {
      display: none;
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.4);
      z-index: 200;
      backdrop-filter: blur(4px);
      align-items: center;
      justify-content: center;
      padding: 24px;
    }
    .modal-overlay.open { display: flex; }
    .modal {
      background: #fff;
      border: 1px solid var(--border);
      border-radius: 16px;
      max-width: 560px;
      width: 100%;
      max-height: 82vh;
      overflow-y: auto;
      padding: 40px;
      position: relative;
    }
    .modal-close {
      position: absolute;
      top: 16px; right: 16px;
      font-size: 18px;
      color: var(--fg-muted);
      cursor: pointer;
      background: none;
      border: none;
      font-family: var(--font);
      width: 36px; height: 36px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: background 0.2s;
    }
    .modal-close:hover { background: var(--bg); }
    .modal-cat {
      font-size: 11px; font-weight: 700;
      letter-spacing: 0.1em; text-transform: uppercase;
      color: var(--accent); margin-bottom: 12px;
    }
    .modal h2 {
      font-size: 26px; font-weight: 700;
      letter-spacing: -0.02em;
      margin-bottom: 16px;
    }
    .modal-desc { font-size: 15px; color: #555; line-height: 1.8; margin-bottom: 24px; }
    .modal-tags { display: flex; flex-wrap: wrap; gap: 8px; }

    /* === SCROLL REVEAL === */
    .reveal {
      opacity: 0;
      transform: translateY(24px);
      transition: opacity 0.6s ease, transform 0.6s ease;
    }
    .reveal.visible { opacity: 1; transform: translateY(0); }

    /* === RESPONSIVE === */
    @media (max-width: 1024px) {
      .about-detail { grid-template-columns: 1fr; gap: 48px; }
      .projects-grid { grid-template-columns: 1fr; }
    }
    @media (max-width: 768px) {
      .nav-inner { padding: 0 20px; }
      .nav-link { padding: 8px 14px; font-size: 14px; }
      .hero-viewport, .about-detail, #projects, #contact, footer { padding-left: 20px; padding-right: 20px; }
    }
    @media (max-width: 480px) {
      .nav-link { padding: 7px 12px; font-size: 13px; }
    }
    @media (prefers-reduced-motion: reduce) {
      .reveal { opacity: 1; transform: none; transition: none; }
      .float-icon { animation: none; }
    }
  </style>
</head>
<body>

<!-- NAVBAR -->
<nav id="navbar">
  <div class="nav-inner">
    <a href="#about" class="nav-logo">lynx <span class="logo-star">✦</span></a>
    <div class="nav-pill">
      <a href="#about" class="nav-link active" data-section="about">About</a>
      <a href="#projects" class="nav-link" data-section="projects">Projects</a>
      <a href="#contact" class="nav-link" data-section="contact">Contact</a>
    </div>
  </div>
</nav>

<!-- ABOUT -->
<section id="about">
  <div class="hero-viewport">
    <div class="hero-content">
      <div class="hero-floats">
        <span class="float-icon" style="--delay:0s">🤖</span>
        <span class="float-icon" style="--delay:0.5s">⚡</span>
        <span class="float-icon" style="--delay:1s">📈</span>
      </div>
      <h1 class="hero-title">Hey, I'm Kirill <span class="accent-star">✦</span></h1>
      <p class="hero-sub">I'm an AI Growth Manager.<br>Helping businesses automate<br>processes and grow with AI.</p>
      <p class="hero-location">📍 Based in Belgrade &middot; Moving to Uruguay &middot; Open for work worldwide</p>
    </div>
    <a href="#about-detail" class="hero-arrow" aria-label="Scroll down">↓</a>
  </div>
  <div class="about-detail reveal" id="about-detail">
    <div>
      <p class="section-label">About me</p>
      <p class="about-text">AI Growth Product Manager with 5+ years in iGaming, Crypto, and FinTech. Shipped 0→1 AI products: VIP detection, personalized bonus engines, content pipelines — +65% revenue, +130% MAU, ×12 content output. GTM strategy across 40+ geos, B2B &amp; B2C.</p>
    </div>
    <div>
      <p class="section-label">Stack</p>
      <div class="tag-cloud">
        <span class="tag-pill">n8n</span>
        <span class="tag-pill">Claude API</span>
        <span class="tag-pill">LLM / RAG</span>
        <span class="tag-pill">HubSpot</span>
        <span class="tag-pill">Airtable</span>
        <span class="tag-pill">Python</span>
        <span class="tag-pill">JavaScript</span>
        <span class="tag-pill">Telegram Bots</span>
      </div>
    </div>
  </div>
</section>

<!-- PROJECTS -->
<section id="projects">
  <h2 class="reveal">Projects</h2>
  <div class="projects-grid">
    <div class="project-card reveal" data-id="0">
      <p class="project-cat">Automation</p>
      <h3>Haos General Inbox</h3>
      <p>Telegram bot that routes messages, logs conversations to HubSpot CRM and triggers n8n workflows automatically.</p>
      <div class="project-tags">
        <span class="project-tag">Python</span><span class="project-tag">Telethon</span>
        <span class="project-tag">n8n</span><span class="project-tag">HubSpot</span>
      </div>
    </div>
    <div class="project-card reveal" data-id="1">
      <p class="project-cat">AI &middot; Automation</p>
      <h3>Content Factory</h3>
      <p>7-workflow n8n system that parses RSS, writes articles, generates images and publishes to Telegram — fully automated.</p>
      <div class="project-tags">
        <span class="project-tag">n8n</span><span class="project-tag">Claude API</span>
        <span class="project-tag">RSS</span><span class="project-tag">Telegram</span>
      </div>
    </div>
    <div class="project-card reveal" data-id="2">
      <p class="project-cat">Web dev</p>
      <h3>kr-lynx portfolio</h3>
      <p>Personal portfolio site. No frameworks — pure HTML, CSS and vanilla JS. Riegla font, light editorial aesthetic, GitHub Pages.</p>
      <div class="project-tags">
        <span class="project-tag">HTML</span><span class="project-tag">CSS</span>
        <span class="project-tag">JS</span><span class="project-tag">GitHub Pages</span>
      </div>
    </div>
    <div class="project-card reveal" data-id="3">
      <p class="project-cat">Automation &middot; Finance</p>
      <h3>Finance Automatization</h3>
      <p>Personal finance tracking with automated data collection, categorization and Telegram reporting via n8n.</p>
      <div class="project-tags">
        <span class="project-tag">n8n</span><span class="project-tag">Google Sheets</span>
        <span class="project-tag">Telegram</span>
      </div>
    </div>
  </div>
</section>

<!-- CONTACT -->
<section id="contact">
  <h2 class="reveal">Let's talk</h2>
  <p class="contact-sub reveal">Open for freelance and full-time. Reply within 24h.</p>
  <div class="contact-rows reveal">
    <a class="contact-row" href="mailto:kirillromashov202@gmail.com">
      <span class="contact-icon">📧</span>
      <span class="contact-label">Email</span>
      <span class="contact-value">kirillromashov202@gmail.com</span>
    </a>
    <a class="contact-row" href="https://t.me/GoldLynx" target="_blank" rel="noopener">
      <span class="contact-icon">✈️</span>
      <span class="contact-label">Telegram</span>
      <span class="contact-value">@GoldLynx</span>
    </a>
    <a class="contact-row" href="https://github.com/kr-lynx" target="_blank" rel="noopener">
      <span class="contact-icon">🐙</span>
      <span class="contact-label">GitHub</span>
      <span class="contact-value">kr-lynx</span>
    </a>
    <a class="contact-row" href="https://docs.google.com/document/d/1GZOwFYLEXzHaOD_r4zs4UfWUPAjb1phEiOvyAv7grgQ/export?format=pdf" target="_blank" rel="noopener">
      <span class="contact-icon">📄</span>
      <span class="contact-label">CV</span>
      <span class="contact-value">Download PDF →</span>
    </a>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <p>&copy; 2026 kr-lynx</p>
</footer>

<!-- PROJECT MODAL -->
<div class="modal-overlay" id="modal" role="dialog" aria-modal="true">
  <div class="modal">
    <button class="modal-close" id="modalClose" aria-label="Close">&#x2715;</button>
    <p class="modal-cat" id="modalCat"></p>
    <h2 id="modalTitle"></h2>
    <p class="modal-desc" id="modalDesc"></p>
    <div class="modal-tags" id="modalTags"></div>
  </div>
</div>

<script>
const projects = [
  { cat:'Automation', title:'Haos General Inbox',
    desc:'A Telegram bot system that listens to incoming messages, routes them by category, logs full conversation history to HubSpot CRM as Notes, and triggers n8n workflows for automated responses. Built with Telethon, running as a persistent systemd service.',
    stack:['Python','Telethon','n8n','HubSpot API','Linux'] },
  { cat:'AI · Automation', title:'Content Factory',
    desc:'7-workflow n8n pipeline for a tech-science Telegram channel. Parses RSS feeds, selects stories, generates long-read articles via Claude API, creates matching images, and publishes on schedule — fully autonomous.',
    stack:['n8n','Claude API','RSS','Telegram Bot API','Image gen'] },
  { cat:'Web dev', title:'kr-lynx portfolio',
    desc:'Personal portfolio site. No frameworks, no templates — pure HTML, CSS and vanilla JS. Light editorial aesthetic, Riegla font, hosted on GitHub Pages.',
    stack:['HTML5','CSS3','Vanilla JS','GitHub Pages'] },
  { cat:'Automation · Finance', title:'Finance Automatization',
    desc:'Personal finance system that collects spending data from multiple sources, categorizes transactions automatically, and delivers structured weekly reports via Telegram. Implemented with n8n workflows.',
    stack:['n8n','Google Sheets','Telegram','Excalidraw'] }
];

// Scroll reveal
const revealIO = new IntersectionObserver(entries => {
  entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('visible'); });
}, { threshold: 0.08 });
document.querySelectorAll('.reveal').forEach(el => revealIO.observe(el));

// Navbar border on scroll
window.addEventListener('scroll', () => {
  document.getElementById('navbar').classList.toggle('scrolled', window.scrollY > 20);
}, { passive: true });

// Nav active section tracking
const sectionIO = new IntersectionObserver(entries => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      document.querySelectorAll('.nav-link').forEach(l => l.classList.remove('active'));
      const link = document.querySelector(`.nav-link[data-section="${e.target.id}"]`);
      if (link) link.classList.add('active');
    }
  });
}, { threshold: 0.3, rootMargin: '-60px 0px -40% 0px' });
document.querySelectorAll('section[id]').forEach(s => sectionIO.observe(s));

// Project modal
const modal = document.getElementById('modal');
function openModal(id) {
  const p = projects[id];
  document.getElementById('modalCat').textContent = p.cat;
  document.getElementById('modalTitle').textContent = p.title;
  document.getElementById('modalDesc').textContent = p.desc;
  document.getElementById('modalTags').innerHTML = p.stack.map(s =>
    `<span class="project-tag">${s}</span>`).join('');
  modal.classList.add('open');
  document.body.style.overflow = 'hidden';
}
function closeModal() {
  modal.classList.remove('open');
  document.body.style.overflow = '';
}
document.getElementById('modalClose').addEventListener('click', closeModal);
modal.addEventListener('click', e => { if (e.target === modal) closeModal(); });
document.querySelectorAll('[data-id]').forEach(el => {
  el.addEventListener('click', () => openModal(+el.dataset.id));
});
document.addEventListener('keydown', e => { if (e.key === 'Escape') closeModal(); });
</script>
</body>
</html>
```

- [ ] **Step 2: Open the site locally and verify all sections**

```bash
open ~/kr-lynx-site/index.html
```

Check in browser:
1. Font loads (Riegla renders in hero heading — rounded, not monospace)
2. Navbar: `lynx ✦` logo left, pill `About · Projects · Contact` right
3. Hero: floating 🤖 ⚡ 📈 animate; big "Hey, I'm Kirill ✦"; subtext and location visible; `↓` arrow bottom-right
4. Clicking `↓` arrow scrolls to About detail (bio + stack tags)
5. Projects section: 2×2 card grid, hover lifts card with blue left border
6. Clicking a project card opens modal with title, description, stack tags; Esc closes it
7. Contact section: 4 rows (Email, Telegram, GitHub, CV), hover turns accent blue
8. Footer: `© 2026 kr-lynx`
9. Scroll reveal: sections fade in as you scroll down
10. Nav active state updates as you scroll through sections

- [ ] **Step 3: Commit**

```bash
cd ~/kr-lynx-site
git add index.html
git commit -m "feat: redesign site v4 — light theme, Riegla font, hero + SPA layout"
```

---

### Task 3: Deploy to GitHub Pages

**Files:** None (push existing changes)

- [ ] **Step 1: Push to origin**

```bash
cd ~/kr-lynx-site
git push origin main
```

- [ ] **Step 2: Wait ~60 seconds, then open live site**

```bash
open https://kr-lynx.github.io/main/
```

Verify the site looks identical to local. If fonts don't load on live, check network tab — font paths must match exactly.

- [ ] **Step 3: Check font paths if needed**

If fonts fail on GitHub Pages, the issue is the path prefix. The repo is served from `/main/` so font paths need to be relative (starting with `fonts/`, not `/fonts/`). The `url('fonts/...')` in the plan already uses relative paths, so this should work.

---
