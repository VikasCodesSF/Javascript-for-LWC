# 🔧 JavaScript Functions 

## Topics Covered

| # | Topic |
|---|-------|
| 1 | Function Declaration (Way 1) |
| 2 | Function Expression (Way 2) |
| 3 | Function with Parameters |
| 4 | Missing / Extra Arguments — No Error |
| 5 | `arguments` Object |
| 6 | Closures |
| 7 | Functions Inside Objects (`this` keyword) |
| 8 | IIFE — Immediately Invoked Function Expression |
| 9 | Declaration vs Expression vs IIFE — Comparison |

---

## 1. Function Declaration (Way 1)

### Definition
A **named function** defined using the `function` keyword. It is **hoisted** — meaning you can call it **before** it is defined in the code.

### Syntax
```js
function functionName() {
    // body
}

functionName(); // call it
```

### Example
```js
function sayHello() {
    alert('HELLO Babe');
}

sayHello(); // → alerts 'HELLO Babe'

// Referencing without calling — returns the function definition, no error
sayHello;
// ƒ sayHello(){ alert('HELLO Babe'); }
```

> 💡 Writing `sayHello` (no parentheses) just references the function object — it does NOT execute it.

### 🔷 LWC Use Case
Standard event handler methods in LWC JS controllers are written as function declarations (or methods).

```js
// In LWC — handleClick is declared and wired to template event
handleClick() {
    this.isVisible = !this.isVisible;
}
```

---

## 2. Function Expression (Way 2)

### Definition
A function assigned to a **variable**. It is **NOT hoisted** — you cannot call it before the line where it is defined.

### Syntax
```js
let variableName = function() {
    // body
};

variableName(); // call it
```

### Example
```js
let redFunction = function() {
    alert('Hello Dev');
};

redFunction(); // → alerts 'Hello Dev'
```

> ⚠️ **Key difference from Declaration:**
> ```js
> sayHello();          // ✅ Works — declarations are hoisted
> function sayHello() { ... }
>
> redFunction();       // ❌ Error — expressions are NOT hoisted
> let redFunction = function() { ... }
> ```

### 🔷 LWC Use Case
Assigning a reusable utility function inside a method, or storing a callback.

```js
// Storing a formatter as a function expression
let formatCurrency = function(value) {
    return '₹ ' + value.toFixed(2);
};

this.formattedTotal = formatCurrency(this.orderTotal);
```

---

## 3. Function with Parameters

### Definition
**Parameters** are named variables listed in the function definition.  
**Arguments** are the actual values passed when calling the function.

### Syntax
```js
function functionName(param1, param2, param3) {
    // use param1, param2, param3
}

functionName(value1, value2, value3); // pass arguments
```

### Example
```js
function multiple(number1, number2, number3) {
    alert(number1 * number2 * number3);
}

multiple(2, 3, 4); // → alerts 24
```

### 🔷 LWC Use Case
Reusable utility functions that accept field values as parameters.

```js
calculateDiscount(price, discountPercent) {
    return price - (price * discountPercent / 100);
}

this.finalPrice = this.calculateDiscount(1000, 10); // → 900
```

---

## 4. Missing / Extra Arguments — No Error

### Definition
JavaScript does **NOT throw an error** if you call a function with:
- **Fewer arguments** than parameters → missing ones become `undefined`
- **More arguments** than parameters → extras are silently ignored

### Example

**Fewer arguments:**
```js
function multiple(number1, number2, number3) {
    alert(number2);              // undefined
    alert(number3);              // undefined
    alert(number1 * number1 * number1);  // 2*2*2 = 8
}

multiple(2);  // No error! number2 and number3 are undefined
```

**Extra arguments:**
```js
function qube() {
    // no parameters defined
}

qube('Hello World!'); // No error — argument is silently ignored
```

> 💡 This is why the `arguments` object exists — to handle cases where you don't know how many arguments will be passed.

---

## 5. `arguments` Object

### Definition
Every regular function (not arrow functions) has access to a **built-in `arguments` object** — an array-like object containing **all arguments passed** to the function, regardless of how many parameters are defined.

### Syntax
```js
function functionName() {
    console.log(arguments);        // all arguments
    console.log(arguments[0]);     // first argument
    console.log(arguments[1]);     // second argument
}
```

### Example
```js
// Single argument
function qube() {
    console.log(arguments[0] * arguments[0] * arguments[0]);
}
qube(3);
// → 27

// Multiple arguments
function qube() {
    console.log(arguments);
}
qube('Hello World!', 'How are you!');
// → Arguments(2) ['Hello World!', 'How are you!', callee: ƒ, Symbol(Symbol.iterator): ƒ]
```

**`arguments` object structure:**
```
Arguments(2) [
  0: 'Hello World!',
  1: 'How are you!',
  callee: ƒ,               ← reference to the function itself
  Symbol(Symbol.iterator): ƒ
]
```

> ⚠️ `arguments` is **NOT available** in arrow functions `() => {}`.  
> For modern code, use **rest parameters** instead: `function fn(...args) {}`

### 🔷 LWC Use Case
Building a flexible logger utility that accepts any number of messages.

```js
function logMessages() {
    for (let i = 0; i < arguments.length; i++) {
        console.log('[LWC Log]', arguments[i]);
    }
}

logMessages('Component loaded', 'User:', 'Raj', 'Records:', 25);
```

---

## 6. Closures

### Definition
A **closure** is when an **inner function remembers and accesses variables from its outer function's scope**, even after the outer function has finished executing.

> In simple terms: The inner function "closes over" the variable from the outer function and keeps it alive.

### Syntax
```js
let outerFunction = function(outerVariable) {
    let innerFunction = function() {
        return outerVariable; // inner function uses outer variable
    };
    return innerFunction;    // return inner function (not its result)
};

let myClosure = outerFunction('someValue');
myClosure(); // → 'someValue'  (outer variable still accessible!)
```

### Example
```js
let pet = function(name) {
    let getName = function() {
        return name;   // 'name' is from the outer function
    };
    return getName;    // return the function reference, NOT getName()
};

let myPet = pet('John');  // outer function executes, name = 'John'

myPet();  // → 'John'  (getName still remembers 'name' = 'John')

alert(myPet);
// → shows the function body:
// function(){ return name; }
// (it's just the function definition, not the result)
```

> 💡 **Key insight:**
> - `pet('John')` → executes the outer function, returns `getName` (the function reference)
> - `myPet` → holds the `getName` function
> - `myPet()` → executes `getName`, which still has access to `name = 'John'`

### 🔷 LWC Use Case
**Creating private state** in a utility module — exposing only a getter, keeping the value encapsulated.

```js
// utils/counter.js
let makeCounter = function(start) {
    let count = start;
    return {
        increment: function() { count++; },
        getCount:  function() { return count; }
    };
};

let pageCounter = makeCounter(0);
pageCounter.increment();
pageCounter.increment();
console.log(pageCounter.getCount()); // → 2
// 'count' variable is private — not accessible directly
```

---

## 7. Functions Inside Objects & `this` Keyword

### Definition
Functions can be stored as **properties of an object** — these are called **methods**.  
Inside a method, `this` refers to the **object that owns the method**.

### Syntax
```js
let objectName = {
    property1: 'value',
    methodName: function() {
        // use this.property1 to access own properties
        alert(this.property1);
    }
};

objectName.methodName();
```

### Example

**Without `this` — ❌ ReferenceError:**
```js
let user = {
    firstName: 'Amit',
    lastName: 'Singh',
    sayHello: function() {
        alert('Hello There!' + firstName); // ❌ ReferenceError: firstName is not defined
    }
};
user.sayHello(); // Uncaught ReferenceError: firstName is not defined
```

**With `this` — ✅ Works:**
```js
let user = {
    firstName: 'Amit',
    lastName: 'Singh',
    sayHello: function() {
        alert('Hello There!' + this.firstName); // ✅ this.firstName = 'Amit'
    }
};
user.sayHello(); // → alerts 'Hello There!Amit'
```

> ⚠️ `firstName` alone looks for a **global variable** named `firstName` — doesn't exist → error.  
> `this.firstName` looks for `firstName` **on the current object** → finds it ✅

### 🔷 LWC Use Case
`this` is used extensively in LWC to access component properties and call other methods.

```js
// In LWC JS controller
handleSubmit() {
    let fullName = this.firstName + ' ' + this.lastName; // access tracked properties
    this.showSuccessMessage(fullName);                   // call another method
}

showSuccessMessage(name) {
    this.message = 'Hello, ' + name + '!';
}
```

---

## 8. IIFE — Immediately Invoked Function Expression

### Definition
An **IIFE** (pronounced "iffy") is a function that is **defined and executed immediately** at the same time.  
It runs once automatically — no need to call it separately.

### Syntax
```js
// Using const/let
const result = function() {
    // body — runs immediately
}(); // ← the () at the end invokes it right away

// Classic IIFE pattern (wrapping in parens)
(function() {
    // body
})();
```

### Example
```js
// IIFE — runs immediately on definition
const hello = function() {
    alert('Hello');
}();
// → alerts 'Hello' instantly, no need to call hello()

const greet = function() {
    alert('HelloVikas');
}();
// → alerts 'HelloVikas' instantly
```

> ⚠️ After an IIFE executes, the variable holds the **return value** (or `undefined` if nothing returned), NOT the function itself.

---

## 9. Declaration vs Expression vs IIFE — Full Comparison

| Feature | Function Declaration | Function Expression | IIFE |
|---|---|---|---|
| **Syntax** | `function fn() {}` | `let fn = function() {}` | `(function() {})()` |
| **Hoisted?** | ✅ Yes | ❌ No | ❌ No |
| **Has a name?** | ✅ Yes | Optional (usually anonymous) | Usually anonymous |
| **When does it run?** | When called | When called | Immediately, once |
| **Can be called later?** | ✅ Yes | ✅ Yes | ❌ No (runs once and done) |
| **Use case** | Reusable named logic | Callbacks, stored functions | Init code, private scope |

### Code Comparison
```js
// 1. Declaration — define now, call whenever
function sayHi() { alert('Hi'); }
sayHi();           // called explicitly

// 2. Expression — assign to variable, call whenever
let sayHi = function() { alert('Hi'); };
sayHi();           // called explicitly

// 3. IIFE — runs immediately, cannot be called again
const sayHi = function() { alert('Hi'); }();  // runs RIGHT NOW
// sayHi is now undefined (or the return value)
```

### 🔷 LWC Use Case — IIFE
**Running initialization logic once** when a module loads, without polluting the outer scope.

```js
// In an LWC utility module — runs once when the module is imported
const CONFIG = (function() {
    const BASE_URL = '/api/v1';
    const TIMEOUT  = 5000;
    return { BASE_URL, TIMEOUT };
})();

// Usage in LWC
console.log(CONFIG.BASE_URL); // → '/api/v1'
```

---

## ⚠️ Common Mistakes

| Mistake | What Happened | Fix |
|---|---|---|
| `cosole.log(...)` | `ReferenceError: cosole is not defined` | Correct spelling: `console.log()` |
| `alert(firstName)` inside object method | `ReferenceError: firstName is not defined` | Use `this.firstName` |
| `sayHello` vs `sayHello()` | `sayHello` just references the function; `sayHello()` calls it | Always use `()` to invoke |
| Calling expression before definition | `TypeError: redFunction is not defined` | Move call after the expression line |
| `arguments` in arrow function | Returns `undefined` or error | Use rest params `(...args)` in arrow functions |

---

## 📊 Quick Reference

```
Function Types
│
├── Declaration     → function fn() {}          → hoisted ✅
├── Expression      → let fn = function() {}    → not hoisted ❌
├── Arrow           → let fn = () => {}         → no 'this', no 'arguments'
└── IIFE            → (function() {})()         → runs once immediately
```

---