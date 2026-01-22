
---

# ✅ 1) `html.classNames()`

You wrote:

> *we can conditionally apply classes based on a server variable*

✔️ Correct — with one small clarification:

👉 It applies classes **at server-side render time**, inside Edge templates, based on variables passed from the controller.

Better phrasing:

> **`html.classNames()` conditionally builds CSS class strings in Edge templates using server-side variables.**

---

---

# ✅ 2) `@assign()` vs `@let()`

You wrote:

> *assign → reassign, let → declare*

✔️ Exactly.

More clearly:

* **`@let()`** → declares a new variable in the current scope
* **`@assign()`** → updates an existing variable from an outer scope

Interview-ready:

> **`@let()` is used to declare local variables in Edge templates, while `@assign()` is used to modify an already-declared variable.**

---

---

# ✅ 3) `html.attrs()`

You wrote:

> *it show value in inspect*

You’re close — but let’s be more precise:

👉 `html.attrs()` converts an object into valid HTML attributes and injects them into an element.

Example:

```edge
<div {{
  html.attrs({
    id: movie.slug,
    class: ['card', isActive && 'border-blue-500'],
  })
}}>
```

This becomes real HTML in the browser:

```html
<div id="leo" class="card border-blue-500"></div>
```

Better phrasing:

> **`html.attrs()` serializes an object into HTML attributes so they appear correctly in the rendered markup.**

---

