node ace migration:rollback --batch=2

---

# 🗄️ Core Migration Commands

---

## ✅ `node ace migration:run`

👉 Runs **all pending migrations**.

Use when:

• setting up DB first time
• after creating a new migration
• pulling new migrations from GitHub

---

---

## ✅ `node ace migration:rollback`

👉 Rolls back the **last batch only**.

Each time you run migrations, Adonis groups them into a batch.

Use when:

• last migration was wrong
• you want to undo only recent changes

---

---

## ✅ `node ace migration:refresh`

👉 Rolls back **everything** and re-runs all migrations.

Equivalent to:

```
rollback all → run again
```

Use when:

• changing many migrations locally
• resetting dev DB
• starting fresh

⚠️ Dangerous in production.

---

---

# ➕ Other Important Migration Commands

---

## ✅ `node ace migration:status`

👉 Shows which migrations:

• have run ✅
• are pending ⏳

Very useful to debug.

---

---

## ✅ `node ace migration:reset`

👉 Rolls back **ALL migrations**, but does NOT rerun them.

Leaves DB empty (no tables).

Good for:

• total cleanup
• switching schemas
• before refresh manually

---

---

## ✅ `node ace migration:fresh` (if available in your version)

👉 Drops all tables and reruns migrations from scratch.

More aggressive than refresh.

---

---

## ✅ `node ace make:migration create_movies_table`

👉 Creates a new migration file.

Example:

```
node ace make:migration create_movies_table
```

Generates timestamped file in:

```
database/migrations/
```

---

---

# 🧠 Quick Comparison Table

| Command            | What it does    | Typical Use |
| ------------------ | --------------- | ----------- |
| migration:run      | Run pending     | Normal dev  |
| migration:rollback | Undo last batch | Fix mistake |
| migration:refresh  | Reset + rerun   | Dev reset   |
| migration:reset    | Rollback all    | Clean DB    |
| migration:status   | Show state      | Debug       |
| make:migration     | Create file     | New table   |

---

---

