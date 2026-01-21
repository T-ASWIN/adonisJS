1.What declare means

“This property exists at runtime, trust me.”

2.Why do we use ?

abstract may or may not exist

workflow in projects
---

## What this method does (in simple terms)

```ts
static async find(slug: string) {
  const md = await MovieService.read(slug)

  const movie = new Movie()

  movie.title = md.frontmatter.title
  movie.summary = md.frontmatter.summary
  movie.slug = slug
  movie.abstract = toHtml(md).contents

  return movie
}
```

### ✔ Correct understanding (clean version)

> This method takes a `slug` from the user, uses a service to locate and read the corresponding Markdown file, converts the Markdown into structured data, and returns a `Movie` object where:
>
> * basic fields come from front-matter
> * `abstract` contains the full HTML version of the Markdown content

---

## Step-by-step execution flow

### 1️⃣ Slug comes from the user

Example URL:

```
/movies/leo
```

Controller:

```ts
Movie.find(params.slug)
```

So:

```ts
slug = 'leo'
```

---

### 2️⃣ Service locates and reads the file

```ts
const md = await MovieService.read(slug)
```

Internally:

* Converts `leo` → `leo.md`
* Finds `resources/movies/leo.md`
* Reads file from disk
* Processes Markdown (`md.process()`)

Now `md` contains:

* `md.frontmatter`
* `md.html`
* `md.title`

---

### 3️⃣ Create an in-memory Movie object

```ts
const movie = new Movie()
```

⚠️ Important:

* This is **NOT a database record**
* Just a TypeScript object in memory

---

### 4️⃣ Map Markdown data → Movie fields

```ts
movie.title = md.frontmatter.title
movie.summary = md.frontmatter.summary
movie.slug = slug
```

From Markdown:

```md
---
title: Leo
summary: Bloody sweet action movie
---
```

Mapped into:

```ts
movie.title = 'Leo'
movie.summary = 'Bloody sweet action movie'
```

---

### 5️⃣ Convert Markdown to HTML

```ts
movie.abstract = toHtml(md).contents
```

### What this means

* `toHtml(md)` converts processed markdown → HTML
* `.contents` is the actual HTML string

Example value of `abstract`:

```html
<h1>Leo</h1>
<p>Bloody sweet action movie</p>
```

📌 That’s why `abstract` contains **full HTML code**.

---

### 6️⃣ Return the Movie object

```ts
return movie
```

Now the controller or view receives a **ready-to-use Movie object**.

---

## Final Mental Model 🧠

```
URL slug
  ↓
Service reads .md file
  ↓
Markdown processed
  ↓
Markdown → Movie object
  ↓
abstract = full HTML
```

---

## Important Clarifications (Very Useful)

### ❌ It does NOT:

* Save to database
* Fetch from database
* Persist data

### ✅ It DOES:

* Read files
* Convert Markdown → HTML
* Prepare data for UI

---

## Interview-Ready One-Liner

> The `find` method takes a slug, reads the corresponding Markdown file using a service, maps the front-matter to a Movie object, converts the Markdown content into HTML, and returns a fully prepared in-memory Movie instance.

---


