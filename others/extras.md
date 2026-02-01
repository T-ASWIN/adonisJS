Good one 👍 — **`$extras`** is a Lucid ORM feature that often confuses people at first.

Let’s explain it **simply and clearly**.

---

# 🧠 What Is `$extras`?

In Adonis Lucid, **`$extras` is a special object on a model instance** that stores:

👉 columns that were selected in the SQL query
👉 but are **NOT part of the model’s normal columns**.

These usually come from:

• pivot tables
• raw selects
• joins
• aggregates
• aliases

---

# 🎯 Most Common Use: Pivot Tables

When you load a many-to-many relationship with extra pivot columns:

```ts
@manyToMany(() => Cineast, {
  pivotColumns: ['character_name', 'sort_order'],
})
declare castMembers: ManyToMany<typeof Cineast>
```

And query:

```ts
const movie = await Movie.query().preload('castMembers')
```

Each related Cineast will have:

```
actor.$extras.pivot_character_name
actor.$extras.pivot_sort_order
```

---

# 🔍 Why `pivot_` Prefix?

Lucid automatically prefixes pivot fields with:

```
pivot_
```

to avoid clashing with real model properties.

So:

| Pivot column   | In `$extras`         |
| -------------- | -------------------- |
| character_name | pivot_character_name |
| sort_order     | pivot_sort_order     |

---

---

# 🧠 Simple Explanation

> `$extras` holds “extra” columns returned by the query that aren’t mapped to the model — especially pivot table data.

---

# ✅ Another Example: Raw Select

If you run:

```ts
const movies = await Movie.query()
  .select('movies.*')
  .count('* as total')
```

You can access:

```
movies[0].$extras.total
```

Because `total` is not a real Movie column.

---

---

# ⚠️ Important Things to Know

• `$extras` is read-only by default
• not saved back to DB
• disappears after request
• not included unless selected
• pivot extras only appear if you declare `pivotColumns` or query them

---

# 📌 Quick Mental Rule

👉 **DB columns → normal properties**
👉 **Joined / pivot / raw columns → `$extras`**

---

