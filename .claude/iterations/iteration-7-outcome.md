# Iteration 7 — Outcome

## index.json re-scan
No changes from last iteration. `birdlabs_hero` remains first in order; all other sections unchanged.

## CLAUDE.md update
Added "Per-Iteration Checklist" rule requiring `templates/index.json` to be re-scanned at the start of every iteration to catch Shopify theme editor auto-updates.

## sections/birdlabs-multirow.liquid (new)
Identical copy of `sections/multirow.liquid`. Only change: schema `"name"` updated from `"t:sections.multirow.name"` to `"BirdLabs — Multi Row"` (20 chars ✓), and preset name updated to match. All Dawn CSS dependencies (`component-image-with-text.css`) retained as-is per the "identical copy" requirement.

## sections/birdlabs-minerals.liquid (new)

A new BirdLabs section with 5 distinct content areas, all CMS-configurable.

### Block types (3)

| Type | Name | Purpose |
|---|---|---|
| `periodic_item` | "Periodic element" | Area 1 — periodic table strip |
| `feature_card` | "Feature card" | Area 2 — chemistry feature cards |
| `analysis_row` | "Analysis row" | Area 3 — guaranteed analysis table |

`max_blocks: 50` — allows ~6 periodic + ~6 feature + ~15+ analysis rows comfortably.

### Area 1: Periodic table strip
- Flexbox row, wraps on mobile
- Each card: atomic number (top-right, small), large element symbol (green accent), element name (small, gray)
- Per-card accent color configurable
- Hover: scale + lift + green glow

### Area 2: Feature cards
- CSS grid, configurable desktop (1–6) and mobile (1–2) columns, section-scoped ID
- White rounded cards with drop shadow
- Inside: small rounded symbol box (green tint border), element name heading, benefit text
- Hover: lift + deeper shadow

### Area 3: Guaranteed Analysis
- Uses HTML `<details>/<summary>` — no JS required, semantically correct, keyboard accessible
- Custom CSS hides default browser marker; down-arrow SVG rotates 180° when open
- Table: symbol (green, left) | name | percentage (right-aligned)
- `<hr>` divider below table, then descriptive text

### Area 4: Additional descriptive text
- `richtext` setting — supports bold, links, lists in CMS
- Color configurable

### Area 5: Element symbol pills
- Single `textarea` setting with comma-separated symbols (e.g. `Mg,Fe,Zn,Cu,Mn,B`)
- Liquid `split: ','` + `strip` handles whitespace around commas
- `max_blocks: 50` is irrelevant here — any number of symbols work since it's a free-text field
- Pills: white rounded box, drop shadow, hover lift effect

### Design decisions
- `<details>/<summary>` chosen over JS toggle — avoids extra JavaScript, works without page load, and is natively accessible
- Element symbol pills use a textarea CSV rather than individual blocks because 10+ symbols would require 10+ blocks which is awkward in Shopify's CMS sidebar
- Periodic strip and feature cards are independent block types so they can have different element lists (e.g., show 6 periodic cards but only 3 detailed feature cards)

## Known issues / Follow-up
- `birdlabs-multirow.liquid` references Dawn translation keys (e.g. `t:sections.multirow.settings.header.content`) — these will resolve correctly since they're inherited from the theme's locale files
- The `analysis_row` block `percentage` field is free text (e.g. "0.5%") — could be validated in a future iteration if needed
