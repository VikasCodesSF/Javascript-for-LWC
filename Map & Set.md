# JavaScript `Map` and `Set` – Notes

---

## PART 1 – `Map`

### What is a Map?
A `Map` is a collection of **key-value pairs** where keys can be of **any type** (string, number, object, etc.), and insertion order is preserved.

---

### 1.1 Creating a Map

```js
// ❌ Wrong – keywords are case-sensitive, must be lowercase var/new and capitalized Map
VAR MYmAP = NEW mAP();  // SyntaxError: Unexpected identifier 'MYmAP' 

// ✅ Correct
var myMap = new Map();   // Map(0) {size: 0}
```

---

### 1.2 Adding Entries – `.set(key, value)`

```js
myMap.set('FirstName', 'Vikas');   // Map(1) {'FirstName' => 'Vikas'}
myMap.set('LastName', 'Pandey');   // Map(2) {'FirstName' => 'Vikas', 'LastName' => 'Pandey'}
```

> Setting an **existing key** updates its value (no duplicate keys):
```js
myMap.set('FirstName', 'Shweta');  // Map(2) {'FirstName' => 'Shweta', 'LastName' => 'Pandey'}
```

---

### 1.3 Checking for a Key – `.has(key)`

```js
myMap.has('Shweta')    // false → checks KEYS, not values
myMap.has('FirstName') // true
```

> ⚠️ `.has()` checks **keys only**, not values.

---

### 1.4 Getting a Value – `.get(key)`

```js
myMap.get('FirstName')  // 'Shweta'
myMap.get('LastName')   // 'Pandey'
```

---

### 1.5 Deleting an Entry – `.delete(key)`

```js
myMap.delete('LastName')  // true → entry removed
myMap                     // Map(1) {'FirstName' => 'Shweta'}
```

---

### 1.6 Iterators – `.keys()`, `.values()`, `.entries()`
### `.keys()` - returns an iterable for keys,
### `.values()` - returns an iterable for values,
### `.entries()` - returns an iterable for entries [key, value], it's used by default in for ..of .

```js
myMap.keys()     // MapIterator {'FirstName', 'Email'}
myMap.values()   // MapIterator {'Shweta', 'Shweta@gmail.com'}
myMap.entries()  // MapIterator {'FirstName' => 'Shweta', 'Email' => 'Shweta@gmail.com'}
```

---

### 1.7 Size – `.size` (property, not method)

```js
myMap.size()  // ❌ TypeError: myMap.size is not a function
myMap.size    // ✅ 2  → no parentheses, it's a property
```

---

### 1.8 Clearing All Entries – `.clear()`

```js
myMap.clear();  // undefined
myMap           // Map(0) {size: 0}
```

---

### 1.9 Map Method Summary

| Method / Property | Description | Returns |
|---|---|---|
| `new Map()` | Creates an empty Map | `Map` |
| `.set(key, value)` | Adds or updates a key-value pair | `Map` (chainable) |
| `.get(key)` | Returns the value for a key | Value or `undefined` |
| `.has(key)` | Checks if a key exists | `true` / `false` |
| `.delete(key)` | Removes an entry by key | `true` / `false` |
| `.keys()` | Returns iterator of all keys | `MapIterator` |
| `.values()` | Returns iterator of all values | `MapIterator` |
| `.entries()` | Returns iterator of key-value pairs | `MapIterator` |
| `.size` | Number of entries (property!) | `number` |
| `.clear()` | Removes all entries | `undefined` |

---

## PART 2 – `Set`

### What is a Set?
A `Set` is a collection of **unique values** — duplicates are automatically ignored. Insertion order is preserved.

---

### 2.1 Creating a Set

```js
var mySet = new Set();  // Set(0) {size: 0}
```

---

### 2.2 Adding Values – `.add(value)`

```js
mySet.add('Vikas');   // Set(1) {'Vikas'}
mySet.add('Pandey');  // Set(2) {'Vikas', 'Pandey'}
mySet.add('Shweta');  // Set(3) {'Vikas', 'Pandey', 'Shweta'}
```

> Duplicates are silently ignored:
```js
mySet.add('Shweta');  // Set(3) {'Vikas', 'Pandey', 'Shweta'} → no change
```

---

### 2.3 Checking for a Value – `.has(value)`

```js
mySet.has('Vikas')   // true
mySet.has('Shweta')  // false (if deleted)
```

---

### 2.4 Deleting a Value – `.delete(value)`

```js
mySet.delete('Shweta')  // true
```

---

### 2.5 Size – `.size` (property, not method)

```js
mySet.size  // 3  → no parentheses
```

---

### 2.6 Clearing All Values – `.clear()`

```js
mySet.clear();  // undefined
mySet           // Set(0) {size: 0}
```

---

### 2.7 Iterators – `.keys()`, `.values()`, `.entries()`

```js
mySet.keys()     // SetIterator {'Shweta', 'Vikas'}
mySet.values()   // SetIterator {'Shweta', 'Vikas'}

// ❌ Wrong syntax – missing opening parenthesis
mySet.entries);  // SyntaxError: Unexpected token ')'

// ✅ Correct
mySet.entries()  // SetIterator {'Shweta' => 'Shweta', 'Vikas' => 'Vikas'}
```

> In a Set, `.keys()` and `.values()` return the **same thing** (the values).
> `.entries()` returns each value **twice** as `value => value` (for Map compatibility).

---

### 2.8 Set Method Summary

| Method / Property | Description | Returns |
|---|---|---|
| `new Set()` | Creates an empty Set | `Set` |
| `.add(value)` | Adds a unique value | `Set` (chainable) |
| `.has(value)` | Checks if a value exists | `true` / `false` |
| `.delete(value)` | Removes a value | `true` / `false` |
| `.keys()` | Iterator of values (same as `.values()`) | `SetIterator` |
| `.values()` | Iterator of values | `SetIterator` |
| `.entries()` | Iterator of `value => value` pairs | `SetIterator` |
| `.size` | Number of unique values (property!) | `number` |
| `.clear()` | Removes all values | `undefined` |

---

## PART 3 – Map vs Set Comparison

| Feature | `Map` | `Set` |
|---|---|---|
| Stores | Key-value pairs | Unique values only |
| Duplicate keys/values | Keys are unique (updates value) | Values are unique (ignored) |
| Access by key | ✅ `.get(key)` | ❌ Not applicable |
| `.keys()` | Returns keys | Returns values (same as `.values()`) |
| `.values()` | Returns values | Returns values |
| `.entries()` | `key => value` pairs | `value => value` pairs |
| `.size` | Property (no `()`) | Property (no `()`) |
| Use case | Dictionary / lookup table | Unique list / deduplication |

---

## PART 4 – Error Reference

| Error | Cause | Fix |
|---|---|---|
| `SyntaxError: Unexpected identifier 'MYmAP'` | Used `VAR` / `NEW` (uppercase) | Use `var` / `new` (lowercase) |
| `TypeError: myMap.size is not a function` | Called `.size()` with parentheses | Use `.size` (it's a property) |
| `SyntaxError: Unexpected token ')'` | Wrote `mySet.entries)` — missing `(` | Use `mySet.entries()` |
