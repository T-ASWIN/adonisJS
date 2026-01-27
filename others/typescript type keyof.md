No worries at all, Aswin — this one trips *everyone* at first 😄
Let’s slow it way down and make `keyof` feel obvious.

---

# 🧩 Think of `keyof` like this:

> 👉 **“Give me all the property names of this type.”**

Not the values.
Not the data types.
Just the **keys / field names**.

---

## ✅ Tiny Example

```ts
type Person = {
  name: string
  age: number
}
```

Now ask TypeScript:

```ts
type PersonKeys = keyof Person
```

What is `PersonKeys`?

👉 It becomes:

```
"name" | "age"
```

Meaning:

> A variable of type `PersonKeys` can ONLY be `"name"` or `"age"`.

---

---

# 🎯 Use It Like This

```ts
let key: keyof Person

key = "name"   // ✅ OK
key = "age"    // ✅ OK
key = "email"  // ❌ ERROR (not in Person)
```

---

---

# 📦 Now Map It to Your Case

You wrote:

```ts
field: keyof Movie
```

That means:

👉 `field` must be one of the property names defined in the `Movie` class.

So if Movie has:

```ts
class Movie {
  id: number
  title: string
  slug: string
}
```

Then:

```
keyof Movie = "id" | "title" | "slug"
```

---

So this:

```ts
{ field: "title" }
```

✅ valid

But this:

```ts
{ field: "rating" }
```

❌ invalid (if Movie has no `rating` property).

---

---

# 🧠 One-Line Definition

> **`keyof` creates a union type of all property names of another type.**

---

---

# 🍪 Real-Life Analogy

Imagine a form:

```
Movie Form Fields:
- title
- slug
- releasedAt
```

`keyof Movie` is like:

👉 a dropdown that ONLY lets you pick from that list.

No typing random stuff.

---

---

If it still feels fuzzy, tell me:

👉 should I explain with a drawing-style mental model?
👉 or with a DB column analogy?
Gotcha 👍 — the reason **ASC / DESC sorting isn’t working** is simple:

👉 You defined sort options…
👉 But you **never actually use `filters.sort`** inside the query.

Right now your service always does:

```ts
.orderBy('title')
```

That means:

📌 default DB behavior → **ascending only**, no matter what user selects.

---

# ❌ Current Problem

You created:

```ts
static sortOptions = [
  { id: 'title_asc', text: 'Title (asc)', field: 'title', dir: 'asc' },
  { id: 'title_desc', text: 'Title (desc)', field: 'title', dir: 'desc' },
]
```

But in `getFiltered()` you never:

• read `filters.sort`
• match it with `sortOptions`
• apply `dir`

---

# ✅ FIX: Use the Selected Sort

Update your service like this:

---

## ✔ Corrected `MovieService`

```ts
static getFiltered(filters: Record<string, any>) {
  const sort =
    this.sortOptions.find((option) => option.id === filters.sort) ||
    this.sortOptions[0] // default title_asc

  return Movie.query()
    .if(filters.search, (query) =>
      query.whereILike('title', `%${filters.search}%`)
    )
    .if(filters.status, (query) =>
      query.where('statusId', filters.status)
    )
    .preload('director')
    .preload('writer')
    .preload('status')
    .orderBy(sort.field, sort.dir)   // ✅ apply direction
    .limit(15)
}
```

---

# 🧠 Why This Fix Works

Now:

• User selects → `?sort=title_desc`
• `filters.sort` contains `"title_desc"`
• You map it to config
• DB receives:

```
ORDER BY title DESC
```

---

# 📌 Quick Checklist

Make sure:

✔ `<select name="sort">` exists in view
✔ option values = `title_asc`, `title_desc`
✔ query string shows:

```
?sort=title_desc
```

✔ service uses `filters.sort`

---

# 🔥 Debug Tip

Temporarily log:

```ts
console.log(filters.sort)
```

in controller to verify value is coming.

---

# 🎯 Summary

**Bug cause:** hardcoded `.orderBy('title')`
**Fix:** dynamic `.orderBy(sort.field, sort.dir)`

---

If you want next, Aswin 👇

👉 add default selected sort
👉 add validation for `sort`
👉 allow multiple sort fields
👉 add pagination

Just say the word 😄🎬
