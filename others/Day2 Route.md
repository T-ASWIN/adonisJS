
---

## Your code

```ts
router.get('/', async (ctx) => {
```

### 🔹 What this does

* Defines a **GET route** for `/`
* When someone opens `http://localhost:3333/`, this function runs
* `ctx` = **HTTP context** (request, response, view, params, etc.)

---

```ts
  const moviesPath = app.makePath('resources/movies')
```

### 🔹 What this does

* Builds an **absolute filesystem path** to the folder:

  ```
  resources/movies
  ```
* Example result:

  ```
  C:\Finmet\AdonisJs\adonis-dashboard\resources\movies
  ```

✅ Correct usage
👉 `makePath()` is meant for **filesystem access**

---

```ts
  const files = await fs.readdir(moviesPath)
```

### 🔹 What this does

* Reads **all file names** inside `resources/movies`
* Returns an array like:

  ```ts
  ['leo.md', 'mangatha.md']
  ```

⚠️ This reads **names only**, not file contents

---

```ts
  const movies: Record<string, any>[] = []
```

### 🔹 What this does

* Creates an **empty array**
* Each item will be an **object**
* `Record<string, any>` means:

  * keys → string
  * values → any type

This array will finally look like:

```ts
[
  { title: 'Leo', summary: '...', slug: 'leo' },
  { title: 'Mangatha', summary: '...', slug: 'mangatha' }
]
```

---

```ts
  for (const filename of files) {
```

### 🔹 What this does

* Loops through each file name
* Example:

  * `filename = 'leo.md'`
  * `filename = 'mangatha.md'`

---

```ts
    const movieURL = app.makeURL(`resources/movies/${filename}`)
```

### ❌ PROBLEM HERE (important)

* `makeURL()` creates a **web URL**
* You are using it for **filesystem reading**

Example result:

```
/resources/movies/leo.md
```

But `fs.readFile()` expects:

```
C:\Finmet\AdonisJs\adonis-dashboard\resources\movies\leo.md
```

📌 **This is a bug**, even if it sometimes appears to work.

✅ Correct API for this purpose:

```ts
app.makePath()
```

(I know you said you’ll fix bugs later — just explaining why this line is risky.)

---

```ts
    const file = await fs.readFile(movieURL, 'utf8')
```

### 🔹 What this does

* Reads the **entire Markdown file**
* Converts it into a **string**

Example content:

```md
---
title: Leo
summary: Action movie
---

# Leo Movie
Some content here
```

---

```ts
    const md = new MarkdownFile(file)
```

### 🔹 What this does

* Creates a **Markdown parser instance**
* Holds:

  * raw content
  * frontmatter
  * AST (after processing)

---

```ts
    await md.process()
```

### 🔹 What this does

* Parses the Markdown
* Extracts:

  * `frontmatter`
  * headings
  * content structure

After this:

```ts
md.frontmatter.title
md.frontmatter.summary
```

become available.

---

```ts
    movies.push({
      title: md.frontmatter.title,
      summary: md.frontmatter.summary,
      slug: filename.replace('.md', ''),
    })
```

### 🔹 What this does

* Creates a **movie object**
* Pushes it into the `movies` array

Example result:

```ts
{
  title: 'Leo',
  summary: 'Action movie',
  slug: 'leo'
}
```

---

```ts
  }
```

### 🔹 End of loop

* Runs once per Markdown file

---

```ts
  return ctx.view.render('pages/home', { movies })
```

### 🔹 What this does

* Renders the Edge view:

  ```
  resources/views/pages/home.edge
  ```
* Passes `movies` to the template

In Edge:

```edge
@each(movie in movies)
  {{ movie.title }}
@endeach
```

---

## 🧠 One-line summary (very important)

> This route reads all Markdown files from `resources/movies`, extracts frontmatter, builds a movie list, and sends it to the home page.

---

## ⚠️ Key takeaway (remember this rule)

| Purpose              | Correct API              |
| -------------------- | ------------------------ |
| Filesystem access    | `app.makePath()`         |
| Browser links / URLs | `route()` or `makeURL()` |

---





...........................


---

## ✅ What is `Record`?

`Record` is a **TypeScript utility type** used to describe the **shape of an object**.

### Definition (conceptually)

```ts
Record<K, V>
```

Means:

> “An object whose **keys are of type `K`** and **values are of type `V`**.”

---

## 🔹 Your example

```ts
const movies: Record<string, any>[] = []
```

Breakdown:

* `Record<string, any>` → an object

  * keys → `string`
  * values → `any`
* `[]` → an array of those objects

So this means:

> **`movies` is an array of objects with string keys and any type of values.**

---

## 🔍 Is `Record` a keyword?

❌ **Not a JavaScript keyword**
✅ **Yes, a TypeScript built-in utility type**

It exists only at **compile time** (for type checking).
It is **removed at runtime**.

---

## 🔎 Where does `Record` come from?

It is provided by TypeScript itself:

```ts
// internally defined by TypeScript like this
type Record<K extends keyof any, T> = {
  [P in K]: T
}
```

You don’t import it.
You don’t define it.
It’s globally available in TypeScript.

---

## 🧠 Why use `Record`?

### 1️⃣ When object keys are not fixed

```ts
const data: Record<string, number> = {
  apples: 10,
  oranges: 5,
}
```

### 2️⃣ When shape is dynamic (like your movies list)

```ts
Record<string, any>
```

You don’t care about exact fields yet.

---

## 🔁 Comparison with other types

### Using `{}` (bad)

```ts
const obj: {} = {}
```

❌ No type safety
❌ Cannot access properties safely

---

### Using `Record<string, any>` (better)

```ts
const obj: Record<string, any> = {}
```

✅ Keys are strings
✅ Properties allowed

---

### Using a strict type (best, later)

```ts
type Movie = {
  title: string
  summary?: string
  slug: string
}

const movies: Movie[] = []
```

---

## 🧪 Simple mental model

Think of:

```ts
Record<string, any>
```

as:

> “A dictionary / map / JSON-like object”

---

## 🎯 Interview-ready one-liner

> **`Record` is a TypeScript utility type used to define an object with specific key and value types.**

---


...................

---

## The route code (reference)

```ts
router
  .get('/movies/:slug', async (ctx) => {
    const filePath = app.makePath(`resources/movies/${ctx.params.slug}.md`)

    try {
      const file = await fs.readFile(filePath, 'utf8')

      const md = new MarkdownFile(file)
      await md.process()

      const movie = toHtml(md).contents

      return ctx.view.render('pages/movies/show', { movie, md })
    } catch {
      throw new Exception(
        `could not find a movie called ${ctx.params.slug}`,
        {
          status: 404,
          code: 'E_NOT_FOUND',
        }
      )
    }
  })
  .as('movies.show')
```

---

## 1️⃣ `/movies/:slug` — what does this mean?

```ts
.get('/movies/:slug', ...)
```

* `/movies/leo`
* `/movies/mangatha`

Here:

* `:slug` is a **dynamic URL parameter**
* It captures whatever comes after `/movies/`

### Example:

```
URL: /movies/leo
ctx.params.slug === 'leo'
```

---

## 2️⃣ `async (ctx) => { ... }`

* This function runs when the route is hit
* `ctx` = **HTTP Context**

  * `ctx.params` → URL params
  * `ctx.view` → render Edge templates
  * `ctx.request`, `ctx.response`, etc.

---

## 3️⃣ Build the file path

```ts
const filePath = app.makePath(`resources/movies/${ctx.params.slug}.md`)
```

### What this does:

* Converts a **relative path** into a **real filesystem path**

Example result:

```
C:\Finmet\AdonisJs\adonis-dashboard\resources\movies\leo.md
```

### Why this matters:

* `fs.readFile()` can only read **filesystem paths**
* `makeURL()` would NOT work here

---

## 4️⃣ `try { ... }`

```ts
try {
```

* Wraps the logic that **might fail**
* If any error occurs inside, it jumps to `catch`

Possible failures:

* File doesn’t exist
* Permission issue
* Invalid markdown

---

## 5️⃣ Read the Markdown file

```ts
const file = await fs.readFile(filePath, 'utf8')
```

* Reads `leo.md`
* Converts it to a **string**
* `'utf8'` means text, not binary

---

## 6️⃣ Create Markdown parser

```ts
const md = new MarkdownFile(file)
```

* Creates a Markdown processor object
* Stores raw content

---

## 7️⃣ Process Markdown

```ts
await md.process()
```

* Parses the markdown
* Extracts:

  * Frontmatter (`md.frontmatter`)
  * Content structure
  * Headings, lists, etc.

After this:

```ts
md.frontmatter.title
md.frontmatter.summary
```

are available.

---

## 8️⃣ Convert Markdown → HTML

```ts
const movie = toHtml(md).contents
```

* Converts parsed markdown into **HTML**
* `movie` is now an HTML string

Example:

```html
<h1>Leo</h1>
<p>Some content...</p>
```

---

## 9️⃣ Render the Edge view

```ts
return ctx.view.render('pages/movies/show', { movie, md })
```

* Renders:

  ```
  resources/views/pages/movies/show.edge
  ```
* Passes:

  * `movie` → HTML string
  * `md` → markdown metadata (frontmatter)

In Edge:

```edge
{{{ movie }}}
{{ md.frontmatter.title }}
```

---

## 🔟 `catch { ... }`

```ts
catch {
```

* Runs if **anything fails above**
* Typically means:

  * File not found (`leo.md` missing)

---

## 1️⃣1️⃣ Throw a 404 error

```ts
throw new Exception(
  `could not find a movie called ${ctx.params.slug}`,
  {
    status: 404,
    code: 'E_NOT_FOUND',
  }
)
```

* Sends a proper **404 Not Found**
* Error message shows the slug
* Stops execution

---

## 1️⃣2️⃣ `.as('movies.show')`

```ts
.as('movies.show')
```

* Names the route
* Allows Edge usage:

```edge
<a href="{{ route('movies.show', { slug: 'leo' }) }}">
```

---

## 🧠 Big-picture summary

> This route takes a movie name from the URL, finds the matching Markdown file, converts it to HTML, and renders it on a page. If the file doesn’t exist, it returns a 404 error.

---

## 🎯 One-line explanation (interview ready)

> “`/movies/:slug` is a dynamic route that maps URLs to Markdown files and renders them as HTML pages.”

---


