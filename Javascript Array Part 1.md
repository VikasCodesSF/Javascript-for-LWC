# 📦 JavaScript Arrays – Part 1

> **SwiftNotes** | Beginner-friendly reference with definitions, syntax, and examples.

---

## 📌 What is an Array?

An **Array** is a special variable that can hold **multiple values** in a single variable, stored in an ordered list.

Each item in an array has a **index** (position number) starting from `0`.

```js
let fruits = ['Apples', 'Bananas', 'Oranges'];
//              [0]        [1]         [2]
```

---

## 🔧 Creating an Array

```js
// Empty array
let fruits = [];

// Array with values
let fruits = ['Apples', 'Bananas', 'Oranges'];
```

---

## 📐 Array Basics

### Accessing Elements

```js
fruits[0]              // 'Apples'
fruits[2]              // 'Oranges'
```

### Last Element (Dynamic)

```js
fruits[fruits.length - 1]   // Always gives the last element
```

> **Why?** Arrays are 0-indexed, so the last index is always `length - 1`.

### Array Length

```js
fruits.length    // Returns total number of elements
```

### Mixed Arrays

Arrays can hold **any data type** — strings, numbers, even other arrays (nested arrays).

```js
let fruits = ['Apples', 'Bananas', 'Oranges', 23, ['23']];
```

---

## 🗑️ `delete` Keyword

**Definition:** Removes an element from a specific index but **leaves an empty slot** (does not shift other elements or reduce array length).

> ⚠️ Avoid using `delete` on arrays. Use `.splice()` instead for clean removal.

```js
delete fruits[1];
// fruits → ['Apples', empty, 'Oranges']
// fruits.length is still 3
// fruits[1] → undefined
```

---

## 🔗 Array Methods

---

### 1. `concat()`

**Definition:** Merges two or more arrays (or values) into a **new array**. Does NOT modify the original array.

**Syntax:**
```js
array.concat(value1, value2, anotherArray, ...)
```

**Example:**
```js
let fruits = ['Apples', 'Bananas', 'Oranges', 23];
let moreFruits = fruits.concat("Grapes", [23, "yellow"]);
// moreFruits → ['Apples', 'Bananas', 'Oranges', 23, 'Grapes', 23, 'yellow']
```

---

### 2. `keys()`

**Definition:** Returns an **Array Iterator** that contains the **index (key) of each element** in the array.

**Syntax:**
```js
array.keys()
```

**Example:**
```js
let arr = ['a', 'b', 'c'];
for (let key of arr.keys()) {
  console.log(key);   // 0, 1, 2
}
```

---

### 3. `values()`

**Definition:** Returns an **Array Iterator** that contains the **value of each element** in the array.

**Syntax:**
```js
array.values()
```

**Example:**
```js
let arr = ['Apples', 'Bananas'];
for (let val of arr.values()) {
  console.log(val);   // 'Apples', 'Bananas'
}
```

---

### 4. `entries()`

**Definition:** Returns an **Array Iterator** that contains **[index, value] pairs** for each element.

**Syntax:**
```js
array.entries()
```

**Example:**
```js
let arr = ['Apples', 'Bananas'];
for (let [index, value] of arr.entries()) {
  console.log(index, value);  // 0 'Apples', 1 'Bananas'
}
```

---

### 5. `every()`

**Definition:** Tests whether **ALL elements** in the array pass a given condition (callback function). Returns `true` only if **every** element passes; otherwise `false`. Stops early as soon as one element fails.

**Syntax:**
```js
array.every(function(currentValue, index, originalArray) {
  return condition;
});
```

| Parameter | Description |
|---|---|
| `currentValue` | The current element being processed |
| `index` | The index of the current element |
| `originalArray` | The original array |

**Example:**
```js
let users = [234, 34, 45, 56, 12, 78, 89];

users.every(function(currentVal) {
  return currentVal > 10;
});
// → true  (all values are > 10)

users.every(function(currentVal) {
  return currentVal > 12;
});
// → false  (12 is NOT > 12)
```

> 💡 **Use case:** Validate that all users are adults, all prices are positive, etc.

---

### 6. `some()`

**Definition:** Tests whether **AT LEAST ONE element** in the array passes a given condition. Returns `true` as soon as one element passes; otherwise `false`. Stops early as soon as one match is found.

**Syntax:**
```js
array.some(function(currentValue, index, originalArray) {
  return condition;
});
```

**Example:**
```js
let users = [234, 34, 45, 56, 12, 78, 89];

users.some(function(currentVal) {
  return currentVal > 10;
});
// → true  (many values are > 10)
```

> 💡 **Difference from `every()`:** `every()` = ALL must pass. `some()` = AT LEAST ONE must pass.

---

### 7. `Array.from()`

**Definition:** Creates a **new Array** from an array-like or iterable object (like a `Set`, `String`, `NodeList`, etc.).

**Syntax:**
```js
Array.from(iterable)
```

**Examples:**
```js
// From a Set
let mySet = new Set([23, 234, 243]);
let arr = Array.from(mySet);
// → [23, 234, 243]

// From a String
Array.from('This is My New Array');
// → ['T', 'h', 'i', 's', ' ', 'i', 's', ...]
```

---

### 8. `Array.isArray()`

**Definition:** Checks whether a value **is an Array**. Returns `true` or `false`.

**Syntax:**
```js
Array.isArray(value)
```

**Example:**
```js
let arr = [23, 234, 243];

Array.isArray(arr);       // → true
Array.isArray("hello");   // → false
Array.isArray(42);        // → false
```

> 💡 **Use case:** Before calling array methods on a variable, confirm it is actually an array.

```js
if (Array.isArray(conArray)) {
  conArray.forEach(function(val) {
    console.log(val);
  });
}
```

---

### 9. `forEach()`

**Definition:** Executes a provided function **once for each element** in the array. Does NOT return anything (returns `undefined`). Used purely for side effects (logging, DOM updates, etc.).

**Syntax:**
```js
array.forEach(function(currentValue, index, originalArray) {
  // your logic here
});
```

**Example:**
```js
let conArray = [23, 234, 243];

conArray.forEach(function(currentValue, index) {
  let result = currentValue + index;
  console.log(result);
  // 23+0=23, 234+1=235, 243+2=245
});
```

---

### 10. `includes()`

**Definition:** Checks if a specific **value exists** in the array. Returns `true` or `false`. Case-sensitive for strings.

**Syntax:**
```js
array.includes(valueToSearch)
```

**Example:**
```js
let conArray = [23, 234, 243];

conArray.includes(23);       // → true
conArray.includes('Apples'); // → false
```

---

### 11. `indexOf()`

**Definition:** Returns the **first index** at which a given value is found. Returns `-1` if the value is NOT found.

**Syntax:**
```js
array.indexOf(valueToSearch)
```

**Example:**
```js
let conArray = [23, 234, 243];

conArray.indexOf(23);     // → 0
conArray.indexOf(234);    // → 1
conArray.indexOf(99999);  // → -1 (not found)
```

> 💡 **Tip:** Use `indexOf()` when you need the position. Use `includes()` when you just need a yes/no answer.

---

### 12. `join()`

**Definition:** Converts all elements of an array into a **single string**, separated by a specified separator. Default separator is a comma `,`.

**Syntax:**
```js
array.join(separator)
```

**Example:**
```js
let conArray = [23, 234, 243];

conArray.join();    // → '23,234,243'   (default comma)
conArray.join(','); // → '23,234,243'
conArray.join('-'); // → '23-234-243'
conArray.join(' '); // → '23 234 243'
```

---

### 13. `split()` *(String Method — related to `join()`)*

**Definition:** Splits a **string** into an array of substrings based on a separator. This is the **reverse of `join()`**.

> Note: `split()` is a String method, not an Array method.

**Syntax:**
```js
string.split(separator)
```

**Example:**
```js
'23,234,243'.split(',');
// → ['23', '234', '243']
```

---

## ⚠️ Common Syntax Errors (Learned from Practice)

| Wrong | Correct | Issue |
|---|---|---|
| `fruits.[fruits.length - 1]` | `fruits[fruits.length - 1]` | Don't use `.` before `[` |
| `{alert(index) console.log(...)}` | Put `;` after each statement | Missing semicolons inside function body |
| `{alert(index); console.log(...) return x}` | Add `;` after `console.log(...)` | Missing semicolon before `return` |

---

## 📊 Quick Method Summary

| Method | Returns | Mutates Original? | Use For |
|---|---|---|---|
| `concat()` | New array | ❌ No | Merging arrays |
| `keys()` | Iterator (indexes) | ❌ No | Looping over indexes |
| `values()` | Iterator (values) | ❌ No | Looping over values |
| `entries()` | Iterator ([i, v] pairs) | ❌ No | Looping with index + value |
| `every()` | `true` / `false` | ❌ No | Check if ALL pass condition |
| `some()` | `true` / `false` | ❌ No | Check if ANY pass condition |
| `Array.from()` | New array | ❌ No | Convert Set/String to Array |
| `Array.isArray()` | `true` / `false` | ❌ No | Type-check a variable |
| `forEach()` | `undefined` | ❌ No | Loop and perform side effects |
| `includes()` | `true` / `false` | ❌ No | Check if value exists |
| `indexOf()` | Index or `-1` | ❌ No | Find position of value |
| `join()` | String | ❌ No | Convert array to string |

---

*Part 2 will cover: `pop()`, `push()`, `shift()`, `unshift()`, `reverse()`, `slice()`, `splice()`, `toString()`, `values()` and more.*