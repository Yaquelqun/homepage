# Status Pills Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the percentage-based `.progress-bar` on each `.figuring-card` with a corner-positioned status pill that names the exploration's current energy honestly (`digging in` / `dipping toes` / `gave up (for now)`).

**Architecture:** Pure HTML + CSS. No JS, no new dependencies. The pill is the first child of each `.figuring-card`, floated right so it pins to the top-right corner and short titles flow beside it while long titles reclaim full card width on subsequent lines. The `.progress-bar` and `.progress-fill` rules + their inline-styled markup are removed.

**Tech Stack:** Static HTML / CSS. No build step. Verification via Playwright screenshots (browsers already installed at `/tmp/portfolio-shots/`).

**Project conventions:**
- Commit messages are lowercase, casual, imperative-ish.
- No test framework — visual verification is the test.
- Static server: `cd /Users/sandjivparassouramanaick/Workspace/Learning/homepage && python3 -m http.server 8765 --bind 127.0.0.1`.

**Design spec:** `docs/superpowers/specs/2026-05-12-status-pills-design.md`.

---

## File Structure

| File | Action | Responsibility |
|---|---|---|
| `style.css` | Modify (around lines 365–382) | Add `.status-pill` + three modifier rules; add `clear: right` to `.figuring-card .card-desc`; remove `.progress-bar` and `.progress-fill` rules. |
| `index.html` | Modify (lines 120–150) | For each of the 3 `.figuring-card` divs: insert a `<span class="status-pill status-*">…</span>` as the first child; delete the `<div class="progress-bar">…</div>` element. |

No new files.

---

## Task 1: Add `.status-pill` CSS + remove dead progress-bar rules

**Files:**
- Modify: `style.css` — add new rules near `.card-tags` (around line 384), remove old rules at lines 371–382, add `clear: right` to existing rule at lines 365–369

The new CSS is additive *and* removes dead rules in the same pass — they're tightly coupled to the markup change in Task 2, and the page is broken between the two anyway. Do not commit between Tasks 1 and 2; the controller verifies and commits at the end.

- [ ] **Step 1: Add `clear: right` to the existing `.figuring-card .card-desc` rule**

Locate this rule (currently around lines 365–369 of `style.css`):

```css
.figuring-card .card-desc {
  font-size: 0.8125rem;
  color: var(--ctp-subtext0);
  margin-bottom: 0.75rem;
}
```

Change it to:

```css
.figuring-card .card-desc {
  font-size: 0.8125rem;
  color: var(--ctp-subtext0);
  margin-bottom: 0.75rem;
  clear: right;
}
```

- [ ] **Step 2: Delete the `.progress-bar` and `.progress-fill` rules**

Locate this block (currently around lines 371–382 of `style.css`):

```css
.progress-bar {
  height: 4px;
  background: var(--ctp-surface0);
  border-radius: 2px;
  margin-bottom: 1rem;
}

.progress-fill {
  height: 100%;
  border-radius: 2px;
  box-shadow: 0 0 12px rgba(203, 166, 247, 0.3);
}
```

Delete the entire block, including the blank line after it. The next rule (`.card-tags`) should sit directly below `.figuring-card .card-desc`.

- [ ] **Step 3: Add the `.status-pill` rules**

Insert this block immediately before the `.card-tags` rule (i.e. directly after the `.figuring-card .card-desc` rule from Step 1):

```css
.status-pill {
  float: right;
  margin-left: 0.75rem;
  display: inline-flex;
  align-items: center;
  gap: 0.35rem;
  font-size: 0.6875rem;
  padding: 0.15rem 0.55rem;
  border-radius: 99px;
  font-weight: 500;
  letter-spacing: 0.02em;
}

.status-pill::before {
  content: '';
  width: 5px;
  height: 5px;
  border-radius: 50%;
}

.status-digging {
  background: rgba(166, 227, 161, 0.15);
  color: var(--ctp-green);
}
.status-digging::before {
  background: var(--ctp-green);
  box-shadow: 0 0 6px rgba(166, 227, 161, 0.6);
}

.status-dipping {
  background: rgba(250, 179, 135, 0.15);
  color: var(--ctp-peach);
}
.status-dipping::before {
  background: var(--ctp-peach);
}

.status-gaveup {
  background: rgba(108, 112, 134, 0.18);
  color: var(--ctp-overlay0);
}
.status-gaveup::before {
  background: var(--ctp-overlay0);
}
```

- [ ] **Step 4: Sanity-grep for removed selectors**

```bash
grep -n -E '\.progress-(bar|fill)' /Users/sandjivparassouramanaick/Workspace/Learning/homepage/style.css /Users/sandjivparassouramanaick/Workspace/Learning/homepage/index.html
```

Expected output: zero matches in `style.css`. Three matches in `index.html` are still expected at this point (the markup is removed in Task 2).

- [ ] **Step 5: Do NOT commit.** Move directly to Task 2.

---

## Task 2: Update card markup in `index.html`

**Files:**
- Modify: `index.html` — three `.figuring-card` blocks at lines 120–150

For each of the three cards: insert one `<span class="status-pill status-*">…</span>` as the first child (before `<h4>`), and delete the `<div class="progress-bar">…</div>` block (2 lines per card).

- [ ] **Step 1: Replace the Go CLI Tools card (currently lines 120–130)**

Replace:

```html
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
```

With:

```html
          <div class="figuring-card" data-tags="building learning">
            <span class="status-pill status-digging">digging in</span>
            <h4>Go CLI Tools</h4>
            <p class="card-desc">Building small utilities to learn the language</p>
            <div class="card-tags">
              <span class="card-tag" style="background: rgba(137,180,250,0.15); color: var(--ctp-blue);">Building</span>
              <span class="card-tag" style="background: rgba(166,227,161,0.15); color: var(--ctp-green);">Learning</span>
            </div>
          </div>
```

- [ ] **Step 2: Replace the Designing Data-Intensive Apps card (currently lines 131–140)**

Replace:

```html
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
```

With:

```html
          <div class="figuring-card" data-tags="reading">
            <span class="status-pill status-dipping">dipping toes</span>
            <h4>Designing Data-Intensive Apps</h4>
            <p class="card-desc">The Kleppmann book everyone keeps recommending</p>
            <div class="card-tags">
              <span class="card-tag" style="background: rgba(245,194,231,0.15); color: var(--ctp-pink);">Reading</span>
            </div>
          </div>
```

- [ ] **Step 3: Replace the Neovim Config card (currently lines 141–150)**

Replace:

```html
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
```

With:

```html
          <div class="figuring-card" data-tags="tools">
            <span class="status-pill status-gaveup">gave up (for now)</span>
            <h4>Neovim Config</h4>
            <p class="card-desc">Giving in to the dark side (maybe)</p>
            <div class="card-tags">
              <span class="card-tag" style="background: rgba(148,226,213,0.15); color: var(--ctp-teal);">Tools</span>
            </div>
          </div>
```

- [ ] **Step 4: Sanity-grep that no `progress-` markup remains**

```bash
grep -n 'progress-' /Users/sandjivparassouramanaick/Workspace/Learning/homepage/index.html /Users/sandjivparassouramanaick/Workspace/Learning/homepage/style.css
```

Expected output: zero matches in both files.

- [ ] **Step 5: Do NOT commit.** Controller will visually verify and commit (Task 3).

---

## Task 3: Visual verification + commit

**Goal:** Confirm the cards render correctly at desktop / tablet / mobile, including the "Designing Data-Intensive Apps" card whose title is long enough to wrap. Then commit.

This task is run by the controller, not the implementer.

- [ ] **Step 1: Start a local server**

```bash
cd /Users/sandjivparassouramanaick/Workspace/Learning/homepage && python3 -m http.server 8765 --bind 127.0.0.1
```

Run in the background.

- [ ] **Step 2: Write a screenshot script targeting the "Currently Figuring Out" section**

Create `/tmp/portfolio-shots/shoot-pills.js`:

```javascript
const { chromium } = require('playwright');
(async () => {
  const browser = await chromium.launch();
  const viewports = [
    { name: 'desktop', width: 1440, height: 900 },
    { name: 'tablet',  width: 820,  height: 1180 },
    { name: 'mobile',  width: 390,  height: 844 },
  ];
  for (const v of viewports) {
    const ctx = await browser.newContext({ viewport: { width: v.width, height: v.height } });
    const page = await ctx.newPage();
    await page.goto('http://127.0.0.1:8765/index.html', { waitUntil: 'networkidle' });
    await page.waitForTimeout(300);
    const heading = page.locator('h2.section-heading', { hasText: 'Currently Figuring Out' });
    const card = heading.locator('..');
    await card.screenshot({ path: `/tmp/portfolio-shots/pills-${v.name}.png` });
    await ctx.close();
  }
  await browser.close();
  console.log('done');
})().catch(e => { console.error(e); process.exit(1); });
```

- [ ] **Step 3: Run the script**

```bash
cd /tmp/portfolio-shots && node shoot-pills.js
```

- [ ] **Step 4: Read the screenshots and verify**

Read `/tmp/portfolio-shots/pills-desktop.png`, `pills-tablet.png`, `pills-mobile.png`. Confirm:

- Each card has exactly one status pill in the top-right corner.
- `Go CLI Tools` card shows `digging in` in green with a glowing green dot.
- `Designing Data-Intensive Apps` card shows `dipping toes` in peach with a peach dot. The title is long — first line of the title sits beside the pill; if it wraps, line 2 uses the full card width and is not constrained by the pill.
- `Neovim Config` card shows `gave up (for now)` in grey (overlay0) with a grey dot. No dot glow.
- No progress bar appears on any card.
- Filter chips at the top (`All`, `Reading`, `Building`, `Learning`, `Tools`) still render and still filter the cards (clicking is not required for visual verification; just confirm presence).
- The hover transition on the cards still works visually — borders + translate + shadow.

- [ ] **Step 5: Stop the server**

```bash
pkill -f "http.server 8765"
```

- [ ] **Step 6: Stage and commit**

```bash
cd /Users/sandjivparassouramanaick/Workspace/Learning/homepage
git add index.html style.css
git status
```

Expected status: `index.html` and `style.css` modified, nothing else staged.

```bash
git commit -m "$(cat <<'EOF'
replace progress bars with status pills on figuring-out cards

drops the percentage metaphor (which implied tracking that didn't
exist) for an honest snapshot of energy: digging in / dipping toes /
gave up (for now). pill floats top-right; long titles wrap below
naturally.

design: docs/superpowers/specs/2026-05-12-status-pills-design.md
plan:   docs/superpowers/plans/2026-05-12-status-pills.md

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

- [ ] **Step 7: Confirm clean tree**

```bash
git status
```

Expected: `working tree clean`, branch `main` ahead of `origin/main` by N+1 commits.

---

## Done

`.figuring-card`s now carry an honest status pill instead of a faux-precise percentage. `.progress-bar` and `.progress-fill` are gone from both files. No JS added. No other sections of the page touched.

Next on the UX review list (independent work):
- Setup page: syntax highlighting + style `{{TOKEN}}` placeholders distinctly
- Kumiko background: commit to it or drop it
- Visual rhythm across the four glass cards
