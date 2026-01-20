## What is AdonisJS?

AdonisJS is a **full-stack Node.js web framework** designed to feel familiar to developers coming from **Laravel (PHP)** or **Rails**.
It helps you build **backend applications, REST APIs, and full-stack web apps** in a **structured, opinionated way**.

![Image](https://res.cloudinary.com/adonisjs/image/upload/v1472842310/MVC-Flow-Chart_ccz2zb.jpg)

![Image](https://miro.medium.com/v2/resize%3Afit%3A802/1%2A7AQBGRch_09sncTLVqtLUA.jpeg)

![Image](https://i.sstatic.net/2M9R8.png)

---

## Why AdonisJS? (Advantages)

### ✅ 1. **All-in-One Framework**

Adonis gives you everything out of the box:

* Routing
* Controllers
* Views
* ORM
* Authentication
* Validation
* Middleware

👉 No need to glue multiple libraries like Express + Sequelize + Passport manually.

---

### ✅ 2. **MVC Architecture (Very Clean)**

Adonis follows **MVC (Model–View–Controller)** strictly:

* **Model** → Database logic
* **View** → UI / templates
* **Controller** → Business logic

This makes code:

* Easier to read
* Easier to debug
* Easier to scale

---

### ✅ 3. **TypeScript First**

Adonis is **built with TypeScript**, not added later.

* Better autocomplete
* Fewer runtime bugs
* Strong typing

Perfect for large applications.

---

### ✅ 4. **Lucid ORM (Powerful Database Layer)**

Built-in ORM similar to Laravel’s Eloquent:

* Models
* Relationships
* Migrations
* Seeders

Example:

```ts
await User.query().where('email', email).first()
```

---

### ✅ 5. **Built-in Authentication**

Supports:

* Session auth
* Token auth
* API auth
* Guards & providers

No need for external auth libraries.

---

### ✅ 6. **Great for APIs**

Adonis is excellent for:

* Admin panels
* Dashboards
* Mobile app backends
* SaaS products

---

### ✅ 7. **CLI & Developer Experience**

The `ace` CLI helps you generate everything:

```bash
node ace make:controller User
node ace make:model Product
node ace make:migration users
```

---

## Important Concepts You MUST Know in AdonisJS

### 🔑 1. **Routing**

Defines URLs and what they do.

```ts
router.get('/users', 'UsersController.index')
```

---

### 🔑 2. **Controllers**

Handles request logic.

```ts
export default class UsersController {
  async index() {
    return 'User list'
  }
}
```

---

### 🔑 3. **Views (Edge Templates)**

Adonis uses **Edge** templating.

```ts
return view.render('pages/home')
```

Used mainly for:

* Admin panels
* Server-side rendered pages

---

### 🔑 4. **Lucid Models**

Represents database tables.

```ts
export default class User extends BaseModel {
  @column({ isPrimary: true })
  id: number
}
```

---

### 🔑 5. **Middleware**

Runs before/after requests.
Used for:

* Auth checks
* Logging
* Permissions

```ts
router.get('/dashboard', 'DashboardController.index')
  .middleware('auth')
```

---

### 🔑 6. **Validation**

Validate incoming data safely.

```ts
await request.validate({
  schema: schema.create({
    email: schema.string({}, [rules.email()])
  })
})
```

---

### 🔑 7. **Dependency Injection**

Automatically injects services.

```ts
async store({ request, response }) {}
```

---

### 🔑 8. **Environment & Config**

Uses `.env` for:

* DB credentials
* API keys
* App secrets

---

## When Should You Use AdonisJS?

✅ Use AdonisJS if:

* You want **structured backend code**
* You like **Laravel-style architecture**
* You’re building **dashboards / APIs**
* You prefer **TypeScript**

❌ Avoid if:

* You want minimal setup (Express is lighter)
* You only need a tiny API

---

## Simple Comparison

| Feature    | AdonisJS    | Express    |
| ---------- | ----------- | ---------- |
| Structure  | Opinionated | Free-form  |
| ORM        | Built-in    | External   |
| Auth       | Built-in    | External   |
| TypeScript | Native      | Manual     |
| Best for   | Large apps  | Small APIs |

---
