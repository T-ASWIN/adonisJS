1.ctx.view.share() 

It makes data available to the view without passing it directly to render().
Data is needed across multiple views


2.app.makePath()

const path = app.makePath('resources/movies/leo.md')
fs.readFile()
fs.writeFile()
fs.readdir()

Used when you want to read/write files on disk.

3.app.makeURL() 
Used to generate URLs for the browser, not files.

Links
Redirects
API responses
const url = app.makeURL('movies/leo')
