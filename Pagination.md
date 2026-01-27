
👉 What pagination is
👉 Why we need it
👉 How Adonis/Lucid pagination works
👉 Controller role
👉 Service role
👉 View role
👉 Important methods & keywords
👉 Full mini example with code


---

📘 1️⃣ What is Pagination?

Pagination means:

> Showing data in chunks/pages instead of loading everything at once.



Example:

Instead of 10,000 movies:

Page 1 → 15 movies
Page 2 → 15 movies
Page 3 → 15 movies


---

✅ Why pagination?

• Faster queries
• Better UX
• Less memory
• Easier navigation
• SEO-friendly URLs


---


---

⚙️ 2️⃣ How Pagination Works in AdonisJS (Lucid)

Lucid provides:

query.paginate(page, limit)

It runs:

1️⃣ main query with LIMIT/OFFSET
2️⃣ another query to count total rows

Returns a Paginator object, not a simple array.

Shape:

{
  meta: {
    total: 132,
    perPage: 15,
    currentPage: 2,
    lastPage: 9
  },
  data: [ Movie, Movie, ... ]
}


---


---

🎮 3️⃣ Controller — Pagination Orchestrator

Example:

async index({ request, view }: HttpContext) {
  const page = request.input('page', 1)

  const filters = await movieFilterValidator.validate(request.qs())

  const movies = await MovieService
    .getFiltered(filters)
    .paginate(page, 15)

  movies.baseUrl('/movies')
  movies.queryString(filters)

  return view.render('pages/movies/index', {
    movies,
    filters,
  })
}


---

🔍 What each part does:


---

📌 Get page number

const page = request.input('page', 1)

Reads:

?page=3

Default = 1.


---


---

📌 paginate()

.paginate(page, 15)

• page = which page
• 15 = records per page


---


---

📌 baseUrl()

movies.baseUrl('/movies')

Tells paginator where to link:

/movies?page=2


---


---

📌 queryString()

movies.queryString(filters)

Preserves filters while paging:

?page=2&sort=title_desc&search=batman

🔥 very important UX detail.


---


---

🧠 Keywords so far

Method	Purpose

paginate()	Slice data
baseUrl()	Pagination links
queryString()	Keep filters
request.input()	Read page



---


---

🛠 4️⃣ Service — Returns Query Builder

Important pattern:

👉 Service should NOT paginate.
👉 It should return query builder.

static getFiltered(filters) {
  return Movie.query()
    .where(...)
    .orderBy(...)
}

Controller decides pagination.


---


---

🖼 5️⃣ View — Rendering Paginated Data

Because paginator contains .data, you loop like:


---

✅ Loop movies

@each (movie in movies.data)
  <MovieCard :movie="movie" />
@end


---


---

✅ Pagination Links

In Edge:

<div class="mt-6">
  {{ movies.links() }}
</div>

Adonis automatically builds:

« Prev 1 2 3 Next »


---


---

📦 6️⃣ Full Mini Example


---

🎮 Controller

async index({ request, view }: HttpContext) {
  const page = request.input('page', 1)

  const movies = await Movie.query()
    .orderBy('title')
    .paginate(page, 10)

  movies.baseUrl('/movies')

  return view.render('movies/index', { movies })
}


---


---

🛠 Service Version

static list() {
  return Movie.query().orderBy('title')
}

Controller:

const movies = await MovieService.list().paginate(page, 10)


---


---

🖼 View

@each (movie in movies.data)
  <p>{{ movie.title }}</p>
@end

{{ movies.links() }}


---


---

🔥 7️⃣ Flow Summary

Browser → GET /movies?page=2
            ↓
Controller reads page
            ↓
Service builds query
            ↓
paginate()
            ↓
Paginator object
            ↓
View loops movies.data
            ↓
movies.links()


---


---

🎯 Interview-Ready One-Liner

> Pagination limits results returned per request and uses LIMIT/OFFSET at the DB level while returning metadata to generate page navigation links.




---

