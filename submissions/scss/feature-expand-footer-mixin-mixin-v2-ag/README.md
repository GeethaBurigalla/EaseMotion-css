# Expand Footer Mixin (`feature-expand-footer-mixin-mixin-v2-ag`)

Standalone **SCSS track** submission for [Issue #54765](https://github.com/SAPTARSHI-coder/EaseMotion-css/issues/54765).

A beginner-friendly **expand footer** hover effect: the footer sits collapsed at a fixed height and smoothly expands to reveal its full content on hover or keyboard focus.

> **Naming note:** the issue's suggested folder (`feature-expand-footer-mixin-mixin-ag`) was already used by an earlier submission for a duplicate issue (#375). This submission uses the `-v2-ag` suffix instead, to avoid a collision while keeping the same naming convention.

---

## What it does

- Collapses a footer to a small preview height (`$collapsed-height`) by default
- Smoothly expands to its full content on `:hover` **and** `:focus-within`, so keyboard users tabbing into a footer link trigger the same reveal
- Uses `max-height` (not `height`) so it works without knowing the exact content height ahead of time
- Optional `ease-expand-footer-surface-v2-ag` helper for quick footer chrome (background, spacing, focus ring)
- Automatically disables the animated transition under `prefers-reduced-motion: reduce`

---

## Folder structure
submissions/scss/feature-expand-footer-mixin-mixin-v2-ag/
├── _expand-footer-mixin.scss ← mixin
└── README.md ← this file


---

## Installation

Copy `_expand-footer-mixin.scss` into your SCSS folder, then load it:

```scss
@use 'expand-footer-mixin' as *;
// or: @import 'expand-footer-mixin';
```

---

## Parameters — `ease-expand-footer-mixin-mixin-v2-ag`

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `$duration` | Time | `0.4s` | Length of the expand/collapse transition |
| `$easing` | String | `ease` | Timing function |
| `$collapsed-height` | Length | `3.5rem` | Visible height while collapsed |
| `$expanded-max-height` | Length | `40rem` | Upper bound for the expanded state |

---

## Usage

### Basic (matches issue example shape)

```scss
@use 'expand-footer-mixin' as *;

.site-footer {
  @include ease-expand-footer-mixin-mixin-v2-ag(0.4s);
}
```

### Taller footer, slower reveal

```scss
.site-footer--rich {
  @include ease-expand-footer-mixin-mixin-v2-ag(
    $duration: 0.6s,
    $collapsed-height: 4rem,
    $expanded-max-height: 55rem
  );
}
```

### Surface helper (styles + motion)

```scss
.site-footer {
  @include ease-expand-footer-surface-v2-ag;
}
```

### HTML example

```html
<footer class="site-footer" tabindex="0">
  <p>&copy; 2026 MyCompany. All rights reserved.</p>
  <nav aria-label="Footer links">
    <a href="/about">About</a>
    <a href="/privacy">Privacy</a>
    <a href="/contact">Contact</a>
  </nav>
</footer>
```

Adding `tabindex="0"` to the footer itself lets keyboard users focus the region directly and trigger the expand via `:focus-within`, even before reaching a link inside it.

---

## Why this is useful

Footers often bury secondary links (legal, sitemap, social) to keep the page visually light. An expand-on-interaction footer keeps that content out of the way by default but makes it fully reachable — for both mouse and keyboard users — without needing JavaScript.

---

## Accessibility

- Expansion triggers on both `:hover` and `:focus-within`, so keyboard-only users get the same reveal as mouse users
- Transition is removed entirely under `prefers-reduced-motion: reduce` — the footer still expands/collapses, just instantly
- `max-height` avoids layout thrash from unknown content height
- The surface helper includes a visible `:focus-visible` ring on footer links

---

## Unique identifier

All mixin names and helper mixins use the **`-v2-ag`** suffix to avoid collisions with parallel submissions (per contribution policy), given the originally suggested `-ag` slug was already taken.

---

## Checklist (issue acceptance)

- [x] Files live under `submissions/scss/feature-expand-footer-mixin-mixin-v2-ag/`
- [x] Required `_expand-footer-mixin.scss` + `README.md`
- [x] Unique identifier on folder / mixins (adjusted to `-v2-ag` due to a naming collision — flagged above)
- [x] Smooth Expand effect
- [x] `prefers-reduced-motion` handled
- [x] README explains what / how / why

