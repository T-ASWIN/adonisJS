
---

# 🧠 What is Method Spoofing?

Method spoofing =

> Send a POST request, but **tell the server** to treat it as DELETE / PUT / PATCH.

This is done by adding:

* `_method=DELETE` in query string **or**
* hidden input field

Adonis middleware sees `_method` and overrides the request method.

---

---

# ✅ Your example explained

You wrote:

```edge
<form 
  action="{{ route('redis.flush', {}, { qs: { _method: 'DELETE' } }) }}"
  method="POST"
>
```

That produces:

```
/redis/flush?_method=DELETE
```

Browser sends POST → Adonis reads `_method=DELETE` → treats as DELETE.

Perfect 👌

---

---

# 🧩 Another (cleaner) way — hidden input

Instead of query string:

```edge
<form action="{{ route('redis.flush') }}" method="POST">
  {{ csrfField() }}
  <input type="hidden" name="_method" value="DELETE" />

  @button({ type: 'submit' })
    Flush Redis Db
  @end
</form>
```

This is actually more common.

---

---

# 🧠 Why we need spoofing

Because HTML spec:

❌ `<form method="DELETE">` → invalid
❌ `<form method="PATCH">` → invalid

Only GET/POST allowed.

---

---

# 📊 Summary

| Thing            | Meaning                                        |
| ---------------- | ---------------------------------------------- |
| Method spoofing  | Override HTTP method                           |
| `_method=DELETE` | Tell server to treat POST as DELETE            |
| Used for         | REST routes                                    |
| Handled by       | Adonis bodyparser / method override middleware |

---

---

# 🔥 One-liner

> **We POST from browser, but spoof DELETE so backend follows REST rules.**

---
