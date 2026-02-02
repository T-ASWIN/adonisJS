Nice 👍 — this is another Vue `<script setup>` macro.

Let’s explain **`defineProps`** in the simplest way possible 👇

---

## ✅ What is `defineProps`?

`defineProps` is a macro in Vue `<script setup>` that lets you:

👉 **declare which props this component receives from its parent (or Inertia).**

Props = data passed into a component.

---

## 📌 In Your Code

```ts
defineProps<{
  title: string
  description: string
}>()
```

This means:

✔ This component expects two props:

* `title` (string)
* `description` (string)

Vue now knows their types (because of TypeScript).

---

## 🧠 Why Use It?

You use `defineProps` to:

✅ tell Vue what data is coming in
✅ get TypeScript checking
✅ avoid runtime mistakes
✅ document the component’s API

---

## 🧩 How It’s Used in Template

Because of `<script setup>`, you can directly write:

```vue
{{ title }}
```

even though you didn’t create a variable manually.

Vue automatically exposes props to the template.

---

## 📌 Inertia Context

In Inertia apps, props usually come from the controller:

```ts
return ctx.inertia.render('Page', {
  title: 'Login',
  description: 'Welcome back',
})
```

Those land here via `defineProps`.

---

## 🟢 Super Simple Version

👉 **`defineProps` tells Vue what data the component will receive.**

That’s it 😄
