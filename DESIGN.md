# Neves Labs — Design System

Status: approved, direction "Lab Notebook". Content and structure come from `.scratch/landing-page/spec.md`; this file owns look and motion.

Scope: one static, one-screen `index.html` (plus `favicon.svg`, `apple-touch-icon.png`, `/fonts/*.woff2`) deployed by Vercel. No build step, no framework, no Tailwind, no JS. Everything is plain CSS custom properties in one `<style>` block, and motion is CSS only.

---

## 1. Product and tone

- **What it is**: Neves Labs is a product studio that builds and ships its own apps and tools; it is not a consultancy. The page is its public front door, with one job: say what the lab is and give one way to get in touch (mailto).
- **Tone**: precise, calm, maker-like, with no hype and no sales pitch. It reads like a well-kept lab notebook: plain sentences, exact labels, generous whitespace, and one accent colour used sparingly. The brand should feel like it could sit on the splash screen of a shipped app.
- **Personality cues**: a monospace eyebrow label, a thin accent rule, and a geometric monogram. No stock imagery, no gradients, no glassmorphism, no emoji.
- **Density**: spacious. It is one screen, with no sections below the fold on a 375×667 phone beyond the footer.

## 2. Brand — "Lab Notebook"

The brand keeps the existing teal accent and IBM Plex, so it evolves the current placeholder instead of replacing it.

### 2.1 Logo mark

The mark is a monoline "N" built from three strokes. The two uprights use `currentColor` and the diagonal uses the accent. Read together, the diagonal is the "lab" stroke, echoing the accent rule under the current `h1`.

```html
<svg class="mark" viewBox="0 0 32 32" width="32" height="32" aria-hidden="true" focusable="false">
  <path d="M8 25V7"  stroke="currentColor"       stroke-width="3.5" stroke-linecap="square" fill="none"/>
  <path d="M24 25V7" stroke="currentColor"       stroke-width="3.5" stroke-linecap="square" fill="none"/>
  <path d="M8 7l16 18" stroke="var(--color-accent)" stroke-width="3.5" stroke-linecap="square" fill="none"/>
</svg>
```

Rules:
- Minimum size is 16px (favicon). Use the 32-unit geometry at every size; at 16px the strokes render 1.75px, which stays legible.
- The clear space around it equals the stroke width ×2, about 8px at 32px.
- The mark is always paired with text that names the brand, so it stays `aria-hidden`. The accessible name comes from the wordmark text.

### 2.2 Wordmark

- Lockup: `[mark] Neves Labs`. "Neves" is IBM Plex Sans 600 in `--color-fg` and "Labs" is IBM Plex Mono 500 in `--color-fg-2`. Set at `--text-lg` with `letter-spacing: -0.01em` and a 10px gap between mark and text.
- Markup is real text, not an image: `<a class="brand" href="/"><svg …/><span>Neves</span> <span class="brand-labs">Labs</span></a>`.

### 2.3 Brand device

The accent rule is a 2rem × 3px bar in `--color-accent`. It sits under the `h1` only, once per page, and is never repeated as a decorative divider.

### 2.4 Favicon and touch icon

| File | Spec |
|---|---|
| `favicon.svg` | The §2.1 paths with hard-coded colours: uprights `#161614` and diagonal `#0f766e`. An inner `<style>@media (prefers-color-scheme: dark){…}</style>` swaps them to `#ecece8` and `#2dd4bf`. Use classes on the paths, not `currentColor` or CSS variables (the favicon can't see page CSS). Link: `<link rel="icon" href="/favicon.svg" type="image/svg+xml">`. |
| `apple-touch-icon.png` | 180×180. The mark in light colours (`#161614` and `#0f766e`) on a full-bleed `#f5f5f2` tile, at about 60% of the tile and centred (iOS rounds the corners). Link: `<link rel="apple-touch-icon" href="/apple-touch-icon.png">`. |

### 2.5 Share preview

There is no social image. The share preview is `twitter:card=summary` plus Open Graph text tags (title, description, url, site_name), with values set by the spec. Platforms show the text card without an image.

## 3. Color tokens

All colours are semantic tokens on `:root`, with dark values swapped under `@media (prefers-color-scheme: dark)`. There is no manual theme toggle (YAGNI, and it would need JS). Set `color-scheme: light dark` on `:root`. Raw hex values never appear outside the token block.

| Token | Light | Dark | Use | Contrast (checked) |
|---|---|---|---|---|
| `--color-bg` | `#f5f5f2` | `#0c0c0b` | page | — |
| `--color-surface` | `#ffffff` | `#151513` | future cards | — |
| `--color-fg` | `#161614` | `#ecece8` | headings, body | 16.6 / 16.5 |
| `--color-fg-2` | `#5e5e58` | `#9c9c95` | secondary text, "Labs", labels | 6.0 / 7.1 |
| `--color-border` | `#dcdcd6` | `#2a2a27` | decorative hairlines only | — |
| `--color-border-strong` | `#8a8a83` | `#6b6b65` | reserved for any future UI boundary (3:1 non-text) | 3.2 / ≥3 |
| `--color-accent` | `#0f766e` | `#2dd4bf` | mark diagonal, rule, links, primary button | 5.0 / 10.5 on bg |
| `--color-accent-hover` | `#115e59` | `#5eead4` | primary button hover, link hover | — |
| `--color-on-accent` | `#ffffff` | `#0c0c0b` | text on primary button | 5.5 / 10.5 |
| `--color-accent-subtle` | `color-mix(in oklab, var(--color-accent) 10%, transparent)` | same formula | pressed/hover tint on ghost elements | — |
| `--color-ring` | `var(--color-accent)` | `var(--color-accent)` | focus ring | — |
| `--color-selection` | `color-mix(in oklab, var(--color-accent) 22%, transparent)` | same | `::selection` | — |

Success feedback reuses `--color-accent`, so no separate green is needed.

## 4. Typography

The fonts are **IBM Plex Sans** (400, 600) and **IBM Plex Mono** (400, 500), self-hosted as woff2 in `/fonts/` with Latin subset only. Preload the Sans 400 file, use `font-display: swap`, and add a metric-matched fallback (`@font-face { font-family: "Plex Fallback"; src: local("Arial"); size-adjust: 100%; ascent-override: 102.5%; descent-override: 27.5%; }`, tuned once by eye) so the swap doesn't shift layout.

```
--font-sans: "IBM Plex Sans", "Plex Fallback", system-ui, sans-serif;
--font-mono: "IBM Plex Mono", ui-monospace, "SF Mono", Menlo, monospace;
```

The scale is fluid from 375px to 1280px. Base body text is 17px (16px is the floor).

| Token | Size | Weight / LH / Tracking | Use |
|---|---|---|---|
| `--text-display` | `clamp(2.5rem, 1.9rem + 2.6vw, 4rem)` | 600 / 1.05 / -0.03em | headline `h1` (one per page) |
| `--text-2xl` | `clamp(1.5rem, 1.3rem + 0.8vw, 1.875rem)` | 600 / 1.2 / -0.02em | reserved (future sections) |
| `--text-lg` | `1.1875rem` | 600 / 1.4 / -0.01em | wordmark |
| `--text-base` | `1.0625rem` | 400 / 1.6 / 0 | body, lede (in `--color-fg-2`) |
| `--text-sm` | `0.9375rem` | 400 / 1.5 / 0 | footer |
| `--text-label` | `0.75rem` | Mono 500 / 1.3 / +0.08em, uppercase | eyebrow |

Other rules:
- Keep the measure at 60ch or less for body, 20ch or less for the display headline (so the headline sets in two or three balanced lines), and use `text-wrap: balance` on headings and `text-wrap: pretty` on paragraphs.
- Numerals use `font-variant-numeric: tabular-nums` in labels.

## 5. Spacing, layout, radius, shadow

**Spacing** uses a 4px base and spacious marketing rhythm.

`--space-1: 4px; --space-2: 8px; --space-3: 12px; --space-4: 16px; --space-6: 24px; --space-8: 32px; --space-12: 48px; --space-16: 64px; --space-24: 96px; --space-32: 128px`

- Page gutter is `--space-6` at the base width and `--space-8` from 640px up.
- `main` has a vertical padding of `--space-12` at the base width and `--space-24` from 1024px up.
- Stack gaps are `--space-4` from eyebrow to `h1`, `--space-6` around the rule, and `--space-8` from lede to CTA.

**Container**: `width: min(100% - 2*gutter, 68rem); margin-inline: auto`. Text blocks are capped at 60ch inside it.

**Breakpoints** are mobile-first `min-width` queries only:

| Name | Min width | What changes |
|---|---|---|
| base | 0 (design at 375px) | single column; CTA button full width; footer stacked |
| `sm` | 640px | CTA button auto width; footer in one row; gutter 32px |
| `lg` | 1024px | larger display size reaches its max; `main` gets more top space |

There is never horizontal scroll. Verify at 320px, where no single headline word may overflow at the 40px clamp floor.

**Radius**: `--radius-sm: 6px` for buttons and focus ring, `--radius-md: 10px` for (future) cards, and `--radius-full: 999px` (unused unless needed). Keep corners crisp and modest, with nothing pill-shaped.

**Shadow**: none by default; the flat lab-notebook look relies on hairlines.
- Cards use a 1px `--color-border` border instead of a shadow.
- No element currently uses a shadow.

## 6. Page structure (index.html)

The page is one screen: `body` is a column with `min-height: 100dvh` (with a `100vh` fallback first). Header and footer hug the edges and `main` centres vertically in the remaining space. Content is left-aligned on the container; it is not centred text. Below the minimum height the page simply scrolls.

1. **Header**: brand lockup at the left and nothing else, since there is no nav for a single screen. Height is 64px at the base width. The brand is a link to `/`.
2. **Main**: one hero block, in order:
   - **Eyebrow** (`.label`)
   - **Headline** as the single `h1` in `--text-display`, 20ch max
   - **Accent rule** (`.rule`)
   - **Lede** in `--text-base` `--color-fg-2`, 60ch max
   - **CTA row** with one primary button: `<a class="btn btn--primary" href="mailto:franklin.neves.filho@gmail.com">` with a 16px mail icon. There's no email text under the button.
3. **Footer**: a 1px `--color-border` top hairline, then `© 2026 Neves Labs` in `--color-fg-2` and the email address as a visible `.link` (`mailto:franklin.neves.filho@gmail.com`), both in `--text-sm`. They stack at the base width and sit in one row (copyright left, email right) from `sm` up. There are no social links.

The spec owns the copy (an agent drafts it and Franklin edits it at QA). Don't invent product names, clients, metrics, or claims. A future products list is out of scope; if one ships, it becomes a `.card` row under the lede.

Semantics: `header`, `main`, `footer`, exactly one `h1`, and a skip link ("Skip to content") as the first focusable element, visually hidden until it receives focus.

## 7. Component inventory

These are all plain HTML and CSS classes in `index.html`; there is no component library. Each class owns its pattern, so don't create one-off variants.

| Component | Class | Owns |
|---|---|---|
| Brand lockup | `.brand` | header logo link, mark + wordmark |
| Button | `.btn` + `.btn--primary` | the mailto CTA (the only button) |
| Text link | `.link` | footer email link (accent colour, animated underline) |
| Eyebrow label | `.label` | mono uppercase label |
| Accent rule | `.rule` | the single bar under the `h1` |
| Card | `.card` | reserved for a future products row; not on the page now |
| Skip link | `.skip-link` | a11y |

**Button spec**:
- Size: min-height 48px, padding `0 var(--space-6)`, `--radius-sm`, `--text-base` 600, and an inline-flex row with gap `--space-2` (room for an optional 16px Lucide icon, which is inline SVG with `aria-hidden`).
- **Primary**: `--color-accent` background, `--color-on-accent` text. Full width at the base breakpoint, auto width from `sm` up.

**Card spec** (future use): `--color-surface` background, 1px `--color-border`, `--radius-md`, padding `--space-6` (`--space-8` from `lg` up). Cards are not interactive, so they have no hover state; don't fake affordance.

**Touch targets**: everything interactive is at least 44×44px. The footer email link gets `padding-block: 10px` (or a `::after` hit-area extension) to reach 44px. Adjacent targets are 8px or more apart.

## 8. States

The page is static, with no fetching, no forms, and no JS, so there is nothing to put in a skeleton, an empty state, or a loading spinner. The states that do exist are below.

| Surface | State | Design |
|---|---|---|
| Page load | loading | Static HTML paints in the first frame. The metric-matched fallback font prevents layout shift on swap (CLS target 0). The entrance animation is decorative; the default styles are the final state, so content is readable even if CSS animations never run. |
| Mailto CTA | success | Native navigation opens the mail client, so there are no app-side states. |
| Mailto CTA | error | No mail client is configured. The footer prints the email as visible text, which is the fallback for copying by hand. |
| Route | not found | Vercel's default 404; no custom page is in scope. |
| Social share | preview | A text-only `summary` card per §2.5. |

## 9. Micro-interactions and motion (CSS only)

Animate only `transform` and `opacity`, plus `color` and `background-color` on hover/press, which are cheap and non-layout. Never animate `width`, `height`, or `top`. Exits are faster than entrances.

```
--ease-out:    cubic-bezier(0.22, 1, 0.36, 1);   /* enters, hovers */
--ease-in:     cubic-bezier(0.4, 0, 1, 1);       /* exits */
--dur-press:   100ms;
--dur-hover:   160ms;
--dur-exit:    160ms;
--dur-enter:   240ms;
--dur-reveal:  480ms;
```

| Element | Hover (pointer devices only, `@media (hover: hover)`) | Press `:active` | Focus-visible |
|---|---|---|---|
| `.btn--primary` | bg → `--color-accent-hover`, `--dur-hover` | `transform: scale(.98)`, `--dur-press` | ring (below) |
| `.link` (footer email) | underline grows via `::after` `transform: scaleX(0→1)` from left with `--dur-enter` `--ease-out`, shrinking back with `--dur-exit` | colour → `--color-accent-hover` | ring |
| `.brand` | mark diagonal: `stroke-dashoffset` draw (opacity fallback), 320ms, played once per hover | none | ring |

**Focus ring**: `outline: 2px solid var(--color-ring); outline-offset: 3px; border-radius: var(--radius-sm)` on `:focus-visible` only. Never remove outlines without this replacement.

**Transitions between states**:
- **Entrance**: eyebrow, `h1`, rule, lede, and CTA fade up (`opacity 0→1`, `translateY(8px→0)`) with `--dur-reveal` `--ease-out`, staggered 60ms via `animation-delay` (`nth-child`). The rule animates `scaleX(0→1)` from the left at 300ms delay. This plays once on load only. There are no scroll-triggered reveals, since it is one screen.

**Reduced motion**: `@media (prefers-reduced-motion: reduce)` sets every animation and transition duration to 0.01ms, removes `translateY`/`scale`, and keeps colour changes. Content renders in its final state.

## 10. Accessibility checklist

- Text contrast is at least 4.5:1 and UI boundaries at least 3:1. The tokens above are verified, so new colours must be verified too.
- Page meta: `lang="en"`, title, description, canonical, OG and Twitter `summary` tags (exact values are in the spec), and `theme-color` (`<meta name="theme-color" content="#f5f5f2" media="(prefers-color-scheme: light)">`, and `#0c0c0b` for dark).
- Remove `<meta name="robots" content="noindex">` at launch.
- Keyboard order follows: skip link → brand → CTA → footer email.
- Don't disable zoom; the viewport meta stays `width=device-width, initial-scale=1`.

## 11. Known drift (current placeholder vs this system)

- IBM Plex Sans is declared but never loaded, so every visitor currently sees `system-ui`. Self-host it per §4.
- There is no logo mark, favicon, meta description, canonical, or OG/Twitter tags.
- `body { min-height: 100vh; display: grid; place-items: center }` is a centred placeholder layout. Replace it with the §6 header/main/footer column, using `min-height: 100dvh` only if still needed.
- The placeholder is centred text; the system is left-aligned on a container with header and footer.
- Token names are `--color-fg-2`/`--color-accent` and are kept. Add the missing tokens (`surface`, `border`, `border-strong`, `accent-hover`, `on-accent`, `ring`).
- Light bg `#f4f4f2` becomes `#f5f5f2`, a negligible change that aligns with the contrast checks.
- `noindex` is present (intentional for the placeholder, but remove it at launch).
- There is no focus style and no reduced-motion block.

