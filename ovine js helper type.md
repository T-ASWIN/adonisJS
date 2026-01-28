
---

# ✅ 1) Manual Interface / Type (Without `Infer`)

If you *didn’t* use Vine’s `Infer`, you would write something like this:

```ts
// types/movie_filters.ts

export interface MovieFilters {
  search?: string
  status?: number
  sort?: string
}
```

And then your service would be:

```ts
import type { MovieFilters } from '#types/movie_filters'

static getFiltered(filters: MovieFilters) {
  ...
}
```

---

## ❌ Problem With This Approach

If later you update your validator:

```ts
status: vine.number().min(1).optional()
```

You must remember to update:

```ts
interface MovieFilters {
  status?: number
}
```

Easy to forget → bugs 😬

---

# 🔥 2) What `Infer` REALLY Does

This is the key concept:

> ⚠️ **`Infer` does NOT store values.**
> It only works at **compile time** for TypeScript.

---

## 👉 What it actually is:

`Infer<T>` is a **TypeScript type helper**.

It:

✔ reads the validator’s type definition
✔ generates a TypeScript type
✔ disappears after compilation
✔ has ZERO effect at runtime

---

### So NO:

❌ It does NOT hold request data
❌ It does NOT validate
❌ It does NOT fetch user input

---

### What actually holds the data?

In your controller you probably have:

```ts
const filters = await request.validateUsing(movieFilterValidator)
```

That line:

👉 runs validation
👉 reads user input
👉 returns an object with real values

Example:

```ts
{
  search: "batman",
  status: 2,
  sort: "title_desc"
}
```

THAT object is passed into:

```ts
MovieService.getFiltered(filters)
```

---

## 🧠 Flow in Real Life

```
User Request
   ↓
VineJS Validator runs
   ↓
returns validated object
   ↓
TypeScript type of that object = Infer<typeof validator>
   ↓
Service method receives it
```

So:

* runtime value → comes from `validateUsing()`
* compile-time type → comes from `Infer`

They work together but are different layers.

---

# 🎯 Simple Explanation

Think of:

* **Validator** = security guard at the door 🧍‍♂️
* **Infer** = blueprint of what the guard allows 📐

The guard checks real people (runtime data).
The blueprint just tells TypeScript what shape to expect.

---

