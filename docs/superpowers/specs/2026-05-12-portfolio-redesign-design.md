# Portfolio Redesign — Design Spec

## Goal

Rework the visual design and content of sandjiv.com from a generic dark template into a distinctive personal site with personality. Keep the casual/funny tone. Apply a cohesive aesthetic rooted in Catppuccin Mocha, mikado kumiko patterns, and the Inter font.

## Visual Direction

### Color Palette — Catppuccin Mocha

All colors come from the Catppuccin Mocha palette:

- **Backgrounds:** Base `#1e1e2e`, Mantle `#181825`, Crust `#11111b`
- **Surfaces:** Surface 0 `#313244`, Surface 1 `#45475a`, Surface 2 `#585b70`
- **Text:** Text `#cdd6f4`, Subtext 1 `#bac2de`, Subtext 0 `#a6adc8`
- **Primary accents:** Mauve `#cba6f7`, Pink `#f5c2e7` (used as gradient pair)
- **Secondary accents:** Sapphire `#74c7ec` (links), Green `#a6e3a1` (current/active states), plus full Catppuccin accent palette for card tags
- **Nav brand:** Lavender `#b4befe`

### Typography

- **Font:** Inter (loaded from Google Fonts), fallback to system sans-serif
- **Monospace (code blocks on setup page):** Fira Code

### Background Pattern — Mikado Kumiko

A subtle SVG mikado kumiko pattern (isometric tumbling blocks / hexagons divided into 3 rhombuses) applied to `body` with `background-attachment: fixed`. This creates one continuous, seamless geometric layer behind the entire page.

- Stroke color: Surface 1 `#45475a` at 0.5px, opacity 0.35
- Tile size: 60x34.64px (flat-top hexagon, radius 20)
- The pattern is a nod to the user's interest in fountain pens and Japanese woodcraft

### Layout Philosophy

Content floats as semi-transparent glass cards on top of the kumiko background:
- Card background: `rgba(24, 24, 37, 0.85)` with `backdrop-filter: blur(8px)`
- Card border: 1px solid Surface 0
- Card border-radius: 12px
- Cards have horizontal margin (not full-width) so the kumiko pattern is visible on both sides

The hero section is fully transparent (no card) so the pattern shows through behind the name. A subtle radial gradient glow (mauve, 10% opacity, blurred) sits behind the hero text.

### Navbar

- Sticky, semi-transparent with backdrop blur
- Brand name in Lavender, nav links in Subtext 1
- Links: Home, Setup
- Same navbar on both pages

## Home Page Sections

### 1. Hero

- Full-width, transparent background (kumiko visible)
- Large title: "Hi, I'm **Sandjiv**" with mauve-to-pink gradient on the name
- Annotation in italic overlay color: "(an apparently rare name given that i got that domain name)"
- Subtitle: role description
- Short description with the existing humor

### 2. About Me

- Centered glass card (max-width ~640px)
- Section heading with mauve-to-pink gradient underline bar (40px wide, 3px)
- Existing copy preserved (boring tech link, Claude Overlords joke)
- Links in Sapphire

### 3. Experience

- Glass card
- Section heading with gradient underline
- Horizontal timeline: dots connected by a line
- Each item: year (above dot), role title, company name (below dot)
- Dot color: Mauve for past roles, Green for current role with matching glow
- Timeline line: Surface 1
- Content is placeholder — user will fill in actual roles

### 4. Currently Figuring Out

- Glass card
- Section heading with gradient underline
- **Tag filter row** at top: pill-shaped buttons (All, Reading, Building, Learning, Tools). Active filter gets mauve background/border. Clicking a tag filters the cards below to show only matching items.
- **Card grid** below filters: `grid-template-columns: repeat(auto-fill, minmax(240px, 1fr))`
- Each card contains:
  - Title (bold, Text color)
  - One-line description (Subtext 0)
  - Progress bar (4px, Surface 0 track, gradient fill using different Catppuccin accent pairs per card)
  - Tag pills (small, colored background at 15% opacity with matching text)
- Cards have subtle hover: translateY(-2px), border brightens, shadow deepens
- Content is placeholder — user will fill in actual items

### 5. Contact

- Not a glass card — just a slim transparent strip
- One-liner quip centered ("or don't, i'm not your dad")
- Row of icon buttons (email, GitHub, LinkedIn) — small rounded squares with Surface 0 background, Subtext 1 icons
- Existing links preserved: sandjiv.parassou@gmail.com, github.com/Yaquelqun, linkedin.com/in/sandjiv

### 6. Footer

- Thin strip, solid Mantle background with Surface 0 top border
- Existing quip preserved

## Setup Page

- Same navbar and footer
- Same kumiko background, same glass-card treatment
- Restyle the existing content (dotfiles, editor, shell, tmux, tool version manager) into the new card-based layout
- Sidebar navigation restyled to match Catppuccin palette
- Code blocks use Fira Code, styled with Surface 0/Mantle backgrounds
- Content unchanged — just visual refresh

## Technical Approach

- Pure HTML + CSS (no build tools, no framework) — matching the existing stack
- Single `style.css` shared between both pages
- Inter loaded via Google Fonts `<link>` tag
- Mikado kumiko SVG inlined as CSS `background-image` data URI
- Tag filtering in "Currently Figuring Out" uses vanilla JavaScript (show/hide cards based on `data-tag` attributes)
- Responsive: cards stack on mobile, sidebar hides on small screens (existing behavior preserved)

## What's NOT Changing

- Static HTML site — no frameworks, no build step
- Two-page structure (index.html + setup.html)
- The casual/self-deprecating tone in all copy
- Contact links (email, GitHub, LinkedIn)
- Setup page content (dotfiles, editor, shell, tmux, mise)

## Open Items (to fill during implementation)

- Actual professional experience entries (company, role, year)
- Actual "currently figuring out" cards (title, description, progress, tags)
- Whether the hero description/subtitle text should be updated
