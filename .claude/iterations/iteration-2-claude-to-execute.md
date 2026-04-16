# Iteration #2 — Claude To Execute
## Hydrogen Decomposition → Shopify Liquid Plan

> Status: READY — awaiting `go`

---

## A. System Overview — Hydrogen App Architecture

The Hydrogen frontend is a **React Router v7 / Shopify Hydrogen** app. The authoritative UI lives in:

```
app/componentsMockup2/
  components/   ← production UI components
  pages/        ← page-level compositions
  contexts/     ← React state (cart, env, wishlist, flags, toast)
  data/         ← staticData.ts (fallback product/review data)
```

The older `componentsMockup/` is a deprecated prototype. The `app/components/` folder contains Hydrogen SDK primitives (ProductForm, CartMain, Header skeleton, etc.) — these are infrastructure, not the design system.

### Design System (Brand Identity)
- **Primary green:** `#7cb342`
- **Dark sections:** `#0a0a0a`, `#2d1b4e`
- **Off-white sections:** near-white gradient (CSS class `gradient-offwhite`)
- **Accent colors:** `#d4ff00` (neon), `#ff1493` (pink), `#8b6f47` (earth)
- **Typography:** `heading-font` (bold custom serif), body in gray scale
- **Motion:** Floating leaf animations, parallax, scale-on-hover, glassmorphism cards
- **Logo:** centered in header, `/flora_bella_logo.png`

---

## B. Page Mapping — Hydrogen Routes → Shopify Templates

| Hydrogen Route | Page Type | Shopify Template | Status |
|---|---|---|---|
| `_index.tsx` | Homepage | `templates/index.json` | EXISTS — needs resection |
| `product.$slug.tsx` | Product detail | `templates/product.json` | EXISTS — needs resection |
| `collections/$handle.tsx` | Collection | `templates/collection.json` | EXISTS — needs resection |
| `collections.all.tsx` | All collections | `templates/list-collections.json` | EXISTS |
| `blogs.$handle._index.tsx` | Blog listing | `templates/blog.json` | EXISTS |
| `blogs.$handle.$article.tsx` | Blog article | `templates/article.json` | EXISTS |
| `search.tsx` | Search results | `templates/search.json` | EXISTS |
| `cart.tsx` | Cart | `templates/cart.json` | EXISTS |
| `contact.tsx` | Contact | `templates/page.contact.json` | EXISTS |
| `about.tsx` | About brand story | NEW: `templates/page.about.json` | **NEEDS CREATION** |
| `faq.tsx` | FAQ page | NEW: `templates/page.faq.json` | **NEEDS CREATION** |
| `learn.tsx` | Education hub | Maps to `templates/blog.json` | EXISTS (blog) |
| `learn.$handle.tsx` | Article | Maps to `templates/article.json` | EXISTS |
| `community.tsx` | Community gallery | NEW: `templates/page.community.json` | **NEEDS CREATION** |
| `technical-docs.tsx` | Tech docs | NEW: `templates/page.technical-docs.json` | **NEEDS CREATION** |
| `shop.tsx` | Shop redirect | Maps to `templates/collection.json` | EXISTS |
| `account.*` | Customer account | `templates/customers/*.json` | EXISTS |
| `policies.$handle.tsx` | Policy pages | Maps to Shopify built-in policies | EXISTS (Shopify handles) |

---

## C. Component Mapping — Hydrogen → Shopify Sections/Snippets

### Global Layout

| Hydrogen Component | Role | Shopify Equivalent | Action |
|---|---|---|---|
| `AnnouncementBar` | Fixed top promo bar, coupon code, green gradient | `sections/announcement-bar.liquid` | **MODIFY** — restyle to match design |
| `Header` | Dark glassmorphism, centered logo, nav links, search, cart, account | `sections/header.liquid` | **MAJOR RESTYLE** — new dark theme, layout |
| `Footer` | Dark 4-col: brand, links, support, email signup | `sections/footer.liquid` | **MAJOR RESTYLE** + Omnisend embed |

### Homepage Sections (index.json)

| Hydrogen Component | Role | Shopify Section | Action |
|---|---|---|---|
| `HeroSection` | Full-height hero, "Wake Up Your Soil", product image, CTAs, trust bullets | NEW: `sections/custom-hero.liquid` | **CREATE** |
| `ReassuranceStrip` | Dark purple, 3 icons: shipping / payment / support | NEW: `sections/reassurance-strip.liquid` | **CREATE** |
| `BenefitsGrid` | Dark bg, 6-card benefit grid with Shopify CDN images | NEW: `sections/benefits-grid.liquid` | **CREATE** |
| `HowItWorks` | Off-white, 3-step image+text: Microbes / Humic / Fulvic | NEW: `sections/how-it-works.liquid` | **CREATE** |
| `MineralComposition` | Periodic table cards, 6 key minerals, GuaranteedAnalysis | NEW: `sections/mineral-composition.liquid` | **CREATE** |
| `SocialProof` | Black bg, 3 hardcoded review cards | NEW: `sections/social-proof.liquid` | **CREATE** |
| `EducationSection` | Dark gradient, 3 blog post cards from Shopify blog | NEW: `sections/education-preview.liquid` | **CREATE** |
| `VideoReelsIframe` | Embedded video section | NEW: `sections/video-reels.liquid` | **CREATE** (use existing `video.liquid` base) |
| `CommunityCallout` | Off-white, email signup + WhatsApp CTA | NEW: `sections/community-callout.liquid` | **CREATE** |
| `FeatureProduct` | Dynamic product card with buy button | NEW: `sections/featured-product-hero.liquid` | **CREATE** (replaces current `featured-product`) |

### Product Page Sections (product.json)

| Hydrogen Component | Role | Shopify Section | Action |
|---|---|---|---|
| `ProductDetailPage` tab system | Tabbed PDP: Overview / Ingredients / How-to / Technical / FAQ | Modify `sections/main-product.liquid` | **EXTEND** — add tab accordion blocks |
| `DiscountBox` | Discount tier display | NEW: `snippets/discount-box.liquid` | **CREATE** |
| Review display | Star ratings, review cards | Use existing rating + custom snippet | **CREATE** `snippets/review-card.liquid` |

### Custom Page Sections

| Custom Page | Components Used | Shopify Approach |
|---|---|---|
| About (`/about`) | Off-white, mission/story, 4-stat grid, team | `sections/main-about.liquid` + `templates/page.about.json` |
| FAQ (`/faq`) | Accordion FAQ list | `sections/main-faq.liquid` or extend `collapsible-content.liquid` |
| Community (`/community`) | Image gallery grid, lightbox | `sections/main-community.liquid` + `templates/page.community.json` |
| Technical Docs (`/technical-docs`) | Structured content with GuaranteedAnalysis table | `sections/main-technical-docs.liquid` + `templates/page.technical-docs.json` |

### Reusable Snippets

| Hydrogen Pattern | Shopify Snippet | Action |
|---|---|---|
| Benefit card (icon + title + desc + image) | `snippets/benefit-card.liquid` | **CREATE** |
| How-it-works step card | `snippets/how-it-works-step.liquid` | **CREATE** |
| Mineral element card (periodic table style) | `snippets/mineral-card.liquid` | **CREATE** |
| Review card | `snippets/review-card.liquid` | **CREATE** |
| Education article card | `snippets/education-card.liquid` | **CREATE** |
| Guaranteed analysis table | `snippets/guaranteed-analysis.liquid` | **CREATE** |

---

## D. Section Build Plan

### Sections to Create (Homepage priority)

#### 1. `sections/custom-hero.liquid`
- **Purpose:** Full-height hero section — brand headline, product image, CTA buttons, trust bullets
- **Content:** Hardcoded headline "Wake Up Your Soil", editable subtitle, two CTA buttons (Shop / Community), 3 trust checkmarks
- **Schema blocks:** heading, subheading, button_primary, button_secondary, trust_items, hero_image
- **Used in:** `templates/index.json`

#### 2. `sections/reassurance-strip.liquid`
- **Purpose:** 3-icon trust strip below hero
- **Content:** Shipping / Secure Checkout / Grower Support — icon + text
- **Schema:** 3 editable blocks (icon choice, label text), background color setting
- **Used in:** `templates/index.json`

#### 3. `sections/benefits-grid.liquid`
- **Purpose:** 6-card product benefits grid
- **Content:** Compost-Friendly / Ancient Minerals / Born in USA / Iron-ore Derived / Biology Included / Lab Verified
- **Schema:** Up to 6 repeatable blocks each with: title, description, image (Shopify file picker)
- **Used in:** `templates/index.json`

#### 4. `sections/how-it-works.liquid`
- **Purpose:** 3-step science explanation with alternating image/text layout
- **Content:** Beneficial Microbes / Humic Acid / Fulvic Acid — each with step number, title, description, bullet list, image
- **Schema:** Up to 4 repeatable step blocks
- **Used in:** `templates/index.json`

#### 5. `sections/mineral-composition.liquid`
- **Purpose:** Periodic table card grid + guaranteed analysis table
- **Content:** 6 mineral cards (Mg, Fe, Zn, Cu, Mn, B) + full mineral analysis block
- **Schema:** Section heading, 6 repeatable mineral blocks (symbol, name, atomic number, benefit)
- **Used in:** `templates/index.json`

#### 6. `sections/social-proof.liquid`
- **Purpose:** Customer review cards (static)
- **Content:** 3 reviews with quote, name, location, star rating, avatar initials
- **Schema:** Up to 6 review blocks (quote, name, location, rating)
- **Used in:** `templates/index.json`

#### 7. `sections/education-preview.liquid`
- **Purpose:** Preview 3 blog articles from a specified blog
- **Content:** Article image, title, description, read time, link to `/blogs/` article
- **Implementation:** Liquid `for` loop over `blogs['blog-handle'].articles` — first 3 articles
- **Schema:** Blog handle input, heading text
- **Used in:** `templates/index.json`

#### 8. `sections/video-reels.liquid`
- **Purpose:** Embedded video section
- **Content:** Video via iframe or native `<video>` element
- **Schema:** Video URL (YouTube/Vimeo embed or Shopify hosted), heading, subheading
- **Used in:** `templates/index.json`

#### 9. `sections/community-callout.liquid`
- **Purpose:** Email newsletter signup + WhatsApp CTA
- **Content:** Heading, body text, Omnisend embedded form div, WhatsApp link button
- **Schema:** Heading, body text, omnisend_form_id, whatsapp_url, show_whatsapp toggle
- **Used in:** `templates/index.json`

#### 10. `sections/featured-product-hero.liquid`
- **Purpose:** Dynamic featured product card with buy button
- **Content:** Pulls a configurable product, shows image, price, variant selector, add-to-cart
- **Schema:** Product picker, show_rating toggle, custom badge text
- **Used in:** `templates/index.json`

### Sections to Create (Custom Pages)

#### 11. `sections/main-about.liquid`
- **Purpose:** Full About page: hero, mission statement, stat grid, team section
- **Schema:** Section blocks for each content area
- **Used in:** `templates/page.about.json`

#### 12. `sections/main-faq.liquid`
- **Purpose:** FAQ accordion page — extend existing `collapsible-content` pattern
- **Schema:** Question/answer blocks
- **Used in:** `templates/page.faq.json`

#### 13. `sections/main-community.liquid`
- **Purpose:** Community gallery — image grid, lightbox modal
- **Schema:** Up to 20 image blocks with caption/author
- **Used in:** `templates/page.community.json`

#### 14. `sections/main-technical-docs.liquid`
- **Purpose:** Application guide / Guaranteed Analysis table / soil testing content
- **Schema:** Structured content blocks + guaranteed analysis table
- **Used in:** `templates/page.technical-docs.json`

### Sections to Restyle (Existing Dawn)

| Section | Changes Required |
|---|---|
| `sections/announcement-bar.liquid` | Restyle to green gradient, pulsing text, coupon display |
| `sections/header.liquid` | Dark glassmorphism, centered logo, new nav links (Learn, About, Community), scrolled-state |
| `sections/footer.liquid` | Dark theme, 4-column layout, Omnisend div embed, social icons, policy links |

### New Templates to Create

| Template | Sections |
|---|---|
| `templates/page.about.json` | `main-about` |
| `templates/page.faq.json` | `main-faq` |
| `templates/page.community.json` | `main-community` |
| `templates/page.technical-docs.json` | `main-technical-docs` |

---

## E. Gaps & Constraints

### Cannot Replicate Directly in Liquid

| Hydrogen Feature | Constraint | Resolution |
|---|---|---|
| React Context (CartContext, EnvContext, FeatureFlagsContext) | No runtime state management | Use Shopify's built-in cart, theme settings for config |
| Omnisend `window.omnisend.push(...)` SDK calls | Requires JS injection | Add via `{% javascript %}` block in section or in `theme.liquid` |
| WhatsApp Widget (floating) | React component | Add as raw `<script>` embed or HTML widget in `theme.liquid` |
| Predictive search with fetcher | React Router fetcher | Dawn already has `sections/predictive-search.liquid` — reuse |
| Abandoned cart popup | React state + timer | Not reproducible in Liquid; use Shopify app (Klaviyo/Omnisend) |
| Survey popup | React component | Not reproducible in Liquid; use app or remove |
| Feature flags (FeatureFlagsContext) | Runtime JSON config | Use Shopify theme settings as flag toggles |
| Subscription purchase type (one-time vs. recurring) | Requires Recharge/Bold app | Not in scope without 3rd party app |
| `storeLocked` env flag with custom locked header | Runtime env var | Shopify has native password page — use `layout/password.liquid` |

### Simplifications Required

| Hydrogen Pattern | Shopify Simplification |
|---|---|
| Floating leaf animations (`floating`, `floating-delayed` CSS classes) | Port CSS classes to `assets/base.css` or a new `assets/brand.css` |
| Glassmorphism cards (`glass`, `glass-strong` classes) | Port to `assets/brand.css` |
| `gradient-green`, `gradient-dark`, `gradient-offwhite` CSS helpers | Port to `assets/brand.css` |
| Lucide React icons | Replace with inline SVG or Dawn's icon system in snippets |
| Tailwind CSS | Dawn uses vanilla CSS — must translate Tailwind utility classes to custom CSS |

### Requires Minimal JavaScript

| Interactive Feature | JS Approach |
|---|---|
| Header scroll behavior (glassmorphism on scroll) | ~20-line vanilla JS in section `{% javascript %}` |
| Mobile menu open/close | Vanilla JS toggle — Dawn pattern already exists |
| Search dropdown | Reuse Dawn's `sections/predictive-search.liquid` |
| Tab system on product page | Vanilla JS `{% javascript %}` block |
| Lightbox on community gallery | Vanilla JS `{% javascript %}` block |

---

## F. Execution Plan

### Phase 1 — Brand Foundation (do first)

1. **Create `assets/brand.css`** — port all custom CSS classes:
   - `gradient-green`, `gradient-dark`, `gradient-offwhite`
   - `glass`, `glass-strong`
   - `floating`, `floating-delayed`, `floating-slow`, `floating-x`, `floating-diagonal` (keyframe animations)
   - `heading-font`, `parallax-float`
   - `shiny-border`
   - Add `brand.css` to `layout/theme.liquid` stylesheet includes

2. **Restyle `sections/announcement-bar.liquid`**
   - Green gradient background
   - Pulsing animated text
   - Schema settings: `enabled`, `message`, `coupon_code`

3. **Restyle `sections/header.liquid`**
   - Dark semi-transparent background with backdrop-filter blur
   - Centered logo image (configurable via file_picker)
   - Navigation links: Shop, Learn, About, Community, Contact
   - Scroll-triggered class change via `{% javascript %}` block
   - Keep existing cart icon bubble + predictive search

4. **Restyle `sections/footer.liquid`**
   - Dark background (`#0a0a0a`)
   - 4 columns: Brand desc, Quick Links, Help & Support, Stay Connected
   - Omnisend embedded form div (ID configurable in schema)
   - Social icons (Instagram, Facebook, YouTube)
   - Policy links row
   - Copyright line

### Phase 2 — Homepage Sections (build in order)

5. Create `sections/custom-hero.liquid`
6. Create `sections/reassurance-strip.liquid`
7. Create `sections/benefits-grid.liquid`
8. Create `sections/how-it-works.liquid`
9. Create `sections/mineral-composition.liquid`
10. Create `sections/social-proof.liquid`
11. Create `sections/education-preview.liquid`
12. Create `sections/video-reels.liquid`
13. Create `sections/community-callout.liquid`
14. Create `sections/featured-product-hero.liquid`

15. **Update `templates/index.json`** — wire new sections in order:
    ```
    custom-hero → reassurance-strip → featured-product-hero →
    benefits-grid → how-it-works → mineral-composition →
    social-proof → education-preview → video-reels → community-callout
    ```

### Phase 3 — Custom Pages

16. Create `sections/main-about.liquid` + `templates/page.about.json`
17. Create `sections/main-faq.liquid` + `templates/page.faq.json`
18. Create `sections/main-community.liquid` + `templates/page.community.json`
19. Create `sections/main-technical-docs.liquid` + `templates/page.technical-docs.json`

### Phase 4 — Reusable Snippets

20. Create `snippets/benefit-card.liquid`
21. Create `snippets/how-it-works-step.liquid`
22. Create `snippets/mineral-card.liquid`
23. Create `snippets/review-card.liquid`
24. Create `snippets/education-card.liquid`
25. Create `snippets/guaranteed-analysis.liquid`
26. Create `snippets/discount-box.liquid`

### Phase 5 — Product Page Enhancement

27. Extend `sections/main-product.liquid` with tab accordion blocks (Overview / How-to / FAQ)
28. Wire `snippets/review-card.liquid` into product page
29. Wire `snippets/discount-box.liquid` into product page

---

## Implementation Notes

- All new sections must include a `{% schema %}` block with `name`, `settings`, and `blocks`
- All CSS goes into `assets/brand.css` (new file) — never modify `assets/base.css` directly
- JavaScript goes into section `{% javascript %}` blocks (scoped) unless truly global
- Images referenced in section schemas use Shopify's `image_picker` type
- Blog article previews use: `{% for article in blogs['learn'].articles limit:3 %}`
- Navigation links in header should be configurable via `header-group.json` menu settings
- Tailwind classes must be translated to explicit CSS — no Tailwind CDN

---

## Success Criteria for This Iteration

- [ ] `assets/brand.css` created with all ported CSS utilities
- [ ] All 3 global sections restyled (announcement bar, header, footer)
- [ ] All 10 homepage sections created
- [ ] `templates/index.json` updated with new section order
- [ ] 4 new custom page templates + sections created
- [ ] All 7 reusable snippets created
- [ ] Product page tab system extended
- [ ] No React/Hydrogen imports anywhere in Liquid files
- [ ] All sections deployable via Shopify CLI
