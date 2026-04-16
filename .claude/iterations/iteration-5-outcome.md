# Iteration 5 — Outcome

## What Was Done

### 1. Re-scanned sections/birdlabs-hero.liquid
- User had revised the file significantly: cleaner grid layout, section-scoped padding via range sliders, square image container with `aspect-ratio: 1/1`, glow layer, and animation moved entirely into the `<style>` block
- The class `birdlabs-hero__img` was renamed back to `fb-hero__img`

### 2. Diagnosis: why top/bottom margin was excessive

Two compounding causes from `assets/brand.css`:

**Root cause — `min-height: 100vh` on `.fb-hero`:**
The global `.fb-hero` class sets `min-height: 100vh`, forcing the section to fill the full viewport height regardless of content or any padding overrides. This is the primary reason the section appears to have massive top/bottom space — it isn't margin or padding, it's the section being stretched to 100% of the screen height.

**Secondary cause — padding slider defaults were 64px:**
The section schema had `padding_top` and `padding_bottom` defaulting to 64px each, compounding the visual space even after `min-height` is addressed.

### 3. Fix applied to sections/birdlabs-hero.liquid

- Added `min-height: unset` to `#fb-hero-{{ section.id }}` CSS rule — this overrides the brand.css `min-height: 100vh` scoped to this section only, without touching global styles
- Changed `padding_top` and `padding_bottom` schema defaults from `64` to `10` (step changed from 4 to 2 for finer control)

> Note: Existing placed instances of this section that already have `padding_top`/`padding_bottom` values stored in JSON will retain those values. The defaults only apply to newly added instances. If the live page still shows excess padding, the user should manually adjust the sliders in the theme editor to `10`.

### 4. Body text callout border

Added to `.fb-hero__body` in the section `<style>` block:
- `border: 1px solid rgba(0,0,0,0.12)` — subtle, slightly darker border
- `border-radius: 0.75rem` — rounded corners
- `padding: 0.875rem 1rem` — internal breathing room
- `background: rgba(0,0,0,0.04)` — very slight darkening of background to visually separate the body text block

## Known Issues / Follow-up

- Existing `index.json` references `custom_hero` (the non-birdlabs version). If the user wants the birdlabs-hero to be live on the homepage, `index.json` needs to be updated to reference `birdlabs-hero`
- The `min-height: 100vh` in `brand.css` `.fb-hero` remains and will affect any other section using that class — intentional for now since it may be desired on full-screen hero variants
