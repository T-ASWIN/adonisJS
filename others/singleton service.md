Great set of questions, Aswin 👌 — you’re now digging into **service classes**, **static vs instance methods**, **dependency injection with `@inject()`**, and **what “action” means** in AdonisJS / MVC terms.

Let’s go point-by-point, clearly and practically, with small code examples.

---

# ✅ 1) Services — Static vs Instance Methods

You wrote:

> services → a class or collection of methods that helps us complete one or more tasks, typically grouped by purpose/resource.

That’s 💯 correct.

A **service** usually contains:

• business logic
• DB-related transformations
• file uploads
• calculations
• syncing relations
• reusable workflows

Controllers call services so controllers stay thin.

---

## 🔵 Static Service Methods

Example (like your `MovieService`):

```ts
export default class MovieService {
  static async getFormData() {
    return {
      statuses: [],
      cineasts: [],
    }
  }

  static async storePoster(file) {
    return '/path/to/file'
  }
}
```

### 👉 How you call it:

```ts
const data = await MovieService.getFormData()
```

No `new` keyword.

---

### 🧠 When static is good:

✔ utility-like behavior
✔ no internal state
✔ pure functions
✔ grouping logic only

---

---

## 🟡 Instance Service (non-static)

If you remove `static`, the method belongs to an **object**, not the class itself.

```ts
export default class MovieService {
  async getFormData() {
    return { statuses: [] }
  }
}
```

Now you MUST create an instance:

```ts
const service = new MovieService()
await service.getFormData()
```

---

### 🧠 Why use instance services?

Because instance services can:

• hold internal state
• receive dependencies
• be injected
• mock easily in tests

---

---

# ⚖️ Static vs Instance — Comparison

| Static                     | Instance                         |
| -------------------------- | -------------------------------- |
| MovieService.getFormData() | new MovieService().getFormData() |
| No DI                      | Works with DI                    |
| Utility style              | Object-oriented                  |
| Harder to mock             | Easier to mock                   |
| No state                   | Can store state                  |

---

---

# ✅ 2) `@inject()` — Dependency Injection

You wrote:

> inject dependency, it will spy the type of the parameters, find it, instantiate a new instance.

Yes 👍 — that’s **dependency injection (DI)**.

---

## 🧠 What DI Means

Instead of:

❌ manually creating objects:

```ts
const service = new MovieService()
```

Adonis creates it for you and passes it into the class.

---

---

## 🔵 Injecting into a Controller

Example:

```ts
import { inject } from '@adonisjs/core'

@inject()
export default class MoviesController {
  constructor(protected movieService: MovieService) {}

  async index() {
    const data = await this.movieService.getFormData()
  }
}
```

---

### 👉 What happens:

• Adonis sees `@inject()`
• reads constructor types
• creates MovieService
• passes it in

You must import the class:

```ts
import MovieService from '#services/movie_service'
```

DI uses the type info.

---

---

## 🔵 Injecting Multiple Dependencies

```ts
@inject()
export default class TestController {
  constructor(
    protected movieService: MovieService,
    protected mailService: MailService
  ) {}
}
```

---

---

## 🔵 Injecting in a Service

Services can inject other services:

```ts
@inject()
export default class OrderService {
  constructor(protected paymentService: PaymentService) {}
}
```

---

---

## 🧠 Why Use DI?

✔ loose coupling
✔ easy testing
✔ easy mocking
✔ cleaner constructors
✔ no `new` everywhere
✔ follows SOLID principles

---

---

# ✅ 3) What is “Action”?

This word is overloaded, so let’s separate meanings.

---

---

## 🔵 A) Controller Action

In MVC / Adonis:

> **Action = a controller method that handles a route.**

Example:

```ts
router.get('/movies', 'MoviesController.index')
```

Here:

👉 `index` is the **action**.

---

So:

```
MoviesController.index()
MoviesController.store()
MoviesController.update()
```

All of these are **actions**.

---

---

## 🔵 B) HTML form `action` attribute

In your Edge view:

```html
<form method="POST" action="{{ action }}">
```

This `action` is:

👉 the URL where the form submits.

Usually:

```ts
route('admin.movies.store')
```

---

---

## 🔵 C) “Action” in services / logic

Sometimes devs say:

> “call this action”

Meaning:

👉 run this operation / method.

Not special syntax — just English.

---

---

# 🧠 Summary

---

## ✅ Services

• group business logic
• static → call on class
• instance → use `new` or DI

---

## ✅ Static vs Instance

```ts
MovieService.method()
```

vs

```ts
new MovieService().method()
```

---

## ✅ `@inject()`

• enables dependency injection
• inspects constructor types
• auto-instantiates
• full import required
• works for classes

---

## ✅ Action

Can mean:

1️⃣ controller method
2️⃣ HTML form target
3️⃣ generic operation word

---

If you want next, I can explain:

👉 why Adonis prefers DI
👉 service vs controller responsibility
👉 middleware vs service
👉 when NOT to use static services
👉 repository pattern vs service

Just tell me 😎🚀
