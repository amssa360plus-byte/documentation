# Pagination with JavaScript

This is Jawad Amsa's beginner pagination exercise. It uses HTML, CSS, and vanilla JavaScript to fetch products from DummyJSON and show them in groups of ten. The original student code is kept in `pagination.html`; this README explains what that code does and points out one pagination issue as a learning note.

## Files

- `pagination.html` — Jawad's original pagination exercise.
- `index.html` — a small GitHub Pages landing page with a link to the exercise.

## Run the exercise

Open `pagination.html` in a browser while connected to the internet. The script fetches data from `https://dummyjson.com/products?limit=100`.

## What pagination means

Pagination divides a long list into smaller groups, called pages. This exercise uses `limit = 10`, so its JavaScript displays up to ten products at a time. A visitor chooses a page by clicking its numbered button.

This is client-side pagination: JavaScript receives a list, stores it in an array, then uses `slice()` to choose the part for the selected page.

## How the original code works

### HTML

- `<h1>Users</h1>` is the page heading.
- `<div id="list"></div>` is an initially empty container where JavaScript inserts product titles and prices.
- `<div id="pagination"></div>` is an initially empty container where JavaScript inserts numbered buttons.
- The `<style>` block styles the page, list grid, cards, and pagination buttons. In this version, the product markup created by `render()` does not use the `.card` class, so those card-specific styles are not applied.

### Variables

- `list` finds the element with `id="list"` so JavaScript can update it.
- `pageination` finds the pagination container. The name is misspelled, but that is okay as long as the same spelling is used wherever the variable appears.
- `page` stores the currently selected page number. It starts at 1.
- `limit` is the number of products to show per page: 10.
- `totalpage` stores the number of pages calculated after data arrives.
- `alldata` stores the products returned by the API.

### `getdata()` — request the products

`getdata()` is an `async` function because it waits for a network request. `fetch()` sends a GET request to DummyJSON. The `if (!response.ok)` check detects an unsuccessful HTTP response and throws an error. `response.json()` reads the response as JavaScript data. The products array is saved as `data.products`, and `Math.ceil(alldata.length / limit)` calculates how many pages are needed. Finally, `showpage(1)` displays the first page.

The `catch` block logs a technical error in the browser console and puts `w93 mochkil` (there is a problem) in the list if the request fails.

### `showpage(p)` — select which products to display

The parameter `p` is the requested page number. The function saves it in `page`, then calculates the first array index for that page:

```js
const start = (p - 1) * limit
```

For page 1, `start` is 0. For page 2, it is 10. `slice(start, start + limit)` returns up to ten products from `alldata`. The function then calls `render(items)` to show those products and `renderButton()` to refresh the numbered buttons.

### `render(items)` — put products in the page

`map()` goes through each product and creates a small HTML string containing its title and price. The template literal `${items.title}` inserts the title, and `${items.price}` inserts the price. `join("")` combines all the generated strings into one string, which is assigned to `list.innerHTML`.

### `renderButton()` — create page buttons

The function first clears the existing pagination buttons. It then loops through page numbers, creates a `<button>` for each number, marks the selected page with the `active` CSS class, and adds a click listener that calls `showpage(i)` when the button is clicked.

## A learning note about the last page

The current loop condition is `i < totalpage`. Since `i` starts at 1, this stops before `i` reaches `totalpage`, so the last page button is missing. For example, if `totalpage` is 10, the loop creates buttons 1 through 9 only. The expected correction is `i <= totalpage`.

Also, the API URL asks for up to 100 products. If the response contains 100 products, ten items per page means ten pages. The heading says “Users” while the API returns products; this is only a wording mismatch and does not stop the pagination logic from working.

---

# Pagination b JavaScript — شرح بالدارجة

هاد المشروع تمرين ديال Jawad Amsa باش يتعلم pagination بـHTML وCSS وJavaScript. الكود الأصلي باقي محفوظ فـ`pagination.html`، وهاد الشرح كيوضح شنو كيدير وكيبيّن ملاحظة على زر آخر صفحة.

## الملفات

- `pagination.html` — التمرين الأصلي ديال Jawad.
- `index.html` — صفحة صغيرة ديال GitHub Pages فيها رابط للتمرين.

## كيفاش تشغّل التمرين

حلّ `pagination.html` فـbrowser وخلي الإنترنت خدام، حيت JavaScript كيجبد البيانات من `https://dummyjson.com/products?limit=100`.

## شنو هي pagination؟

Pagination هي تقسيم لائحة طويلة لمجموعات صغار كنسميوهم صفحات. هنا `limit = 10`، يعني JavaScript كتعرض حتى لـ10 منتجات فكل مرة. المستخدم كيختار الصفحة بالضغط على الزر ديال الرقم ديالها.

هاد النوع سميتو client-side pagination: JavaScript كتجيب البيانات، كتخزنها فـarray، ومن بعد `slice()` كتختار غير العناصر اللي خاصين يبانوا فالصفحة المختارة.

## شرح الكود الأصلي

### HTML

- `<h1>Users</h1>` هو العنوان اللي كيبان فوق.
- `<div id="list"></div>` هو المكان الخاوي اللي JavaScript كتزيد فيه عناوين المنتجات والثمن ديالها.
- `<div id="pagination"></div>` هو المكان اللي كيتحطو فيه أزرار أرقام الصفحات.
- `<style>` فيه تنسيق الصفحة والgrid والأزرار. كاين style ديال `.card`، ولكن `render()` ما كتضيفش class `card` للعناصر، يعني داك التنسيق ديال cards ما كيتطبقش فهاد النسخة.

### المتغيرات

- `list` كيشد العنصر اللي عندو `id="list"` باش JavaScript تقدر تبدل المحتوى ديالو.
- `pageination` كيشد container ديال pagination. الاسم فيه typo، ولكن ما كاين مشكل إلا استعملتي نفس الاسم فكل بلاصة.
- `page` فيه رقم الصفحة الحالية، والبداية هي 1.
- `limit` هو عدد المنتجات فكل صفحة، هنا 10.
- `totalpage` فيه العدد ديال الصفحات من بعد ما توصل البيانات.
- `alldata` فيه array ديال المنتجات اللي رجعهم API.

### `getdata()` — جلب المنتجات

`getdata()` دالة `async` حيت كتسنى network request. `fetch()` كتصيفط طلب لـDummyJSON. `if (!response.ok)` كيتأكد واش الطلب نجح؛ إلا ما نجحش كيطلق error. `response.json()` كتقرا جواب API وكتحولو لـJavaScript data. المنتجات كيتخزنو فـ`data.products`، ومن بعد `Math.ceil(alldata.length / limit)` كتحسب شحال خاص من صفحة. فالأخير `showpage(1)` كتعرض الصفحة الأولى.

إلا وقع مشكل، `catch` كتكتب التفاصيل فالconsole وكتعرض `w93 mochkil` فبلاصة اللائحة.

### `showpage(p)` — اختيار شنو يبان

`p` هو رقم الصفحة اللي بغا المستخدم. الدالة كتخزنو فـ`page` وكتحسب أول index ديال هاد الصفحة:

```js
const start = (p - 1) * limit
```

فالصفحة 1، `start` كيساوي 0. فالصفحة 2 كيساوي 10. `slice(start, start + limit)` كترجع حتى لـ10 منتجات من `alldata`. من بعد `render(items)` كتعرض المنتجات و`renderButton()` كتجدّد أزرار الصفحات.

### `render(items)` — عرض المنتجات

`map()` كتدوز على كل product وكتصايب HTML string صغيرة فيها العنوان والثمن. `${items.title}` كتحط العنوان فالنص و`${items.price}` كتحط الثمن. `join("")` كتلصق strings كاملين، والنتيجة كتتحط فـ`list.innerHTML`.

### `renderButton()` — تصايب أزرار الصفحات

الدالة كتمسح الأزرار القديمة. من بعد كتدور على أرقام الصفحات، وكتصايب `<button>` لكل رقم. إلا كان الرقم هو الصفحة الحالية كتزيد class `active`. وكتربط click event باش الضغط على الزر يعيط لـ`showpage(i)`.

## ملاحظة على آخر صفحة

شرط الحلقة دابا هو `i < totalpage`. حيث `i` كتبدا من 1، الحلقة كتوقف قبل ما توصل لرقم آخر صفحة، وبالتالي زر آخر صفحة ما كيتصاوبش. مثلاً إلا كانو 10 صفحات، كيبانو الأزرار من 1 حتى 9 فقط. التصحيح المتوقع هو `i <= totalpage`.

وزيد عليها أن API كيتسول على حتى 100 product. إلا رجعو 100، وكل صفحة فيها 10، غادي يكونو 10 صفحات. العنوان مكتوب فيه “Users” ولكن API كيرجع products؛ هادي غير مسألة ديال التسمية وما كتوقفش الحساب ديال pagination.
