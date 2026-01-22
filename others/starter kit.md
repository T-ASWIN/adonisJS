
---

# 🧠 What are Starter Kits?

A **starter kit** is a **pre-built project template**.

Instead of starting with:

```
empty folder
npm init
install packages
configure auth
setup linting
setup DB
configure views
configure tests
```

…the starter kit gives you:

👉 a ready-made AdonisJS app
👉 with common features already wired up.

---

# 📦 In AdonisJS, when you run:

```bash
npm init adonisjs@latest -- -K="github:..."
```

You’re telling the Adonis CLI:

➡️ “Create a new project using this starter kit from GitHub.”

So:

* `-K` = kit
* `github:...` = repo URL
* `./kit-example` = folder name
* `y` = confirm

---

# 🎯 Why do we use starter kits?

---

## ✅ 1) Save time

No need to manually configure:

* authentication
* sessions
* CSRF
* database
* migrations
* Edge views
* Tailwind
* ESLint
* tests

All that comes pre-done.

---

---

## ✅ 2) Follow best practices

Starter kits are usually:

* written by framework maintainers
* structured properly
* production-ready
* secure by default

So you avoid beginner mistakes.

---

---

## ✅ 3) Faster onboarding

For teams:

* new dev clones repo
* runs `npm install`
* works immediately

No 2-day setup.

---

---

## ✅ 4) Opinionated structure

They enforce:

* folder layout
* naming conventions
* auth patterns
* middleware
* config

This keeps big projects consistent.

---

---

## ✅ 5) Built-in features

Typical starter kits include:

* login/register
* roles
* email
* queues
* admin UI
* Tailwind
* pagination
* flash messages

---

# 🧠 When should you use a starter kit?

Use one when:

✔ building CRUD apps
✔ dashboards
✔ SaaS MVPs
✔ admin panels
✔ internal tools
✔ learning Adonis quickly
✔ hackathons

---

# ❌ When NOT to use a starter kit?

Skip if:

* you want full custom architecture
* very minimal API-only service
* learning internals step-by-step
* library package

---

# 📝 Your line — improved

You wrote:

> **“why we use starter kits”**

Better:

> **“Why do we use starter kits?”**

---
