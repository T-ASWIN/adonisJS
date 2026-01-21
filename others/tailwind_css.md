1.aswin@ASWIN:~/projects/firstproject$ npm install -D tailwindcss postcss autoprefixer

2.import '../css/app.css' add this into Inside resources/js/app.js:

3.add this @vite(['resources/js/app.js']) into home.edge
<head>
    <title>AdonisJS with Tailwind</title>
    @vite(['resources/js/app.js'])
  </head>


4.In resources/css/app.css:

@tailwind base;
@tailwind components;
@tailwind utilities;


5.create tailwind.config.js to this:

/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './resources/views/*/.edge',
    './resources/js/*/.js',
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}



6. # Rebuild everything
npm run build

7. # Restart Vite and Adonis servers
npm run dev



explain tailwind.config.js

content: ['./resources/views/*/.edge', './resources/js/*/.js'],
“Scan these files and folders to find Tailwind class names”

theme: {
  extend: {},
},
🔹 What this does
This is where you customize Tailwind design.

Add new colors

Add custom fonts

Add spacing, shadows, sizes

🔹 Example (real use)
theme: {
  extend: {
    colors: {
      brand: '#2563eb',
    },
    fontFamily: {
      sans: ['Inter', 'sans-serif'],
    },
  },



  class="text-brand font-sans"


plugins

👉 We can add extra features to Tailwind only when we need them.



