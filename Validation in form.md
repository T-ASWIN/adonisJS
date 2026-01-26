
---

# 🧠 1️⃣ What is VineJS?

**VineJS** is the validation library used by AdonisJS v6.

It checks:

✔ user input
✔ email format
✔ required fields
✔ min/max length
✔ database uniqueness
✔ normalization

before data reaches your DB.

---

---

# 🛠 2️⃣ `node ace make:validator auth`

This CLI command generates a validator file.

Usually creates:

```
app/validators/auth.ts
```

Inside you define rules like `registerValidator`.

It keeps validation logic separate from controllers 👌.

---

---

# 📜 Your Code — line by line

---

## 🔹 Import vine

```ts
import vine from '@vinejs/vine'
```

This gives access to:

• schema builders
• rules
• compile()

---

---

---

# 🔵 Main validator

```ts
export const registerValidator = vine.compile(
```

### 👉 `vine.compile()`

“Prepare this validation ruleset so it can be used quickly later.”

Takes a schema and **compiles it** into something fast to run.

Used later in controller:

```ts
await request.validateUsing(registerValidator)
```

---

---

---

## 🔹 vine.object()

```ts
vine.object({
```

Means:

> Input must be an object with these fields.

---

---

---

## 🔹 fullName

```ts
fullName: vine.string().maxLength(100).optional(),
```

### Means:

• must be a string
• max 100 characters
• optional (can be missing)

---

---

---

## 🔹 email

```ts
email: vine
  .string()
  .email()
  .normalizeEmail()
```

Chain of rules:

### ✔ string → must be text

### ✔ email() → valid email format

### ✔ normalizeEmail() → lowercases & trims

Example:

```
" TEST@GMAIL.COM "
→ "test@gmail.com"
```

---

---

### 🔥 unique() rule

```ts
.unique(async (db, value, _field) => {
  const result = await db.from('users').select('id').where('email', value)
  return result.length ? false : true
})
```

This is a **custom uniqueness check**.

---

### Parameters:

| Param  | Meaning                      |
| ------ | ---------------------------- |
| db     | DB client                    |
| value  | email entered                |
| _field | field metadata (unused here) |

---

### Query:

```ts
db.from('users').select('id').where('email', value)
```

SQL equivalent:

```sql
SELECT id FROM users WHERE email = ?
```

---

### Return:

• if rows found → ❌ email already exists → return false
• else → ✅ allowed → return true

---

---

---

## 🔹 password

```ts
password: vine.string().minLength(8),
```

Means:

✔ must be string
✔ minimum 8 characters

---

---

---

# 🧠 How is this used in controller?

In RegisterController:

```ts
async store({ request }: HttpContext) {
  const payload = await request.validateUsing(registerValidator)

  // payload now contains validated data
}
```

If validation fails:

➡ Adonis auto redirects back
➡ shows errors
➡ preserves input

---

---

# 🎯 FLOW

1️⃣ user submits form
2️⃣ controller runs validator
3️⃣ VineJS checks rules
4️⃣ DB uniqueness check
5️⃣ success → continue
6️⃣ fail → redirect back with errors

---

---

# 📌 Summary (interview ready)

• VineJS handles request validation
• `vine.compile()` prepares schema
• `.object()` defines shape
• `.optional()` makes field optional
• `.unique()` runs DB check
• validation runs before DB insert
• normalized email prevents duplicates

---


What is request.validateUsing()?

This comes from Adonis’s HTTP Request object.

It:

👉 runs the given validator (your registerValidator)
👉 checks the incoming form data
👉 returns only validated fields
👉 automatically handles errors


custom validation



---

# ✅ PART 1 — Your OLD inline `.unique()` validator

You wrote:

```ts
.unique(async (db, value, _field) => {
  const result = await db.from('users').select('id').where('email', value)
  return result.length ? false : true
})
```

---

## 🧠 What is this doing?

Inside Vine:

• `db` → database connection
• `value` → input value (email)
• `_field` → metadata (unused)

---

### SQL this runs:

```
SELECT id FROM users WHERE email = ?
```

---

### Logic:

| Result     | Meaning                |
| ---------- | ---------------------- |
| rows found | ❌ email already exists |
| no rows    | ✅ email is unique      |

Returning:

```ts
false → validation fails
true → validation passes
```

---

## ⚠ Downsides of this approach

• logic repeated in many validators
• tied only to email
• not reusable
• messy in big apps

---

---

---

# ✅ PART 2 — New version = Custom rule `isUnique`

Now you created your own rule once and reuse it everywhere 👍

Let’s break it down.

---

---

# 📦 Imports

```ts
import { FieldContext } from '@vinejs/vine/types'
import db from '@adonisjs/lucid/services/db'
import vine from '@vinejs/vine'
import { VineString, VineNumber } from '@vinejs/vine'
```

---

### These give:

• FieldContext → report validation errors
• db → run queries
• vine → rule creator
• VineString/VineNumber → extend schema types

---

---

---

# 🧠 Options type

```ts
type Options = {
  table: string
  column: string
}
```

Used to pass:

```
isUnique({ table: 'users', column: 'email' })
```

---

---

---

# 🔵 Core function — isUnique()

```ts
async function isUnique(value: unknown, options: Options, field: FieldContext)
```

This is the **actual validation logic**.

---

### Parameters:

| Param   | Meaning                 |
| ------- | ----------------------- |
| value   | user input              |
| options | table & column          |
| field   | context to report error |

---

---

### Type check:

```ts
if (typeof value !== 'string' && typeof value !== 'number') {
  return
}
```

Only check strings/numbers.

---

---

### DB query:

```ts
const result = await db
  .from(options.table)
  .select(options.column)
  .where(options.column, value)
  .first()
```

⚠️ NOTE: In your snippet you wrote:

```
.from(options.column)
```

That looks like a typo — it should be:

```
.from(options.table)
```

Otherwise it will query wrong table.

---

---

### If found:

```ts
if (result) {
  field.report('This {{field}} is already taken', 'isUnique', field)
}
```

This tells Vine:

👉 validation failed for this field.

---

---

---

# 🧱 Create rule

```ts
export const isUniqueRule = vine.createRule(isUnique)
```

Wraps function into Vine rule.

---

---

---

# 🧬 Module augmentation

```ts
declare module '@vinejs/vine' {
  interface VineString {
    isUnique(options: Options): this
  }

  interface VineNumber {
    isUnique(options: Options): this
  }
}
```

This tells TypeScript:

> “Hey — `vine.string()` now has `.isUnique()` method.”

Otherwise TS would error.

---

---

---

# 🧩 Macros

```ts
VineString.macro('isUnique', function (this: VineString, options: Options) {
  return this.use(isUniqueRule(options))
})
```

Same for numbers.

This is what lets you write:

```ts
vine.string().isUnique(...)
```

---

---

---

# ✅ PART 3 — Using it in validator

Now in validator:

```ts
email: vine
  .string()
  .email()
  .normalizeEmail()
  .isUnique({ table: 'users', column: 'email' }),
```

Meaning:

✔ must be string
✔ valid email
✔ normalized
✔ must NOT exist in users.email column

---

---

---

# 🎯 FINAL COMPARISON

| Old way         | New way                    |
| --------------- | -------------------------- |
| inline DB query | reusable rule              |
| repeated        | DRY                        |
| tied to email   | works for any table/column |
| messy           | clean                      |
| not extendable  | extend Vine properly       |

---

# 🧠 Ultra-simple summary

### 👉 Old `.unique()`:

Manual DB check inside validator.

### 👉 New `isUnique()`:

Custom Vine rule that:
• queries DB
• reports error
• reusable everywhere
• clean syntax

---
---

# 🧠 The Problem First

You created a custom rule:

```
isUnique()
```

But by default…

👉 `vine.string()` does **NOT** have `.isUnique()`.

So if you wrote:

```ts
vine.string().isUnique(...)
```

TypeScript would scream:

❌ Property 'isUnique' does not exist.

---

So you must:

1️⃣ Teach Vine at runtime
2️⃣ Teach TypeScript at compile time

Those are two different things.

---

# 🔵 Why MACRO is used

This part:

```ts
VineString.macro('isUnique', function (this: VineString, options: Options) {
  return this.use(isUniqueRule(options))
})
```

### 👉 Macro = adds a new method dynamically.

At runtime this literally does:

```
vine.string().isUnique(...)
```

➡ works.

Without macro → method doesn’t exist.

---

# 🔵 Why MODULE + INTERFACE is used

This part:

```ts
declare module '@vinejs/vine' {
  interface VineString {
    isUnique(options: Options): this
  }
}
```

### 👉 This is for TypeScript only.

It says:

> “Trust me — VineString now has an isUnique() method.”

So:

✔ no red underline
✔ autocomplete works
✔ type checking works

---

# ⚡ Key idea

| Thing                 | Purpose               |
| --------------------- | --------------------- |
| macro                 | add method at runtime |
| interface declaration | tell TypeScript       |
| both                  | needed                |

---

# 🎯 Real-life analogy

You buy a new drill attachment.

• **macro** = physically attach it to the drill
• **interface declaration** = update the instruction manual

If you skip one:

• attach but no manual → confusion
• manual but no attachment → useless

---

# 🧠 Ultra-simple summary

👉 **Macro = makes `.isUnique()` actually exist.**
👉 **Interface/module = tells TypeScript that it exists.**

Both are required.

---

