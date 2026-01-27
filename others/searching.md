
### 1. `const qs = request.qs()` (The Input)

**"qs" stands for Query String.**
This line grabs any parameters that are added to the end of your URL after the `?` symbol.

* **Scenario:** A user types "Matrix" into your search bar and hits Enter.
* **The URL becomes:** `mysite.com/movies?search=Matrix`
* **What `request.qs()` reads:** It parses that URL and creates a Javascript object:
```javascript
{
  search: "Matrix"
}

```


* **Why we use it:** This allows us to read "GET" request data (like search filters) easily.

---

### 2. `.if(qs.search, ...)` (The Safety Check)

This is a helper method provided by the AdonisJS Lucid ORM.

* **The Logic:** It says, "Only run the code inside the parentheses **IF** `qs.search` actually has a value."
* **Why it matters:**
* If the user just visits `/movies` (no search), `qs.search` is undefined. The code inside skips, and we simply show **all** movies.
* If the user visits `/movies?search=something`, the code inside runs, and we filter the list.



---

### 3. `whereILike('title', ...)` (The Filter)

This is the actual SQL command that filters the database results. Let's break down the name:

* **Where**: Standard database filtering (like "Where ID is 1").
* **I (Insensitive)**: Case-**I**nsensitive. This is crucial for search bars.
* If you used standard `where`, searching "matrix" would **not** find "Matrix" (because 'm' is not 'M').
* With `whereILike`, "matrix", "MATRIX", and "Matrix" are treated as the same thing.


* **Like**: This tells the database to look for a **pattern** rather than an exact match.

#### The Pattern: ``%${qs.search}%``

Inside the function, you are wrapping the user's search term in percent signs (`%`). These are SQL **wildcards**.

| User types | The Query becomes | Logic | Result |
| --- | --- | --- | --- |
| `bat` | `%bat%` | Contains "bat" anywhere | Matches "Batman", "Combat", "Wombat" |
| `bat` | `bat%` | Starts with "bat" | Matches "Batman", "Battle" (but **not** "Combat") |
| `bat` | `bat` | Exact match only | Matches only "bat" (nothing else) |

**In your code:**

```typescript
// If qs.search is "man"
.whereILike('title', '%man%')
// It finds any movie where the title has "man" anywhere inside it (e.g., "Iron Man", "Superman", "The Many Saints").

```
<form method='GET' action="{{ route('movies.index') }}">
    <div class="flex items-end gap-3 pt-4">
      @!form.input({
        label: 'Title Search',
        name:'search',
        type:'search'
        value:filters.search
      })

      @button({type:'submit'})
        Search
      @end
    </div>
  </form>
