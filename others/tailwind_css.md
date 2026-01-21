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
