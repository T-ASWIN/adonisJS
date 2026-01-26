
---

# 🧠 Where do `request` and `response` come from?

In Adonis controller methods, you write:

```ts
async store({ request, response }: HttpContext) {
```

That `{ request, response }` is:

👉 **destructuring** from the `HttpContext` object.

---

## 🔍 What is `HttpContext`?

Adonis creates one `HttpContext` per HTTP request.

It contains:

```
{
  request,
  response,
  params,
  view,
  auth,
  session,
  logger,
  ...
}
```

So you’re just picking the parts you need.

---

# 🎯 Think of it like this:

Browser → Server

Adonis creates:

```
context = {
  request: RequestObject,
  response: ResponseObject
}
```

You write:

```ts
function store(context) {
  const request = context.request
  const response = context.response
}
```

Destructuring just shortens that 👇

```ts
function store({ request, response }) {
```

---

# 📌 Same with `view`, `params`, etc.

```ts
async show({ view }: HttpContext)
```

means:

```
const view = ctx.view
```

---

# 🧠 Quick summary

✅ Not JS keywords
✅ Framework-provided objects
✅ Taken from HttpContext
✅ You can rename them if you want (but don’t 😅)

Example:

```ts
async store(ctx: HttpContext) {
  ctx.request.all()
  ctx.response.redirect('/')
}
```

---
