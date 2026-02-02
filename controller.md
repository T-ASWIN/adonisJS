Great question 👍 — this is **core AdonisJS knowledge**, and interviewers *love* this.

When you write a controller method like:

```ts
async index({ request, response, auth, inertia, params }: HttpContext) {
```

Those things inside `{}` come from **HttpContext**.

👉 **HttpContext = one object that contains everything about the current HTTP request.**

You destructure only what you need.

Let’s go through **each important one**, what it is, when to use it, and what kind of data it carries 👇

---

# 🧠 HttpContext — Big Picture

Every request gives you:

```
request  → incoming data
response → outgoing reply
auth     → logged-in user
params   → route params
session  → session data
inertia  → render Inertia pages
view     → render Edge templates
logger   → logs
```

You **never need all of them** — only pick what the method uses.

---

# ✅ 1️⃣ `request` — Incoming Data

### 📌 What it is:

Represents the HTTP request.

### 📦 Contains:

* query string (`?page=2`)
* form body
* JSON payload
* headers
* cookies
* files
* method (GET/POST)
* IP address

### 🛠️ When to use:

👉 Anytime you accept data from the client.

---

### Example:

```ts
const email = request.input('email')
const data = request.only(['email', 'password'])
const page = request.qs().page
```

---

---

# ✅ 2️⃣ `response` — Sending Back Data

### 📌 What it is:

Used to send the HTTP response.

### 📦 Can do:

* redirect
* set cookies
* set status codes
* send JSON
* download files
* return plain text

### 🛠️ When to use:

👉 After processing something — especially POST/PUT/DELETE.

---

### Example:

```ts
return response.redirect('/login')

response.status(201).json({ success: true })
```

---

---

# ✅ 3️⃣ `auth` — Authentication Info

### 📌 What it is:

Handles logged-in users.

### 📦 Contains:

* current user → `auth.user`
* login/logout methods
* guards

### 🛠️ When to use:

👉 When the route requires login or user info.

---

### Example:

```ts
const user = auth.user

await auth.use('web').logout()
```

---

---

# ✅ 4️⃣ `params` — Route Parameters

### 📌 What it is:

Dynamic URL parts.

### 📦 Example URL:

```
/movies/5
```

👉 `params.id === 5`

### 🛠️ When to use:

👉 For `show`, `edit`, `delete` routes.

---

### Example:

```ts
const id = params.id
```

---

---

# ✅ 5️⃣ `session` — Flash & Session Data

### 📌 What it is:

Stores small data between requests.

### 📦 Used for:

* flash messages
* temporary state
* auth session

### 🛠️ When to use:

👉 To show success messages after redirect.

---

### Example:

```ts
session.flash('success', 'Account created')
```

---

---

# ✅ 6️⃣ `inertia` — Render Inertia Pages 🚀

### 📌 What it is:

Used in Inertia apps to return frontend pages.

### 📦 Sends:

* component name
* props

### 🛠️ When to use:

👉 In SPA-style apps (Vue/React frontend).

---

### Example:

```ts
return inertia.render('movies/index', {
  movies,
})
```

---

---

# ✅ 7️⃣ `view` — Render Server Templates

### 📌 What it is:

Used with Edge templates (non-Inertia).

### 📦 Renders:

* `.edge` HTML files

### 🛠️ When to use:

👉 Traditional server-rendered apps.

---

### Example:

```ts
return view.render('welcome')
```

---

---

# ✅ 8️⃣ `logger` — Logging

### 📌 What it is:

Write logs.

### 🛠️ When to use:

👉 Debugging or production logs.

---

### Example:

```ts
logger.info('User registered')
```

---

---

# ✅ 9️⃣ `bouncer` — Authorization (Roles/Permissions)

### 📌 What it is:

Checks permissions.

### 🛠️ When to use:

👉 Before allowing sensitive actions.

---

### Example:

```ts
await bouncer.authorize('deleteMovie', movie)
```

---

---

# ✅ 1️⃣0️⃣ `route` — Current Route Info

### 📌 What it is:

Metadata about matched route.

---

---

# 🧠 When to Use Which?

| Param      | Use When               |
| ---------- | ---------------------- |
| `request`  | Reading input          |
| `response` | Redirect / send result |
| `auth`     | Logged-in user         |
| `params`   | URL ID                 |
| `session`  | Flash messages         |
| `inertia`  | SPA frontend           |
| `view`     | Server HTML            |
| `logger`   | Debugging              |
| `bouncer`  | Authorization          |

---

# 🎤 Interview-Ready Summary

Memorize this 👇

> In AdonisJS, controller methods receive an HttpContext object that contains helpers like request, response, auth, params, session, inertia, and view. Request is used to read client input, response sends the output, auth provides the logged-in user, params holds route parameters, inertia renders SPA pages, and view renders Edge templates. Developers destructure only what they need.

---

If you want next 😄 —
I can explain:

👉 middleware
👉 guards
👉 policies
👉 validators
👉 services vs controllers

just tell me 🔥
