# 📝 var, let, const
> **Topic:** Variable Declarations in JavaScript  
> **Level:** Beginner → Intermediate

---

## 📊 Quick Comparison Table

| Feature | `var` | `let` | `const` |
|---|---|---|---|
| Introduced in | ES5 (1995) | ES6 (2015) | ES6 (2015) |
| Scope | Function | Block | Block |
| Hoisting | ✅ (as `undefined`) | ✅ (TDZ — unusable) | ✅ (TDZ — unusable) |
| Declare without value | ✅ | ✅ | ❌ |
| Reassign value | ✅ | ✅ | ❌ |
| Re-declare same variable | ✅ | ❌ | ❌ |
| Global object property | ✅ (`window.x`) | ❌ | ❌ |
| Recommended in modern JS | ❌ Avoid | ✅ Yes | ✅ Yes (prefer) |

---

## 🔵 var — The Old Way

### What it is
`var` is the original variable declaration keyword in JavaScript (pre-ES6). It works but has quirky, bug-prone behavior due to its **function scope** and **hoisting**.

### Key Characteristics

**1. Function Scoped**
A `var` variable is accessible anywhere inside the function it was declared in — even inside nested `if`, `for`, or `while` blocks.

```js
function greet() {
  if (true) {
    var message = 'Hello!';   // declared inside if block
  }
  console.log(message);       // ✅ 'Hello!' — still accessible!
}
greet();
```

> This is the **bug trap** — `var` leaks out of blocks. With `let`/`const`, this would throw a ReferenceError.

**2. Re-declaration Allowed**
You can accidentally re-declare the same variable with `var` and JS won't complain.

```js
var x = 10;
var x = 20;      // ✅ no error — silently overwrites
console.log(x);  // 20
```

> This is dangerous in large codebases — you may accidentally overwrite a variable without realizing it.

**3. Hoisting**
`var` declarations are hoisted (moved) to the top of their function scope by the JS engine, and initialized as `undefined`.

```js
console.log(name);  // undefined (not an error!)
var name = 'Vikas';
console.log(name);  // 'Vikas'
```

Behind the scenes, JS interprets it as:
```js
var name;           // hoisted to top, value = undefined
console.log(name);  // undefined
name = 'Vikas';
console.log(name);  // 'Vikas'
```

**4. Becomes a Global Property**
When declared at the top level (outside any function), `var` attaches itself to the global `window` object in browsers.

```js
var city = 'Mumbai';
console.log(window.city);  // 'Mumbai' — pollutes global scope
```

---

## 🟡 let — The Modern Standard

### What it is
Introduced in ES6 (2015), `let` fixes the scope issues of `var`. It is **block-scoped** and does not allow re-declaration.

### Key Characteristics

**1. Block Scoped**
A block is any code inside `{}`. `let` variables are confined strictly to the block they're declared in.

```js
if (true) {
  let score = 100;
  console.log(score);  // ✅ 100
}
console.log(score);    // ❌ ReferenceError: score is not defined
```

```js
for (let i = 0; i < 3; i++) {
  console.log(i);   // ✅ 0, 1, 2
}
console.log(i);     // ❌ ReferenceError — i is block scoped to the for loop
```

**2. Can Declare Without Value**
```js
let vname;           // value is undefined
vname = 'Vikas';     // assign later — valid
console.log(vname);  // 'Vikas'
```

**3. Re-declaration Not Allowed**
```js
let age = 25;
let age = 30;  // ❌ SyntaxError: Identifier 'age' has already been declared
```

**4. Reassignment Allowed**
```js
let salary = 50000;
salary = 60000;   // ✅ perfectly fine
```

**5. Temporal Dead Zone (TDZ)**
`let` is hoisted but NOT initialized. Accessing it before the declaration line throws a ReferenceError.

```js
console.log(emp);  // ❌ ReferenceError: Cannot access 'emp' before initialization
let emp = 'Vikas';
```

---

## 🔴 const — For Fixed Values

### What it is
`const` (constant) means the variable binding cannot be changed after declaration. It shares all block-scoping and TDZ behavior with `let`, but adds the constraint of **immutable binding**.

### Key Characteristics

**1. Must Be Initialized at Declaration**
```js
const PI = 3.14159;   // ✅
const tax;            // ❌ SyntaxError: Missing initializer in const declaration
```

> Because `const` can never be reassigned, you must give it a value immediately.

**2. Reassignment Not Allowed**
```js
const country = 'India';
country = 'USA';   // ❌ TypeError: Assignment to constant variable
```

**3. Re-declaration Not Allowed**
```js
const rate = 18;
const rate = 28;   // ❌ SyntaxError: Identifier 'rate' has already been declared
```

**4. Block Scoped (same as let)**
```js
if (true) {
  const gst = 18;
}
console.log(gst);  // ❌ ReferenceError — block scoped
```

**5. ⚠️ const with Objects and Arrays — Important!**
`const` makes the **reference** constant, NOT the contents. You can still mutate (change) the properties of an object or elements of an array.

```js
const user = { name: 'Vikas', age: 26 };

user.name = 'Raj';       // ✅ mutation allowed — changing a property
user.city = 'Mumbai';    // ✅ adding a new property also allowed
user = { name: 'Raj' };  // ❌ TypeError — cannot reassign the reference itself

console.log(user);  // {name: 'Raj', age: 26, city: 'Mumbai'}
```

```js
const nums = [1, 2, 3];
nums.push(4);   // ✅ [1, 2, 3, 4] — mutation allowed
nums = [5, 6];  // ❌ TypeError — reassignment blocked
```

> **Think of it this way:** `const` locks the box. You can't swap the box — but you can still change what's inside it.

---

## 🧠 Scope Deep Dive

### What is Scope?
Scope determines **where a variable is accessible** in your code. JS has three levels:
- **Global Scope** — accessible everywhere in the file
- **Function Scope** — accessible only inside the function (`var`)
- **Block Scope** — accessible only inside `{}` (`let`, `const`)

### Scope Visualized

```
Global Scope
│
├── var globalVar = 1       ← accessible everywhere
├── let globalLet = 2       ← accessible everywhere (but NOT on window object)
│
└── function foo() {
      │  ← Function Scope starts
      │
      ├── var funcVar = 'A'        ← accessible anywhere in foo()
      ├── let funcLet = 'B'        ← accessible anywhere in foo()
      │
      └── if (true) {
            │  ← Block Scope starts
            │
            ├── var blockVar = 'X'       ← LEAKS out to foo() scope! (var bug)
            ├── let blockLet = 'Y'       ← confined here only
            └── const blockConst = 'Z'   ← confined here only
          }
          │
          console.log(blockVar);    // ✅ 'X' — var leaked out
          console.log(blockLet);    // ❌ ReferenceError
    }
```

### The var Loop Bug (Classic Gotcha)

```js
// Using var — BUG
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 3, 3, 3  ← i was shared across all iterations!

// Using let — FIXED
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 0, 1, 2  ← each iteration has its own i
```

> This is one of the most common reasons to use `let` instead of `var` in loops.

---

## ⚡ Hoisting Deep Dive

**Hoisting** = JS engine scans your code before running it and moves variable/function declarations to the top of their scope — before any code executes.

### How Each Keyword is Hoisted

```js
// What you write:
console.log(a);   // ?
console.log(b);   // ?
console.log(c);   // ?

var a = 1;
let b = 2;
const c = 3;

// What JS engine actually does internally:
var a;            // hoisted → initialized as undefined
// let b          // hoisted → placed in TDZ, NOT initialized
// const c        // hoisted → placed in TDZ, NOT initialized

console.log(a);   // undefined  — no error
console.log(b);   // ❌ ReferenceError: Cannot access 'b' before initialization
console.log(c);   // ❌ ReferenceError: Cannot access 'c' before initialization
```

### Temporal Dead Zone (TDZ)

The TDZ is the period between when a `let`/`const` variable is **hoisted** and when it is **actually declared** in your code. During this window, the variable exists in memory but is completely inaccessible.

```js
// TDZ starts here for 'empName' ↓
console.log(empName);  // ❌ ReferenceError — in TDZ
let empName = 'Vikas'; // ← TDZ ends here
console.log(empName);  // ✅ 'Vikas'
```

---

## 🔁 Re-declaration vs Reassignment

These two are often confused:

```js
// Re-declaration — using let/var/const keyword again for same name
var x = 1;
var x = 2;   // ✅ var allows re-declaration

let y = 1;
let y = 2;   // ❌ SyntaxError — let does NOT allow re-declaration

// Reassignment — changing value WITHOUT using keyword again
let z = 1;
z = 2;       // ✅ reassignment is fine for let

const w = 1;
w = 2;       // ❌ TypeError — const cannot be reassigned
```

---

## ✅ When to Use What

| Situation | Use | Why |
|---|---|---|
| Fixed values (PI, TAX_RATE, API_URL) | `const` | Value never changes |
| Loop counters, flags, accumulators | `let` | Value needs to update |
| Object/array whose reference won't change | `const` | Reference fixed; contents can mutate |
| Supporting old browsers (pre-ES6) | `var` | Legacy support only |
| Default choice in modern JS | `const` | Safe default; switch to `let` only if needed |

---

## ❌ Common Errors

| Error | Cause | Fix |
|---|---|---|
| `SyntaxError: Missing initializer in const` | `const x;` — no value given | `const x = value;` |
| `TypeError: Assignment to constant variable` | Reassigning a `const` | Use `let` instead |
| `SyntaxError: Identifier already declared` | Re-declaring `let`/`const` | Remove the duplicate declaration |
| `ReferenceError: Cannot access before initialization` | Using `let`/`const` before its declaration line | Move usage after declaration |
| `ReferenceError: x is not defined` | Variable never declared | Declare with `let`/`const` first |

---

> 💡 **Golden Rule:** Always start with `const`. If you need to reassign, switch to `let`. Never use `var` in modern JavaScript.
>
> **Memory trick:**
> - `const` = **constant** — set it and forget it
> - `let` = **let it change** — use when value will update
> - `var` = **very avoid** — legacy only