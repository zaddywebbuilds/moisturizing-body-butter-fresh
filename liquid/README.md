# Fresh Body Butter, Shopify port

Shopify OS 2.0 version of the landing page. Everything is editable from the theme
customizer, and nothing is hardcoded to one product.

## Files

| File | Goes in your theme at |
|---|---|
| `assets/pfb-body-butter.css` | `assets/pfb-body-butter.css` |
| `sections/pfb-body-butter.liquid` | `sections/pfb-body-butter.liquid` |
| `templates/product.fresh-body-butter.json` | `templates/product.fresh-body-butter.json` |

## Install

1. Shopify admin, **Online Store > Themes > ... > Edit code**.
2. Create the three files above and paste the contents in.
3. **Products > Fresh Body Butter > Theme template**, choose `fresh-body-butter`, save.
4. Open the product in the customizer and pick your images (see below).

## Images

The section uses image pickers with a fallback chain, so it renders immediately
even before you pick anything. Each picker falls back to a product image:

| Setting | Falls back to | Suggested file |
|---|---|---|
| Hero image | featured image | open jar with monstera and water |
| Problem image | product image 2 | leg application |
| Texture image | product image 3 | whipped macro |
| Scent image | product image 4 | lemon and silk |
| Band image | product image 5 | three jars, golden hour |
| Closing image | featured image | jar on pedestal with swirl |
| Popup image | featured image | jar with lemon |
| Benefit blocks | none, blank if unset | arm, scoop, two jars, jar with wheat |

Upload the files from `../images/` (use the `.webp` versions, they are 83% smaller)
via **Content > Files**, then select them in the customizer.

## What is wired up

- **Real add to cart.** One `{% form 'product' %}` in the hero. Every other CTA on
  the page submits it through the HTML `form` attribute, so there is a single
  source of truth for variant and quantity.
- **Variants are read from the product**, not hardcoded. Sold-out variants are
  disabled automatically and the button switches to "Sold out".
- **Prices use `money`** and the shop's own `money_format`, so currency and
  decimal conventions follow the store. Totals are computed client-side with a
  `formatMoney` helper that mirrors Shopify's.
- **Free shipping meter** reads the threshold from a setting (default 35).
- **Email popup posts to `{% form 'customer' %}`**, creating a real customer
  tagged `newsletter, fresh-body-butter`. This is the thing the GitHub Pages
  version could not do. On success Shopify redirects back and the popup reopens
  on the confirmation state.
- **FAQ structured data** is generated from the FAQ blocks, so the schema can
  never drift from the visible text. Turn it off if another app already outputs
  FAQ schema.
- **Analytics** push to `window.dataLayer`: `select_size`, `change_quantity`,
  `add_to_cart` (GA4 item shape), `popup_shown`, `newsletter_signup`, `faq_open`.

## Notes

- **CSS is scoped under `.pfb`.** No selector can reach theme markup, so this will
  not restyle your header, footer or any other section.
- **No product JSON-LD is output.** Most themes already emit it, and two copies
  causes a duplicate-structured-data warning. If your theme does not emit it, say
  so and it can be added behind a setting.
- The header and footer come from your theme. The standalone version's own header
  is not ported, so your site navigation stays consistent.
- The scent section bleeds to the viewport edge using `100vw`. If your theme wraps
  sections in a padded container, it will stop at that container instead. Harmless,
  just slightly less dramatic.
- The section can be added to any product. It is not specific to Fresh Body Butter.

## Still open

- **Ingredients section.** The product description has no ingredient list, so one
  was not invented. Add the real list and it can be built.
- Review blocks are static text. If you add a reviews app (Judge.me, Loox), its
  block should replace the hardcoded ones so ratings stay in sync.
