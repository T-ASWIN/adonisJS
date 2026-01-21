.where('slug', router.matchers.slug())

This is route-level validation
It runs before the controller function executes.

---

# 📘 AdonisJS Routing – Complete Guide

This document explains different types of routing in **AdonisJS**, including normal routes, parameterized routes, route handler functions, resource-based routing, file access and display, and route validations.

---

## 1️⃣ Normal Route (Static Page Route)

Used to render a page without passing or modifying data.

```ts
router.on('/').render('pages/home')
```

### Explanation

* Renders an EdgeJS view
* No controller or logic involved
* Used for home pages, dashboards, static pages

---

## 2️⃣ Route with Route Handler Function

Inline function used to handle a request.

```ts
router.get('/welcome', async () => {
  return 'Welcome to AdonisJS'
})
```

### Explanation

* Returns a response directly to the browser
* Output is shown in the **UI**, not the console

---

## 3️⃣ Route with URL Parameters (Params Route)

Used when data is passed through the URL.

```ts
router.get('/user/:id', async ({ params }) => {
  return `User ID is ${params.id}`
})
```

### Example URL

```
/user/101
```

### Explanation

* `:id` is a dynamic parameter
* Accessed using `params.id`

---

## 4️⃣ Route with Query Parameters

```ts
router.get('/search', async ({ request }) => {
  const keyword = request.input('q')
  return `Searching for: ${keyword}`
})
```

### Example URL

```
/search?q=movies
```

---

## 5️⃣ Route Handler Using Controller

```ts
router.get('/users', 'UsersController.index')
```

**UsersController**

```ts
export default class UsersController {
  async index() {
    return ['User 1', 'User 2']
  }
}
```

### Explanation

* Keeps routes clean
* Business logic moves to controller
* Best practice for large applications

---

## 6️⃣ Named Routes (`as`)

```ts
router
  .get('/profile', 'ProfileController.show')
  .as('profile.show')
```

### Usage in EdgeJS

```edge
<a href="{{ route('profile.show') }}">Profile</a>
```

---

## 7️⃣ Resource-Based Routing (CRUD)

Automatically creates routes for CRUD operations.

```ts
router.resource('posts', 'PostsController')
```

### Generated Routes

* GET `/posts`
* POST `/posts`
* GET `/posts/:id`
* PUT `/posts/:id`
* PATCH `/posts/:id`
* DELETE `/posts/:id`

### Explanation

* Follows REST standards
* Reduces repetitive route definitions

---

## 8️⃣ File Access and Display Route

Used to read a file from the server and display it in the UI.

```ts
import fs from 'node:fs/promises'
import app from '@adonisjs/core/services/app'

router.get('/page/:name', async ({ params, view }) => {
  const filePath = app.makeURL(`resources/pages/${params.name}.html`)
  const content = await fs.readFile(filePath, 'utf8')

  return view.render('pages/webpage', { content })
})
```

### Explanation

* Reads file using `fs`
* Sends content to EdgeJS view
* Displays file content in browser

---

## 9️⃣ Route Validation Using `where()`

Validates URL parameters before route execution.

```ts
router
  .get('/movie/:slug', async ({ params, view }) => {
    return view.render('movies/show', { slug: params.slug })
  })
  .where('slug', router.matchers.slug())
```

### Valid URLs

```
/movie/avatar
/movie/the-dark-knight
```

### Invalid URLs (Blocked Automatically)

```
/movie/../../secret
/movie/@#$%
```

### Explanation

* Prevents unsafe or invalid input
* Improves security and performance

---

## 🔟 Multiple Route Handler Functions (Middleware Style)

```ts
router.get(
  '/dashboard',
  async ({ auth }) => {
    await auth.authenticate()
  },
  async () => {
    return 'Dashboard Page'
  }
)
```

### Explanation

* First handler performs authentication
* Second handler sends response
* Executes handlers sequentially

---

## 🔁 HTTP Method Summary

| Method | Purpose               |
| ------ | --------------------- |
| GET    | Fetch data            |
| POST   | Create data           |
| PUT    | Update full record    |
| PATCH  | Update partial record |
| DELETE | Remove data           |

---

## ✅ Summary

* Normal routes render views
* Parameter routes accept dynamic values
* Route handler functions process requests
* Resource routes simplify CRUD
* File access routes read and display files
* Route validations protect URLs
* Named routes improve maintainability

---

