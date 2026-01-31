
---

# ✅ What is CRUD?

CRUD = the four basic data operations:

| Letter | Meaning | SQL idea |
| ------ | ------- | -------- |
| C      | Create  | INSERT   |
| R      | Read    | SELECT   |
| U      | Update  | UPDATE   |
| D      | Delete  | DELETE   |

In web apps:

* Create → add new movie
* Read → list movies / show one movie
* Update → edit existing movie
* Delete → remove movie

---

# 🧠 What is a Resource Controller?

A **resource controller** groups all CRUD actions for one entity (`Movie`) into one class.

In Adonis:

```
MoviesController
```

Typical methods:

```
index()    -> list all
create()   -> show create form
store()    -> save new
show()     -> show single
edit()     -> show edit form
update()   -> update existing
destroy()  -> delete
```

These are standard REST-style actions.

---

# 📦 Your Controller — Method by Method

---

---

## 🟢 `index()` → READ (List)

```ts
async index({ request, view }: HttpContext) {
```

### Purpose:

Show paginated list of movies.

---

### What happens:

```ts
const page = request.input('page', 1)
```

Gets page number from query string.

---

```ts
const movies = await Movie.query()
  .preload('status')
  .preload('director')
  .preload('writer')
  .withCount('castMembers')
  .withCount('crewMembers')
  .orderBy('updatedAt', 'desc')
  .paginate(page, 30)
```

Fetch movies:

* preload relations
* count cast/crew
* sort newest first
* paginate

---

```ts
return view.render('pages/admin/movies/index', { movies })
```

Send to UI.

---

---

---

## 🟢 `create()` → Show CREATE form

```ts
async create({ view }: HttpContext) {
```

### Purpose:

Render empty form.

---

### What happens:

```ts
const data = await MovieService.getFormData()
```

Loads:

* statuses
* cineasts

---

```ts
return view.render(...)
```

---

---

---

## 🟡 `store()` → CREATE (Insert)

```ts
async store({ request, response }: HttpContext) {
```

### Purpose:

Save new movie.

---

### Steps:

---

### 🔹 1. Validate input

```ts
const { poster, cast, crew, ...data } =
  await request.validateUsing(movieValidator)
```

Ensures clean input.

---

---

### 🔹 2. Upload poster

```ts
if (poster) {
  data.posterUrl = await MovieService.storePoster(poster)
}
```

Stores file.

---

---

### 🔹 3. DB Transaction

```ts
await db.transaction(async (trx) => {
```

Ensures:

> either movie + cast + crew all succeed
> or nothing is saved.

---

---

### 🔹 4. Create movie

```ts
const movie = await Movie.create(data, { client: trx })
```

Insert movie row.

---

---

### 🔹 5. Sync relations

```ts
await MovieService.syncCastAndCrew(movie, cast, crew)
```

Insert pivot table rows.

---

---

### 🔹 6. Redirect

---

---

---

## 🔵 `edit()` → READ (Edit Form)

```ts
async edit({ view, params }: HttpContext) {
```

### Purpose:

Show edit page for one movie.

---

### Steps:

* fetch movie
* fetch crew + cast pivot rows
* fetch dropdown data
* render form

---

---

---

## 🟡 `update()` → UPDATE

```ts
async update({ params, request, response }: HttpContext) {
```

### Purpose:

Update existing movie.

---

### Steps:

---

### 🔹 1. Validate

---

### 🔹 2. Handle poster replacement

Delete old if needed.

---

---

### 🔹 3. Transaction

---

### 🔹 4. Update movie

```ts
movie.merge(data).save()
```

---

---

### 🔹 5. Sync cast & crew again

Keeps pivot tables correct.

---

---

---

## 🔴 `destroy()` → DELETE

```ts
async destroy({ response, params }: HttpContext) {
```

### Purpose:

Delete movie.

---

```ts
await movie.delete()
```

Removes record.

---

---

---

## ⚪ `show()` → READ (Single)

Currently empty.

Usually:

```ts
const movie = await Movie.findOrFail(params.id)
return view.render('...', { movie })
```

---

---

# 🗺️ Mapping Methods to HTTP Routes

If you define:

```ts
router.resource('movies', MoviesController)
```

Adonis generates:

| Method    | URL              | Controller |
| --------- | ---------------- | ---------- |
| GET       | /movies          | index      |
| GET       | /movies/create   | create     |
| POST      | /movies          | store      |
| GET       | /movies/:id      | show       |
| GET       | /movies/:id/edit | edit       |
| PUT/PATCH | /movies/:id      | update     |
| DELETE    | /movies/:id      | destroy    |

---

---

# 🧠 Mental Model

Think:

```
index  -> list page
create -> blank form
store  -> insert
edit   -> filled form
update -> update + sync relations
destroy-> delete
```

---

---

# ⭐ Why Your Architecture is Clean

You did:

✔ Controller = orchestration
✔ Service = heavy logic
✔ Validator = input safety
✔ Transaction = consistency
✔ Model = relations

That’s solid layered design 👍.

---

