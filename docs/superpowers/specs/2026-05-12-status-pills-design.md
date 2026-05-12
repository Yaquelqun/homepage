# Currently Figuring Out — status pills

Replaces the `.progress-bar` / `.progress-fill` element on each `.figuring-card` with a status pill in the **top-right corner** of the card. Drops the implied "this exploration is X% done" semantics in favour of an honest snapshot of energy level.

## Vocabulary

Three statuses, lowercase:

| Status              | Meaning                                              | Colour                |
|---------------------|------------------------------------------------------|-----------------------|
| `digging in`        | actively working on it / touched in the last week    | `--ctp-green`         |
| `dipping toes`      | exploring, not committed; on/off                     | `--ctp-peach`         |
| `gave up (for now)` | parked; not actively touching but not closed         | `--ctp-overlay0` (grey) |

Each card carries exactly one status.

## Position and layout

- The pill sits absolutely positioned in the top-right corner of the card: `top: 0.85rem; right: 0.85rem`.
- The card title (`.figuring-card h4`) gets `padding-right` reserved so the title never overlaps the pill. Use `padding-right: 8rem` (enough for the widest pill `gave up (for now)` plus a buffer).
- Card description and tags below the title use the full card width unchanged — only `h4` needs the padding.

## Visual treatment

Pill structure:

```html
<span class="status-pill status-digging">digging in</span>
```

Pill CSS:

```css
.status-pill {
  position: absolute;
  top: 0.85rem;
  right: 0.85rem;
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

Notes:
- `digging in` gets a glow on the dot (matching the timeline's current-commit treatment) to give it visual energy.
- `dipping toes` and `gave up` do not glow — they sit calm in the corner.
- Background colour is the status colour at 15-18% alpha (matches existing `.card-tag` style language).

## Markup change per card

Before:

```html
<div class="figuring-card" data-tags="building learning">
  <h4>Go CLI Tools</h4>
  <p class="card-desc">Building small utilities to learn the language</p>
  <div class="progress-bar">
    <div class="progress-fill" style="width: 35%; background: linear-gradient(...);"></div>
  </div>
  <div class="card-tags">
    <span class="card-tag" style="...">Building</span>
    <span class="card-tag" style="...">Learning</span>
  </div>
</div>
```

After:

```html
<div class="figuring-card" data-tags="building learning">
  <span class="status-pill status-digging">digging in</span>
  <h4>Go CLI Tools</h4>
  <p class="card-desc">Building small utilities to learn the language</p>
  <div class="card-tags">
    <span class="card-tag" style="...">Building</span>
    <span class="card-tag" style="...">Learning</span>
  </div>
</div>
```

The `.progress-bar` and `.progress-fill` divs go away entirely. The card-tags row collapses upward to fill the freed space.

## Initial statuses (one per existing card)

| Card                              | Status              |
|-----------------------------------|---------------------|
| Go CLI Tools                      | `digging in`        |
| Designing Data-Intensive Apps     | `dipping toes`      |
| Neovim Config                     | `gave up (for now)` |

These match the previous progress-bar percentages roughly (35% → digging in, 60% → dipping toes — yes, 60% is higher but Kleppmann is a long-haul read so "dipping toes" matches the actual cadence, 15% → gave up).

User can edit per card by changing the `status-*` class.

## CSS that gets removed

In `style.css`:
- `.progress-bar` rule
- `.progress-fill` rule

In `index.html`:
- Each `<div class="progress-bar"><div class="progress-fill" ...></div></div>` is removed (3 occurrences).

## CSS that gets added

In `style.css`, near where `.figuring-card` is defined:
- `.figuring-card { position: relative; }` (or confirm it's already present; if so, no change)
- `.figuring-card h4 { padding-right: 8rem; }` — add the `padding-right` to the existing rule
- `.status-pill` + the three status modifiers as shown above

In `index.html`:
- One `<span class="status-pill status-*">...</span>` as the first child of each `<div class="figuring-card">` (before `h4`).

## Mobile

The pill is absolutely positioned; on mobile (≤768px) the card has `grid-template-columns: 1fr` so it spans the viewport. The corner pill works at any card width. Title `padding-right: 8rem` remains workable on a 320px+ card (title gets 252px+ of width for the text). For very long titles, the title wraps under the pill — same behaviour as desktop.

No mobile-specific overrides needed.

## Out of scope

- Filtering by status. The filter chips at the top still filter by topic tags only.
- Animated state transitions.
- Tooltips explaining each status.
- A "last updated" timestamp.

## Reference

Validated mockup: `.superpowers/brainstorm/90448-1778620639/content/pill-options.html` (option C).
