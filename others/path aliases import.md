
```
#controllers
#models
#services
#app
```

These let you **avoid ugly long relative paths** like:

```
../../../controllers/UserController.js
```

and instead write:

```
#controllers/UserController
```

Much cleaner 😄

Let’s go step by step in **simple terms**.

---

# ✅ What Problem Do Path Aliases Solve?

Normally in TypeScript/Node projects, imports look like:

```ts
import UserController from '../../../controllers/UserController.js'
```

Problems:

❌ Hard to read
❌ Easy to break when folders move
❌ Annoying to maintain
❌ Looks messy

---

# 🌟 Solution: Path Aliases

Path aliases are **short names** that map to real folders.

So:

```
#controllers → ./app/controllers
#models → ./app/models
```

---

# ✅ Where Are These Defined?

In **tsconfig.json**.

Example:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "#controllers/*": ["./app/controllers/*"],
      "#models/*": ["./app/models/*"],
      "#services/*": ["./app/services/*"]
    }
  }
}
```

---

# 🧠 What This Means

| Alias        | Real Folder     |
| ------------ | --------------- |
| #controllers | app/controllers |
| #models      | app/models      |
| #services    | app/services    |

---

---

# ✅ How You Use It in Controllers / Files

Instead of 👇

```ts
import User from '../../models/User.js'
import AuthService from '../../services/AuthService.js'
```

You write 👇

```ts
import User from '#models/user'
import AuthService from '#services/auth_service'
```

✨ Short, clean, professional.

---

---

# ⚙️ In AdonisJS (Important!)

AdonisJS already configures these aliases for you by default.

You often see imports like:

```ts
import User from '#models/user'
import router from '@adonisjs/core/services/router'
```

Those `#models` etc come from the internal TS config.

---

# 🔥 Why This Is So Popular?

Because:

✅ Cleaner imports
✅ Easier refactor
✅ No ../../../../
✅ Professional codebase
✅ Fewer bugs

---

# 📌 Simple Explanation in One Line

> Path aliases are shortcuts defined in tsconfig.json that map to folders, allowing you to import files using short names instead of long relative paths.

---

---

# 🎯 Interview-Style Answer

If they ask:

> Why use TS path aliases?

You can say:

> They simplify imports by mapping custom aliases to folders in tsconfig.json, making code cleaner, easier to maintain, and less error-prone than deep relative paths.

---

---

