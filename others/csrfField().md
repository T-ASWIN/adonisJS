
---

# 🛡 What is `csrfField()`?

`csrfField()` outputs a hidden `<input>` inside your form that contains a **CSRF token**.

Example HTML it generates:

```html
<input type="hidden" name="_csrf" value="random-long-secure-token" />
```

---

# 🧠 What is CSRF?

CSRF = **Cross-Site Request Forgery**.

It’s a type of attack where:

• you’re logged into a site
• another website secretly submits a form to your app
• your browser sends cookies
• server thinks it’s you 😬

---

# 🎯 Why we use it

When the form is submitted:

1️⃣ browser sends `_csrf` token
2️⃣ Adonis checks token vs session
3️⃣ if match → OK
4️⃣ if not → ❌ reject request

This proves:

👉 **“This form really came from our site.”**

---

# 📌 Why in Edge templates?

Because Edge is generating HTML forms.

So in form:

```edge
<form method="POST">
  {{ csrfField() }}
```

Edge helper injects the hidden token automatically.

---

---

# 🧠 One-liner:

**`csrfField()` adds a hidden CSRF token input to protect your form from fake submissions.**

---

