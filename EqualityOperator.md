# JavaScript – Equality Operators Deep Dive

---

## 1. The Three Ways to Compare in JavaScript

JavaScript has **3 types of equality**:

| Operator / Method | Name | Type Check | Coercion |
|---|---|---|---|
| `==` | Loose Equality | ❌ No | ✅ Yes |
| `===` | Strict Equality | ✅ Yes | ❌ No |
| `Object.is()` | Same Value Equality | ✅ Yes | ❌ No (+ handles edge cases) |

---

## 2. `==` – Loose Equality (Abstract Equality)

Compares values **after type coercion** — JavaScript tries to convert both sides to the same type before comparing.

```js
let x = 10;
let y = '10';

x == y   // true  → '10' is coerced to number 10, then 10 == 10
```

### How coercion works with `==`:
- `Number == String` → String is converted to Number
- `Boolean == Number` → Boolean converted to Number (`true` → 1, `false` → 0)
- `null == undefined` → `true` (special built-in rule)
- `null == 0` → `false` (null only equals undefined)

### ⚠️ Why `==` is dangerous – inconsistent results:
```js
0 == false      // true   (false → 0)
'' == false     // true   (both → 0)
null == undefined  // true
0 == ''         // true
0 == '0'        // true
'' == '0'       // false  ← inconsistent!
```

> These inconsistencies make `==` unpredictable. **Avoid `==` in production code.**

---

## 3. `===` – Strict Equality

Compares **both value AND type** — no coercion happens at all.

```js
let x = 10;     // type: number
let y = '10';   // type: string

x === y   // false → same value but DIFFERENT types (number vs string)
```

```js
10 === 10           // true  → same value, same type
10 === '10'         // false → different types
0 === false         // false → different types
null === undefined  // false → different types
```

> ✅ **Always prefer `===` over `==`** for predictable comparisons.

---

## 4. Edge Cases Where Even `===` Falls Short

### 4.1 `+0` vs `-0`

JavaScript has two zeros: positive zero and negative zero.

```js
+0 == -0    // true  → loose equality ignores the sign
+0 === -0   // true  → strict equality ALSO ignores the sign ⚠️
```

> Even `===` cannot distinguish `+0` from `-0`.

---

### 4.2 `NaN` – Not a Number

`NaN` is the result of invalid math (e.g. `0/0`, `parseInt('abc')`).

```js
NaN == NaN    // false  → NaN is never equal to anything, even itself
NaN === NaN   // false  → same behavior with strict equality
```

> This is defined by the **IEEE 754 floating point standard**.  
> **`NaN` is the only JavaScript value that is not equal to itself.**

---

## 5. `Object.is()` – Same Value Equality

`Object.is(a, b)` behaves like `===` but **correctly handles both edge cases** above.

```js
Object.is(0, -0)      // false ✅ → correctly tells +0 and -0 apart
Object.is(NaN, NaN)   // true  ✅ → correctly identifies NaN === NaN
```

### `Object.is()` vs `===` side by side:

| Comparison | `===` result | `Object.is()` result |
|---|---|---|
| `+0` vs `-0` | `true` ❌ | `false` ✅ |
| `NaN` vs `NaN` | `false` ❌ | `true` ✅ |
| `1` vs `1` | `true` ✅ | `true` ✅ |
| `'a'` vs `'a'` | `true` ✅ | `true` ✅ |
| `null` vs `null` | `true` ✅ | `true` ✅ |

---

## 6. How to Detect `NaN` Practically

Since `NaN !== NaN`, JavaScript provides dedicated tools:

```js
// ❌ isNaN() – coerces argument first, can give false positives
isNaN('hello')      // true  → 'hello' is coerced to NaN first
isNaN(NaN)          // true

// ✅ Number.isNaN() – strict, no coercion, most reliable
Number.isNaN(NaN)       // true
Number.isNaN('hello')   // false → doesn't coerce, only true for actual NaN

// ✅ Object.is()
Object.is(NaN, NaN)  // true
```

> ✅ **Prefer `Number.isNaN()`** for accurate NaN detection.

---

## 7. All Comparisons from the Session – Explained

```js
let x = 10;
let y = '10';

x == y            // true  → '10' coerced to 10, values match
x === y           // false → number vs string, types don't match

+0 == -0          // true  → both treated as 0
+0 === -0         // true  → === can't distinguish signed zeros

NaN == NaN        // false → NaN is never equal to itself
NaN === NaN       // false → same rule applies

Object.is(0, -0)      // false → the only way to tell +0 and -0 apart
Object.is(NaN, NaN)   // true  → the only way to confirm NaN equality
```

---

## 8. When to Use Which

| Situation | Best Tool |
|---|---|
| Everyday value comparison | `===` |
| Checking if a value is `NaN` | `Number.isNaN()` |
| Distinguishing `+0` from `-0` | `Object.is()` |
| Precise same-value check (libraries/algorithms) | `Object.is()` |
| Avoid entirely | `==` |

---

## 9. Golden Rules

> 1. **Always use `===`** — never `==` — to avoid unexpected type coercion.
> 2. **`NaN` is never equal to anything**, including itself. Use `Number.isNaN()` to check for it.
> 3. **`===` cannot distinguish `+0` from `-0`** — use `Object.is()` for that.
> 4. **`Object.is()`** is the most precise equality check in JavaScript — use it for edge cases.