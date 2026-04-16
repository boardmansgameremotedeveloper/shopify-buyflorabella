# Iteration #1 Outcome — Repository Understanding

## Status: COMPLETE

---

## Cache Manifest

Written to: `.claude/cache/manifest.json`
- Total tracked files: ~351 (all non-.git files)
- All files share mtime `1776350186` (single commit) except `.claude/CLAUDE.md` (mtime `1776355777`)

---

## File Classification

### layout/ (2 files)
| File | Role |
|------|------|
| `layout/theme.liquid` | Primary layout wrapper — wraps every storefront page. Entry point for head, header, footer, cart-drawer, JS/CSS. |
| `layout/password.liquid` | Alternate layout for the coming-soon/password page |

### templates/ (14 files)
| File | Page Type |
|------|-----------|
| `templates/index.json` | Homepage |
| `templates/product.json` | Product detail page |
| `templates/collection.json` | Collection listing |
| `templates/list-collections.json` | All collections page |
| `templates/cart.json` | Cart page |
| `templates/blog.json` | Blog listing |
| `templates/article.json` | Single blog post |
| `templates/search.json` | Search results |
| `templates/page.json` | Generic page |
| `templates/page.contact.json` | Contact page (alternate page template) |
| `templates/404.json` | 404 error page |
| `templates/password.json` | Password/coming soon |
| `templates/gift_card.liquid` | Gift card (Liquid, not JSON) |
| `templates/customers/*.json` | 7 customer account pages (account, login, register, addresses, order, activate, reset_password) |

### sections/ (54 files)
**Global/Layout sections (rendered via section groups):**
- `header.liquid` / `header-group.json` — site header with nav, search, cart
- `footer.liquid` / `footer-group.json` — site footer
- `announcement-bar.liquid` — top announcement strip

**Cart/Checkout helpers:**
- `cart-drawer.liquid`, `cart-icon-bubble.liquid`, `cart-live-region-text.liquid`
- `cart-notification-button.liquid`, `cart-notification-product.liquid`
- `main-cart-items.liquid`, `main-cart-footer.liquid`

**Homepage/Marketing sections:**
- `image-banner.liquid` — hero banner with image + CTA buttons
- `featured-collection.liquid` — product grid pull
- `featured-product.liquid` — single product highlight
- `featured-blog.liquid` — blog post preview
- `slideshow.liquid` — image carousel
- `collage.liquid` — mixed media collage
- `multicolumn.liquid` — column layout (icons/text)
- `multirow.liquid` — multi-row content
- `rich-text.liquid` — text blocks
- `video.liquid` — video embed section
- `image-with-text.liquid` — side-by-side image + text
- `newsletter.liquid` — email signup
- `email-signup-banner.liquid` — full-width email banner
- `collection-list.liquid` — collection cards grid
- `contact-form.liquid` — contact form
- `custom-liquid.liquid` — freeform Liquid injection

**Product page sections:**
- `main-product.liquid` — full product detail (media, info, buy box)
- `related-products.liquid` — "You may also like"
- `pickup-availability.liquid` — store pickup info
- `collapsible-content.liquid` — accordion FAQ/details

**Collection/Search page sections:**
- `main-collection-banner.liquid` — collection hero
- `main-collection-product-grid.liquid` — product grid + filters
- `main-search.liquid` — search results grid
- `main-list-collections.liquid` — all collections grid

**Blog sections:**
- `main-blog.liquid`, `main-article.liquid`

**Customer/Account sections:**
- `main-account.liquid`, `main-login.liquid`, `main-register.liquid`
- `main-addresses.liquid`, `main-order.liquid`
- `main-activate-account.liquid`, `main-reset-password.liquid`

**Utility sections:**
- `main-404.liquid`, `main-page.liquid`, `page.liquid`
- `apps.liquid` — Shopify app embed block target
- `predictive-search.liquid` — live search dropdown
- `quick-order-list.liquid`, `bulk-quick-order-list.liquid`
- `main-password-header.liquid`, `main-password-footer.liquid`

### snippets/ (37 files)
**Product display:**
- `card-product.liquid` — product card (used in grids)
- `price.liquid`, `unit-price.liquid` — price display
- `product-media.liquid`, `product-media-gallery.liquid`, `product-media-modal.liquid`, `product-thumbnail.liquid`
- `product-variant-picker.liquid`, `product-variant-options.liquid`
- `buy-buttons.liquid`
- `swatch.liquid`, `swatch-input.liquid`

**Navigation/Header:**
- `header-drawer.liquid` — mobile drawer nav
- `header-dropdown-menu.liquid` — desktop dropdown
- `header-mega-menu.liquid` — mega menu
- `header-search.liquid` — search bar

**Cart:**
- `cart-drawer.liquid`, `cart-notification.liquid`

**Collection/Search:**
- `facets.liquid`, `price-facet.liquid`
- `card-collection.liquid`
- `pagination.liquid`

**Content/Utility:**
- `article-card.liquid`
- `icon-accordion.liquid`, `icon-with-text.liquid`
- `social-icons.liquid`, `share-button.liquid`
- `loading-spinner.liquid`
- `meta-tags.liquid` — SEO head tags
- `country-localization.liquid`, `language-localization.liquid`
- `quantity-input.liquid`, `progress-bar.liquid`
- `gift-card-recipient-form.liquid`
- `quick-order-list.liquid`, `quick-order-list-row.liquid`, `quick-order-product-row.liquid`

### assets/ (185 files)
- **CSS** — `base.css` (global), `component-*.css` (per-component), `section-*.css` (per-section), `template-*.css`
- **JS** — `global.js`, `pubsub.js`, `constants.js`, feature JS files per section/component
- **SVG icons** — 60+ icon SVGs (account, cart, social, product features, etc.)
- **Other** — `sparkle.gif`, `mask-arch.svg`, `mask-blobs.css`

### config/ (2 files)
- `settings_schema.json` — Theme customizer schema (defines all editor settings)
- `settings_data.json` — Active saved values for those settings

### locales/ (51 files)
- `en.default.json` — Primary English strings
- `en.default.schema.json` — English schema strings (editor labels)
- 24 additional language translations + schema variants

---

## Page Composition Model

```
Request → template/*.json
            └── Defines ordered list of section IDs + settings
                    └── sections/*.liquid  (primary content)
                            └── {% render 'snippet-name' %}  (reusable fragments)

All pages wrapped by:
  layout/theme.liquid
    ├── <head> meta-tags snippet, CSS assets
    ├── sections/header (via header-group.json)
    ├── sections/announcement-bar
    ├── snippets/cart-drawer
    ├── {{ content_for_layout }}  ← template sections render here
    └── sections/footer (via footer-group.json)
```

### Key Entry Points
| File | Role |
|------|------|
| `layout/theme.liquid` | Root layout shell for all storefront pages |
| `templates/index.json` | Homepage — image-banner + featured-collection |
| `templates/product.json` | PDP — main-product + related-products |
| `templates/collection.json` | Collection — banner + product grid |
| `sections/header.liquid` | Global nav, search, cart icon |
| `sections/main-product.liquid` | Core product UI with media, buy box, variants |
| `snippets/card-product.liquid` | Product card used across all grids |

---

## Ready for Iteration #2

Claude is ready to:
- Create new `.liquid` sections
- Modify JSON templates
- Add custom page templates
- Extend snippets
- Apply design from `.claude/design.md` once provided
