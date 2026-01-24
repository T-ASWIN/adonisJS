1.node ace configure @adonisjs/lucid

2.if we want to creat a db image we can use 

dbdiagram.io

3.
---

# 🧠 1️⃣ What is Lucid?

**Lucid** is AdonisJS’s official **ORM (Object Relational Mapper)**.

ORM means:

> You work with JavaScript/TypeScript objects → Lucid converts that into SQL → DB runs it.

Instead of writing raw SQL:

```sql
SELECT * FROM users WHERE id = 1;
```

You do:

```ts
const user = await User.find(1)
```

✨ Much cleaner.

---

## ✅ What Lucid gives you

* Models (`User`, `Movie`, etc.)
* Relationships (`hasMany`, `belongsTo`)
* Querying API
* Migrations
* Schema builder
* Hooks
* Serialization

---

---

# 🧠 2️⃣ What is Schema?

**Schema** is part of **Lucid migrations**.

It’s used to:

👉 define database table structure in code.

You don’t create tables manually in MySQL/Postgres—
you write **migration files**.

---

## ✅ Example migration

```ts
import { BaseSchema } from '@adonisjs/lucid/schema'

export default class extends BaseSchema {
  protected tableName = 'users'

  async up() {
    this.schema.createTable(this.tableName, (table) => {
      table.increments('id')
      table.string('email').unique()
      table.timestamps(true)
    })
  }

  async down() {
    this.schema.dropTable(this.tableName)
  }
}
```

Here:

* `this.schema` = Schema builder
* describes columns
* runs SQL behind the scenes

---

---

# 🧠 3️⃣ What is Query Builder?

The **Query Builder** is the lower-level API Lucid uses to build SQL queries.

If ORM = high-level
then Query Builder = mid-level.

---

## ✅ Example Query Builder

```ts
const users = await db
  .from('users')
  .where('active', true)
  .orderBy('created_at', 'desc')
```

This:

* doesn’t use a Model
* directly queries table
* still safer than raw SQL
* chainable API

---

---

# 📊 Lucid vs Schema vs Query Builder

| Layer         | Purpose                      | Used when                    |
| ------------- | ---------------------------- | ---------------------------- |
| Lucid ORM     | Work with models & relations | Normal app logic             |
| Schema        | Define tables via migrations | DB structure                 |
| Query Builder | Build SQL queries            | Reporting, joins, raw access |

---

---

# 🧠 How they connect (flow)

```
Migration (Schema) → creates tables
        ↓
Lucid Model → represents table
        ↓
Query Builder → builds SQL queries
        ↓
Database
```

---

---

# 🔑 One-liner memory trick

• **Schema** = table design
• **Lucid** = JS object for rows
• **Query Builder** = SQL generator

---



4.migrations

build up and tear down system its also us to easily make changes


5.
---

# ✅ 1️⃣ Migration Commands Explained

---

## 🔹 `migration:status`

```bash
node ace migration:status
```

Shows:

* Which migrations have **run**
* Which are still **pending**

Output looks like:

```
✓ 169020101_users
✗ 169020102_posts
```

👉 Great for debugging or CI pipelines.

---

## 🔹 `migration:run`

```bash
node ace migration:run
```

Runs **all pending migrations**.

---

## 🔹 `migration:rollback`

```bash
node ace migration:rollback
```

Rolls back the **last batch** of migrations.

👉 A *batch* = group of migrations executed together.

---

### 🔥 Rollback a Specific Batch

```bash
node ace migration:rollback --batch=8
```

This undoes only migrations from batch 8.

Useful when:

* One deployment broke something
* You want to revert only that release.

---

---

## 🔹 `migration:refresh`

```bash
node ace migration:refresh
```

👉 Does:

1. Rollback **all**
2. Run everything again

Used mostly in **development** when schema changed a lot.

⚠️ **Deletes data** — never in production unless planned.

---

---

## 🔹 `migration:reset`

```bash
node ace migration:reset
```

👉 Rolls back **everything**
Does NOT re-run.

Database becomes empty.

---

---

# ✅ 2️⃣ Models in AdonisJS (Lucid ORM)

Create model:

```bash
node ace make:model User
```

Creates:

```
app/Models/User.ts
```

Models represent **tables** and let you:

* Query data
* Define relations
* Apply hooks

---

---

# 🔹 `@column()` Decorator

Used to map table columns to model properties.

Example:

```ts
import { BaseModel, column } from '@ioc:Adonis/Lucid/Orm'

export default class User extends BaseModel {
  @column()
  public email: string
}
```

---

---

## 🔹 `isPrimary`

Marks primary key.

```ts
@column({ isPrimary: true })
public id: number
```

---

---

## 🔹 `autoCreate` & `autoUpdate`

Used for timestamps.

```ts
@column.dateTime({ autoCreate: true })
public createdAt: DateTime

@column.dateTime({ autoCreate: true, autoUpdate: true })
public updatedAt: DateTime
```

👉 Lucid will:

* Set createdAt automatically on insert
* Update updatedAt automatically on update.

---

---

# ✅ 3️⃣ Nullable Columns → TypeScript Best Practice

If DB column allows NULL:

```ts
table.string('avatar_url').nullable()
```

Then in model:

```ts
@column()
public avatarUrl: string | null
```

🎯 **Why?**

* Matches DB reality
* Type-safe
* Prevents runtime bugs
* TypeScript forces null checks

👉 YES — this is **best practice**.

---

---

# ✅ 4️⃣ Table Names vs Model Names

Convention:

| DB Table   | Model    |
| ---------- | -------- |
| users      | User     |
| blog_posts | BlogPost |

Plural table, singular model.

Lucid automatically maps:

```
User → users
```

You can override:

```ts
public static table = 'my_users'
```

---

---

# ✅ 5️⃣ Do We Need a Model for Every Table?

👉 **No — not strictly.**

You can:

* Use Query Builder directly
* Or access table via relationships from another model

Example:

You might not create `Role.ts` model if:

* It's simple lookup data
* Only accessed via joins

But 💡 **best practice** in real projects:

➡ Create models for tables that:

* Have business logic
* Are queried often
* Have relationships
* Use hooks

---

---

# ✅ 6️⃣ Pivot Tables Explained (Many-to-Many)

A **pivot table** connects two tables in a many-to-many relationship.

Example:

```
users
roles

role_user   ← pivot
```

Pivot columns:

```
user_id
role_id
```

---

---

## 🔹 Migration for Pivot Table

```ts
this.schema.createTable('role_user', (table) => {
  table.increments('id')

  table
    .integer('user_id')
    .unsigned()
    .references('id')
    .inTable('users')
    .onDelete('CASCADE')

  table
    .integer('role_id')
    .unsigned()
    .references('id')
    .inTable('roles')
    .onDelete('CASCADE')

  table.unique(['user_id', 'role_id'])
})
```

---

---

## 🔹 Many-to-Many in Models

### User model:

```ts
@manyToMany(() => Role)
public roles: ManyToMany<typeof Role>
```

---

### Role model:

```ts
@manyToMany(() => User)
public users: ManyToMany<typeof User>
```

Lucid automatically uses:

```
role_user
```

unless specified.

---

