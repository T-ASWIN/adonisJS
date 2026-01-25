
---

# 🧠 What is `@beforeCreate()`?

`@beforeCreate()` is a **Lucid lifecycle hook**.

It runs:

👉 automatically
👉 right BEFORE a new row is inserted into DB
👉 only on `.create()` / `.save()` for new records

Perfect for:

✔ generating slugs
✔ hashing passwords
✔ setting defaults
✔ cleaning data
✔ validations
✔ timestamps
✔ UUIDs

---

# 🎯 Purpose Here

This hook ensures:

> Every movie gets a **unique URL-friendly slug** based on its title.

Example:

```
Title: The Dark Knight
Slug → the-dark-knight
```

If already taken:

```
the-dark-knight-2
```

---

---

# 🧩 Line-by-Line Explanation

---

## 🔵 1️⃣ Decorator

```ts
@beforeCreate()
```

Registers this method as a **hook**.

Lucid now knows:

👉 call `slugify()` automatically before insert.

---

---

## 🟢 2️⃣ Method signature

```ts
static async slugify(movie: Movie) {
```

• `static` → belongs to class, not instance
• `async` → DB query inside
• receives the model being saved (`movie`)

---

---

## 🟡 3️⃣ If slug already exists, skip

```ts
if (movie.slug) return
```

If developer already manually set a slug → don’t override it.

Smart guard 👍

---

---

## 🟣 4️⃣ Generate base slug

```ts
const slug = string.slug(movie.title, {
  replacement: '-',
  lower: true,
  strict: true,
})
```

Uses Adonis helper.

From title:

```
"My Movie!!!"
```

→

```
my-movie
```

Options:

• `replacement: '-'` → spaces → dash
• `lower: true` → lowercase
• `strict: true` → remove symbols

---

---

## 🔵 5️⃣ Query DB for existing similar slugs

```ts
const rows = await Movie.query()
  .select('slug')
  .whereRaw('lower(??)=?', ['slug', slug])
  .orWhereRaw('lower(??)like ?', ['slug', `${slug}-%`])
```

This finds:

• exact match → `my-movie`
• numbered versions → `my-movie-2`, `my-movie-3`

### ❓ Why `whereRaw`?

Because SQL functions like `LOWER()` and `LIKE` aren’t directly supported cleanly in Lucid helpers.

---

---

## 🟢 6️⃣ If none found — use slug

```ts
if (!rows.length) {
  movie.slug = slug
  return
}
```

If slug is free → assign it and stop.

---
---

# 🧠 What is `incrementors`?

This line:

```ts
const incrementors = rows.reduce<number[]>((result, row) => {
```

Means:

👉 “Loop through all existing slugs and collect the numbers at the end.”

Example rows from DB:

```
[
  { slug: 'batman' },
  { slug: 'batman-2' },
  { slug: 'batman-5' },
]
```

We want to extract:

```
[2, 5]
```

Those numbers are called **incrementors**.

---

# 🔵 What is `reduce()`?

`reduce()` is an array method that:

👉 takes many items
👉 and “reduces” them into ONE thing (array, number, object, etc.)

Syntax:

```js
array.reduce((accumulator, currentItem) => {
  return accumulator
}, initialValue)
```

In your case:

• accumulator = `result`
• currentItem = `row`
• initialValue = `[]` (empty array)

So:

```ts
rows.reduce<number[]>((result, row) => { ... }, [])
```

means:

👉 start with empty array
👉 build it up while looping

---

# 🧩 Step-by-Step with Example

Let’s simulate 👇

```js
const rows = [
  { slug: 'batman' },
  { slug: 'batman-2' },
  { slug: 'batman-5' },
]
```

Now reduce:

```js
const incrementors = rows.reduce((result, row) => {
  // result = collected numbers so far
  // row = current object

  const tokens = row.slug.split('batman-')

  if (tokens.length < 2) {
    return result
  }

  const increment = Number(tokens[1])

  if (!Number.isNaN(increment)) {
    result.push(increment)
  }

  return result
}, [])
```

---

## 🔁 Iteration 1

row.slug = 'batman'

```
tokens = ['batman']
tokens.length < 2 → true
→ return result []
```

---

## 🔁 Iteration 2

row.slug = 'batman-2'

```
tokens = ['', '2']
increment = 2
push → [2]
```

---

## 🔁 Iteration 3

row.slug = 'batman-5'

```
tokens = ['', '5']
increment = 5
push → [2, 5]
```

---

# 🎯 Final result:

```
incrementors = [2, 5]
```

---

# 🟢 Why is it called incrementors?

Because these numbers are used to **increment** the slug:

```
batman-6
```

It’s not a JS keyword — just a variable name the developer chose.

---

# 🟣 Then this line:

```ts
const increment = incrementors.length
  ? Math.max(...incrementors) + 1
  : 1
```

Means:

👉 If array has numbers → pick biggest → add 1
👉 Else → start at 1

---
