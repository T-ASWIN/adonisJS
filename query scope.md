qwait models.movie.query().withScopes(scope=>scope.released())
---

# 🧠 What is a Scope in Lucid?

A **scope** is:

👉 a reusable query fragment
👉 attached to a model
👉 used to avoid repeating common filters
👉 runs at SQL level
👉 chainable
👉 keeps controllers clean

Think of it like:

> “Saved filter logic for this model.”

---

# 📌 Your Example Explained

```ts
static released = scope((query) => {
  query.where((group) =>
    group
      .where('statusId', MovieStatuses.RELEASED)
      .whereNotNull('releasedAt')
      .where('releasedAt', '<=', DateTime.now().toSQL())
  )
})
```

This defines a scope named **released**.

When used:

```ts
await Movie.query().apply(scopes => scopes.released())
```

SQL roughly:

```sql
WHERE (
  status_id = 2
  AND released_at IS NOT NULL
  AND released_at <= NOW()
)
```

---

# 🧩 Why Use Scopes?

Instead of repeating:

```ts
Movie.query()
  .where('statusId', MovieStatuses.RELEASED)
  .whereNotNull('releasedAt')
  .where('releasedAt', '<=', DateTime.now().toSQL())
```

…everywhere…

You define once → reuse everywhere 😍

---

# 📍 Where Can We Use Scopes?

You can use scopes in:

✔ Controllers
✔ Services
✔ Jobs
✔ Anywhere you query the model

Basically:

```ts
Model.query().apply(scopes => scopes.scopeName())
```

---

# ✍️ Scope Syntax (AdonisJS v5+)

Import:

```ts
import { scope } from '@ioc:Adonis/Lucid/Orm'
```

Define inside model:

```ts
static active = scope((query) => {
  query.where('isActive', true)
})
```

---

# 🧪 How to Call a Scope

### Method 1 – `.apply()`

```ts
const movies = await Movie
  .query()
  .apply(scopes => scopes.released())
```

---

### Method 2 – chain with others

```ts
const movies = await Movie
  .query()
  .apply(scopes => {
    scopes.released()
    scopes.popular()
  })
```

---

# 🔥 Few Real-World Examples

---

## ✅ Example 1 — Active Users

```ts
static active = scope((query) => {
  query.where('status', 'active')
})
```

Usage:

```ts
await User.query().apply(s => s.active())
```

---

---

## ✅ Example 2 — Recently Created

```ts
static recent = scope((query) => {
  query.where('created_at', '>=', DateTime.now().minus({ days: 7 }).toSQL())
})
```

---

---

## ✅ Example 3 — Published Posts

```ts
static published = scope((query) => {
  query.whereNotNull('publishedAt')
})
```

---

---

## ✅ Example 4 — Grouped Conditions

```ts
static visible = scope((query) => {
  query.where((group) => {
    group
      .where('isPublic', true)
      .orWhere('ownerId', auth.user!.id)
  })
})
```

---

---

## ✅ Example 5 — Parameterized Scope

Scopes can accept arguments 👀

```ts
static byYear = scope((query, year: number) => {
  query.whereRaw('EXTRACT(YEAR FROM released_at) = ?', [year])
})
```

Usage:

```ts
Movie.query().apply(s => s.byYear(2024))
```

---

# ⚠️ Important Things to Know

### ❌ Scope ≠ Computed

| Feature            | Scope     | Computed         |
| ------------------ | --------- | ---------------- |
| Runs in            | SQL query | JS serialization |
| Used for filtering | ✅         | ❌                |
| Stored in DB       | ❌         | ❌                |

---

### ❌ Scopes don’t return values

They **modify the query** — not return data.

---

---

# 🧠 When Should You Use Scopes?

Use scopes when:

✔ you reuse same filters
✔ business rules live in model
✔ controllers are getting messy
✔ common WHERE clauses repeat
✔ permission logic is shared

---

# 🧾 TL;DR

👉 Scope = reusable SQL filter
👉 Defined in model using `static name = scope(...)`
👉 Called using `.apply()`
👉 Can accept params
👉 Keeps code DRY

---

