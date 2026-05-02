# 📝 JavaScript Loops
> **Level:** Beginner | **Topic:** for, for...in, for...of, while, do...while  
> **Source:** Live Console Practice Session

---

## 🔁 for Loop

Used when you know **how many times** to repeat.

### Syntax

```js
for (initialization; condition; increment) {
  // code to repeat
}
```

### Example 1 — Loop through an Array

```js
let comName = ['Infosys', 'PwC', 'Deloitte'];

for (i = 0; i < comName.length; i++) {
  alert(comName[i]);
}
// ✅ alerts: 'Infosys' → 'PwC' → 'Deloitte'
```

### Example 2 — Store length in a variable (optimized)

```js
let comName = ['Infosys', 'PwC', 'Deloitte'];
let len = comName.length;

for (i = 0; i < len; i++) {
  alert(comName[i]);
}
// ✅ Same result — but avoids recalculating .length on every iteration
```

> 💡 Storing `comName.length` in `len` is a minor performance optimization — useful for large arrays.

---

## 🌍 Real-World Example (Google-style)

Displaying a list of search suggestions — like Google Autocomplete:

```js
let suggestions = ['javascript tutorial', 'javascript loops', 'javascript for loop'];

for (let i = 0; i < suggestions.length; i++) {
  console.log((i + 1) + '. ' + suggestions[i]);
}
// 1. javascript tutorial
// 2. javascript loops
// 3. javascript for loop
```

---

## 🔑 for...in Loop

Used to loop over the **keys (property names)** of an **Object**.

### Syntax

```js
for (let key in object) {
  // key = property name
}
```

### Example — Loop through Object keys

```js
let user = {
  isAdmin: true,
  Name: 'Vikas',
  adminInfo: {
    country: 'IN',
    access: ['IP', 'Email']
  }
};

for (let x in user) {
  alert(x);
}
// ✅ alerts: 'isAdmin' → 'Name' → 'adminInfo'  (keys only)
```

### Example — Access values using bracket notation

```js
for (let x in user) {
  alert(user[x]);
}
// ✅ alerts: true → 'Vikas' → {country: 'IN', access: [...]}
```

> ⚠️ **`for...in` gives you the KEY (`x`), not the value.**  
> To get the value, use `object[key]` — bracket notation.

---

## 🌍 Real-World Example (Google-style)

Reading user profile settings — like Google Account preferences:

```js
let settings = {
  darkMode: true,
  language: 'English',
  notifications: false
};

for (let key in settings) {
  console.log(key + ': ' + settings[key]);
}
// darkMode: true
// language: English
// notifications: false
```

---

## 🔄 for...of Loop

Used to loop over the **values** of an **iterable** (Array, Map, Set, String).

### Syntax

```js
for (let value of iterable) {
  // value = each item directly
}
```

### Example — Loop through Array values

```js
let comName = ['Infosys', 'PwC', 'Deloitte'];

for (let x of comName) {
  alert(x);
}
// ✅ alerts: 'Infosys' → 'PwC' → 'Deloitte'
```

---

### for...of with Map

```js
let myMap = new Map();
myMap.set('FirstName', 'Vikas');
myMap.set('LastName', 'Pandey');
// Map(2) {'FirstName' => 'Vikas', 'LastName' => 'Pandey'}

for (let x of myMap) {
  alert(x);
}
// ✅ alerts: 'FirstName,Vikas' → 'LastName,Pandey'
// (each x is a [key, value] pair — displayed as comma-separated)
```

### Loop only Map values

```js
for (let x of myMap.values()) {
  alert(x);
}
// ✅ alerts: 'Vikas' → 'Pandey'
```

> 💡 Use `.keys()`, `.values()`, or `.entries()` on a Map for targeted iteration.

---

### ⚠️ Why `for...in` doesn't work on Map

```js
for (let x in myMap) {
  alert(x);
}
// ❌ Nothing alerts — for...in doesn't iterate Map entries
```

> `for...in` is for **plain Object properties**.  
> `for...of` is for **iterables** like Array, Map, Set.

---

## 🌍 Real-World Example (Google-style)

Iterating a Map of user roles — like Google Workspace permissions:

```js
let roles = new Map();
roles.set('vikas@gmail.com', 'Admin');
roles.set('john@gmail.com', 'Editor');
roles.set('sara@gmail.com', 'Viewer');

for (let [email, role] of roles) {
  console.log(email + ' → ' + role);
}
// vikas@gmail.com → Admin
// john@gmail.com → Editor
// sara@gmail.com → Viewer
```

---

## 🔁 while Loop

Runs **while** a condition is true. Checks condition **before** each iteration.

### Syntax

```js
while (condition) {
  // code
  // must update condition to avoid infinite loop!
}
```

### Example

```js
let i = 0;

while (i < 5) {
  alert(i);
  i++;
}
// ✅ alerts: 0 → 1 → 2 → 3 → 4
```

> ⚠️ Always increment (`i++`) inside the loop — otherwise it runs forever (infinite loop).

---

## 🌍 Real-World Example (Google-style)

Retrying a failed API call — like Google retrying a Maps request:

```js
let attempts = 0;
let success = false;

while (attempts < 3 && !success) {
  console.log('Attempt ' + (attempts + 1) + ': Fetching data...');
  attempts++;
  // success = true; // uncomment when request succeeds
}
// Attempt 1: Fetching data...
// Attempt 2: Fetching data...
// Attempt 3: Fetching data...
```

---

## 🔁 do...while Loop

Runs the code **at least once**, then checks the condition.  
Difference: condition is checked **after** the first run.

### Syntax

```js
do {
  // code runs at least once
} while (condition);
```

### Example

```js
let i = 1;

do {
  alert(i);
  i++;
} while (i < 5);
// ✅ alerts: 1 → 2 → 3 → 4
```

> Even if `i` started at 10 (condition false from the start), the code inside `do{}` runs **once** before checking.

---

## 🌍 Real-World Example (Google-style)

Showing a prompt at least once — like Google asking you to verify your account:

```js
let verified = false;
let attempts = 0;

do {
  console.log('Please verify your account. Attempt: ' + (attempts + 1));
  attempts++;
  // verified = true; // set when user completes verification
} while (!verified && attempts < 3);
```

---

## ❌ Common Errors Explained

### 1. `TypeError: user is not a function`

```js
for (let x in user) {
  alert(user(x));   // ❌ using () — calls user as a function
}
```

**Cause:** `user(x)` tries to call `user` as a function. `user` is an object, not a function.  
**Fix:** Use **bracket notation** — `user[x]`

```js
alert(user[x]);   // ✅
```

---

### 2. `ReferenceError: x is not defined`

```js
let i = 1;
do {
  alert(x);   // ❌ x is not declared anywhere
  i++;
} while (i < 5);
```

**Cause:** `x` was never declared — probably meant to use `i`.  
**Fix:**

```js
do {
  alert(i);   // ✅ use the correct variable
  i++;
} while (i < 5);
```

---

### 3. `for...in` on Map returns nothing

```js
for (let x in myMap) { alert(x); }   // ❌ silent — no output
```

**Cause:** `for...in` only works on plain object properties — Map is not a plain object.  
**Fix:** Use `for...of` for Map.

```js
for (let x of myMap.values()) { alert(x); }   // ✅
```

---

## 📊 Loop Comparison

| Loop | Best For | Checks Condition |
|---|---|---|
| `for` | Known number of iterations | Before each run |
| `for...in` | Object keys | Before each run |
| `for...of` | Array / Map / Set values | Before each run |
| `while` | Unknown iterations, condition first | Before each run |
| `do...while` | Must run at least once | After first run |

---

## 🔑 for...in vs for...of

| | `for...in` | `for...of` |
|---|---|---|
| Works on | Plain Objects | Arrays, Map, Set, String |
| Gives you | **Keys** (property names) | **Values** directly |
| Use with Map? | ❌ No output | ✅ Yes |
| Use with Array? | ⚠️ Gives index (0,1,2) | ✅ Gives values |

---

## 🧠 Key Takeaways

> - `for` loop — use when you know the count; store `.length` in a variable for optimization.
> - `for...in` — iterates **keys** of an Object; use `obj[key]` to get the value.
> - `for...of` — iterates **values** of Arrays, Maps, Sets.
> - `for...in` does **NOT** work on `Map` — use `for...of` instead.
> - `user(x)` tries to call `user` as a function — always use `user[x]` for object value access.
> - `while` checks condition **before** running — may never run if condition is false from start.
> - `do...while` runs **at least once** — condition checked after the first run.
> - Always increment inside `while` / `do...while` to avoid **infinite loops**.