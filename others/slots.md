
---

# 🧩 What Is a Slot? (Quick Recap)

A **slot** is how you pass **HTML / markup / child content** into a component.

Think:

> Props = configuration
> Slots = body/content

---

---

# ✅ 1) Normal Slot (Default Slot)

The **normal slot** (also called `main`) is the content placed **inside** the component tag.

---

## 📁 Component: `components/card.edge`

```edge
<div class="border p-4 rounded">
  {{{ await $slots.main() }}}
</div>
```

Here:

👉 `$slots.main()` = default slot.

---

---

## 📁 Usage:

```edge
<x-card>
  <h2>Profile</h2>
  <p>Name: Aswin</p>
</x-card>
```

Everything between `<x-card>` and `</x-card>` goes into:

```
{{{ await $slots.main() }}}
```

---

---

## 🧠 Simple Explanation

> The normal slot is where the main body of the component is injected.

---

---

# ✅ 2) Named Slot

A **named slot** is for **specific sections** of a component — like header, footer, meta, sidebar, etc.

---

## 📁 Component: `components/layout.edge`

```edge
<html>
<head>
  <title>{{ title }}</title>

  @if ($slots.meta)
    {{{ await $slots.meta() }}}
  @endif
</head>

<body>
  <header>
    {{{ await $slots.header() }}}
  </header>

  <main>
    {{{ await $slots.main() }}}
  </main>
</body>
</html>
```

This component expects:

• `meta` slot
• `header` slot
• default slot (`main`)

---

---

## 📁 Usage:

```edge
<x-layout title="Dashboard">

  @slot('meta')
    <meta name="description" content="Dashboard page">
  @endslot

  @slot('header')
    <h1>Admin Panel</h1>
  @endslot

  <p>Welcome back!</p>

</x-layout>
```

---

---

## 🧠 Simple Explanation

> Named slots let you inject content into specific regions of a component.

---

---

# 🔥 Side-by-Side Comparison

| Slot Type            | Where it goes     | When to use      |
| -------------------- | ----------------- | ---------------- |
| Normal slot (`main`) | `$slots.main()`   | Main body        |
| Named slot           | `$slots.header()` | Specific section |

---

---

# 📌 Another Clean Example

### 📁 Alert Component

```edge
<div class="alert {{ type }}">
  <strong>{{ title }}</strong>

  <div>
    {{{ await $slots.main() }}}
  </div>

  @if ($slots.actions)
    <div class="mt-2">
      {{{ await $slots.actions() }}}
    </div>
  @endif
</div>
```

---

### 📁 Usage:

```edge
<x-alert title="Warning" type="error">

  Password is too short.

  @slot('actions')
    <a href="/reset">Reset password</a>
  @endslot

</x-alert>
```

---

---

# 🎯 Interview-Style One-Liner

If they ask:

> What is the difference between normal and named slots?

Say:

> A normal slot passes the main child content of a component, while named slots allow injecting content into specific areas like headers or footers.

---

