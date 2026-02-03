Nice, Aswin 👍 — **`extras`** is another Lucid ORM concept that usually appears when you start doing advanced queries.

Let’s make it very clear and simple.

---

# 🧠 What is `$extras` in AdonisJS?

In Lucid models:

> 👉 **`$extras` stores values that come from the query but are NOT actual table columns.**

These are:

* computed columns
* aggregate results (`count`, `sum`, etc.)
* pivot table fields (sometimes)
* raw SQL selects
* joined columns

---

---

# 🔍 Simple Example — Aggregate

```ts
const movies = await Movie.query()
  .select('*')
  .count('* as total')
```

Now each `movie` instance will have:

```ts
movie.$extras.total   // 👈 comes from COUNT(*)
```

Because `total` is **not a real column** in `movies` table.

---

---

# 🔍 Example — Raw select

```ts
const movies = await Movie.query()
  .select('*')
  .selectRaw('YEAR(released_at) as year')
```

Then:

```ts
movie.$extras.year
```

---

---

# 🔍 Example — Join

```ts
const movies = await Movie.query()
  .join('movie_statuses', 'movies.status_id', 'movie_statuses.id')
  .select('movies.*', 'movie_statuses.name as status_name')
```

Access:

```ts
movie.$extras.status_name
```

---

---

# 🔍 Example — Many-to-many pivot fields

In some cases pivot fields land in `$extras`:

```ts
const movie = await Movie.query()
  .preload('castMembers', (query) => {
    query.pivotColumns(['character_name'])
  })
```

Then:

```ts
movie.castMembers[0].$extras.character_name
```

---

---

# ⚠️ Important

* `$extras` are **read-only** — not saved back to DB.
* They exist only for that query result.
* Not included in JSON output unless you manually add them.

---

---

# 📦 How to include extras in JSON?

You can:

### ✅ manually:

```ts
return movie.serialize({
  extras: true,
})
```

or

```ts
movie.toJSON({ extras: true })
```

---

---

# 🎯 Mental model

> `$extras = query-time computed or joined data that doesn’t belong to the model schema`

---

---

# ⚖️ Compare quickly

| Thing         | Holds                            |
| ------------- | -------------------------------- |
| `$attributes` | real table columns               |
| `$extras`     | computed / joined / pivot values |
| `$dirty`      | unsaved changes                  |

---

If you want next:

👉 `$pivot` vs `$extras`
👉 how preload + pivotColumns works
👉 when extras appear automatically

Say the word 😄
