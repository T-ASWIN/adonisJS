
---

# ✅ 1️⃣ `loadDb()` — What & Why

In Adonis REPL:

```bash
node ace repl
```

Sometimes DB is not initialized automatically.

Run:

```ts
loadDb()
```

👉 It boots the database connection so queries work.

Often you’ll do:

```ts
loadModels()
loadDb()
```

Then:

```ts
await User.all()
```

---

# ✅ 2️⃣ Important Query Builder Methods

These are used with:

```ts
User.query()
```

---

## 🔹 `where()`

Filter rows.

```ts
await User.query().where('role_id', 1)
```

---

## 🔹 `orderBy()`

Sort results.

```ts
await Movie.query().orderBy('created_at', 'desc')
```

---

## 🔹 `limit()` (limitations)

Limit number of rows.

```ts
await Movie.query().limit(5)
```

---

## 🔹 Combine:

```ts
await Movie.query()
  .where('status_id', 5)
  .orderBy('title', 'asc')
  .limit(10)
```

---

# ✅ 3️⃣ `.pojo()` — Plain Object Output

Lucid returns **model instances** by default.

`.pojo()` converts result to plain JS objects.

```ts
const users = await User.query().pojo()
```

Useful for:

* APIs
* logging
* sending raw JSON
* performance

---

# ✅ 4️⃣ `andWhere()`

Same as chaining `.where()` — explicit AND.

```ts
await Movie.query()
  .where('status_id', 5)
  .andWhere('director_id', 2)
```

SQL:

```sql
WHERE status_id = 5 AND director_id = 2
```

---

# ✅ 5️⃣ Callback Function in `where`

Used for grouped conditions.

Example:

```ts
await Movie.query().where((query) => {
  query
    .where('status_id', 5)
    .orWhere('status_id', 4)
})
```

SQL:

```sql
WHERE (status_id = 5 OR status_id = 4)
```

---

Another:

```ts
await User.query().where((q) => {
  q.where('role_id', 1).andWhere('email', 'like', '%gmail%')
})
```

---

# ✅ 6️⃣ if / else with Queries

In REPL or code:

```ts
const isAdmin = true

let query = User.query()

if (isAdmin) {
  query = query.where('role_id', 2)
} else {
  query = query.where('role_id', 1)
}

const users = await query
```

---

---

# 🔥 REPL Cheat Session Example

Run REPL:

```bash
node ace repl
```

Then:

```ts
loadModels()
loadDb()

await User.query().where('role_id', 1).orderBy('created_at', 'desc').limit(3)

await Movie.query().pojo()

await Movie.query().where((q) => {
  q.where('status_id', 4).orWhere('status_id', 5)
})

let q = User.query()
if (true) q.where('email', 'like', '%company%')
await q
```

---

