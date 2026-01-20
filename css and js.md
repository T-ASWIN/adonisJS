Since you have confirmed your `vite.config.js` is correct, the reason your styles only appear on the home page is likely due to the **Layout pattern**.

In AdonisJS (using Edge templates), you typically shouldn't manually type the `@vite` tag in every single file. Instead, you create a **Main Layout** and have your other pages "inherit" from it.

### 1. Create a Base Layout

Create a file at `resources/views/layouts/main.edge`. This file will hold the "skeleton" of your HTML, including the Vite tags.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>{{ title || 'My App' }}</title>

  {{-- This applies the JS and CSS to EVERY page that uses this layout --}}
  @vite(['resources/js/app.js'])
</head>
<body>
  
  {{-- This is where the specific page content will be injected --}}
  {{{ await $slots.main() }}}

</body>
</html>

```

---

### 2. Update your Pages (e.g., Home or Slugs)

Now, instead of writing the whole HTML structure in every file, you just "wrap" your content with that layout.

**resources/views/pages/home.edge:**

```html
@layout('layouts/main')
  @set('title', 'Home Page')

  <h1>This is a slug HTML page.</h1>
  <nav>
    <a href="{{ route('web.index', { slug: 'mangatha' }) }}">Home page</a>
  </nav>
  {{{ movie }}}
@end

```

---

### Why this is better:

* **Centralized Control:** If you decide to add a new library (like Bootstrap or Tailwind), you only add the `@vite` tag in **one** place (the layout file), and it updates your entire website.
* **Clean Code:** Your specific page files stay small and only focus on their own content.
* **Consistency:** You won't accidentally forget a meta tag or a script on a new page.

### Important Note on the "Typo"

In your previous message, you wrote `@vite(['resoureces/js/app.js'])`. In your `vite.config.js`, the path is correctly spelled `resources/js/app.js`.

**Make sure you removed that extra "e" in your Edge template, or the CSS still won't load!**

Would you like me to help you set up a specific layout for your "Appointment Summary" dashboard?
