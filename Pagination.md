
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
Got you 👍 — now you’re asking about pagination navigation helpers:

👉 next page
👉 previous page
👉 first page
👉 URL generation

These all come from the Paginator object that Adonis/Lucid returns after:

query.paginate(page, limit)

Let’s break it cleanly with what methods exist + how to use them + example code.


---

📦 What .paginate() Returns

When you do:

const movies = await Movie.query().paginate(page, 15)

movies is NOT an array — it’s a Paginator object:

movies = {
  meta: {
    total,
    perPage,
    currentPage,
    lastPage
  },
  data: [...]
}

Lucid also adds helper methods to navigate pages.


---


---

🔵 1️⃣ Next Page URL

✅ Method:

movies.getNextPageUrl()

Returns:

• URL string → /movies?page=3&sort=title_asc
• or null if no next page.


---

📌 Example in Edge:

@if(movies.getNextPageUrl())
  <a href="{{ movies.getNextPageUrl() }}">Next</a>
@end


---


---

🔵 2️⃣ Previous Page URL

✅ Method:

movies.getPreviousPageUrl()

Returns:

• URL string
• or null if on first page.


---

📌 Edge:

@if(movies.getPreviousPageUrl())
  <a href="{{ movies.getPreviousPageUrl() }}">Previous</a>
@end


---


---

🔵 3️⃣ First Page URL

There is no direct getFirstPageUrl() method, but you can generate it manually:

✅ Use:

movies.getUrl(1)


---

📌 Edge:

<a href="{{ movies.getUrl(1) }}">First</a>


---


---

🔵 4️⃣ Last Page URL

Same pattern:

movies.getUrl(movies.meta.lastPage)


---


---

🔵 5️⃣ Get Any Page URL

This is the generic method 👇

✅ Method:

movies.getUrl(pageNumber)


---

Example:

<a href="{{ movies.getUrl(5) }}">Page 5</a>


---


---

🔑 Important: Keep Filters While Paging

In controller you already did:

movies.queryString(filters)

That ensures:

?page=2&sort=title_desc&search=batman

stays in every link.

🔥 Super important.


---


---

🧠 Summary of Pagination Helpers

Method	Purpose

getNextPageUrl()	Next page link
getPreviousPageUrl()	Prev page
getUrl(n)	Any page
movies.meta.currentPage	Current
movies.meta.lastPage	Last
movies.links()	Auto UI



---


---

🎯 Mini Navigation Example in Edge

<div class="flex gap-3 mt-6">

  @if(movies.getPreviousPageUrl())
    <a href="{{ movies.getPreviousPageUrl() }}">Prev</a>
  @end

  <a href="{{ movies.getUrl(1) }}">First</a>

  @if(movies.getNextPageUrl())
    <a href="{{ movies.getNextPageUrl() }}">Next</a>
  @end

  <a href="{{ movies.getUrl(movies.meta.lastPage) }}">Last</a>

</div>


---

