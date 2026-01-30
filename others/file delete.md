Nice — this is a really solid real-world AdonisJS file-upload + serve + delete flow 👌
Let’s go through it **piece by piece**, then I’ll explain:

✅ how files are **inserted (uploaded)**
✅ how they are **served (downloaded/viewed)**
✅ how they are **deleted**
✅ what each important keyword/function means

---

# 🗂️ 1) `StorageController` — serving files securely

This controller lets users access files like:

```
/storage/posters/abc.png
```

---

## 🔹 Code:

```ts
const filePath = params['*'].join('/')
```

This grabs the wildcard route:

If route is:

```
Route.get('/storage/*', 'StorageController.show')
```

and user hits:

```
/storage/posters/x.png
```

Then:

```
params['*'] = ['posters', 'x.png']
```

Join → `"posters/x.png"`

---

---

## 🔹 normalize()

```ts
const normalizedPath = normalize(filePath)
```

Cleans path separators and resolves:

```
../secret.txt
```

into normalized form.

---

---

## 🔹 PATH_TRAVERSAL_REGUX

```ts
/(?:^|[\\/])\.\.(?:[\\/]|$)/
```

Detects `../` or `..\` → **directory traversal attack**.

So users cannot download:

```
/storage/../../.env
```

---

---

## 🔹 response.download()

```ts
return response.download(app.makePath('storage', normalizedPath))
```

Sends the file from:

```
<project-root>/storage/posters/file.png
```

to browser.

---

# 🗂️ 2) `MovieService.storePoster()` — inserting/uploading file

This is where files are saved.

---

## 🔹 MultipartFile

```ts
poster: MultipartFile
```

Represents uploaded file from form.

---

---

## 🔹 cuid()

```ts
const fileName = `${cuid()}.${poster.extname}`
```

Creates **unique filename** → avoids collisions:

```
iocw92n9sd.png
```

---

---

## 🔹 poster.move()

```ts
await poster.move(app.makePath('storage/posters'), {
  name: fileName,
})
```

Moves uploaded temp file into:

```
storage/posters/
```

---

---

## 🔹 return path

```ts
return `/storage/posters/${fileName}`
```

Saved into DB as `posterUrl`.

---

# 🗂️ 3) MoviesController.store() — insert record

```ts
if (poster) {
  data.posterUrl = await MovieService.storePoster(poster)
}

await Movie.create(data)
```

Flow:

1️⃣ Validate form
2️⃣ If file uploaded → save it
3️⃣ Save DB record with file path

---

# 🗂️ 4) MoviesController.update() — update + delete old file

---

## 🔹 If new poster uploaded:

```ts
if (poster) {
  data.posterUrl = await MovieService.storePoster(poster)
}
```

→ saves new file.

⚠️ You probably should delete old one here too (improvement).

---

---

## 🔹 If poster removed:

```ts
else if (!data.posterUrl && movie.posterUrl) {
  await unlink(app.makePath('.', movie.posterUrl))
  data.posterUrl = ''
}
```

This means:

👉 user cleared poster
👉 DB had one
👉 delete file from disk

`unlink()` = Node delete-file function.

---

## 🔹 app.makePath('.', movie.posterUrl)

If:

```
movie.posterUrl = /storage/posters/x.png
```

then:

```
app.makePath('.', '/storage/posters/x.png')
```

→ `<root>/storage/posters/x.png`

---

---

# ⚠️ One problem in YOUR delete logic

You earlier got:

```
ENOENT unlink posters/...
```

Because:

Your upload path =

```
app.makePath('storage/posters')
```

but delete uses:

```
app.makePath('.', movie.posterUrl)
```

If `posterUrl` starts with `/storage/...` → ok.

But if folder is different — mismatch happens.

---

# ✅ Better delete code (SAFE)

Replace delete block with:

```ts
import { existsSync } from 'node:fs'

else if (!data.posterUrl && movie.posterUrl) {
  const filePath = app.makePath(movie.posterUrl.replace(/^\//, ''))

  if (existsSync(filePath)) {
    await unlink(filePath)
  }

  data.posterUrl = ''
}
```

---

# 🗂️ Keywords You Used — Explained

| Keyword               | Meaning              |
| --------------------- | -------------------- |
| `MultipartFile`       | Uploaded file object |
| `poster.move()`       | Save uploaded file   |
| `cuid()`              | Generate unique ID   |
| `app.makePath()`      | Build absolute path  |
| `unlink()`            | Delete file          |
| `response.download()` | Send file to browser |
| `normalize()`         | Clean file path      |
| `$params['*']`        | wildcard route       |
| `.validateUsing()`    | validate form        |
| `.merge().save()`     | update DB            |
| `.create()`           | insert DB            |

---

# 🎯 FINAL SUMMARY

### Upload:

✔ poster.move()
✔ save path in DB

### Serve:

✔ StorageController
✔ response.download

### Delete:

✔ unlink(path)
✔ path must match upload folder

---

If you want, I can:

👉 rewrite your **update()** so old poster is deleted when uploading new one
👉 or convert everything to use Adonis Drive (recommended for prod) 😎
