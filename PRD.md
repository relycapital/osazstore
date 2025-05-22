---

# 🛍️ Shopify PRD — UX + Functionality Fix Instructions for Osaz Store

**Storefront**: [https://byosaz.com](https://byosaz.com)
**Platform**: Shopify (Online Store 2.0)
**Theme ID**: 11 (custom theme)
**Agent Role**: DevOps/Frontend Agent
**Asset Base**: CDN-hosted

---

## 🔧 1. Header/Footer Navigation

### ✅ Tasks:

* Fix **Header and Footer navigation links** using Shopify **Navigation API** or Admin > Online Store > Navigation

  * Update or validate `main-menu` and `footer-menu` handles.
  * API Ref: [Shopify Admin REST - Navigation](https://shopify.dev/docs/api/admin-rest/2023-01/resources/menu)

### ✅ Liquid Snippet (for rendering links):

```liquid
<ul>
  {% for link in linklists.main-menu.links %}
    <li><a href="{{ link.url }}">{{ link.title }}</a></li>
  {% endfor %}
</ul>
```

---

## 🖼️ 2. Logo & Cart Icon

### ✅ Tasks:

* Replace existing logo with:

  ```
  https://byosaz.com/cdn/shop/t/11/assets/osaz.png?v=151089513622157250711746219666
  ```

* Confirm or update `<img>` tag in `header.liquid` or theme settings schema:

```liquid
<img src="{{ 'osaz.png' | asset_url }}" alt="Osaz Logo" />
```

*or hardcode the CDN path temporarily:*

```html
<img src="https://byosaz.com/cdn/shop/t/11/assets/osaz.png?v=151089513622157250711746219666" alt="Osaz Logo" />
```

### 🛠 Cart Badge (Liquid):

```liquid
<a href="/cart">
  <span class="cart-icon">🛒</span>
  <span class="cart-count">{{ cart.item_count }}</span>
</a>
```

> ✅ **API note**: Logo replacement **must be done via theme file update**, not API.

---

## 🌎 3. Country Selector

* ✅ Remove `localization-form.liquid` block if no international markets configured.
* To check via API:
  `GET /admin/api/2023-01/shop.json`
  → Check `shop.currency`, `shop.primary_locale`, and any enabled [Shopify Markets](https://shopify.dev/docs/api/admin-rest/2023-01/resources/market)

---

## 🎨 4. Design & Image Handling

### ✅ Load All Images via Shopify Files/CDN:

* Upload via Admin → **Settings > Files**
* Or reference existing ones using:

```liquid
<img src="{{ 'filename.jpg' | file_url }}" />
```

### ✅ API Alternative:

* Upload files using [GraphQL Admin API - fileCreate](https://shopify.dev/docs/api/admin-graphql/2023-01/mutations/fileCreate)

---

## 🏠 5. Home Page Fixes

### ✅ Tasks:

* Fix scrolling text: align text + add inline icons.
* Activate popup (Klaviyo or built-in newsletter modal).
* Fix broken "Shop All" link → `/collections/all`
* Add brand value images/text.
* Fix reviews slider (Loox, Judge.me, etc).

### 💡 Review Section Integration:

If using Loox or Judge.me:

* Embed via their app block or use this:

```liquid
{% include 'judgeme_widgets', widget_type: 'judgeme_review_widget', concierge_install: true %}
```

---

## 🦶 6. Footer Fixes

* Ensure "Don’t miss out" section appears only once (global snippet preferred).
* Replace payment icons via **Theme > Footer** block or custom HTML:

```html
<img src="https://cdn.shopify.com/s/files/1/0000/0001/files/visa.svg" alt="Visa">
```

---

## 🟡 7. Product Page Enhancements

### ✅ Fixes:

* Load `product.images` via loop:

```liquid
{% for image in product.images %}
  <img src="{{ image | img_url: 'large' }}" alt="{{ image.alt | escape }}">
{% endfor %}
```

* Separate `description` vs `highlights`:

  * Use **metafields** for highlights.
  * Example:

    ```liquid
    {{ product.metafields.custom.product_highlights }}
    ```

* Add content to Science block or hide if empty:

```liquid
{% if product.metafields.custom.science %}
  <div class="science-block">{{ product.metafields.custom.science }}</div>
{% endif %}
```

* Fix duplicate "Shop All" link and review slider as noted above.

---

## 📄 8. Static Pages

### ✅ Tasks:

* Fix Blog link → `/blogs/news`
* Add **About** and **Contact** pages via:

  * Admin > Online Store > Pages > Add
  * Assign templates via:

    * About → `page.about`
    * Contact → `page.contact` or use built-in form:

      ```liquid
      {% form 'contact' %}
        ...
      {% endform %}
      ```

---

## 🔍 Developer Reference Sheet

| Feature             | Implementation Method                 | API Support                       |
| ------------------- | ------------------------------------- | --------------------------------- |
| Navigation Links    | Shopify Navigation settings or Liquid | ✅                                 |
| Logo/Icon Update    | Theme editor or `header.liquid`       | ❌ (No API for theme logo setting) |
| Cart Icon + Count   | `cart.item_count`                     | ✅                                 |
| Newsletter Modal    | Klaviyo, Theme Popup                  | ✅ (via app scripts)               |
| Metafields          | Admin > Metafields UI or API          | ✅                                 |
| File/Image Uploads  | Admin UI or `fileCreate` API          | ✅                                 |
| Page Creation       | Admin UI or `POST /pages.json`        | ✅                                 |
| Reviews Integration | Loox, Judge.me, App Embed             | ⚠️ (Varies by app)                |

---

## ✅ Final Deliverables Checklist

| Item                                        | Status |
| ------------------------------------------- | ------ |
| Logo loads from `cdn.shopify.com/...`       | ⬜      |
| Navigation links working (header/footer)    | ⬜      |
| Product image and science block fixed       | ⬜      |
| Newsletter popup live                       | ⬜      |
| Static pages (About, Contact, Blog) present | ⬜      |
| Design aligned, icons added                 | ⬜      |

---
