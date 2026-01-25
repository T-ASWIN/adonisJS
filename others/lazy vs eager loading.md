
---

# 🧠 Big Idea

When you fetch records **with relationships**, you have two main strategies:

👉 **Lazy loading**
👉 **Eager loading**

The difference is **when** related data is fetched.

---

# 🟡 Lazy Loading = “Fetch later if needed”

You first fetch the main model…

Then later, when you access the relation, another query runs.

---

## 📌 Example (AdonisJS)

```ts
const user = await User.find(1)

// Later...
await user?.load('profile')
```

### What happens:

1️⃣ Query #1 → fetch user
2️⃣ Query #2 → fetch profile for that user

That’s **lazy loading**.

---

### ⚠️ Problem with lazy loading:

If you do this in a loop:

```ts
const users = await User.all()

for (const user of users) {
  await user.load('profile')
}
```

That becomes:

👉 1 query for users
👉 N queries for profiles

This is called the **N+1 problem** 😬

---

---

# 🟢 Eager Loading = “Fetch everything up front”

You load relations in the **same operation**.

---

## 📌 Example

```ts
const users = await User
  .query()
  .preload('profile')
```

### What happens:

1️⃣ Query #1 → users
2️⃣ Query #2 → all profiles for those users

Still only **2 queries**, not N+1 👍

---

---

# 📊 Comparison

| Feature               | Lazy Loading     | Eager Loading      |
| --------------------- | ---------------- | ------------------ |
| When query runs       | Later, on demand | Immediately        |
| Risk of N+1 queries   | High             | Low                |
| Performance for lists | ❌ Bad            | ✅ Good             |
| Code style            | Simple sometimes | Explicit           |
| Best for              | Single record    | Lists / dashboards |

---

# 🎯 When Should You Use Which?

### Use **Lazy loading** when:

✔ you only have ONE record
✔ relation is optional
✔ you’re not sure you’ll need it
✔ admin screens
✔ debugging

---

### Use **Eager loading** when:

✔ you know you’ll show the relation
✔ you are fetching many records
✔ API response includes relation
✔ frontend expects nested data

---

# 🧾 Memory Hook

👉 **Lazy = later**
👉 **Eager = early**

or

👉 **Lazy = extra queries**
👉 **Eager = efficient**

---

# 🔥 Bonus: Factory Tie-in

Earlier you saw:

```ts
UserFactory.with('profile')
```

That’s basically **eager creation** 😄 — create related rows immediately.

---

