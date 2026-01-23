
---

# 🧠 What is CSRF?

CSRF = **Cross-Site Request Forgery**

It’s a security attack where:

* You’re logged into Site A
* You visit a malicious Site B
* That site secretly sends a POST request to Site A
* Your browser includes cookies automatically
* Boom 💥 — action happens without your consent.

Example:
👉 malicious page auto-submits a form to `/redis/flush`.

---

---

# ✅ Why frameworks protect against CSRF

To prevent that, the server:

* generates a secret token per session/request
* expects it in every POST/PUT/PATCH/DELETE form
* rejects requests without it.

---

---

# 🧩 What is `{{ csrfField() }}` in AdonisJS?

(In Adonis 6 / Edge)

It outputs a hidden input like:

```html
<input type="hidden" name="_csrf" value="RANDOM_TOKEN_HERE">
```

That token must be sent with the form.

---

---

# 📍 Where do we use it?

👉 **Inside every form that changes data:**

* POST
* PUT
* PATCH
* DELETE

Example:

```edge
<form action="{{ route('redis.flush') }}" method="POST">
  {{ csrfField() }}

  @button({ type: 'submit' })
    Flush Redis Db
  @end
</form>
```

---

---

# ❌ If you don’t include it?

Server responds:

🚫 `403 Forbidden – Invalid CSRF token`

---

---

# 🧠 What about GET?

GET is read-only → no CSRF required.

---

---

# 📊 Summary

| Thing       | Meaning                            |
| ----------- | ---------------------------------- |
| CSRF        | Security attack protection         |
| csrfField() | Generates hidden token             |
| Where       | Inside POST/PUT/PATCH/DELETE forms |
| Required?   | YES                                |

---

---

# 🔥 One-liner

> **CSRF token proves the form came from your site — not from a random attacker.**

---
