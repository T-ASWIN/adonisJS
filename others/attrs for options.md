
### 1. The Problem it Solves

Without `html.attrs`, you would have to write this logic manually to keep the option selected after the page reloads:

```edge
{{-- The "Messy" Way --}}
<option 
  value="{{ status.id }}" 
  @if(status.id == filters.status) selected="selected" @endif
>
  {{ status.name }}
</option>

```

If you have multiple attributes (like `disabled`, `readonly`, `class`), this gets very hard to read.

### 2. How `html.attrs` Works

The `html.attrs()` helper takes an object.

* **Key:** The name of the HTML attribute (e.g., `selected`, `disabled`, `class`).
* **Value:** A boolean (true/false) condition.

If the condition is **true**, it adds the attribute.
If the condition is **false**, it completely removes the attribute.

### 3. Breakdown of Your Code

```javascript
html.attrs({ 
  selected: status.id == filters.status 
})

```
Almost 😄 — but this is an important correction, Aswin:

> ❌ **We do NOT use `attrs()` to *get* data from user input.**
> ✅ We use `attrs()` to **output data safely into HTML attributes**.

Let me make it crystal clear.

---

# 🚫 What `attrs()` is NOT for

It is **not** used to:

* read form input
* access query params
* get request body
* validate user data
* sanitize incoming data

Those jobs are done by:

👉 `request.input()`
👉 `request.qs()`
👉 VineJS validators
👉 controllers / middleware

---

# ✅ What `attrs()` IS for

`html.attrs()` is a **view helper**.

It helps:

✔ render attributes dynamically
✔ conditionally add classes
✔ escape values to prevent XSS
✔ keep templates clean

---

# 🧠 Simple Mental Model

* **Controller** → reads user input
* **Validator** → validates
* **Service** → queries DB
* **View (`attrs`)** → safely prints output

Different layers. Different responsibilities.

---

# 📌 Example Comparison

### Getting input (Controller):

```ts
const page = request.input('page')
```

### Rendering output (View):

```edge
<a {{ html.attrs({ href: item.url }) }}>Page</a>
```

---

# 🎯 Interview Answer

> `attrs()` is used in Edge views to safely render dynamic HTML attributes and conditional classes. It is not used to retrieve user input; input handling is done in controllers and validators.

---

Here you go, Aswin 📘 — a clean **README-style reference file** you can keep for pagination concepts in AdonisJS / Lucid.

You can copy this into something like:

👉 `README-pagination.md`

---

# 📄 README — Pagination Helpers in AdonisJS (Lucid ORM)

This document explains common pagination-related methods used in AdonisJS when working with database queries and building page navigation links.

---

## 🔹 `.paginate(page, limit)`

### ✅ What it does

Runs the query and returns **paginated results** instead of all rows.

### Example:

```ts
const movies = await Movie.query().paginate(1, 15)
```

---

### 📌 Returns:

A **Paginator object** containing:

* data rows
* total count
* current page
* last page
* URLs for navigation

---

---

## 🔹 `.currentPage`

### ✅ What it does

Returns the current page number.

### Example:

```ts
movies.currentPage
```

---

---

## 🔹 `.lastPage`

### ✅ What it does

Returns the total number of pages.

### Example:

```ts
movies.lastPage
```

---

---

## 🔹 `.getPreviousPageUrl()`

### ✅ What it does

Returns the URL for the previous page.

### Example:

```ts
movies.getPreviousPageUrl()
```

Output:

```
/movies?page=2
```

Returns `null` if already on the first page.

---

---

## 🔹 `.getNextPageUrl()`

### ✅ What it does

Returns the URL for the next page.

### Example:

```ts
movies.getNextPageUrl()
```

Returns `null` if already on the last page.

---

---

## 🔹 `.getUrlsForRange(start, end)`

### ✅ What it does

Generates page-number URLs for a given range.

### Example:

```ts
movies.getUrlsForRange(1, movies.lastPage)
```

Returns:

```ts
[
  { page: 1, url: '/movies?page=1', isActive: false },
  { page: 2, url: '/movies?page=2', isActive: true },
]
```

---

---

## 🔹 `.baseUrl(url)`

### ✅ What it does

Sets the base URL used by paginator when generating links.

### Example:

```ts
movies.baseUrl(router.makeUrl('movies.index'))
```

---

### 📌 Why it’s used:

* Ensures pagination links point to the correct route
* Prevents incorrect auto-generated URLs
* Useful when using named routes

---

---

## 🔹 `router.makeUrl()`

### ✅ What it does

Generates a URL from a **named route**.

### Example:

```ts
router.makeUrl('movies.index')
```

Returns:

```
/movies
```

---

---

## 🔹 Why Pagination Needs Query Strings

When filters or sorting are applied:

```
/movies?status=2&sort=title_desc&page=3
```

Pagination helpers normally generate:

```
/movies?page=2
```

So we append the filter query string manually:

```
/movies?page=2&status=2&sort=title_desc
```

This keeps filters persistent across pages.

---

---

# 📌 Summary Table

| Method                  | Purpose                      |
| ----------------------- | ---------------------------- |
| `.paginate()`           | Fetch paginated records      |
| `.currentPage`          | Current page number          |
| `.lastPage`             | Total number of pages        |
| `.getPreviousPageUrl()` | Link to previous page        |
| `.getNextPageUrl()`     | Link to next page            |
| `.getUrlsForRange()`    | Page numbers for a range     |
| `.baseUrl()`            | Set base route for paginator |
| `router.makeUrl()`      | Build URL from named route   |

---

---

