# 📝 JavaScript Conditionals & Switch
> **Level:** Beginner | **Topic:** if/else, else if, switch, break  
> **Source:** Live Console Practice Session

---

## 🔀 if / else if / else

### Basic Syntax

```js
if (condition) {
  // runs if condition is true
} else if (anotherCondition) {
  // runs if first is false, but this is true
} else {
  // runs if all above are false
}
```

---

## 🧪 Practice Examples

### Example 1 — Simple if with strict equality

```js
if ('int' === 'int') {
  alert(true);
}
// ✅ alerts: true
```

---

### Example 2 — if / else

```js
if ('int' === '1int') {
  alert(true);
} else {
  alert(false);
}
// ✅ alerts: false  (strings are not equal)
```

---

### Example 3 — if / else if / else

```js
if ('int' === '1int') {
  alert(true);
} else if (true) {
  alert('Second is true');
} else {
  alert(false);
}
// ✅ alerts: 'Second is true'  (else if condition is true)
```

---

### Example 4 — Negation with `!`

```js
if ('int' === '1int') {
  alert(true);
} else if (!true) {        // !true → false
  alert('Second is true');
} else {
  alert(false);
}
// ✅ alerts: false  (both conditions failed)
```

> `!true` evaluates to `false`, so else if is skipped → falls to else.

---

## 🌍 Real-World Example

Checking user login status — just like Google checks if you're signed in:

```js
let isLoggedIn = true;
let isAdmin = false;

if (isLoggedIn && isAdmin) {
  alert('Welcome, Admin!');
} else if (isLoggedIn) {
  alert('Welcome, User!');       // ✅ this runs
} else {
  alert('Please sign in.');
}
```

---

## 🔄 switch Statement

Used when you have **one variable** to compare against **many fixed values**.  
More readable than a long chain of `else if`.

### Syntax

```js
switch (expression) {
  case value1:
    // code
    break;
  case value2:
    // code
    break;
  default:
    // runs if no case matched
}
```

> ⚠️ **Always add `break`** at the end of each case — without it, execution **falls through** to the next case automatically.

---

## 📅 switch — Day of the Week

### Step 1: Get today's day number

```js
let day = new Date().getDay();
alert(day);
// getDay() returns: 0 = Sunday, 1 = Monday ... 6 = Saturday
```

---

### Step 2 — ❌ Without `break` (Fall-through Bug)

```js
switch (day) {
  case 1:
    alert('Monday');        // ⚠️ no break — falls through!
  case 2:
    alert('Tuesday');       // ⚠️ this also runs
  case 3:
    alert('Wednesday');     // ⚠️ and this...
  // ...and so on until end of switch
}
```

> If `day` is `1`, ALL cases from 1 onwards will run — not just Monday!  
> This is called **fall-through** behavior.

---

### Step 3 — ✅ With `break` (Correct)

```js
switch (day) {
  case 1:
    alert('Monday');
    break;
  case 2:
    alert('Tuesday');
    break;
  case 3:
    alert('Wednesday');
    break;
  case 4:
    alert('Thursday');
    break;
  case 5:
    alert('Friday');
    break;
  case 6:
    alert('Saturday');
    break;
  case 0:
    alert('Sunday');
    break;
  default:
    alert('Invalid day');
}
// ✅ Only the matching case runs, then stops
```

> 📌 Note: `new Date().getDay()` returns **0 for Sunday**, not 7.  
> So `case 0` is Sunday, `case 1` is Monday … `case 6` is Saturday.

---

## 🌍 Real-World Example 

Google Maps showing different content based on the day — like "Weekend traffic is lighter":

```js
let day = new Date().getDay();

switch (day) {
  case 0:
  case 6:
    alert('Weekend — traffic is lighter! 🚗');   // both 0 and 6 fall to this
    break;
  case 5:
    alert('Friday — expect heavy traffic! 🚦');
    break;
  default:
    alert('Weekday — normal traffic conditions.');
}
```

> 💡 **Intentional fall-through trick:** `case 0:` has no `break`, so it falls into `case 6:`'s code.  
> This is how you handle **multiple cases with the same logic**.

---

## ❌ Common Errors Explained

### 1. `ReferenceError: True is not defined`

```js
if ('int' === 'int') { alert(True); }   // ❌ capital T
```

**Cause:** `True` (capital T) is treated as a variable name — JS can't find it.  
**Fix:** `alert(true)` — JavaScript booleans are always **lowercase**.

---

### 2. `SyntaxError: missing ) after argument list`

```js
alert(Second is true);   // ❌ unquoted string
```

**Cause:** `Second is true` is not wrapped in quotes, so JS tries to parse it as code.  
**Fix:** `alert('Second is true')` — always wrap string values in quotes.

---

## 📊 if vs switch — When to Use Which

| Situation | Use |
|---|---|
| Comparing ranges (`> 10`, `< 5`) | `if / else if` |
| Checking multiple conditions (`&&`, `\|\|`) | `if / else if` |
| One variable vs many fixed values | `switch` |
| Cleaner, more readable multiple branches | `switch` |

---

## 🧠 Key Takeaways

> - JavaScript booleans are **lowercase**: `true` / `false` — never `True` / `False`.
> - Always **wrap string values in quotes** inside `alert()` and conditions.
> - `===` is **strict equality** — checks both value AND type.
> - `!true` is `false`, `!false` is `true` — the `!` operator flips a boolean.
> - `switch` is cleaner than long `else if` chains when comparing **one variable to many values**.
> - **Always add `break`** in switch cases to stop fall-through.
> - `new Date().getDay()` returns `0` for Sunday, `1` for Monday … `6` for Saturday.
> - **Intentional fall-through** (no `break`) is useful when multiple cases share the same logic.