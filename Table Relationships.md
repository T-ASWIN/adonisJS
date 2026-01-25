
---

# 📘 One-to-One Relationship in AdonisJS (Lucid ORM)

This guide explains how to create and use a **one-to-one relationship** between two models in AdonisJS.

Example:

> A **User** has **one Profile**
> A **Profile belongs to one User**

---

# 🧠 Database Design

We store the foreign key in the child table.

### users table

```
id | name | email
```

### profiles table

```
id | description | user_id
```

👉 `profiles.user_id` → references `users.id`

---

# 📦 Models Setup

---

## 👤 User Model

```ts
import { BaseModel, column, hasOne } from '@adonisjs/lucid/orm'
import Profile from '#models/profile'
import type { HasOne } from '@adonisjs/lucid/types/relations'

export default class User extends BaseModel {
  @column({ isPrimary: true })
  declare id: number

  @hasOne(() => Profile, {
    foreignKey: 'userId',
  })
  declare profile: HasOne<typeof Profile>
}
```

---

---

## 🪪 Profile Model

```ts
import { BaseModel, column, belongsTo } from '@adonisjs/lucid/orm'
import User from '#models/user'
import type { BelongsTo } from '@adononisjs/lucid/types/relations'

export default class Profile extends BaseModel {
  @column({ isPrimary: true })
  declare id: number

  @column()
  declare userId: number

  @column()
  declare description: string

  @belongsTo(() => User, {
    foreignKey: 'userId',
  })
  declare user: BelongsTo<typeof User>
}
```

---

# 🔑 What is `foreignKey`?

```ts
foreignKey: 'userId'
```

This tells Lucid:

👉 “Use `userId` column to link these tables.”

---

# 📥 Insert Data With Relationship (Normal Way)

---

## Create User → Then Profile

```ts
const user = await User.create({
  fullName: 'Aswin',
  email: 'aswin@test.com',
})

await user.related('profile').create({
  description: 'Backend developer',
})
```

Lucid automatically sets:

```
profile.userId = user.id
```

---

# 🏭 Insert Using Factory Relations

Factories help generate fake data **with relationships**.

---

## UserFactory

```ts
.relation('profile', () => ProfileFactory)
```

---

## ProfileFactory

```ts
.relation('user', () => UserFactory)
```

---

## Create Users With Profiles

```ts
await UserFactory.with('profile').createMany(5)
```

👉 This creates:

• 5 users
• each gets a profile
• foreign key is wired automatically

---

# 🧪 Using in REPL

Start REPL:

```
node ace repl
```

---

## Fetch user with profile

```ts
const user = await User.query().preload('profile').first()
```

---

## Access related data

```ts
user!.profile.description
```

---
manu to one 

@hasMany(() => User, {
    localKey: 'id',
    foreignKey: 'roleId',
  })
  declare users: HasMany<typeof User>
