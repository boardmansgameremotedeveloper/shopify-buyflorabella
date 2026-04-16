# Iteration 8 — Claude Questions

## Status: RESOLVED — questions superseded by human clarification

The original questions about YouTube API access, auto-fetch approach, etc. were resolved
when the human clarified the implementation spec:

> "Mimic the functionality in the native Shopify 'video' widget — instead of reading from any
> YouTube account directly, it embeds the video from a YouTube video URL. Alternatively,
> keep the functionality to select a video from Shopify-hosted video."

### Resolution summary

| Question | Resolution |
|----------|------------|
| Q1 — Auto-fetch vs. manual blocks | Manual URL entry per block (no API key needed) |
| Q2 — Channel ID / API key | Not needed — URL-based approach |
| Q3 — Video count | Configurable via blocks (max 12) |
| Q4 — Grid columns | 3 columns, wraps as more blocks are added |
| Q5 — Mobile layout | Player full-width stacked above thumbnails |
| Q6 — View more button | Included, configurable label + URL |

### Implementation approach

Each block has:
- `video_url` — Shopify `video_url` type (accepts YouTube URLs); Claude parses video ID from the URL using Liquid string splits
- `shopify_video` — Shopify `video` type (native hosted video picker); used when no YouTube URL is set
- `custom_thumbnail` — optional image_picker override
- `title` — display title

Thumbnails are auto-generated from `https://img.youtube.com/vi/{VIDEO_ID}/hqdefault.jpg` for YouTube blocks.

Execution proceeded immediately after clarification. See `iteration-8-outcome.md` for results.
