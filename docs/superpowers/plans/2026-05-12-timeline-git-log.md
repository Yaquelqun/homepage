# Experience Timeline (Git Log) Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the current `.timeline` component on `index.html` with a static, monospaced `git log --graph --decorate` rendering of the user's career, per `docs/superpowers/specs/2026-05-12-timeline-design.md`.

**Architecture:** Pure HTML + CSS swap inside the existing Experience `.glass-card`. No JS, no new dependencies. Each commit row is one `<div class="gl-commit">` containing `<span class="gl-line">` rows; tokens within a line carry color classes. Old `.timeline*` rules and markup are removed.

**Tech Stack:** Static HTML / CSS. No build step. Verification via Playwright screenshots (the script + browsers were installed in `/tmp/portfolio-shots/` earlier in this session and can be reused).

**Project conventions:**
- Commit messages are lowercase, casual, imperative-ish (see `git log --oneline -5`).
- No test framework exists — visual verification is the test.
- The site is served by any static HTTP server. Use `python3 -m http.server 8765` from the repo root.

---

## File Structure

| File | Action | Responsibility |
|---|---|---|
| `index.html` | Modify (lines 53–118) | Replace the `<div class="timeline">…</div>` block with the new `<div class="gl">…</div>` block. The outer `<div class="section-gap">` and `<div class="glass-card">` plus the `<h2 class="section-heading">Experience</h2>` stay. |
| `style.css` | Modify (lines 238–320 and 653–696) | Add new `.gl*` token rules. Remove old `.timeline*` rules and their mobile overrides. |

No new files.

---

## Task 1: Add new `.gl` CSS rules

**Files:**
- Modify: `style.css` — append new rules immediately after the existing timeline block (around line 320, before the `/* Currently Figuring Out */` comment)

Add additively so the old timeline still renders during the intermediate step.

- [ ] **Step 1: Open `style.css` and insert the new block**

Insert this entire block immediately after line 320 (the closing `}` of `.timeline-desc`) and before the `/* Currently Figuring Out — tag filters */` comment:

```css
/* Experience — git log */
.gl {
  font-family: 'Fira Code', 'Courier New', monospace;
  font-size: 0.85rem;
  line-height: 1.6;
  margin-top: 1.5rem;
  color: var(--ctp-subtext1);
  overflow-x: auto;
  padding-bottom: 0.5rem;
}

.gl-line {
  white-space: pre;
  display: block;
}

.gl-commit {
  padding: 0.15rem 0;
  border-radius: 4px;
  transition: background 0.2s ease;
}

.gl-commit:hover {
  background: rgba(203, 166, 247, 0.05);
}

.gl-commit.current:hover {
  background: rgba(166, 227, 161, 0.05);
}

.gl-graph { color: var(--ctp-mauve); }
.gl-graph.current { color: var(--ctp-green); }
.gl-hash { color: var(--ctp-peach); }
.gl-paren { color: var(--ctp-overlay0); }
.gl-head { color: var(--ctp-green); font-weight: 600; }
.gl-arrow { color: var(--ctp-overlay0); }
.gl-tag { color: var(--ctp-maroon); }
.gl-prefix { color: var(--ctp-blue); }
.gl-subject { color: var(--ctp-text); }
.gl-body { color: var(--ctp-subtext0); }
```

- [ ] **Step 2: Add the mobile font-shrink rule inside the existing `@media (max-width: 768px)` block**

There is already a `@media (max-width: 768px)` block starting around line 627 of `style.css`. Inside it (NOT in a new media query), add the following rule. The natural place is right after the `.timeline-desc { display: none; }` rule and before `.contact { ... }`. (The `.timeline*` rules in that media block will be deleted in Task 4 — leave them alone for now.)

```css
  .gl {
    font-size: 0.72rem;
  }
```

- [ ] **Step 3: Do NOT commit yet.** The page renders the old timeline still; the new rules are dead until Task 2 lands. Skip straight to Task 2.

---

## Task 2: Replace timeline markup in `index.html`

**Files:**
- Modify: `index.html` lines 53–118 (the `<div class="section-gap">` whose `<h2>` is `Experience`)

- [ ] **Step 1: Locate the block**

The block to replace starts with `<div class="section-gap">` at line 53 and ends with the matching `</div>` at line 118. Inside it is `<div class="glass-card">` containing `<h2 class="section-heading">Experience</h2>` followed by `<div class="timeline">…</div>`.

**Only the `<div class="timeline">…</div>` gets replaced.** The outer `.section-gap`, the `.glass-card`, and the `<h2>` all stay.

- [ ] **Step 2: Replace `<div class="timeline">…</div>` with the new markup**

Delete the entire `<div class="timeline"> … </div>` (lines 56 through 116 inclusive) and replace with:

```html
        <div class="gl">
<div class="gl-commit"><span class="gl-line"><span class="gl-graph">*</span> <span class="gl-hash">2005</span> <span class="gl-paren">(</span><span class="gl-tag">tag: family-computer</span><span class="gl-paren">)</span> <span class="gl-prefix">init:</span> <span class="gl-subject">printf("hello world")</span></span><span class="gl-line"><span class="gl-graph">|</span>     <span class="gl-body">Following obscure C courses on the internet</span></span><span class="gl-line"><span class="gl-graph">|</span>     <span class="gl-body">to code terminal programs that would ask for your age</span></span><span class="gl-line"><span class="gl-graph">|</span>     <span class="gl-body">and make you guess a number between 0 and 100</span></span><span class="gl-line"><span class="gl-graph">|</span></span></div>
<div class="gl-commit"><span class="gl-line"><span class="gl-graph">*</span> <span class="gl-hash">2016</span> <span class="gl-prefix">hack:</span> <span class="gl-subject">lie to recruiter, learn HTML overnight</span></span><span class="gl-line"><span class="gl-graph">|</span>     <span class="gl-body">Lied to a recruiter about having a portfolio</span></span><span class="gl-line"><span class="gl-graph">|</span>     <span class="gl-body">Scrambled to understand HTML/CSS in a night</span></span><span class="gl-line"><span class="gl-graph">|</span>     <span class="gl-body">Did 2 horrible pages, "hosted" them on Dropbox</span></span><span class="gl-line"><span class="gl-graph">|</span>     <span class="gl-body">Sent them the link</span></span><span class="gl-line"><span class="gl-graph">|</span>     <span class="gl-body">on my way to the Dunning-Kruger peak</span></span><span class="gl-line"><span class="gl-graph">|</span></span></div>
<div class="gl-commit"><span class="gl-line"><span class="gl-graph">*</span> <span class="gl-hash">2016</span> <span class="gl-paren">(</span><span class="gl-tag">tag: drawbotics</span><span class="gl-paren">)</span> <span class="gl-prefix">feat:</span> <span class="gl-subject">somehow get hired as an intern</span></span><span class="gl-line"><span class="gl-graph">|</span>     <span class="gl-body">Still can't believe that worked</span></span><span class="gl-line"><span class="gl-graph">|</span></span></div>
<div class="gl-commit"><span class="gl-line"><span class="gl-graph">*</span> <span class="gl-hash">2016</span> <span class="gl-paren">(</span><span class="gl-tag">tag: drawbotics</span><span class="gl-paren">)</span> <span class="gl-prefix">chore:</span> <span class="gl-subject">find footing as backend dev</span></span><span class="gl-line"><span class="gl-graph">|</span>     <span class="gl-body">Imposter syndrome kicking in</span></span><span class="gl-line"><span class="gl-graph">|</span>     <span class="gl-body">at least i'm not in charge of anything</span></span><span class="gl-line"><span class="gl-graph">|</span></span></div>
<div class="gl-commit"><span class="gl-line"><span class="gl-graph">*</span> <span class="gl-hash">2019</span> <span class="gl-paren">(</span><span class="gl-tag">tag: drawbotics/mosaic</span><span class="gl-paren">)</span> <span class="gl-prefix">feat:</span> <span class="gl-subject">promoted to lead backend dev</span></span><span class="gl-line"><span class="gl-graph">|</span>     <span class="gl-body">Oh no</span></span><span class="gl-line"><span class="gl-graph">|</span></span></div>
<div class="gl-commit"><span class="gl-line"><span class="gl-graph">*</span> <span class="gl-hash">2023</span> <span class="gl-paren">(</span><span class="gl-tag">tag: ring-twice</span><span class="gl-paren">)</span> <span class="gl-prefix">refactor:</span> <span class="gl-subject">trade b2b for b2c</span></span><span class="gl-line"><span class="gl-graph">|</span>     <span class="gl-body">i wonder if b2c problematics are as interesting as the b2b ones</span></span><span class="gl-line"><span class="gl-graph">|</span></span></div>
<div class="gl-commit current"><span class="gl-line"><span class="gl-graph current">*</span> <span class="gl-hash">2026</span> <span class="gl-paren">(</span><span class="gl-head">HEAD</span> <span class="gl-arrow">-&gt;</span> <span class="gl-head">main</span><span class="gl-paren">, </span><span class="gl-tag">tag: ring-twice</span><span class="gl-paren">)</span> <span class="gl-prefix">feat:</span> <span class="gl-subject">scaling things, having fun</span></span><span class="gl-line"><span class="gl-graph current">|</span>     <span class="gl-body">Ok this is actually pretty fun</span></span><span class="gl-line"><span class="gl-graph current">|</span>     <span class="gl-body">let's see how far i can scale this thing</span></span></div>
        </div>
```

**Why each `<div class="gl-commit">` is on a single line:** the children `<span class="gl-line">` set `white-space: pre` to keep the leading `|     ` glyphs literal. If we put line breaks between the `<span>`s in the source, those breaks would render as whitespace runs and shift everything. Keep each commit's spans on one physical source line.

- [ ] **Step 3: Do NOT commit yet.** Move to Task 3 for visual verification.

---

## Task 3: Visual verification (intermediate)

**Goal:** Confirm the new timeline renders correctly and matches the v5 mockup before deleting the old CSS.

- [ ] **Step 1: Start a local server**

```bash
python3 -m http.server 8765 --bind 127.0.0.1
```

Run this in the background (e.g. `run_in_background: true` if using the Bash tool). The server serves `index.html` from the repo root at `http://127.0.0.1:8765/`.

- [ ] **Step 2: Confirm Playwright is installed**

The brainstorming work in this session left a Playwright install at `/tmp/portfolio-shots/`. Verify with:

```bash
ls /tmp/portfolio-shots/node_modules/playwright >/dev/null && echo "ready" || echo "needs install"
```

If the output is `needs install`, run:

```bash
mkdir -p /tmp/portfolio-shots && cd /tmp/portfolio-shots && \
  npm init -y >/dev/null && \
  npm install --silent playwright@latest && \
  npx playwright install chromium
```

- [ ] **Step 3: Write a small screenshot script**

Create `/tmp/portfolio-shots/shoot-timeline.js`:

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
    // Crop to the Experience section by locating the h2
    const heading = page.locator('h2.section-heading', { hasText: 'Experience' });
    const card = heading.locator('..'); // the .glass-card
    await card.screenshot({ path: `/tmp/portfolio-shots/timeline-${v.name}.png` });
    await ctx.close();
  }
  await browser.close();
  console.log('done');
})().catch(e => { console.error(e); process.exit(1); });
```

- [ ] **Step 4: Run the script and view the output**

```bash
cd /tmp/portfolio-shots && node shoot-timeline.js
```

Use the Read tool on each of `/tmp/portfolio-shots/timeline-desktop.png`, `/tmp/portfolio-shots/timeline-tablet.png`, and `/tmp/portfolio-shots/timeline-mobile.png` to inspect them visually.

**Expected (must match):**
- Oldest commit (`2005`) at the top, current/HEAD commit (`2026`) at the bottom.
- Each commit shows: graph `*`, peach year, optional maroon `tag: …`, blue prefix (`init:`/`feat:`/etc), text-colored subject.
- Body lines indented under `|` in subdued gray.
- Current commit's `*` and `|` are green; HEAD/main labels green; tag still maroon.
- One bare `|` line between every pair of commits; no trailing `|` after the HEAD commit.
- On mobile (390 wide): font is smaller, horizontal scroll is available on the `.gl` container, body lines are visible (no longer `display: none`).

- [ ] **Step 5: If anything is off**, fix the markup or CSS, re-run the script, re-inspect. Do not proceed until all three viewports match. Do not commit yet.

---

## Task 4: Remove the dead `.timeline*` CSS

**Files:**
- Modify: `style.css` — delete the original timeline rules (lines 238–320 before any prior edits) and the timeline-specific overrides inside the `@media (max-width: 768px)` block (lines 653–696 before any prior edits)

Line numbers below refer to the file *before this task* (i.e., after Task 1 added new rules; the new rules were inserted *after* line 320 so the original block's line numbers are unchanged).

- [ ] **Step 1: Remove the main `.timeline*` rule block**

Delete the entire block starting with the comment `/* Experience timeline */` (line 238) through the closing `}` of `.timeline-desc` (line 320, the line ending `}` that closes the `white-space: pre;` rule). Also delete the blank line directly after.

After deletion, the new `/* Experience — git log */` comment from Task 1 should sit immediately after the `.about-text a:hover` rule (around line 237 before deletion).

- [ ] **Step 2: Remove the timeline overrides inside the `@media (max-width: 768px)` block**

Inside the `@media (max-width: 768px) { … }` block (originally starting around line 627), delete these rules in order:

- `.timeline { … }` (5-property rule, originally ~lines 653–659)
- `.timeline::before { … }` (originally ~lines 661–668)
- `.timeline::after { … }` (originally ~lines 670–679)
- `.timeline-item { … }` (originally ~lines 681–685)
- `.timeline-dot { … }` (originally ~lines 687–692)
- `.timeline-desc { display: none; }` (originally ~lines 694–696)

The `.figuring-grid { grid-template-columns: 1fr; }` rule and the `.contact { padding: 1.5rem 1rem; }` rule that follow stay untouched.

- [ ] **Step 3: Sanity-grep that no `.timeline` references remain**

```bash
grep -n 'timeline' /Users/sandjivparassouramanaick/Workspace/Learning/homepage/style.css /Users/sandjivparassouramanaick/Workspace/Learning/homepage/index.html
```

Expected output: empty (no matches in either file).

If anything still matches, delete it.

---

## Task 5: Final visual verification + commit

- [ ] **Step 1: Re-run the screenshot script**

```bash
cd /tmp/portfolio-shots && node shoot-timeline.js
```

- [ ] **Step 2: Read each screenshot and confirm nothing regressed**

Read `/tmp/portfolio-shots/timeline-desktop.png`, `timeline-tablet.png`, `timeline-mobile.png`. The timeline should still render identically to the Task 3 verification — removing the dead CSS should change nothing visually.

- [ ] **Step 3: Stop the local server**

```bash
pkill -f "http.server 8765"
```

- [ ] **Step 4: Stage and commit**

```bash
cd /Users/sandjivparassouramanaick/Workspace/Learning/homepage
git add index.html style.css
git status
```

Expected status: `index.html` and `style.css` both modified, nothing else staged.

Then commit:

```bash
git commit -m "$(cat <<'EOF'
replace experience timeline with git log layout

career rendered as `git log --graph --decorate` output: year as short
hash, company as tag, subject + indented body block per commit.
fixes the horizontal-flex alignment issues and stops hiding descriptions
on mobile.

design: docs/superpowers/specs/2026-05-12-timeline-design.md

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

- [ ] **Step 5: Confirm clean tree**

```bash
git status
```

Expected: `working tree clean`, branch `main` ahead of `origin/main` by N+1 commits.

---

## Done

The Experience section now renders as a static `git log --graph --decorate` block. The old `.timeline*` markup and styles are gone. No JS was added. No other sections of the page were touched.

Next on the UX review list (see `.notes/ux-review-2026-05-12.md`) — independent of this work:
- Replace "Currently Figuring Out" progress bars with status pills
- One confident sentence in the hero
- Setup-page code-block syntax highlighting + `{{TOKEN}}` placeholder styling
- Visual rhythm / accent variation across sections

Each of those gets its own brainstorm + plan when you're ready.
