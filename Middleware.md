
### 1. Why and When to Use Middleware

You use middleware to handle "cross-cutting concerns"—tasks that apply to many different parts of your application, so you don't have to repeat code in every controller.

| Use Case | When to use | Example |
| --- | --- | --- |
| **Security** | To protect routes from unauthorized access. | Ensuring a user is logged in (`Auth`) or has "Admin" role. |
| **Transformation** | To modify the request data before it reaches the controller. | Trimming whitespace from input strings or parsing JSON bodies. |
| **Logging** | To track activity. | Logging every request URL and IP address for debugging. |
| **Validation** | To check data integrity early. | Verifying an API key exists in the headers. |

---

### 2. How Middleware Works (The Lifecycle)

When a request comes in:

1. It enters the **Middleware Stack**.
2. It passes through each middleware function one by one.
3. It reaches your **Route Handler/Controller** (if no middleware stops it).
4. The response travels **back up** through the middleware (post-processing) and is sent to the user.

### 3. How to Create Middleware

You create middleware using the AdonisJS CLI command:

```bash
node ace make:middleware LogRequest

```

This creates a file in `app/middleware` (or `app/Middleware` in v5). A basic middleware looks like this:

```typescript
import type { HttpContext } from '@adonisjs/core/http'
import type { NextFn } from '@adonisjs/core/types/http'

export default class LogRequest {
  async handle(ctx: HttpContext, next: NextFn) {
    // 1. Code here runs BEFORE the controller
    console.log(`Request made to: ${ctx.request.url()}`)

    // 2. Call next() to pass control to the next middleware/controller
    // If you don't call this, the request hangs!
    await next()

    // 3. Code here runs AFTER the controller (on the way back)
    console.log('Response status:', ctx.response.getStatus())
  }
}

```

---

### 4. The Three Types of Middleware

AdonisJS organizes middleware into three categories based on **when** they run and **how** they are registered. In AdonisJS 6, these are all configured in `start/kernel.ts`.

#### A. Server Middleware

**"The Global Gatekeepers"**
These run on **every single HTTP request** hitting your server, even if no route is defined for that URL (e.g., 404s).

* **Use for:** Static assets, CORS (Cross-Origin Resource Sharing), or request ID generation.
* **Registration:**
```typescript
// start/kernel.ts
server.use([
  () => import('@adonisjs/static/static_middleware'),
  () => import('@adonisjs/cors/cors_middleware'),
])

```



#### B. Router Middleware (Global)

**"The Application Filters"**
These run on every request that **matches a valid route**. If the user requests a page that doesn't exist, these usually won't run.

* **Use for:** Body parsing (reading JSON/Form data), Session initiation, or keeping track of request history.
* **Registration:**
```typescript
// start/kernel.ts
router.use([
  () => import('@adonisjs/core/bodyparser_middleware'),
  () => import('@adonisjs/session/session_middleware'),
])

```



#### C. Named Middleware

**"The Specific Guards"**
These are optional. They do **not** run unless you explicitly attach them to a specific route or group of routes. You register them with a "name" (alias) so you can easily reference them in your routes file.

* **Use for:** Authentication (`auth`), Role checks (`admin`), Guest checks (`guest`).
* **Registration (Defining the collection):**
```typescript
// start/kernel.ts
export const middleware = router.named({
  auth: () => import('#middleware/auth_middleware'),
  guest: () => import('#middleware/guest_middleware'),
})

```



---

### 5. How to Use Named Middleware in Routes

Once you have defined your named middleware in `kernel.ts`, you attach it to routes in `start/routes.ts`.

**Method 1: Using the V6 standard (Type-safe)**

```typescript
import router from '@adonisjs/core/services/router'
import { middleware } from '#start/kernel' // Import the collection we made above

// Apply to a single route
router.get('/dashboard', [DashboardController, 'index'])
  .use(middleware.auth())

// Apply to a group of routes
router.group(() => {
  router.get('/settings', [SettingsController, 'index'])
  router.post('/settings', [SettingsController, 'update'])
})
.use(middleware.auth()) // protects everything in the group

```

### Summary of Differences

| Type | Runs On | Config Location | Best For |
| --- | --- | --- | --- |
| **Server** | Every request (even 404s) | `server.use()` | Static files, CORS, Security Headers |
| **Router** | Every matched route | `router.use()` | BodyParser, Sessions, Global Auth Init |
| **Named** | Only selected routes | `router.named()` | Protected pages, Admin panels, ACL |

**Next Step:** Would you like to see an example of how to pass arguments to a middleware (e.g., `middleware.auth({ guard: 'api' })`)?
