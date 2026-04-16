# Iteration 11 — Outcome

## index.json re-scan
No changes to homepage index.json. Focused work on page.birdlabs-about.json.

---

## Layout mapping (about.tsx → Shopify sections)

| about.tsx section | Shopify section used |
|---|---|
| Badge + h1 "Our Story" + intro paragraph | `main-about` (existing) — hero + intro |
| 2-col image + mission text | `main-about` (existing) — mission_image + mission_body |
| 4 icon cards (Leaf/Award/Heart/Users) | `main-about` (existing) — `stat` blocks |
| "Sourcing & Quality" 4 image-cards w/ overlay | `birdlabs-feature-cards` (NEW) |
| CTA banner "Join Our Wellness Community" | `birdlabs-content-block` (existing) + new `show_card_box` option |

---

## What was done

### sections/birdlabs-feature-cards.liquid — CREATED

New section for image-topped cards with gradient overlay and icon badge, matching the "Sourcing & Quality Standards" block in about.tsx.

**Card structure:**
- Top: fixed-height image area (configurable, default 192px = `h-48`)
- Image: dark gradient overlay (`from-[#0a0a0a]` style matching Hydrogen source)
- Icon badge: absolute positioned bottom-left, green rounded square, rotates 12° on hover
- Hover: `scale(1.04)`, border color shifts to `rgba(124,179,66,0.35)`, green shadow
- Image scales `1.1` on hover (via `transform` on `img`)
- Body: heading + description

**8 icon options** via `select` setting: sprout, beaker, shield, tree, leaf, award, heart, users — all rendered as inline SVGs (no external dependency)

**Schema name:** `"BirdLabs — Feature Cards"` (24 chars ✓)

Pre-populated preset matches the 4 cards from about.tsx.

---

### sections/birdlabs-content-block.liquid — MODIFIED

Added `show_card_box` feature — non-breaking addition:
- `show_card_box` checkbox (default: false — existing instances unaffected)
- When enabled: wraps inner content in `.cb-card-box` div with configurable background, border color, corner radius (default 24px), and inner padding (default 48px)
- CSS: `box-shadow: 0 20px 50px rgba(0,0,0,0.1)` to match the `shadow-xl` in about.tsx CTA

4 new schema settings added: `show_card_box`, `card_box_bg`, `card_box_border`, `card_box_radius`, `card_box_padding`.

---

### templates/page.birdlabs-about.json — UPDATED

Full page rebuilt with 3 sections:

1. **`main_about`** — existing `main-about.liquid`, updated stat blocks to match about.tsx content (Pure Sourcing, Quality Tested, Science-Backed, Community First). `values_heading` cleared (not in about.tsx).

2. **`birdlabs_feature_cards_about`** — new feature cards section with 4 blocks (Pristine Sources/Beaker/Shield/Tree) matching about.tsx sourcing cards. Images are placeholders — add via theme editor.

3. **`birdlabs_about_cta`** — `birdlabs-content-block` with `show_card_box: true`, heading "Join Our Wellness Community", 2 buttons: Shop Products (green) + Contact Us (light gray). Section bg: `#f5f5f0` (matches about.tsx gradient start).

---

## What was NOT modified

- `sections/main-about.liquid` — untouched, existing about page template is unaffected
- `templates/page.about.json` — untouched (the original about page remains working)
- All other existing sections — untouched

## Known issues / follow-up

- Feature cards currently have no images set in the template (image picker fields are empty). The dark placeholder renders until images are added via the theme editor.
- The about.tsx uses a local file `/20260105_163329.jpg` for the mission image — this file would need to be uploaded to Shopify and set via `main-about`'s `mission_image` picker.
- The `birdlabs-feature-cards.liquid` uses two Shopify CDN URLs from about.tsx (`Pristine_Sources_Thumbnail.jpg`, `Bioavailable_forms.jpg`) as reference — these should be set in the image_picker fields for blocks fc_1 and fc_3.
