# Product Pagination — JavaScript Learning Project

A beginner-friendly pagination example built with HTML, CSS, and vanilla JavaScript. It fetches sample products from [DummyJSON](https://dummyjson.com/docs/products), displays ten products per page, and lets the visitor move between pages.

## Files

- `pagination.html` — the complete page, styles, and JavaScript in one file.

## Run it

Open `pagination.html` in a browser with an internet connection. The page requests sample product data from DummyJSON.

## What pagination means

Pagination divides a long list into smaller, numbered pages. Instead of showing every product at once, this page shows at most ten products. The visitor can use **Previous**, **Next**, or a numbered button to choose a page.

This example uses **client-side pagination**: it downloads the complete sample product list once, keeps it in `allProducts`, and selects the products for the current page with JavaScript's `slice()` method. This is easy to understand for a small learning dataset. A large production dataset is usually paginated by the API or server, so the browser only downloads the current page.

## How the page works

### 1. HTML structure

- `#status` tells the visitor that data is loading or explains when loading fails. `role="status"` and `aria-live="polite"` help assistive technology announce changes.
- `#list` is the empty section where product cards are inserted.
- `#pagination` is a navigation landmark that will hold the page buttons.
- `#page-info` reports which items are visible, for example “Showing 11–20 of 194 products.”

### 2. CSS layout

The product list uses CSS Grid. `repeat(auto-fill, minmax(190px, 1fr))` makes as many columns as fit while keeping each card at least 190 pixels wide. The media query changes the spacing and card image size on narrow screens. `:focus-visible` makes keyboard focus easy to see.

### 3. State variables

- `currentPage` — the page currently displayed. Page numbers start at 1 for people.
- `pageSize` — the number of products on one page; here it is 10.
- `totalPages` — how many pages are needed for all products.
- `allProducts` — the complete array returned by the API.
- `productList`, `pagination`, `statusMessage`, and `pageInfo` — references to the page elements that JavaScript updates.

### 4. Loading data: `loadProducts()`

`fetch()` requests `https://dummyjson.com/products?limit=0`. DummyJSON documents `limit=0` as the way to return all products. The function checks `response.ok`, converts the response body from JSON, saves `data.products`, calculates the page count, and displays page 1. If the request fails, the `catch` block shows a useful message and clears the list and buttons.

The page count is:

```js
totalPages = Math.ceil(allProducts.length / pageSize);
```

`Math.ceil()` rounds up so that a partial final page is still counted. For example, 194 products and 10 products per page need 20 pages.

### 5. Selecting a page: `showPage(pageNumber)`

A human-readable page number starts at 1, but array indexes start at 0. The first index for a page is:

```js
startIndex = (currentPage - 1) * pageSize;
```

For page 1, the start index is 0. For page 2, it is 10. `slice(startIndex, endIndex)` returns the products for that page; the end index is exclusive. The function also clamps the requested page so it cannot go below page 1 or beyond the last page.

After choosing the products, `showPage()` calls `renderProducts()` and `renderPagination()` to refresh the visible cards and controls.

### 6. Building product cards: `renderProducts(products)`

For each product, this function creates an `article`, an image, a heading, and a price paragraph. It uses `textContent` for API-provided text and `replaceChildren()` to update the list. This keeps the rendering steps explicit and avoids treating product text as HTML.

### 7. Building controls: `renderPagination()`

The function clears the old controls, then creates **Previous**, one button per page, and **Next**. The current page receives the `active` class and `aria-current="page"`. The Previous button is disabled on page 1; Next is disabled on the final page.

The loop uses `pageNumber <= totalPages` so the final page button is included. Each button calls `showPage()` with its target page.

### 8. Button helper: `createButton()`

This small helper creates a button, sets its label and disabled state, and connects its click event to `showPage()`. Keeping this repeated setup in one function makes the pagination code easier to read.

## Pagination example

With 194 products and 10 products per page:

| Page | Array indexes | Products shown |
| --- | --- | --- |
| 1 | 0–9 | 1–10 |
| 2 | 10–19 | 11–20 |
| 20 | 190–193 | 191–194 |

The last page can contain fewer products than `pageSize`.

## Improvements made to the original draft

- Changed the heading from “Users” to “Products” because the endpoint returns products.
- Changed the request from `limit=100` to `limit=0`, so this client-side example has the complete list before calculating pages.
- Changed the page-button loop from `< totalPages` to `<= totalPages`, which includes the last page.
- Added Previous and Next controls, loading/error feedback, a visible item range, and basic accessibility labels.
- Added the product thumbnail and formatted price to each card.

## API reference

DummyJSON's product documentation describes the product fields and the `limit`/`skip` options: <https://dummyjson.com/docs/products>.

---

# Pagination b JavaScript — شرح بالدارجة

هاد المشروع مثال بسيط على **pagination** باستعمال HTML وCSS وJavaScript. كيجبد لائحة ديال products تجريبية من [DummyJSON](https://dummyjson.com/docs/products)، وكيعرض 10 فكل صفحة. المستخدم يقدر يمشي للصفحة اللي قبل، اللي من بعد، ولا يختار رقم الصفحة مباشرة.

## الملفات

- `pagination.html` — الصفحة كاملة فيها HTML وCSS وJavaScript.

## كيفاش تشغّلو

حلّ `pagination.html` فـbrowser وخلي الإنترنت خدام، حيث المنتجات كتجي من DummyJSON.

## شنو هي pagination؟

Pagination هي تقسيم لائحة طويلة لصفحات صغار. بلا ما نعرضو جميع المنتجات مرة وحدة، كنقسموهم: 10 فصفحة، ومن بعد المستخدم كيتنقّل بين الصفحات.

هاد المثال فيه **client-side pagination**: كيجبد المنتجات كاملين مرة وحدة، كيخزنهم فـ`allProducts`، ومن بعد JavaScript كتختار غير العناصر ديال الصفحة الحالية باستعمال `slice()`. هاد الطريقة مزيانة باش نتعلمو على بيانات قليلة. فـproject كبير، غالباً API ولا server هو اللي كيرجع غير بيانات الصفحة المطلوبة باش ما نحمّلوش كلشي للbrowser.

## شرح كيفاش خدام

### 1. هيكل HTML

- `#status` كيبين “Loading products…”، وإذا وقع مشكل كيعرض رسالة. `role="status"` و`aria-live="polite"` كيساعدو قارئات الشاشة يعلنو تغيّر الرسالة.
- `#list` هو المكان اللي كيتحطو فيه product cards.
- `#pagination` هو المكان ديال أزرار الصفحات.
- `#page-info` كيبين شحال من product باينين، بحال “Showing 11–20 of 194 products”.

### 2. CSS والتنسيق

لائحة المنتجات خدامة بـCSS Grid. `repeat(auto-fill, minmax(190px, 1fr))` كتحط عدد الأعمدة اللي كيساعو فالعرض، وكل card كتكون على الأقل 190px. الـmedia query كتبدل المسافات وحجم الصور فالشاشات الصغار. و`:focus-visible` كيبين فين وصل التركيز ملي كتستعمل clavier.

### 3. المتغيّرات

- `currentPage` — رقم الصفحة اللي باينة دابا؛ الناس كيبداو العد من 1.
- `pageSize` — شحال من product فكل صفحة؛ هنا 10.
- `totalPages` — العدد الكامل ديال الصفحات.
- `allProducts` — array فيها المنتجات اللي رجعها API.
- `productList`, `pagination`, `statusMessage`, و`pageInfo` — مراجع لعناصر HTML اللي JavaScript كتبدل المحتوى ديالها.

### 4. جلب البيانات: `loadProducts()`

`fetch()` كيسول `https://dummyjson.com/products?limit=0`. حسب توثيق DummyJSON، `limit=0` كيرجع جميع المنتجات. كنشوفو `response.ok` باش نعرفو واش الطلب نجح، ومن بعد `response.json()` كتحول الرد لـJavaScript object. كنخزنو `data.products`، كنحسبو عدد الصفحات، وكنعرضو الصفحة الأولى.

إلا وقع خطأ فالشبكة ولا فالطلب، `catch` كتعرض رسالة واضحة وكتحيد النتائج والأزرار القديمة.

حساب الصفحات كيدار هكذا:

```js
totalPages = Math.ceil(allProducts.length / pageSize);
```

`Math.ceil()` كترفع النتيجة للعدد الصحيح اللي من الفوق، باش حتى الصفحة الأخيرة إلا بقى فيها غير شي منتجات تتحسب. مثلاً 194 product، وكل صفحة فيها 10، خاصنا 20 صفحة.

### 5. اختيار الصفحة: `showPage(pageNumber)`

رقم الصفحة كيبدا من 1، ولكن index ديال array كيبدا من 0. لذلك كنحسبو أول index هكذا:

```js
startIndex = (currentPage - 1) * pageSize;
```

- الصفحة 1 كتبدأ من index 0.
- الصفحة 2 كتبدأ من index 10.
- الصفحة 3 كتبدأ من index 20.

`slice(startIndex, endIndex)` كترجع غير products ديال الصفحة، و`endIndex` ما كيدخلش فالنتيجة. `showPage()` حتى كتحد رقم الصفحة باش ما ينقصش على 1 وما يفوتش آخر صفحة.

من بعد كتعيط لـ`renderProducts()` باش ترسم المنتجات، و`renderPagination()` باش تجدّد الأزرار.

### 6. رسم الكروت: `renderProducts(products)`

لكل product كتنشئ `article` فيه الصورة، العنوان والثمن. `textContent` كتحط النص كنص عادي، و`replaceChildren()` كتبدل المحتوى القديم بالجديد. هادشي واضح فالتعلّم وما كيتعاملش مع نص API بحال إلا كان HTML.

### 7. أزرار الصفحات: `renderPagination()`

كتحيد الأزرار القديمة وكتصايب:

- زر **Previous** للرجوع.
- زر لكل رقم صفحة.
- زر **Next** للتقدم.

زر الصفحة الحالية كياخد class `active` و`aria-current="page"`. زر Previous كيتعطل فالصفحة الأولى، وNext كيتعطل فالصفحة الأخيرة.

الحلقة كتستعمل `pageNumber <= totalPages` باش تضمن حتى آخر صفحة تدخل. كل زر عندو click event كينادي `showPage()` برقم الصفحة المختار.

### 8. الدالة المساعدة: `createButton()`

هاد الدالة الصغيرة كتصايب الزر، كتكتب فيه العنوان، كتضبط واش معطل، وكتربط click ديالو بـ`showPage()`. جمعنا هاد الخطوات فدالة وحدة باش ما نعاودوش نفس الكود بزاف.

## مثال ديال الحساب

إلا كانو 194 product و10 فكل صفحة:

| الصفحة | indexes ديال array | المنتجات اللي كتبان |
| --- | --- | --- |
| 1 | 0–9 | 1–10 |
| 2 | 10–19 | 11–20 |
| 20 | 190–193 | 191–194 |

ممكن الصفحة الأخيرة يكون فيها أقل من 10 منتجات.

## شنو تصلّح مقارنة بالمسودة الأولى؟

- تبدّل العنوان من “Users” لـ“Products” حيث API كيرجع products.
- تبدّل `limit=100` لـ`limit=0` باش هاد المثال اللي كيقسم فالbrowser يكون عندو اللائحة كاملة قبل حساب الصفحات.
- تصلّح شرط الحلقة من `< totalPages` لـ`<= totalPages`؛ الشرط الأول كان كينسى زر الصفحة الأخيرة.
- تزادو Previous وNext، ورسائل التحميل والخطأ، ومعلومة عدد المنتجات، وبعض labels ديال accessibility.
- تزادت صورة كل product والثمن ديالو.

## مرجع API

توثيق DummyJSON فيه تفاصيل المنتجات وخيارات `limit` و`skip`: <https://dummyjson.com/docs/products>.
