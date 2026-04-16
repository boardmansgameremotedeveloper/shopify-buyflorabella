# Iteration 4 — Outcome

## What Was Done

### 1. CLAUDE.md — Outcome document rule
- Added requirement that every iteration must produce an `iteration-N-outcome.md` file
- Documented required contents: file summary, decisions, known issues

### 2. CLAUDE.md — Schema name 25-char limit
- Added explicit rule: all schema `"name"` and preset `"name"` values must be 25 characters or fewer (Shopify constraint)

### 3. sections/birdlabs-hero.liquid (new)
- Source: `sections/custom-hero.liquid`
- Schema name: `"BirdLabs — Hero"` (15 chars ✓)
- Added `enable_image_animation` checkbox setting (default: false)
- When enabled: `@keyframes birdlabs-hero-breathe` animates the hero image from `scale(1)` → `scale(1.06)` → `scale(1)` on a 4s ease-in-out infinite loop
- When disabled: existing `fb-parallax-float` (gentle vertical float) is preserved
- Animation scoped via section ID to avoid conflicts with multiple instances
- Image gets class `birdlabs-hero__img` for clean CSS targeting

### 4. header-group.json — Re-read (no action needed)
- Confirmed the theme editor has already switched to `birdlabs-announcement-bar` and `birdlabs-header`
- Both BirdLabs sections are active; Dawn originals (`announcement-bar`, `header`) are no longer in the header group

### 5. Hero image animation — live site scrape
- Attempted to fetch https://buyflorabella.com — animation CSS is in external files not accessible via HTML scrape
- Implemented a clean pulse/breathe animation as the fallback (task 5 spec): image scales up slightly and back, looping

## Decisions

- Animation uses `scale(1.06)` — noticeable but not distracting; can be adjusted
- Animation is off by default so it doesn't affect existing pages without opt-in
- Used section-scoped CSS (`#fb-hero-{{ section.id }}`) so multiple hero instances on a page don't conflict

## Known Issues / Follow-up

- The live site animation at buyflorabella.com could not be confirmed — if the effect differs significantly from the breathe pulse, a future iteration can refine it once the CSS is inspectable
- `sections/custom-hero.liquid` still exists as the Dawn-era original — can be deprecated in a future iteration once `birdlabs-hero` is confirmed live
