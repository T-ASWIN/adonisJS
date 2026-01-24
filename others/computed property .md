
Your snippet:

```js
@computed()
get fullName() {
  return `${this.firstName} ${this.lastName}`
}
```

This means 👉 **`fullName` is NOT a database column** — it’s a *virtual field* that is calculated when the model is serialized.

---

# 🔥 What is a Computed Property in Lucid?

A **computed property** is:

• Derived from existing columns
• Not stored in DB
• Added to JSON response
• Useful for formatting or combining fields
• Runs automatically when model is converted to JSON

---

# 🧠 What Can We Do With Computed Models?

Here are the **main use-cases** 👇

---

## ✅ 1. Combine Fields

Like your example:

```js
@computed()
get fullName() {
  return `${this.firstName} ${this.lastName}`
}
```

DB has:

```
first_name | last_name
```

API response becomes:

```json
{
  "firstName": "Aswin",
  "lastName": "T",
  "fullName": "Aswin T"
}
```

---

## ✅ 2. Format Values

Dates, currency, status labels etc.

```js
@computed()
get joinedDate() {
  return this.createdAt.toFormat('dd LLL yyyy')
}
```

---

## ✅ 3. Boolean Helpers

```js
@computed()
get isActive() {
  return this.status === 'active'
}
```

---

## ✅ 4. Role / Display Text

```js
@computed()
get roleName() {
  return this.roleId === 1 ? 'Admin' : 'User'
}
```

---

## ✅ 5. Mask Sensitive Data

```js
@computed()
get maskedEmail() {
  return this.email.replace(/(.{2}).+(@.+)/, '$1****$2')
}
```

---

## ✅ 6. Derived Calculations

```js
@computed()
get age() {
  return DateTime.now().year - this.dob.year
}
```

---

# ⚠️ Important Rules

### ❌ Computed fields:

• Cannot be used in SQL queries
• Cannot be used in `where()`
• Are calculated in JS after fetching
• Don’t exist in DB
• Only appear when serialized (`toJSON()`)

---

# 🔎 Example Model

```js
import { BaseModel, column, computed } from '@ioc:Adonis/Lucid/Orm'

export default class User extends BaseModel {
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

---

# 🧪 Controller Usage

```js
const users = await User.all()

return users
```

Response:

```json
[
  {
    "id": 1,
    "firstName": "Aswin",
    "lastName": "T",
    "fullName": "Aswin T"
  }
]
```

---

# 🧠 Difference: Computed vs Scope (since you asked earlier)

| Feature         | Computed | Scope     |
| --------------- | -------- | --------- |
| Runs in         | JS layer | SQL query |
| Stored in DB    | ❌        | ❌         |
| Used in where() | ❌        | ✅         |
| Alters output   | ✅        | ❌         |
| Filters data    | ❌        | ✅         |

---
