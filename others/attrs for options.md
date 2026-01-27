
### 1. The Problem it Solves

Without `html.attrs`, you would have to write this logic manually to keep the option selected after the page reloads:

```edge
{{-- The "Messy" Way --}}
<option 
  value="{{ status.id }}" 
  @if(status.id == filters.status) selected="selected" @endif
>
  {{ status.name }}
</option>

```

If you have multiple attributes (like `disabled`, `readonly`, `class`), this gets very hard to read.

### 2. How `html.attrs` Works

The `html.attrs()` helper takes an object.

* **Key:** The name of the HTML attribute (e.g., `selected`, `disabled`, `class`).
* **Value:** A boolean (true/false) condition.

If the condition is **true**, it adds the attribute.
If the condition is **false**, it completely removes the attribute.

### 3. Breakdown of Your Code

```javascript
html.attrs({ 
  selected: status.id == filters.status 
})

```
