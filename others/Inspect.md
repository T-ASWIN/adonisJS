
---

1️⃣ What is inspect(movie)?

inspect() is a debugging helper used inside Edge templates.

It prints the entire structure of a variable in a readable format.

📌 Think of it as:

console.log() → for backend (Node.js)

inspect() → for UI (Edge templates)



---

2️⃣ Where do we use inspect()?

✅ Only inside .edge files

Example:

{{ inspect(movie) }}

You cannot use it in:

Controllers

Services

Models



---

3️⃣ Why do we use inspect(movie)?

Main reasons:

✅ 1. To understand the data structure

When you don’t know:

What keys an object has

Whether it is null or undefined

Whether data is actually reaching the UI


{{ inspect(movie) }}

Output:

{
  "title": "Leo",
  "year": 2024,
  "rating": 8.2
}


---

✅ 2. To debug view issues

If the page is blank or throws an error:

{{ inspect(movie) }}

You can immediately see:

Is data present?

Is it empty?

Is it wrongly named?



---

✅ 3. To avoid guessing variable names

Instead of guessing:

{{ movie.name }}
{{ movie.title }}
{{ movie.movie_name }}

Use:

{{ inspect(movie) }}

Then access correctly:

{{ movie.title }}


---

4️⃣ Example with Controller

Controller

const movie = {
  title: 'Leo',
  director: 'Lokesh',
  year: 2024,
}

return view.render('movies/show', { movie })


---

Edge view

<h1>Movie Details</h1>

{{ inspect(movie) }}


---

5️⃣ Once debugging is done ❗

🚫 Do NOT keep inspect() in production UI

Why?

Exposes internal data

Looks unprofessional

Can leak sensitive info


✅ Remove it after development:

<h1>{{ movie.title }}</h1>


---

6️⃣ inspect() vs JSON.stringify()

Feature	inspect()	JSON.stringify()

Readability	Very high	Medium
Purpose	Debugging	Output / API
Use case	Development only	Data serialization
Edge helper	Yes	No



---

7️⃣ Interview-Ready Explanation 🧠

> inspect() is an Edge debugging helper used to visualize the structure and content of variables passed from the backend to the view. It helps developers verify data flow and avoid incorrect property access during development.




---

8️⃣ One-Line Memory Tip

> Controller → console.log
View → inspect()




---
