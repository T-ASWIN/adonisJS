Nice 😄 — **method spoofing** is one of those backend concepts that sounds scary… but is actually very simple.

You already *used* it in your movie form without realizing 👀

Let’s break it down clearly 👇

---

# ✅ What Is Method Spoofing?

Method spoofing means:

> 👉 sending a request that *pretends* to be PUT / PATCH / DELETE even though the browser only supports GET and POST in HTML forms.

---

# 🧠 Why Do We Need It?

HTML forms only support:

• GET
• POST

But REST APIs often want:

• PUT → update
• DELETE → remove
• PATCH → partial update

So frameworks like Adonis allow:

👉 POST + hidden field = treated as PUT/DELETE.

That trick = **method spoofing**.

---

# 🔥 How You Already Used It (In Your Code)

You had:

```edge
@assign(action = route(
  'admin.movies.update',
  { id: movie.id },
  { qs: { _method: 'PUT' } }
))
```

That creates a URL like:

```
/admin/movies/10?_method=PUT
```

Browser sends POST, but Adonis reads `_method=PUT` and handles it as a PUT request.

---

---

# ✅ Standard Hidden-Input Version

Most common style:

```edge
<form method="POST" action="/movies/10">
  {{ csrfField() }}

  <input type="hidden" name="_method" value="PUT">

  <button type="submit">Update</button>
</form>
```

Even though method is POST, Adonis treats it as PUT.

---

---

# ⚙️ How Adonis Handles This

Adonis has middleware that:

• checks request body or query string
• looks for `_method`
• if found → overrides HTTP method

So controller route:

```ts
router.put('/movies/:id', 'MoviesController.update')
```

will match 👍

---

---

# 🧠 Simple Definition

> Method spoofing allows HTML forms to simulate HTTP methods like PUT or DELETE by sending a POST request with a special `_method` value.

---

---

# 📌 When Do You Use It?

Use it when:

✔ submitting edit forms
✔ deleting records
✔ RESTful routes
✔ resource controllers

---

---

# 🎯 Interview One-Liner

If asked:

> What is method spoofing?

Say:

> Method spoofing is a technique where a POST form includes a hidden `_method` field so the server treats it as PUT, PATCH, or DELETE.

---

If you want next 😄:

👉 how method spoofing interacts with CSRF
👉 `_method` in query vs body
👉 resource routes example
👉 delete button example

Just tell me 👍
