# Iteration 10 — Outcome

## index.json re-scan

No structural changes since last iteration. Shopify editor accepted the iteration-9 sections as confirmed by the system notification.

---

## What was done

### sections/birdlabs-community-gallery.liquid — CREATED

New BirdLabs gallery section for the community page, based on existing `main-community.liquid` with the following additions:

**9-quadrant header alignment:**
- Two settings: `header_h_align` (left/center/right) and `header_v_align` (top/middle/bottom)
- When `header_height > 0`, the header div becomes a flex column container — `justify-content` controls vertical position, `align-items` controls horizontal
- When `header_height == 0` (default), layout is standard flow (top-aligned, width 100%)
- 9 combinations possible: any of 3 horizontal × 3 vertical

**Heading area:** badge label (pill), heading (h1), description text — all inherit alignment

**Gallery grid:**
- 2 cols mobile → 3 cols tablet (≥640px) → configurable 3–5 cols desktop (≥1024px)
- Square aspect-ratio images
- Hover: subtle scale on image + caption/author overlay fades in from bottom
- Click → lightbox modal (full-screen, dark overlay, close via button/click-outside/Escape)
- Lightbox scoped to section ID — safe for multiple instances on a page

**Schema name:** `"BirdLabs — Grower Gallery"` (25 chars ✓)

---

### sections/birdlabs-stats.liquid — CREATED

Stats callout section with bordered rounded-corner cards.

**Section-level:**
- Optional heading + description
- Header alignment: left/center/right
- Configurable heading color, description color

**Grid:** 1 col mobile → 2 cols ≥480px → 2/3/4 cols desktop (configurable)

**Stat card** (block type `stat`, max 12):
- `value` text — the large stat number/string (e.g. "50+")
- `value_size` range — font size 2–6rem per block
- `value_color` color — per block
- `label` text — descriptor below the value (e.g. "Years in Use")
- `label_size` range — 1–2rem per block
- `label_color` color — per block
- Cards have a configurable border (width + color), rounded corners, padding, background
- Subtle hover lift effect (translateY -2px + shadow)

**Schema name:** `"BirdLabs — Stats"` (16 chars ✓)

---

### templates/page.community.json — UPDATED

Replaced `main_community` in the page order with two new sections:
1. `birdlabs_stats_community` — Stats section, pre-populated with 3 default stats (50+, 70+, 10k+)
2. `birdlabs_community_gallery_main` — Gallery section, pre-populated with 4 placeholder image blocks

The original `main_community` section entry is retained in the sections object but removed from the page order — it can be re-enabled via the theme editor if needed.

---

## Decisions made

- **9-quadrant via two settings** (h + v separately) rather than a 9-option single select — simpler to use and understand in the editor
- **Header height setting** gates vertical alignment — without a fixed height, "top/middle/bottom" vertical would have no visible effect; the editor info text explains this
- **Lightbox scoped to `section.id`** — avoids JS conflicts if the section is added multiple times on a page or alongside `main-community.liquid` (which uses global IDs)
- **Stats value/label sizes are per-block** (not global) — allows mixed emphasis (e.g., one stat can have a larger number than others)
- **`main_community` kept in page.community.json sections object** (but not in `order`) — preserves the original data in case the user wants to compare or restore

## Known issues / follow-up

- The `label_size` range on stat blocks has only 1–2rem range (step 1). If finer control is needed, a text input or larger range could be added.
- The community gallery does not currently support video blocks — could be extended in a future iteration.
- `page.community.json` was fully rewritten (not edited) due to the structural change. Shopify editor may reformat it on next save.
