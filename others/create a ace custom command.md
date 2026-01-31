Yep — **you absolutely can create your own custom commands** in AdonisJS 😎🔥
These are called **Ace commands**, and they let you automate tasks like:

✔ generating files
✔ cleaning folders
✔ syncing data
✔ seeding custom logic
✔ scaffolding features
✔ running background jobs

Think of them as **your own mini CLI tools inside the app**.

Let’s go step-by-step.

---

# ✅ What Is an Ace Command?

An **Ace command** is a class that runs when you type:

```
node ace something
```

Examples built into Adonis:

```
node ace make:controller
node ace make:model
node ace db:migrate
```

You can create your own:

```
node ace movies:cleanup
```

---

# 🧠 When Do We Create Custom Commands?

Use a command when:

• a task is repeated often
• needs automation
• is not HTTP-related
• runs in terminal
• maintenance / admin jobs
• dev tooling
• scaffolding

---

---

# ✅ How to Create Your Own Command

Adonis already gives a generator for this 👇

Run:

```
node ace make:command CleanupMovies
```

This creates something like:

```
app/commands/cleanup_movies.ts
```

---

---

# 📦 Basic Custom Command Example

```ts
import { BaseCommand } from '@adonisjs/core/ace'

export default class CleanupMovies extends BaseCommand {
  static commandName = 'movies:cleanup'
  static description = 'Remove old movies from database'

  async run() {
    this.logger.info('Cleaning movies...')

    // logic here
  }
}
```

---

---

# ▶️ Running It

In terminal:

```
node ace movies:cleanup
```

---

---

# 🔑 Important Parts Explained

---

## 🔵 `BaseCommand`

All commands extend this.

---

## 🔵 `commandName`

CLI name.

---

## 🔵 `description`

Shown in:

```
node ace list
```

---

## 🔵 `run()`

Main logic — executed when command runs.

---

---

# ✅ Accepting Arguments & Flags

---

### 📌 Argument Example

```ts
static args = {
  days: {
    description: 'Delete movies older than X days',
  },
}
```

Run:

```
node ace movies:cleanup 30
```

---

---

### 📌 Flag Example

```ts
static flags = {
  dryRun: {
    description: 'Preview what will be deleted',
  },
}
```

Run:

```
node ace movies:cleanup --dry-run
```

---

---

# ✅ Using Services Inside Commands

Commands can call services too:

```ts
import MovieService from '#services/movie_service'

async run() {
  await MovieService.cleanupOldMovies()
}
```

---

Or via DI:

```ts
@inject()
export default class CleanupMovies extends BaseCommand {
  constructor(protected movieService: MovieService) {
    super()
  }

  async run() {
    await this.movieService.cleanupOldMovies()
  }
}
```

---

---

# 🧠 Mental Model

```
Ace Command = Controller for CLI
Controller = HTTP
Command = Terminal
Service = Business logic
```

---

---

# 📌 TL;DR

✔ Yes, you can create your own commands
✔ Use `node ace make:command`
✔ Extend `BaseCommand`
✔ Set `commandName`
✔ Put logic in `run()`
✔ Run via `node ace ...`
✔ Great for automation

---

If you want next, I can explain:

👉 how to create a command inside a package
👉 difference between job vs command
👉 scheduling commands (cron)
👉 flags vs args deeper
👉 interactive prompts

Just say 😏🚀
