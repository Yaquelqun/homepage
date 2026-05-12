# Portfolio Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rework sandjiv.com from a generic dark template into a distinctive Catppuccin Mocha–themed portfolio with mikado kumiko background pattern, glass-card layout, and new content sections.

**Architecture:** Pure static HTML/CSS/JS — no frameworks or build tools. Single shared `style.css` rewritten from scratch with Catppuccin Mocha variables and kumiko SVG background. Two HTML files (`index.html`, `setup.html`) updated to use Inter font, glass-card section layout, and new content structure. Vanilla JS for tag filtering.

**Tech Stack:** HTML, CSS (custom properties, grid, flexbox, backdrop-filter), vanilla JavaScript, Google Fonts (Inter)

---

### Task 1: Rewrite style.css — Full CSS

Replace the entire `style.css` with the new Catppuccin Mocha theme, kumiko background, glass cards, and all section styles.

**Files:**
- Rewrite: `style.css`
- Modify: `.gitignore` (add `.superpowers/`)

- [ ] **Step 1: Add `.superpowers/` to .gitignore**

Append to `.gitignore`:

```
.superpowers/
```

- [ ] **Step 2: Write the complete new style.css**

Replace the entire contents of `style.css` with:

```css
/* Reset */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* Catppuccin Mocha */
:root {
  --ctp-base: #1e1e2e;
  --ctp-mantle: #181825;
  --ctp-crust: #11111b;
  --ctp-surface0: #313244;
  --ctp-surface1: #45475a;
  --ctp-surface2: #585b70;
  --ctp-overlay0: #6c7086;
  --ctp-overlay1: #7f849c;
  --ctp-overlay2: #9399b2;
  --ctp-text: #cdd6f4;
  --ctp-subtext1: #bac2de;
  --ctp-subtext0: #a6adc8;
  --ctp-lavender: #b4befe;
  --ctp-blue: #89b4fa;
  --ctp-sapphire: #74c7ec;
  --ctp-sky: #89dceb;
  --ctp-teal: #94e2d5;
  --ctp-green: #a6e3a1;
  --ctp-yellow: #f9e2af;
  --ctp-peach: #fab387;
  --ctp-maroon: #eba0ac;
  --ctp-red: #f38ba8;
  --ctp-mauve: #cba6f7;
  --ctp-pink: #f5c2e7;
  --ctp-flamingo: #f2cdcd;
  --ctp-rosewater: #f5e0dc;
}

/* Body — kumiko mikado background */
html {
  font-size: 16px;
  scroll-behavior: smooth;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  line-height: 1.6;
  color: var(--ctp-text);
  background-color: var(--ctp-base);
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='60' height='34.64' viewBox='0 0 60 34.64'%3E%3Cg fill='none' stroke='%2345475a' stroke-width='0.5' opacity='0.35'%3E%3Cline x1='10' y1='17.32' x2='20' y2='0'/%3E%3Cline x1='20' y1='0' x2='40' y2='0'/%3E%3Cline x1='40' y1='0' x2='50' y2='17.32'/%3E%3Cline x1='50' y1='17.32' x2='40' y2='34.64'/%3E%3Cline x1='40' y1='34.64' x2='20' y2='34.64'/%3E%3Cline x1='20' y1='34.64' x2='10' y2='17.32'/%3E%3Cline x1='0' y1='17.32' x2='10' y2='17.32'/%3E%3Cline x1='50' y1='17.32' x2='60' y2='17.32'/%3E%3Cline x1='30' y1='17.32' x2='50' y2='17.32'/%3E%3Cline x1='30' y1='17.32' x2='20' y2='34.64'/%3E%3Cline x1='30' y1='17.32' x2='20' y2='0'/%3E%3Cline x1='0' y1='0' x2='20' y2='0'/%3E%3Cline x1='60' y1='0' x2='50' y2='17.32'/%3E%3Cline x1='60' y1='34.64' x2='50' y2='17.32'/%3E%3Cline x1='0' y1='34.64' x2='20' y2='34.64'/%3E%3C/g%3E%3C/svg%3E");
  background-attachment: fixed;
  overflow-x: hidden;
}

/* Container */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

/* Navbar */
.navbar {
  position: sticky;
  top: 0;
  z-index: 1000;
  background: rgba(24, 24, 37, 0.95);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--ctp-surface0);
  padding: 1rem 0;
}

.navbar .container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.nav-brand {
  font-size: 1.5rem;
  font-weight: 700;
  color: var(--ctp-lavender);
  text-decoration: none;
  transition: filter 0.3s ease;
}

.nav-brand:hover {
  filter: brightness(1.2);
}

.nav-links {
  display: flex;
  gap: 2rem;
}

.nav-link {
  color: var(--ctp-subtext1);
  text-decoration: none;
  font-weight: 500;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  transition: all 0.3s ease;
}

.nav-link:hover {
  color: var(--ctp-lavender);
  background-color: var(--ctp-surface0);
}

/* Glass card */
.glass-card {
  margin: 0 2rem;
  padding: 2.25rem 2.5rem;
  background: rgba(24, 24, 37, 0.85);
  backdrop-filter: blur(8px);
  border: 1px solid var(--ctp-surface0);
  border-radius: 12px;
}

/* Section spacing */
.section-gap {
  padding-top: 0.75rem;
  padding-bottom: 3rem;
}

/* Section heading */
.section-heading {
  font-size: clamp(1.5rem, 3vw, 1.75rem);
  font-weight: 600;
  margin-bottom: 1rem;
  color: var(--ctp-text);
}

.section-heading::after {
  content: '';
  display: block;
  width: 40px;
  height: 3px;
  background: linear-gradient(90deg, var(--ctp-mauve), var(--ctp-pink));
  margin-top: 0.5rem;
  border-radius: 2px;
}

/* Hero */
.hero {
  padding: 4rem 2rem 3rem;
  text-align: center;
  position: relative;
}

.hero::before {
  content: '';
  position: absolute;
  top: 20%;
  left: 50%;
  transform: translateX(-50%);
  width: 300px;
  height: 300px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(203, 166, 247, 0.1), transparent 70%);
  filter: blur(40px);
  pointer-events: none;
}

.hero-content {
  position: relative;
  animation: fadeInUp 0.8s ease-out;
}

.hero-title {
  font-size: clamp(2.5rem, 5vw, 3.5rem);
  font-weight: 700;
  margin-bottom: 0.25rem;
  line-height: 1.2;
}

.highlight {
  background: linear-gradient(135deg, var(--ctp-mauve), var(--ctp-pink));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.hero-annotation {
  display: block;
  font-size: 0.875rem;
  font-style: italic;
  color: var(--ctp-overlay0);
  margin-bottom: 1.25rem;
}

.hero-subtitle {
  font-size: clamp(1.25rem, 3vw, 1.5rem);
  color: var(--ctp-subtext1);
  margin-bottom: 0.75rem;
}

.hero-description {
  font-size: 1rem;
  color: var(--ctp-subtext0);
  max-width: 500px;
  margin: 0 auto;
  line-height: 1.8;
}

/* About */
.about {
  display: flex;
  justify-content: center;
}

.about .glass-card {
  max-width: 640px;
  width: 100%;
}

.about-text p {
  font-size: 1rem;
  line-height: 1.8;
  color: var(--ctp-subtext0);
  margin-top: 0.75rem;
}

.about-text a {
  color: var(--ctp-sapphire);
  text-decoration: none;
  border-bottom: 1px solid transparent;
  transition: border-color 0.3s ease;
}

.about-text a:visited {
  color: var(--ctp-sapphire);
}

.about-text a:hover {
  border-bottom-color: var(--ctp-sapphire);
}

/* Experience timeline */
.timeline {
  display: flex;
  position: relative;
  overflow-x: auto;
  padding-bottom: 0.5rem;
  margin-top: 1.5rem;
}

.timeline::before {
  content: '';
  position: absolute;
  top: 36px;
  left: 0;
  right: 0;
  height: 2px;
  background: var(--ctp-surface1);
}

.timeline-item {
  flex: 1;
  min-width: 140px;
  text-align: center;
  position: relative;
  padding: 0 0.75rem;
}

.timeline-dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: var(--ctp-mauve);
  margin: 30px auto 1rem;
  position: relative;
  z-index: 1;
  box-shadow: 0 0 12px rgba(203, 166, 247, 0.3);
}

.timeline-item.current .timeline-dot {
  background: var(--ctp-green);
  box-shadow: 0 0 12px rgba(166, 227, 161, 0.3);
}

.timeline-year {
  font-size: 0.75rem;
  font-weight: 600;
  color: var(--ctp-mauve);
}

.timeline-item.current .timeline-year {
  color: var(--ctp-green);
}

.timeline-role {
  font-size: 0.8125rem;
  font-weight: 500;
  color: var(--ctp-text);
  margin-bottom: 0.125rem;
}

.timeline-company {
  font-size: 0.75rem;
  color: var(--ctp-subtext0);
}

/* Currently Figuring Out — tag filters */
.tag-filters {
  display: flex;
  gap: 0.5rem;
  margin: 1.25rem 0;
  flex-wrap: wrap;
}

.tag-filter {
  padding: 0.3rem 0.875rem;
  border-radius: 99px;
  font-size: 0.75rem;
  font-weight: 500;
  border: 1px solid var(--ctp-surface1);
  color: var(--ctp-subtext1);
  background: transparent;
  cursor: pointer;
  font-family: inherit;
  transition: all 0.3s ease;
}

.tag-filter:hover {
  border-color: var(--ctp-mauve);
  color: var(--ctp-mauve);
}

.tag-filter.active {
  background: rgba(203, 166, 247, 0.15);
  border-color: var(--ctp-mauve);
  color: var(--ctp-mauve);
}

/* Currently Figuring Out — card grid */
.figuring-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
  gap: 1rem;
  margin-top: 1rem;
}

.figuring-card {
  background: rgba(30, 30, 46, 0.7);
  backdrop-filter: blur(4px);
  border: 1px solid var(--ctp-surface0);
  border-radius: 10px;
  padding: 1.25rem;
  transition: all 0.2s ease;
}

.figuring-card:hover {
  border-color: var(--ctp-surface1);
  transform: translateY(-2px);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.3);
}

.figuring-card h4 {
  font-size: 0.875rem;
  font-weight: 600;
  color: var(--ctp-text);
  margin-bottom: 0.375rem;
}

.figuring-card .card-desc {
  font-size: 0.8125rem;
  color: var(--ctp-subtext0);
  margin-bottom: 0.75rem;
}

.progress-bar {
  height: 4px;
  background: var(--ctp-surface0);
  border-radius: 2px;
  overflow: hidden;
  margin-bottom: 0.625rem;
}

.progress-fill {
  height: 100%;
  border-radius: 2px;
}

.card-tags {
  display: flex;
  gap: 0.375rem;
  flex-wrap: wrap;
}

.card-tag {
  font-size: 0.6875rem;
  padding: 0.125rem 0.5rem;
  border-radius: 4px;
  font-weight: 500;
}

/* Contact mini */
.contact {
  padding: 2rem 2rem;
  text-align: center;
}

.contact-quip {
  font-size: 0.875rem;
  color: var(--ctp-subtext0);
  margin-bottom: 1rem;
}

.contact-icons {
  display: flex;
  justify-content: center;
  gap: 1.25rem;
}

.contact-icon {
  width: 40px;
  height: 40px;
  border-radius: 8px;
  background: rgba(49, 50, 68, 0.7);
  backdrop-filter: blur(4px);
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--ctp-subtext1);
  text-decoration: none;
  transition: all 0.3s ease;
}

.contact-icon:hover {
  background: rgba(203, 166, 247, 0.15);
  color: var(--ctp-mauve);
}

.contact-icon svg {
  width: 18px;
  height: 18px;
}

/* Footer */
.footer {
  background: var(--ctp-mantle);
  border-top: 1px solid var(--ctp-surface0);
  padding: 1rem 0;
  text-align: center;
}

.footer p {
  color: var(--ctp-surface2);
  font-size: 0.75rem;
}

/* Setup page */
.setup {
  min-height: 80vh;
}

.setup-content {
  max-width: 900px;
  margin: 0 auto;
}

.setup-content h1 {
  text-align: center;
}

.setup-content h2 {
  font-size: 1.5rem;
  font-weight: 600;
  color: var(--ctp-text);
  margin-top: 2rem;
  margin-bottom: 0.75rem;
  padding-bottom: 0.5rem;
  border-bottom: 2px solid var(--ctp-surface0);
  scroll-margin-top: 80px;
}

.setup-content p {
  font-size: 1rem;
  line-height: 1.8;
  color: var(--ctp-subtext0);
  margin-bottom: 1rem;
}

.setup-content a {
  color: var(--ctp-sapphire);
  text-decoration: none;
  border-bottom: 1px solid transparent;
  transition: border-color 0.3s ease;
}

.setup-content a:visited {
  color: var(--ctp-sapphire);
}

.setup-content a:hover {
  border-bottom-color: var(--ctp-sapphire);
}

.setup-content ul {
  margin: 1rem 0;
  padding-left: 1.5rem;
  color: var(--ctp-subtext0);
}

.setup-content li {
  margin-bottom: 0.5rem;
  line-height: 1.8;
}

/* Setup sidebar */
.setup-sidebar {
  position: fixed;
  left: 2rem;
  top: 50%;
  transform: translateY(-50%);
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  z-index: 100;
  background: rgba(24, 24, 37, 0.85);
  backdrop-filter: blur(10px);
  padding: 1.25rem 0.75rem;
  border-radius: 12px;
  border: 1px solid var(--ctp-surface0);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
}

.sidebar-link {
  color: var(--ctp-subtext0);
  text-decoration: none;
  font-size: 0.8125rem;
  font-weight: 500;
  padding: 0.375rem 0.75rem;
  border-radius: 6px;
  border-left: 3px solid transparent;
  white-space: nowrap;
  transition: all 0.3s ease;
}

.sidebar-link:hover,
.sidebar-link.active {
  color: var(--ctp-mauve);
  background-color: var(--ctp-surface0);
  border-left-color: var(--ctp-mauve);
}

/* Setup container offset for sidebar */
.setup .glass-card {
  margin-left: 240px;
}

/* Code blocks */
pre {
  background: var(--ctp-mantle);
  border: 1px solid var(--ctp-surface0);
  border-radius: 8px;
  padding: 1.25rem;
  margin: 1rem 0;
  overflow-x: auto;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
}

code {
  font-family: 'Fira Code', 'Courier New', monospace;
  font-size: 0.875rem;
  line-height: 1.6;
  color: var(--ctp-text);
}

pre code {
  display: block;
  white-space: pre;
  word-wrap: normal;
}

:not(pre) > code {
  background-color: var(--ctp-surface0);
  padding: 0.125rem 0.375rem;
  border-radius: 4px;
  font-size: 0.8125rem;
  color: var(--ctp-mauve);
}

/* Animations */
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Responsive */
@media (max-width: 1024px) {
  .setup-sidebar {
    left: 1rem;
    padding: 0.75rem;
  }

  .setup .glass-card {
    margin-left: 180px;
  }
}

@media (max-width: 768px) {
  .navbar .container {
    flex-direction: column;
    gap: 0.75rem;
  }

  .nav-links {
    gap: 1rem;
  }

  .hero {
    padding: 3rem 1rem 2rem;
  }

  .glass-card {
    margin: 0 1rem;
    padding: 1.5rem;
  }

  .container {
    padding: 0 1rem;
  }

  .figuring-grid {
    grid-template-columns: 1fr;
  }

  .timeline {
    overflow-x: auto;
    -webkit-overflow-scrolling: touch;
  }

  .timeline-item {
    min-width: 120px;
  }

  .setup-sidebar {
    display: none;
  }

  .setup .glass-card {
    margin-left: 1rem;
  }
}

@media (max-width: 480px) {
  .nav-links {
    flex-direction: column;
    align-items: center;
    gap: 0.5rem;
  }
}
```

- [ ] **Step 3: Verify the CSS file is complete**

Run: `wc -l style.css`
Expected: approximately 430-450 lines.

- [ ] **Step 4: Commit**

```bash
git add style.css .gitignore
git commit -m "rewrite style.css with Catppuccin Mocha theme and kumiko background"
```

---

### Task 2: Rewrite index.html — Head, Navbar, Hero, About

Update the home page structure: add Inter font, rewrite navbar, hero, and about sections.

**Files:**
- Rewrite: `index.html`

- [ ] **Step 1: Write the new index.html through the About section**

Replace the entire contents of `index.html` with the following. Note: experience, figuring-out, contact, and footer sections are added in the next task. This step establishes the document structure.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="can't believe i got that domain name">
  <meta name="author" content="Sandjiv">
  <title>Sandjiv</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <nav class="navbar">
    <div class="container">
      <a href="/" class="nav-brand">Sandjiv</a>
      <div class="nav-links">
        <a href="/" class="nav-link">Home</a>
        <a href="/setup.html" class="nav-link">Setup</a>
      </div>
    </div>
  </nav>

  <main>
    <section class="hero">
      <div class="hero-content">
        <h1 class="hero-title">Hi, I'm <span class="highlight">Sandjiv</span></h1>
        <small class="hero-annotation">(an apparently rare name given that i got that domain name)</small>
        <p class="hero-subtitle">I'm a Backend dev</p>
        <p class="hero-description">
          The best backend dev this side of wherever we are /s
        </p>
      </div>
    </section>

    <div class="section-gap about">
      <div class="glass-card">
        <h2 class="section-heading">About Me</h2>
        <div class="about-text">
          <p>
            Ruby on rails backend dev with an interest in learning new things and
            before deploying <a href="https://boringtechnology.club/">boring tech</a> in production
          </p>
          <p>
            According to claude code, presentation pages are supposed to have 2 paragraphs here, but i'm pretty bad at talking about
            myself so one is enough, but then again who am i to go against our AI Overlords
          </p>
        </div>
      </div>
    </div>

    <!-- Experience, Figuring Out, Contact added in Task 3 -->
  </main>

  <footer class="footer">
    <div class="container">
      <p>&copy; Pro websites have a footer, but i dunno what to put here</p>
    </div>
  </footer>
</body>
</html>
```

- [ ] **Step 2: Open in browser and verify**

Run: `open index.html` (or use a local server)

Verify:
- Kumiko pattern visible as continuous background
- Navbar is sticky with lavender brand name
- Hero text shows gradient on "Sandjiv" with subtle glow behind
- About section is a centered glass card with visible kumiko on sides
- Inter font is loaded and applied

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "rewrite index.html with Inter font, new hero, and glass-card about"
```

---

### Task 3: Add Experience, Figuring Out, Contact, and Footer to index.html

Add the remaining home page sections between the about section and the footer.

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add the Experience section after the About section**

In `index.html`, replace the `<!-- Experience, Figuring Out, Contact added in Task 3 -->` comment with all remaining sections. Insert this block before `</main>`:

```html
    <div class="section-gap">
      <div class="glass-card">
        <h2 class="section-heading">Experience</h2>
        <div class="timeline">
          <div class="timeline-item">
            <div class="timeline-year">2020</div>
            <div class="timeline-dot"></div>
            <div class="timeline-role">Junior Dev</div>
            <div class="timeline-company">Company A</div>
          </div>
          <div class="timeline-item">
            <div class="timeline-year">2022</div>
            <div class="timeline-dot"></div>
            <div class="timeline-role">Backend Dev</div>
            <div class="timeline-company">Company B</div>
          </div>
          <div class="timeline-item">
            <div class="timeline-year">2023</div>
            <div class="timeline-dot"></div>
            <div class="timeline-role">Senior Dev</div>
            <div class="timeline-company">Company C</div>
          </div>
          <div class="timeline-item current">
            <div class="timeline-year">Now</div>
            <div class="timeline-dot"></div>
            <div class="timeline-role">Current Role</div>
            <div class="timeline-company">Company D</div>
          </div>
        </div>
      </div>
    </div>

    <div class="section-gap">
      <div class="glass-card">
        <h2 class="section-heading">Currently Figuring Out</h2>
        <div class="tag-filters">
          <button class="tag-filter active" data-filter="all">All</button>
          <button class="tag-filter" data-filter="reading">Reading</button>
          <button class="tag-filter" data-filter="building">Building</button>
          <button class="tag-filter" data-filter="learning">Learning</button>
          <button class="tag-filter" data-filter="tools">Tools</button>
        </div>
        <div class="figuring-grid">
          <div class="figuring-card" data-tags="building learning">
            <h4>Go CLI Tools</h4>
            <p class="card-desc">Building small utilities to learn the language</p>
            <div class="progress-bar">
              <div class="progress-fill" style="width: 35%; background: linear-gradient(90deg, var(--ctp-sapphire), var(--ctp-blue));"></div>
            </div>
            <div class="card-tags">
              <span class="card-tag" style="background: rgba(137,180,250,0.15); color: var(--ctp-blue);">Building</span>
              <span class="card-tag" style="background: rgba(166,227,161,0.15); color: var(--ctp-green);">Learning</span>
            </div>
          </div>
          <div class="figuring-card" data-tags="reading">
            <h4>Designing Data-Intensive Apps</h4>
            <p class="card-desc">The Kleppmann book everyone keeps recommending</p>
            <div class="progress-bar">
              <div class="progress-fill" style="width: 60%; background: linear-gradient(90deg, var(--ctp-peach), var(--ctp-yellow));"></div>
            </div>
            <div class="card-tags">
              <span class="card-tag" style="background: rgba(245,194,231,0.15); color: var(--ctp-pink);">Reading</span>
            </div>
          </div>
          <div class="figuring-card" data-tags="tools">
            <h4>Neovim Config</h4>
            <p class="card-desc">Giving in to the dark side (maybe)</p>
            <div class="progress-bar">
              <div class="progress-fill" style="width: 15%; background: linear-gradient(90deg, var(--ctp-mauve), var(--ctp-lavender));"></div>
            </div>
            <div class="card-tags">
              <span class="card-tag" style="background: rgba(148,226,213,0.15); color: var(--ctp-teal);">Tools</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <section class="contact">
      <p class="contact-quip">or don't, i'm not your dad</p>
      <div class="contact-icons">
        <a href="mailto:sandjiv.parassou@gmail.com" class="contact-icon" title="Email">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"></path>
            <polyline points="22,6 12,13 2,6"></polyline>
          </svg>
        </a>
        <a href="https://github.com/Yaquelqun" target="_blank" rel="noopener noreferrer" class="contact-icon" title="GitHub">
          <svg viewBox="0 0 24 24" fill="currentColor">
            <path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/>
          </svg>
        </a>
        <a href="https://www.linkedin.com/in/sandjiv/" target="_blank" rel="noopener noreferrer" class="contact-icon" title="LinkedIn">
          <svg viewBox="0 0 24 24" fill="currentColor">
            <path d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"/>
          </svg>
        </a>
      </div>
    </section>
```

- [ ] **Step 2: Open in browser and verify**

Verify:
- Experience timeline displays horizontally with mauve dots and green "Now" dot
- "Currently Figuring Out" shows tag filters and three cards with progress bars
- Contact section is a slim strip with icon row (no full-width card)
- Footer text preserved

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "add experience timeline, figuring-out cards, and slim contact section"
```

---

### Task 4: Tag Filtering JavaScript

Add vanilla JS to index.html for filtering "Currently Figuring Out" cards by tag.

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add the filtering script before `</body>`**

Insert this `<script>` block just before the closing `</body>` tag in `index.html` (after `</footer>`):

```html
  <script>
    document.querySelectorAll('.tag-filter').forEach(function(btn) {
      btn.addEventListener('click', function() {
        document.querySelector('.tag-filter.active').classList.remove('active');
        btn.classList.add('active');
        var filter = btn.getAttribute('data-filter');
        document.querySelectorAll('.figuring-card').forEach(function(card) {
          if (filter === 'all' || card.getAttribute('data-tags').includes(filter)) {
            card.style.display = '';
          } else {
            card.style.display = 'none';
          }
        });
      });
    });
  </script>
```

- [ ] **Step 2: Test in browser**

Verify:
- Click "Reading" — only the Kleppmann card is visible
- Click "Building" — only the Go CLI card is visible
- Click "Tools" — only the Neovim card is visible
- Click "All" — all three cards visible
- Active filter pill is highlighted in mauve

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "add tag filtering for currently-figuring-out cards"
```

---

### Task 5: Restyle setup.html

Update setup.html to use the new design: Inter font, glass-card layout, restyled sidebar.

**Files:**
- Rewrite: `setup.html`

- [ ] **Step 1: Rewrite setup.html with new styling**

Replace the entire contents of `setup.html`. The content (dotfiles, editor, shell, tmux, mise) is preserved — only the structure and head change. The main content area is now wrapped in a `glass-card`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="My development setup">
  <meta name="author" content="Sandjiv">
  <title>Setup - Sandjiv</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <nav class="navbar">
    <div class="container">
      <a href="/" class="nav-brand">Sandjiv</a>
      <div class="nav-links">
        <a href="/" class="nav-link">Home</a>
        <a href="/setup.html" class="nav-link">Setup</a>
      </div>
    </div>
  </nav>

  <main>
    <section class="setup">
      <nav class="setup-sidebar">
        <a href="#dotfiles" class="sidebar-link">Dotfiles</a>
        <a href="#editor" class="sidebar-link">Editor</a>
        <a href="#shell" class="sidebar-link">Shell</a>
        <a href="#tmux" class="sidebar-link">tmux</a>
        <a href="#tool-version-manager" class="sidebar-link">Version Manager</a>
      </nav>

      <div class="glass-card">
        <div class="setup-content">
          <h1 class="section-heading">My Setup</h1>
          <p>Here's what I use for development and daily work.</p>

          <h2 id="dotfiles">Dotfiles</h2>
          <p>
            You can find my configuration files on GitHub:
            <a href="https://github.com/Yaquelqun/dotfiles" target="_blank" rel="noopener noreferrer">
              github.com/Yaquelqun/dotfiles
            </a>
          </p>

          <h2 id="editor">Editor</h2>
          <p><a href="https://code.visualstudio.com/">VS Code</a></p>
          <p>All my settings are synchronized through GitHub.</p>

          <h2 id="shell">Shell</h2>
          <p>Currently working with <a href="https://ghostty.org/">Ghostty</a> mainly because i really like the top-down accessible-anywhere quick terminal.
            The config is pretty simple since i mostly use it to host tmux
          </p>
          <pre><code># Config
theme = Catppuccin Mocha
font-family = Fira Code # First ligature font i ever used so i'm sticking with it
font-size = 19

mouse-hide-while-typing = true
macos-option-as-alt = true

background-opacity=0.7
background-blur-radius=5

quick-terminal-size = 100%
initial-window = false

input = tmux\n

keybind = global:ctrl+equal=toggle_quick_terminal
</code></pre>

          <p>The shell in itself is zsh since it's now native to mac. Since haven't looked up how to move the configuration to the .config folder, might as well
            put it here
          </p>
          <pre><code>
export PATH="/opt/homebrew/bin:$PATH"
eval "$(~/.local/bin/mise activate)"
export PATH="/opt/homebrew/opt/postgresql@18/bin:$PATH"
export PATH="$PATH:/usr/local/bin/node"
export FONTAWESOME_TOKEN={{FA_TOKEN}}
export GITHUB_MCP_TOKEN={{GITHUB_TOKEN}}
export PATH="/opt/homebrew/opt/mysql-client/bin:$PATH"


# Carapace
autoload -U +X compinit && compinit
autoload -U +X bashcompinit && bashcompinit
export CARAPACE_BRIDGES='zsh,fish,bash,inshellisense' # optional
zstyle ':completion:*' format $'\e[2;37mCompleting %d\e[m'
source <(carapace _carapace)

# aliases
alias fucking="bundle exec"

eval "$(starship init zsh)"

echo "zsh operational"
</code></pre>

          <p>Terminal configuration is endless (and i haven't succumbed to vim-like editors yet :p) but some tools that i use and look cool are:</p>
          <ul>
            <li><a href="https://starship.rs/">Starship</a>: Tool to provide info on branch name/folder versions etc</li>
            <li><a href="https://carapace.sh/">Carapace</a>: Autocomplete library, not sure yet how to use it properly</li>
            <li><a href="https://github.com/ajeetdsouza/zoxide">Zoxide</a>: Want to dig into this, seems plug and play</li>
            <li><a href="https://atuin.sh/">Atuin</a>: Looks really nice as a way to store complicated commands (looking at you pg_restore)</li>
          </ul>

          <h2 id="tmux">tmux</h2>
          <p>The panel/splitting logic of Ghostty is kinda subpar + i can't use it in the quick terminal so <a href="https://github.com/tmux/tmux/wiki">tmux</a> allows me to have several terminals
            without too much worry.
            I got the <a href="https://github.com/Yaquelqun/dotfiles/blob/master/tmux/tmux.conf">config</a> by mashing together a bunch of online finds so it's still very much a WIP
          </p>

          <h2 id="tool-version-manager">Tool Version Manager</h2>
          <p>Now that we have polyglot tool version managers, I figured i'd use <a href="https://mise.jdx.dev/">mise</a> since it seems like an easy-to-forget tool</p>
        </div>
      </div>
    </section>
  </main>

  <footer class="footer">
    <div class="container">
      <p>&copy; Pro websites have a footer, but i dunno what to put here</p>
    </div>
  </footer>

  <script>
    document.querySelectorAll('.sidebar-link').forEach(function(link) {
      link.addEventListener('click', function(e) {
        e.preventDefault();
        var targetId = this.getAttribute('href').substring(1);
        var targetElement = document.getElementById(targetId);
        if (targetElement) {
          var navbarHeight = document.querySelector('.navbar').offsetHeight;
          var targetPosition = targetElement.offsetTop - navbarHeight - 20;
          window.scrollTo({
            top: targetPosition,
            behavior: 'smooth'
          });
        }
      });
    });

    function updateActiveLink() {
      var navbarHeight = document.querySelector('.navbar').offsetHeight;
      var sections = document.querySelectorAll('.setup-content h2[id]');
      var links = document.querySelectorAll('.sidebar-link');
      var currentSection = '';
      sections.forEach(function(section) {
        var sectionTop = section.offsetTop - navbarHeight - 100;
        if (window.scrollY >= sectionTop) {
          currentSection = section.getAttribute('id');
        }
      });
      links.forEach(function(link) {
        link.classList.remove('active');
        if (link.getAttribute('href') === '#' + currentSection) {
          link.classList.add('active');
        }
      });
    }

    window.addEventListener('scroll', updateActiveLink);
    window.addEventListener('load', updateActiveLink);
  </script>
</body>
</html>
```

- [ ] **Step 2: Open in browser and verify**

Verify:
- Kumiko background visible behind the glass card
- Sidebar is styled with Catppuccin colors, glass-card–like background
- Code blocks use Fira Code on a Mantle background
- All content is preserved and readable
- Sidebar link highlighting works on scroll
- Links are Sapphire colored

- [ ] **Step 3: Commit**

```bash
git add setup.html
git commit -m "restyle setup page with glass-card layout and Catppuccin theme"
```

---

### Task 6: Final Responsive Check and Polish

Test both pages at multiple viewport sizes and fix any issues.

**Files:**
- Modify: `style.css` (if fixes needed)
- Modify: `index.html` (if fixes needed)
- Modify: `setup.html` (if fixes needed)

- [ ] **Step 1: Test at 1440px (desktop)**

Open both pages in browser at full width. Verify:
- Kumiko pattern is continuous and visible in gaps between cards
- Glass cards have comfortable margins
- Timeline items are spaced evenly
- Setup sidebar is positioned correctly with content offset

- [ ] **Step 2: Test at 768px (tablet)**

Resize browser to 768px. Verify:
- Navbar stacks vertically
- Glass cards go nearly full width (1rem margins)
- Figuring-out cards stack to single column
- Setup sidebar is hidden, content takes full width

- [ ] **Step 3: Test at 480px (mobile)**

Resize to 480px. Verify:
- Nav links stack vertically
- Hero text scales down via clamp()
- Timeline scrolls horizontally if it overflows
- Contact icons remain centered
- All text is readable

- [ ] **Step 4: Fix any issues found**

Apply CSS fixes as needed. Common things to check:
- Card padding on small screens
- Timeline overflow handling
- Code block horizontal scroll on setup page

- [ ] **Step 5: Final commit**

```bash
git add -A
git commit -m "responsive polish and final tweaks"
```
