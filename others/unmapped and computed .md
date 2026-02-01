Good one, Aswin 👍 — **“unmapped” and “computed”** are two special property types you’ll see in **Lucid models** in AdonisJS.

They both describe **fields that are NOT normal database columns**, but they serve different purposes.

Let’s go slow and super clear 👇

---

# 🧠 Big Picture

In a Lucid model, most properties map directly to DB columns:

```
movies table  →  Movie.title
movies table  →  Movie.releasedAt
```

But sometimes you want:

• a field that exists only in code
• or a value calculated from other columns
• or temporary data
• or pivot extras
• or helper flags

That’s where **unmapped** and **computed** come in.

---

# ✅ 1) Unmapped Properties

### 👉 What “unmapped” means:

> A property that **does NOT correspond to any database column** and is **not automatically serialized**.

It lives only in memory.

Think:

🧠 “temporary variable on the model object.”

---

## 📌 Why Use It?

Use unmapped fields for:

• runtime flags
• helper values
• request-only state
• internal logic
• caching something
• UI hints

---

## ✅ Example (Conceptual)

```ts
export default class Movie extends BaseModel {
  declare isEditable: boolean
}
```

Here:

• `isEditable` is NOT in DB
• no column called `is_editable`
• not saved
• not loaded
• you assign it manually:

```ts
movie.isEditable = true
```

Gone after request finishes.

---

---

# ✅ 2) Computed Properties

### 👉 What “computed” means:

> A virtual property calculated from other fields and usually included in JSON output.

It’s derived, not stored.

---

## 📌 Why Use It?

Use computed fields for:

• full names
• formatted dates
• status labels
• URLs
• totals
• booleans based on logic

---

## ✅ Example

```ts
import { computed } from '@adonisjs/lucid/orm'

export default class Cineast extends BaseModel {
  @column()
  declare firstName: string

  @column()
  declare lastName: string

  @computed()
  get fullName() {
    return `${this.firstName} ${this.lastName}`
  }
}
```

Now:

```
cineast.fullName
```

is available…

even though there is no `full_name` column.

---

---

# ⚖️ Unmapped vs Computed (Side by Side)

| Feature               | Unmapped     | Computed          |
| --------------------- | ------------ | ----------------- |
| Stored in DB          | ❌            | ❌                 |
| Derived automatically | ❌            | ✅                 |
| Decorator used        | none         | `@computed()`     |
| Appears in JSON       | ❌ by default | ✅                 |
| Depends on DB fields  | maybe / no   | usually yes       |
| Used for              | temp flags   | calculated output |

---

---

# 🧠 Simple One-Liners

• **Unmapped** = temporary property on model
• **Computed** = virtual calculated field

---

---

