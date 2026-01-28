
---

# 📘 README — VineJS `.exists()` and `!!` Operator

You can paste this into a `README.md` file 👇

---

# 📄 README: VineJS `.exists()` Rule & `!!` Operator

## 🔹 1. `.exists()` in VineJS

`.exists()` is a validation rule used to check whether a given input value is valid by verifying its presence in some data source.

### ✔ Common Use Case: Database Validation

```js
status: vine.number().exists(async (db, value) => {
  const row = await db
    .from('movie_statuses')
    .where('id', value)
    .first()

  return !!row
})
```

### What Happens:

* `value` = input field
* DB is queried.
* If record exists → return `true`
* If not → return `false`

---

---

## 🔹 2. `.exists()` With In-Memory Data

You can also use `.exists()` to validate against arrays or configs.

Example:

```js
sort: vine.string().exists(async (_db, value) => {
  return MovieService.sortOptions.some(
    (option) => option.id === value
  )
})
```

### What Happens:

* Checks whether the value appears in allowed options.
* Prevents invalid sort keys.

---

---

## 🔹 3. Why `!!` Is Used

`!!` is called the **double bang operator**.

It converts any value into a boolean.

### Examples:

```js
!!null        // false
!!undefined   // false
!!{}          // true
!!0           // false
!!"hello"     // true
```

---

### In Validation:

```js
return !!row
```

Ensures:

* true → valid
* false → invalid

Which VineJS requires.

---

---

## 🔹 4. Why This Matters

Using `.exists()`:

✔ prevents invalid foreign keys
✔ protects APIs from bad query params
✔ avoids runtime errors
✔ enforces strict allowed values

Using `!!`:

✔ guarantees boolean returns
✔ avoids accidental truthy objects
✔ makes validation predictable

---

If you want next, I can write a **mini README on `.unique()` vs `.exists()`** too — that combo comes up *a lot* in Adonis/VineJS interviews 😉
