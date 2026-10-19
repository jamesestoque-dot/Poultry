# CluckCart Poultry

A complete, responsive poultry e-commerce website built with **HTML5, CSS3 and vanilla JavaScript**.
Customers can browse chickens grouped by age, read full product details, fill a shopping cart and
place a **sample** order. A separate admin dashboard lets the farm manager add, edit and delete
products, update stock, and review sample orders.

Everything is stored in the browser with **LocalStorage**, so product edits and sample orders
survive a page refresh.

---

## 1. Project structure

```
poultry-ecommerce/
├── index.html              # Home page: hero, categories, featured products, about, contact
├── shop.html               # Product listing: age filters, search, sorting
├── product-details.html    # Full details of one chicken (?id=...)
├── cart.html               # Shopping cart with quantity controls and totals
├── checkout.html           # Validated customer form + sample order confirmation
├── admin.html              # Admin dashboard (demo login, stats, CRUD, orders)
├── css/
│   ├── style.css           # Design tokens, layout, components, forms, footer
│   ├── responsive.css      # Tablet (1024px), mobile (768px) and small (480px) rules
│   └── admin.css           # Login screen, dashboard tables, modal, order cards
├── js/
│   ├── products.js         # Data layer: catalogue, LocalStorage, cart, orders, utils
│   ├── ui.js               # Shared UI: header/footer, toasts, cart badge, product cards
│   ├── main.js             # Page bootstrap + home page logic
│   ├── shop.js             # Shop filters, search and sorting
│   ├── product-details.js  # Product details page and quantity selector
│   ├── cart.js             # Cart rendering and quantity updates
│   ├── checkout.js         # Form validation and sample order saving
│   └── admin.js            # Dashboard: login, statistics, product CRUD, orders
├── images/                 # Logo, hero scene, product illustrations, placeholder
└── README.md
```

---

## 2. How to run it

The project is 100% static - no build step, no package manager, no internet connection required.

### Option A - open it directly
1. Open the project folder in **VS Code**.
2. Right-click `index.html` -> **Open with Live Server** (recommended), or simply double-click
   `index.html` to open it in your browser.

### Option B - with the VS Code Live Server extension
1. Install the **Live Server** extension (by Ritwick Dey).
2. Right-click `index.html` -> **Open with Live Server**.
3. The site opens at `http://127.0.0.1:5500/index.html`.

### Option C - with a simple local server
```bash
# from inside the project folder
python -m http.server 8000
# then visit http://localhost:8000
```

> The pages use plain `<script>` tags (no ES modules), so they also work when opened directly
> with `file://` from the file system.

---

## 3. Demo admin login

| Field | Value |
| ----- | ----- |
| URL | `admin.html` |
| Username | `admin` |
| Password | `cluckcart123` |

> **This is a demonstration login only.** The check happens in JavaScript in your browser, so the
> credentials are visible in the page source and anyone can bypass them. A real business needs a
> backend with server-side authentication, hashed passwords, role-based access control and a
> database. See section 8 for details.

---

## 4. Features

### Home page (`index.html`)
- Large hero section with the poultry farm background image, headline, description and a
  "Shop Chickens" button that opens the product section.
- Live catalogue card and trust strip (breed count and total stock are read from the catalogue).
- Three age-category cards that link into a filtered shop page.
- Featured chicken products rendered from the catalogue.
- "Why choose us" feature cards, About Us section, contact information and a call-to-action band.

### Categories (age groups)
| Category | Rule |
| -------- | ---- |
| 4-Month-Old Chickens | age is exactly 4 months |
| 5-Month-Old Chickens | age is exactly 5 months |
| 6-Month-Old Chickens & Older | age is 6 months or more |
| All Chickens | no age filter |

The category is always derived from the age, both in the shop filters and in the admin form.

### Product listings (`shop.html`)
- Responsive product grid (4 columns on desktop, 2 on tablet, 1-2 on mobile).
- Each card shows: image, name, breed, age, price in pesos, short description, availability,
  "View Details" and "Add to Cart".
- Filter chips for each age group, plus **All Chickens**.
- Live search by **name or breed**.
- Sorting by featured, price low-to-high, price high-to-low, age and name.
- Result counter and an empty state with a "Clear all filters" button.
- If an image cannot load, `images/placeholder.svg` is shown automatically.

### Product details (`product-details.html`)
- Large image, name, breed, exact age in months, price, full description, characteristics,
  availability, stock note and a quantity selector.
- The quantity is limited by the available stock and the "Add to Cart" button is disabled for
  out-of-stock chickens.
- Related chickens from the same age group.

### Shopping cart (`cart.html`)
- Add, increase, decrease and remove items.
- Subtotal, delivery fee (&#8369;150, free over &#8369;5,000) and total are calculated live.
- Item counter in the header on every page.
- Stock is enforced: you cannot order more chickens than are available.
- Clear empty-state message when the cart is empty.

### Checkout (`checkout.html`)
- Validated form: full name, contact number (Philippine mobile format) and delivery address.
- Live order summary with the totals.
- On success a **sample order** is saved in LocalStorage and a confirmation screen shows the
  order code, the ordered items and the total.
- The confirmation clearly states that this is a demonstration: **no payment was processed and the
  order has not been delivered**.

### Admin dashboard (`admin.html`)
- **Overview**: total products, total stock, out-of-stock products, low-stock products and total
  sample orders, plus a "stock that needs attention" list.
- **Chicken Products**: full table with image, name, breed, age, price, stock, status, edit and
  delete buttons. Add, edit and delete products in a modal form (name, breed, age, category,
  price, stock, image URL, description, characteristics, featured flag).
- Availability is calculated automatically from the stock quantity, and the category follows the age.
- Deleting a product always asks for confirmation first.
- **Order Management**: every sample order with the customer details, ordered items, total and a
  status selector (Pending, Confirmed, Processing, Out for Delivery, Delivered, Cancelled).
- A "Reset sample data" button restores the original catalogue.

### Shared touches
- Toast notifications for success and error messages.
- Back-to-top button.
- Sticky header with a mobile menu, active page highlighting and a cart badge.
- Scroll-reveal animations and smooth transitions.

---

## 5. Data and LocalStorage

| Key | Contents |
| --- | -------- |
| `cluckcart.products.v1` | The product catalogue (seeded with sample data on first visit) |
| `cluckcart.cart.v1` | The shopping cart |
| `cluckcart.orders.v1` | Sample orders |
| `cluckcart.session.v1` | The demo admin session |

The sample catalogue is defined in `js/products.js` (`DEFAULT_PRODUCTS`). Change a value there and
use **Reset sample data** in the dashboard to reload it.

To completely reset the demo, open the browser console and run:

```js
Object.keys(localStorage)
  .filter(function (key) { return key.indexOf('cluckcart') === 0; })
  .forEach(function (key) { localStorage.removeItem(key); });
location.reload();
```

---

## 6. Editing the content

| What | Where |
| ---- | ----- |
| Products, prices, stock, images | `js/products.js` -> `DEFAULT_PRODUCTS`, or the admin dashboard |
| Business name, contact details | `js/ui.js` -> `footerHTML()` / `index.html` |
| Colours, fonts, spacing | `css/style.css` -> `:root` variables |
| Breakpoints | `css/responsive.css` |
| Delivery fee / free delivery limit | `js/products.js` -> `DELIVERY_FEE`, `FREE_DELIVERY_THRESHOLD` |

Product images live in `images/`. The default illustrations are hand-made SVG files, so they always
display and never break. You can also paste any image URL into the admin form.

---

## 7. Tests performed

| Check | Result |
| ----- | ------ |
| Home page renders hero, categories, featured products, about and contact | Pass |
| Category filters show only the correct age group | Pass |
| "All Chickens" restores the full catalogue | Pass |
| Search by name and by breed | Pass |
| Sorting by price (both directions), age and name | Pass |
| Empty state appears when nothing matches | Pass |
| Product details show the correct chicken for the selected id | Pass |
| Out-of-stock chicken cannot be added to the cart | Pass |
| Cart quantity increase / decrease / remove | Pass |
| Cart totals (subtotal, delivery, total) calculate correctly | Pass |
| Cart cannot exceed the available stock | Pass |
| Checkout rejects empty and invalid fields | Pass |
| Checkout saves a sample order and shows the confirmation | Pass |
| Admin login accepts the demo credentials and rejects wrong ones | Pass |
| Admin can add, edit and delete a product | Pass |
| Availability updates automatically from the stock value | Pass |
| Category follows the age in the admin form | Pass |
| Admin can change the sample order status | Pass |
| Product edits and orders survive a refresh | Pass |
| Layout checked at 360px, 768px, 1024px and 1440px - no horizontal scrolling | Pass |
| No JavaScript errors in the browser console | Pass |

---

## 8. Limitations and what a production version needs

This is a **front-end demonstration**. It is deliberately simple so it can be read and edited by a
beginner, but it is not suitable for running a real business on its own.

1. **No backend and no database.** Products, stock and orders live in the visitor's own browser.
   Another device sees a different catalogue, and clearing the browser data erases everything.
2. **The admin login is not secure.** It is a plain comparison in JavaScript, so anyone can read
   the password in the source code. Production needs a server, hashed passwords, sessions and roles.
3. **No real payment.** The checkout never charges anything and no confirmation email is sent.
4. **No real delivery or stock reservation.** Adding a chicken to the cart does not reserve it for
   the next customer.
5. **Image hosting.** Product images are local files or links the administrator types in. A real
   site needs an upload service and image resizing.
6. **Order status is only a label.** Nothing updates the stock when a sample order is "delivered".

**Recommended next steps:** move the catalogue, cart and orders to a server (for example Node.js +
Express + a MySQL/PostgreSQL database), replace the demo login with a real session-based
authentication flow, connect a payment gateway such as PayMongo, and add a farm-inventory system
that decreases stock when an order is confirmed.
