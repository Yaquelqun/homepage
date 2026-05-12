# Experience timeline — git-log design

Replaces the current horizontal-flex `.timeline` in `index.html`. The existing one has alignment issues at desktop/tablet widths and hides descriptions on mobile.

## Concept

Render the experience section as the output of `git log --graph --decorate`. Each role is a commit. Year becomes the short hash, company becomes the tag. Subject line carries a conventional-commit prefix (`init:`, `feat:`, `chore:`, `refactor:`, `hack:`). Body is the description, indented under the graph pipe.

## Layout

Plain text, monospace, oldest commit at the top, HEAD at the bottom.

```
* 2005 (tag: family-computer) init: printf("hello world")
|     Following obscure C courses on the internet
|     to code terminal programs that would ask for your age
|     and make you guess a number between 0 and 100
|
* 2016 hack: lie to recruiter, learn HTML overnight
|     Lied to a recruiter about having a portfolio
|     Scrambled to understand HTML/CSS in a night
|     Did 2 horrible pages, "hosted" them on Dropbox
|     Sent them the link
|     on my way to the Dunning-Kruger peak
|
* 2016 (tag: drawbotics) feat: somehow get hired as an intern
|     Still can't believe that worked
|
* 2016 (tag: drawbotics) chore: find footing as backend dev
|     Imposter syndrome kicking in
|     at least i'm not in charge of anything
|
* 2019 (tag: drawbotics/mosaic) feat: promoted to lead backend dev
|     Oh no
|
* 2023 (tag: ring-twice) refactor: trade b2b for b2c
|     i wonder if b2c problematics are as interesting as the b2b ones
|
* 2026 (HEAD -> main, tag: ring-twice) feat: scaling things, having fun
|     Ok this is actually pretty fun
|     let's see how far i can scale this thing
```

## Rules

- **Order**: oldest at top, HEAD at bottom. (Opposite of real `git log`; chronological reading wins over git-fidelity.)
- **Hash slot**: 4-digit year. Duplicates are fine (multiple commits in 2016).
- **Tag slot**: company slug. Multiple commits at the same company share the same tag. Solo work (the 2016 portfolio-lie entry) has no tag.
- **HEAD decoration**: only the current role. Format: `(HEAD -> main, tag: <company>)`.
- **Subject line**: short, lowercase, with a conventional-commit prefix. Allowed prefixes: `init`, `feat`, `chore`, `refactor`, `hack`. One `hack:` (the Dropbox-portfolio entry) as an Easter egg.
- **Body**: 1–5 lines, indented 5 spaces under the `|`. Each line free-form. The current `\n`-broken descriptions in `index.html` carry over with minor edits.
- **Separator**: one bare `|` line between commits. None after the HEAD commit.

## Color palette (Catppuccin Mocha → existing CSS vars)

| Element            | Variable           | Hex      |
|--------------------|--------------------|----------|
| Graph `*` and `\|` | `--ctp-mauve`      | `#cba6f7` |
| Graph (current)    | `--ctp-green`      | `#a6e3a1` |
| Hash (year)        | `--ctp-peach`      | `#fab387` |
| Parens / arrow     | `--ctp-overlay0`   | `#6c7086` |
| HEAD / main        | `--ctp-green`      | `#a6e3a1` |
| Tag                | `--ctp-maroon`     | `#eba0ac` |
| Prefix (`feat:`)   | `--ctp-blue`       | `#89b4fa` |
| Subject            | `--ctp-text`       | `#cdd6f4` |
| Body               | `--ctp-subtext0`   | `#a6adc8` |

## Typography & spacing

- Font: `'Fira Code', 'Courier New', monospace` (Fira Code is already loaded for setup-page code blocks; reuse).
- Size: `0.85rem` desktop, `0.72rem` mobile (≤640px).
- Line height: `1.6`.
- The whole block lives inside the existing `.glass-card` for the Experience section. No structural change to that wrapper.
- Container has `overflow-x: auto` so long lines scroll horizontally on narrow screens rather than wrapping (preserves the monospaced alignment).

## Interaction

- Subtle row hover: `background: rgba(203, 166, 247, 0.05)` on each commit block. Green tint on the current/HEAD commit.
- No click behavior. No expand/collapse. Static text.

## Markup approach

Each commit is one `<div class="gl-commit">` wrapping the subject line and its body lines. Each line is a `<span class="gl-line">` with `white-space: pre` and `display: block` so the leading `|     ` characters render exactly. Tokens inside the line get color classes (`gl-hash`, `gl-tag`, `gl-prefix`, etc.).

```html
<div class="gl-commit">
  <span class="gl-line">
    <span class="gl-graph">*</span>
    <span class="gl-hash">2005</span>
    <span class="gl-paren">(</span><span class="gl-tag">tag: family-computer</span><span class="gl-paren">)</span>
    <span class="gl-prefix">init:</span>
    <span class="gl-subject">printf("hello world")</span>
  </span>
  <span class="gl-line"><span class="gl-graph">|</span>     <span class="gl-body">Following obscure C courses on the internet</span></span>
  ...
</div>
```

No JS required.

## Mobile

Same markup, smaller font, horizontal scroll on the container. The descriptions stay visible (the current timeline hides them on mobile — explicitly fixed here).

## What gets removed

In `index.html`:
- The entire `<div class="timeline"> ... </div>` and its 7 `.timeline-item` children.

In `style.css`:
- `.timeline`, `.timeline::before`, `.timeline::after`
- `.timeline-item`, `.timeline-item.current`
- `.timeline-dot`, `.timeline-year`, `.timeline-role`, `.timeline-company`, `.timeline-desc`
- Both mobile timeline overrides at `@media (max-width: 768px)`

## What gets added

In `style.css`:
- `.gl`, `.gl-line`, `.gl-commit`, `.gl-commit.current`
- Token classes: `.gl-graph`, `.gl-graph.current`, `.gl-hash`, `.gl-paren`, `.gl-head`, `.gl-arrow`, `.gl-tag`, `.gl-prefix`, `.gl-subject`, `.gl-body`
- Hover rules and the `≤640px` media query for font shrink

## Out of scope

- Branches and merge commits. We agreed on a linear log for v1; branches can come back as a follow-up if the linear version reads flat.
- Click-to-expand `git show` panels.
- Animations beyond the existing hover transition.
- Editing the surrounding glass-card layout, kumiko background, or section heading.

## Reference

The validated mockup lives at `.superpowers/brainstorm/<session>/content/git-log-v5.html` for visual reference during implementation.
