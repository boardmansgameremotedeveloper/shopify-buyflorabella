# Iteration 6 — Outcome

## What Was Done

### sections/birdlabs-multicolumn.liquid (new)

A new BirdLabs custom section built from scratch (not a direct copy of multicolumn.liquid — the Dawn original uses Dawn CSS dependencies that don't apply here).

#### Structure
- **Section header:** centered heading + description above the grid
- **Grid:** CSS grid with configurable desktop columns (1–6) and mobile columns (1–2)
- **Each card:** featured image → icon badge overlaid bottom-left of image → heading → description
- **Background layer:** optional bg image with a separate color overlay div (opacity-controlled) sitting behind all content
- **Max blocks:** 6 (Shopify limit matches the 6-area requirement)

#### Schema name
`"BirdLabs — Feature Grid"` — 23 chars ✓

#### All CMS-configurable settings
Section level:
- Heading, description, heading color, description color
- Background color, background image, overlay color, overlay opacity (0–90%)
- Card background, card corner radius, image ratio (square/landscape/portrait/widescreen)
- Card heading color, card description color
- Columns desktop (1–6), columns mobile (1–2), gap between cards
- Top/bottom padding

Block level (per card):
- Featured image
- Icon image (overlaid on image, bottom-left badge)
- Icon badge background color
- Heading
- Description

#### Design decisions
- CSS is fully self-contained in the `<style>` block — no Dawn dependencies
- Background image + overlay uses a 3-layer stack: bg-image div → overlay div → content div (all `position: absolute/relative` with z-index)
- Icon badge uses `position: absolute; bottom: 0.75rem; left: 0.75rem` — avoids covering the center of the image
- Card hover: `translateY(-4px)` + box-shadow, image scales `1.05x`
- Grid uses section-scoped ID (`#birdlabs-mc-grid-{{ section.id }}`) for column count so multiple instances on a page don't interfere

## Known Issues / Follow-up
- Dawn's original `multicolumn.liquid` remains unchanged — this is a parallel BirdLabs version
- The `image_ratio` setting uses CSS `aspect-ratio` values directly (e.g. "4 / 3") — if Shopify theme editor shows these values as raw strings that's fine; they render correctly in CSS
