# Iteration 8 — Outcome

## What was done

### sections/birdlabs-videoreels.liquid — CREATED

New BirdLabs section implementing the Hydrogen `VideoReelsIframe.tsx` design in Shopify Liquid.

**Layout:**
- 2-column desktop grid: 9:16 portrait video player (left) + heading/description/thumbnail grid (right)
- Mobile: single column, player stacked above thumbnails
- Decorative blur blobs (pink + yellow) matching Hydrogen source

**Player:**
- YouTube: `<iframe>` with `?autoplay=1&rel=0`; rendered by JS on load and on thumbnail click
- Shopify-hosted video: `<video controls playsinline autoplay muted>` with all Shopify sources
- Player HTML written dynamically via JS (`innerHTML` swap) to force autoplay on video switch

**Video data pipeline:**
- Per-block, Liquid computes `vid_type` (youtube/shopify) and `vid_id` by string-splitting the URL
- Supports: `youtube.com/watch?v=`, `youtu.be/`, `youtube.com/embed/`, `youtube.com/shorts/`
- All video data serialized into `<script type="application/json">` for JS consumption
- YouTube thumbnails auto-generated: `https://img.youtube.com/vi/{ID}/hqdefault.jpg`

**Thumbnails:**
- 3-column portrait grid, wraps automatically as more blocks are added
- Active thumbnail: `vr-neon-pulse` yellow glow animation (matching Hydrogen CSS)
- Hover overlay: play icon
- Info overlay: title + platform label

**Schema blocks** (`video_reel`, max 12):
- `video_url` — Shopify `video_url` type accepting YouTube URLs
- `shopify_video` — Shopify `video` type (used when no YouTube URL set)
- `custom_thumbnail` — optional image_picker override
- `title` — display text

**Section settings:** heading, description, show_view_more toggle, button label + URL, bg_color, padding top/bottom

**Preset:** Pre-populated with the 4 videos from the Hydrogen source (same YouTube IDs)

---

### templates/index.json — UPDATED

Added `birdlabs_videoreels_main` section entry with 4 pre-configured video blocks (the Hydrogen source videos). Placed after the existing `video_reels` section in page order.

---

### .claude/CLAUDE.md — UPDATED

Expanded the Iteration Workflow section to document the questions-before-execution process:
- When Claude should ask vs. proceed
- That human answers go in the same `iteration-N-claude-questions.md` file
- How to signal Claude to re-read and proceed

---

### .claude/iterations/iteration-8-claude-questions.md — UPDATED

Cleared original questions, replaced with resolution summary documenting which approach was chosen and why.

---

## Decisions made

- **URL-based entry** (Shopify `video_url` input type) rather than auto-fetch — matches how the Shopify native video widget works; no API key needed
- **Liquid string splitting** for YouTube video ID extraction (regex not available in Liquid)
- **JSON data island** pattern for JS — serializes all video metadata at render time so switching videos never requires a page reload or API call
- **`innerHTML` swap** for player — necessary to force autoplay when switching videos (simply changing `src` attribute does not trigger autoplay on most browsers)
- **`neon-pulse` animation** scoped to section ID to avoid conflicts with multiple instances

## Known issues / follow-up

- YouTube autoplay may be blocked on first page load if the user has not interacted with the page (browser autoplay policy). The first video will be rendered in the player but may not autoplay until the user clicks something. This is a browser constraint, not a bug.
- The existing `video_reels` section (type: `video-reels`) remains on the page — the human can hide or remove it via the theme editor.
- `video_url` type in Shopify only accepts YouTube and Vimeo in the editor UI, but only YouTube is accepted (`"accept": ["youtube"]`). Vimeo support can be added by updating the accept array and adding Vimeo ID extraction logic.
