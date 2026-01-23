
---
# node ace make:preload globals
# 🧠 What does “instance” mean here?

In a web app:

👉 **Each incoming HTTP request = one instance (request lifecycle)**

When User A opens `/dashboard` and User B opens `/dashboard`:

* Two separate requests
* Two separate contexts
* Two separate `ctx` objects
* Two separate `auth.user`
* Two separate response renders

So:

> **Instance = one request for one user at one time.**

---

# ✅ Example: UI changes per user

Imagine:

* Admin user → sees “Delete” button
* Normal user → doesn’t

That difference comes from **request-specific data**.

---

---

# 🧩 `view.share()` → request-level (instance-level)

When you do inside a controller or middleware:

```ts
view.share({
  user: auth.user,
})
```

This means:

👉 “For THIS request, make `user` available to all views rendered.”

It does **not** affect other users.

Next request = fresh instance.

---

---

# 🧠 How UI becomes different per user

Flow:

```
Browser request
   ↓
Adonis creates HttpContext
   ↓
auth middleware attaches user
   ↓
controller / middleware calls view.share()
   ↓
views read auth.user / shared data
   ↓
HTML differs for each user
```

---

---

# 🧩 Preload → application-level boot-time

Preload files run when the app starts (server boot).

Used for:

* registering globals
* macros
* helpers
* global view data that is NOT user-specific
* config-level logic

Example:

```ts
View.global('appName', 'MovieApp')
```

That’s same for **everyone**.

---

---

# 📊 Difference table

| Feature                | Instance (`view.share`) | Preload / global |
| ---------------------- | ----------------------- | ---------------- |
| Per request?           | ✅ Yes                   | ❌ No             |
| Per user UI?           | ✅ Yes                   | ❌ No             |
| Access auth.user?      | ✅ Yes                   | ❌ No             |
| Example                | logged-in user          | app name         |
| Changes every refresh? | Yes                     | No               |

---

---

# 🧠 Real-world mapping

### Instance-level → UI personalization:

* username
* role
* cart count
* notifications
* theme preference
* permissions

### Application-level → constants:

* app title
* company name
* CDN url
* feature flags
* global helpers

---

---

# 🔑 One-line summary

> **Instance data = per request / per user**
> **Preload data = global / app-wide**
> That’s how the UI changes for different users.

---
