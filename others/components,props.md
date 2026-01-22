---

# 🧠 What are Edge Components?

In AdonisJS, **components** are **server-side reusable templates**.

They let you:

👉 extract repeated markup
👉 keep views clean
👉 pass data into reusable UI blocks
👉 avoid copy-paste

Think:

* React components — but rendered on the server
* Blade components in Laravel

---

# 📦 What does “server-side” mean?

It means:

* rendered **inside Adonis**, not in the browser
* converted to HTML before sending response
* no JavaScript required for basic rendering

---

# 🧩 Your example

You wrote:

```edge
@!component('components/movie/card', { movie })
```

That means:

👉 render the component file at:

```
resources/views/components/movie/card.edge
```

and pass:

```
movie = movie
```

into it as a prop.

---

# 🧠 Syntax breakdown

---

## ✅ `@component()` vs `@!component()`

### `@component(...)`

* Supports slots (`@slot`)
* Needs `@end`

Example:

```edge
@component('components/movie/card', { movie })
@end
```

---

### `@!component(...)`

* Self-closing version
* No slots
* Cleaner when component is simple

Example:

```edge
@!component('components/movie/card', { movie })
```

---

---

## ✅ Path string

```edge
'components/movie/card'
```

This maps to:

```
resources/views/components/movie/card.edge
```

---

---

## ✅ Props object

```edge
{ movie }
```

This is shorthand for:

```js
{ movie: movie }
```

So inside the component:

```
$props.movie
```

is available.

---

---

# 🧠 Inside the component — how props are used

In:

```
resources/views/components/movie/card.edge
```

You can do:

```edge
<h2>{{ $props.movie.title }}</h2>
```

or if you destructure:

```edge
@let(movie = $props.movie)

<h2>{{ movie.title }}</h2>
```

---

# 📌 Why use components?

---

## ✅ 1) Reuse UI

Cards, buttons, navbars, alerts

One place → many pages.

---

---

## ✅ 2) Clean views

Home page stays short.

---

---

## ✅ 3) Consistency

Same markup everywhere.

---

---

## ✅ 4) Easier to change

Update card once → affects all pages.

---

---

# 🎯 Interview-ready summary

> “Edge components are server-side reusable templates. We render them using `@component` or `@!component` and pass data through props. Inside the component, values are accessed using `$props`.”

🔥 Strong.

---

# ✍️ Language polish

You wrote:

> **“explain how tis works and its syntax”**

Better:

> **“Explain how this works and its syntax.”**

---
