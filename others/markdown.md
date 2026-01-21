
---

## 1️⃣ What is Markdown?

**Markdown** is a **lightweight markup language** used to write formatted text **using plain text syntax**.

Instead of writing complex HTML like this:

```html
<h1>Movie Title</h1>
<p>This is a description.</p>
```

You write simple, readable text:

```md
# Movie Title

This is a description.
```

Markdown is then **converted into HTML** when displayed in a browser.

---

## 2️⃣ Why do we use Markdown?

### ✅ Main reasons

1. **Easy to write**

   * No opening/closing tags
   * Looks clean even before rendering

2. **Easy to maintain**

   * Content writers don’t need HTML knowledge
   * Just edit `.md` files

3. **Separation of concerns**

   * Content → Markdown (`.md`)
   * Logic → JavaScript / Adonis routes
   * UI → Edge templates

4. **Very popular**

   * GitHub README files
   * Documentation sites
   * Blogs, CMS systems

---

## 3️⃣ Common Markdown syntax (quick view)

````md
# Heading 1
## Heading 2

**Bold text**
*Italic text*

- List item
- Another item

[Link](https://example.com)

```js
console.log("Code block")
````

````

➡️ All of this becomes proper HTML automatically.

---

## 4️⃣ Why did you install this?

```bash
npm i @dimerapp/markdown
````

This package is used to:

* **Read Markdown files**
* **Parse and process them**
* **Convert Markdown → HTML**

Perfect for:

* Blog pages
* Movie pages
* Documentation pages
* CMS-style content

---

## 5️⃣ What your code is doing (step-by-step)

### 📌 Route

```ts
router.get('/page/:slug', async (value) => {
```

Example URL:

```
/page/mangatha
```

Here:

* `slug = mangatha`

---

### 📌 Build file path

```ts
const url = app.makeURL(`resources/movies/${value.params.slug}.md`)
```

If slug = `mangatha`, it reads:

```
resources/movies/mangatha.md
```

---

### 📌 Read Markdown file

```ts
const file = await fs.readFile(url, 'utf8')
```

Now `file` contains raw Markdown text.

---

### 📌 Create Markdown object

```ts
const md = new MarkdownFile(file)
```

This prepares the Markdown for processing.

---

### 📌 Process Markdown

```ts
await md.process()
```

* Parses headings
* Parses lists
* Parses code blocks
* Builds an internal AST (structure)

---

### 📌 Convert Markdown → HTML

```ts
const movie = toHtml(md).contents
```

Now:

* `movie` = **HTML string**
* Ready to render in UI

---

### 📌 Share data with Edge view

```ts
value.view.share({ movie })
```

Now `movie` is available inside:

```edge
pages/movies/movie.edge
```

---

### 📌 Render page

```ts
return value.view.render('pages/movies/movie')
```

This displays the page with rendered HTML.

---

## 6️⃣ What is this doing?

```edge
{{ inspect(movie) }}
```

### ✅ Purpose:

* Debugging helper
* Prints the **actual value of `movie`**

You use it to:

* Check if HTML is generated correctly
* Verify Markdown → HTML conversion

⚠️ **Don’t use in production UI**, only for debugging.

---

## 7️⃣ Why this approach is powerful 🚀

With this setup:

* You can add a new page by just creating:

  ```
  resources/movies/new-movie.md
  ```
* No new routes needed
* No new controller needed
* Clean, scalable CMS-style architecture

---

## 8️⃣ Real-world use case

Your system now behaves like:

* **Mini CMS**
* **Static site generator**
* **Markdown-based content engine**

