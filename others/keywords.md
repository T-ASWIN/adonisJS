
---

AdonisJS View Sharing & Path vs URL Utilities

This document explains commonly used AdonisJS utilities related to views, file system access, and URL generation, with clear examples and use cases.


---

1. ctx.view.share()

What it does

ctx.view.share() makes data available to views without passing it directly to view.render().

The data becomes available to:

The rendered view

Layouts

Partials


The shared data lasts for the current HTTP request only


Syntax

ctx.view.share({ key: value })

Example

ctx.view.share({ user })
return view.render('pages/dashboard')

Usage in Edge

<p>Welcome {{ user.username }}</p>

When to use

Data needed across multiple views

Layout-level data such as:

Logged-in user

App name

Navigation data



Key Note

❌ Incorrect:

ctx.view.share(user)

✅ Correct:

ctx.view.share({ user })


---

2. app.makePath()

What it does

app.makePath() generates an absolute file system path inside your AdonisJS project.

It is used when working with files on disk, not URLs.

Syntax

const path = app.makePath('relative/path')

Example

const path = app.makePath('resources/movies/leo.md')

Common Use Cases

Used with Node.js file system methods:

import fs from 'node:fs/promises'

const path = app.makePath('resources/movies/leo.md')

await fs.readFile(path, 'utf8')
await fs.writeFile(path, 'New content')
await fs.readdir(app.makePath('resources/movies'))

When to use

Reading files

Writing files

Listing directories

Working with local storage


📌 Used only on the server


---

3. app.makeURL()

What it does

app.makeURL() generates a browser-accessible URL, not a file path.

It is used for:

Links

Redirects

API responses


Syntax

const url = app.makeURL('path')

Example

const url = app.makeURL('movies/leo')

Output Example

http://localhost:3333/movies/leo

Common Use Cases

Anchor links

Redirect responses

Sending URLs in API responses


return response.redirect(app.makeURL('login'))


---

4. app.makePath() vs app.makeURL()

Feature	app.makePath()	app.makeURL()

Purpose	File system access	Browser URL
Output	Absolute disk path	HTTP URL
Used for	fs.readFile, fs.writeFile	Links, redirects
Runs on	Server	Client / Browser
Example	/home/app/resources/file.md	http://localhost:3333/page



---

5. Quick Summary

Use ctx.view.share() for data needed across layouts and multiple views

Use app.makePath() for reading/writing files on disk

Use app.makeURL() for generating browser URLs



---

6. One-Line Memory Tip 🧠

> share → views
makePath → files
makeURL → browser




---
