
---

## 1️⃣ What is `fs`?

**`fs`** stands for **File System**.

It is a **built-in Node.js module** used to:

* Read files
* Write files
* Delete files
* List folders
* Work with files on the server

📌 It works **only on the backend**, never in the browser.

---

## 2️⃣ Which `fs` do we use in AdonisJS?

In modern AdonisJS, we mostly use:

```ts
import fs from 'node:fs/promises'
```

This is the **promise-based version**, which works perfectly with `async/await`.

---

## 3️⃣ Why `fs/promises` instead of normal `fs`?

### ❌ Old (callback-based)

```ts
fs.readFile('file.txt', (err, data) => {})
```

### ✅ Modern (async/await)

```ts
await fs.readFile('file.txt', 'utf8')
```

✔ Cleaner
✔ Easier to read
✔ No callback hell

---

## 4️⃣ Common `fs` Methods (Most Used)

### 📖 `fs.readFile()` – Read a file

```ts
const content = await fs.readFile(path, 'utf8')
```

Used when:

* Reading markdown files
* Reading JSON
* Loading templates

---

### ✍️ `fs.writeFile()` – Write / Create a file

```ts
await fs.writeFile(path, 'Hello World')
```

Used when:

* Saving content
* Generating files

---

### 📂 `fs.readdir()` – List files in a folder

```ts
const files = await fs.readdir(directoryPath)
```

Used when:

* Listing movies/articles
* Showing file-based pages

---

### ❌ `fs.unlink()` – Delete a file

```ts
await fs.unlink(path)
```

---

### 📁 `fs.mkdir()` – Create a directory

```ts
await fs.mkdir(path, { recursive: true })
```

---

## 5️⃣ Using `fs` with `app.makePath()` (Best Practice)

Never hardcode file paths ❌

### ❌ Wrong

```ts
await fs.readFile('/home/user/project/resources/movies/leo.md')
```

### ✅ Correct

```ts
import app from '@adonisjs/core/services/app'

const path = app.makePath('resources/movies/leo.md')
const content = await fs.readFile(path, 'utf8')
```

📌 This makes your app:

* Portable
* OS-independent
* Production-safe

---

## 6️⃣ Real AdonisJS Example

```ts
router.get('/movie/:slug', async ({ params, view }) => {
  const path = app.makePath(`resources/movies/${params.slug}.md`)
  const content = await fs.readFile(path, 'utf8')

  return view.render('movies/show', { content })
})
```

---

## 7️⃣ Error Handling with `fs`

Always wrap file access in `try/catch`:

```ts
try {
  const content = await fs.readFile(path, 'utf8')
} catch {
  throw new Error('File not found')
}
```

---

## 8️⃣ Interview-Ready Explanation 🧠

> `fs` is Node.js’s file system module used to interact with files and directories on the server. In AdonisJS, we typically use `fs/promises` along with `app.makePath()` to safely read and write files using async/await.

---

## 9️⃣ One-Line Memory Tip

> **fs → server files
> makePath → correct location**

---
