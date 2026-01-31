
---

# ✅ 1) Single Route

A **single route** maps **one URL → one handler**.

### 👉 Example:

```ts
import router from '@adonisjs/core/services/router'

router.get('/hello', async () => {
  return 'Hello World'
})
```

### 🔍 Meaning:

| Part     | What it does             |
| -------- | ------------------------ |
| `/hello` | URL path                 |
| `get`    | HTTP method              |
| function | runs when URL is visited |

📍 If user goes to:

```
http://localhost:3333/hello
```

👉 They see: **Hello World**

---

---

# ✅ 2) Resource Route

A **resource route** auto-creates **CRUD routes** for you.

CRUD = Create, Read, Update, Delete.

### 👉 Example:

```ts
router.resource('posts', 'PostsController')
```

This single line creates 👇

| URL             | Method    | Controller Method |
| --------------- | --------- | ----------------- |
| /posts          | GET       | index             |
| /posts/:id      | GET       | show              |
| /posts          | POST      | store             |
| /posts/:id      | PUT/PATCH | update            |
| /posts/:id      | DELETE    | destroy           |
| /posts/create   | GET       | create            |
| /posts/:id/edit | GET       | edit              |

💡 So instead of writing 7 routes manually…
**one resource does it all** 🚀

---

---

# ✅ 3) Route Group

A **group** lets you organize routes together.

### 👉 Example:

```ts
router.group(() => {
  router.get('/profile', 'UsersController.profile')
  router.post('/logout', 'AuthController.logout')
})
```

🧠 Why use groups?

• To apply middleware to many routes
• To add prefix
• Cleaner code

---

---

# ✅ 4) Prefix

**Prefix** adds something in front of all URLs in the group.

### 👉 Example:

```ts
router.group(() => {
  router.get('/users', 'UsersController.index')
  router.post('/users', 'UsersController.store')
}).prefix('/admin')
```

### 📍 URLs become:

```
/admin/users
/admin/users
```

---

---

# ✅ 5) `as` (Route Name)

`as` gives a **name** to a route.

Useful when generating URLs inside views or redirects.

### 👉 Example:

```ts
router.get('/login', 'AuthController.login').as('login')
```

Now its name is:

```
login
```

You can later refer to it by name instead of URL.

---

### 👉 In group:

```ts
router.group(() => {
  router.get('/dashboard', 'DashboardController.index').as('dashboard')
}).as('admin')
```

Named route becomes:

```
admin.dashboard
```

---

---

# ✅ 6) Middleware

**Middleware = security guard 🛡️**

It runs **before** the request reaches the controller.

Used for:

✔ auth check
✔ role check
✔ logging
✔ rate limit

---

### 👉 Single route middleware:

```ts
router.get('/dashboard', 'DashboardController.index')
  .middleware(['auth'])
```

---

---

### 👉 Group middleware:

```ts
router.group(() => {
  router.get('/dashboard', 'DashboardController.index')
  router.get('/settings', 'SettingsController.index')
}).middleware(['auth'])
```

Now both routes require login 🔐

---

---

# ✅ Combine EVERYTHING (Group + Prefix + Middleware + as)

🔥 Real-world style:

```ts
router.group(() => {
  router.get('/dashboard', 'DashboardController.index').as('dashboard')
  router.get('/users', 'UsersController.index').as('users')
})
  .prefix('/admin')
  .middleware(['auth'])
  .as('admin')
```

---

### 📍 What happens?

| Feature    | Result             |
| ---------- | ------------------ |
| Prefix     | `/admin/dashboard` |
| Middleware | must login         |
| Route name | `admin.dashboard`  |
| Route name | `admin.users`      |

---

---

# 🧠 Simple Summary

| Feature      | Purpose                |
| ------------ | ---------------------- |
| Single route | One URL → one function |
| resource     | CRUD routes auto       |
| group        | organize routes        |
| prefix       | add common URL         |
| as           | name route             |
| middleware   | protect route          |

---
Great question, Aswin 👍 — this is *fundamental* in backend development.

In AdonisJS (and most web frameworks), **GET** and **POST** are HTTP methods.
They tell the server **what kind of action** the client is trying to do.

Think like this:

> 📥 **GET = ask for data**
> 📤 **POST = send data**

Let’s go slow and simple 👇

---

# ✅ When to Use `GET`

Use **GET** when:

✔ You are **fetching / viewing** something
✔ No data is being changed in the database
✔ Opening a page
✔ Loading a list or details

---

### 👉 Example:

```ts
router.get('/users', 'UsersController.index')
```

👆 This means:

> “When someone visits `/users`, show them the users list.”

---

### 📍 Another example:

```ts
router.get('/profile', 'ProfileController.show')
```

Used to:

• open profile page
• show user info

---

---

# ✅ When to Use `POST`

Use **POST** when:

✔ User submits a form
✔ Data is being **created**
✔ Something is saved in DB
✔ Login / Register
✔ Upload file

---

### 👉 Example:

```ts
router.post('/register', 'AuthController.store')
```

👆 This means:

> “User sends registration data → save it.”

---

---

# 🧠 Super Simple Comparison

| Method | What it does  | Changes data? |
| ------ | ------------- | ------------- |
| GET    | Read / fetch  | ❌ No          |
| POST   | Send / create | ✅ Yes         |

---

---

# 🔥 Real-World Website Flow

### 📝 Registration Page:

1️⃣ User opens page → GET

```ts
router.get('/register', 'AuthController.showRegister')
```

2️⃣ User submits form → POST

```ts
router.post('/register', 'AuthController.register')
```

---

---

# 📌 Another Example: Blog

### Show posts:

```ts
router.get('/posts', 'PostsController.index')
```

### Create post:

```ts
router.post('/posts', 'PostsController.store')
```

---

---

# 🛑 Important Rule (Interview Tip 💡)

👉 **GET should NOT change anything in DB**
👉 **POST is for creating data**

Interview-style answer:

> GET is used to retrieve data from the server, while POST is used to send data to the server to create or modify records.

---

---

# ⚡ Quick Mental Trick

Ask yourself:

🧠 “Is the user *asking* for something?” → GET
🧠 “Is the user *sending* something to be saved?” → POST

---


