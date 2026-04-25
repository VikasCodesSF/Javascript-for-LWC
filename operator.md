# JavaScript – Operators, Type Coercion & Math Notes

---

## 1. Variables & Basic Assignment

```js
let firstName = 'Vikas';  // ✅ Correct
firstName  // 'Vikas'

// ⚠️ Tip: Typos in variable names (e.g. firstNmae) won't throw errors
// but cause bugs later — always double-check naming
```

---

## 2. Arithmetic with Numbers

```js
let num1 = 03;   // Note: leading 0 is allowed but treated as octal in strict mode
let num2 = 934;
let num3 = num1 + num2;

num3  // 937
```

---

## 3. Type Coercion – The `+` Operator Trap ⚠️

The `+` operator behaves differently depending on data types involved.

### Number + String → String (concatenation)
```js
num1 + '10'   // '310'  → number 3 is coerced to string "3"
"10" + 3      // '103'  → string wins, 3 becomes "3"
```

### Number + Number + String → String (left to right)
```js
1 + 3 + '4'   // '44'  → 1+3=4 first, then 4+'4' = '44'
```

> ✅ JavaScript evaluates **left to right**. Once a string is encountered, the rest is concatenation.

---

## 4. Other Operators with Strings (Coercion to Number)

For `-`, `*`, `/`, `%` — JavaScript **automatically converts** strings to numbers:

```js
"10" - 3   // 7    → string "10" coerced to number 10
"10" * 3   // 30
"10" / 3   // 3.3333333333333335
"10" % 3   // 1    → remainder
```

> ⚠️ Only `+` causes string concatenation. All other math operators force numeric conversion.

---

## 5. Fixing Coercion with `Number()`

```js
1 + 3 + Number('4')   // 8  → '4' converted to number 4 before addition
Number('34')          // 34
```

---

## 6. Increment & Decrement Operators

### Post-increment (`x++`) — returns current value, THEN increments
```js
let x = 4;
x++   // returns 4  (original value returned first)
x     // 5          (now incremented)
```

### Manual increment
```js
x = x + 1  // 6
```

### Pre-increment (`++x`) — increments FIRST, then returns new value
```js
++x   // 7  (incremented first, then returned)
x     // 7
```

### Post-decrement (`x--`) — returns current value, THEN decrements
```js
x--   // 7  (returns 7 first)
x     // 6  (now decremented)
```

### Pre-decrement (`--x`) — decrements FIRST, then returns new value
```js
--x   // 5  (decremented first, then returned)
```

### Summary Table

| Operator | When is value changed? | Expression returns |
|----------|------------------------|-------------------|
| `x++` | After the expression | Original value |
| `++x` | Before the expression | New value |
| `x--` | After the expression | Original value |
| `--x` | Before the expression | New value |

---

## 7. Exponentiation

Two ways to raise a number to a power:

```js
4 ** 2          // 16  → ES2016+ exponentiation operator
Math.pow(4, 2)  // 16  → traditional Math method
```

| Method | Syntax | Result |
|--------|--------|--------|
| `**` operator | `base ** exponent` | `16` |
| `Math.pow()` | `Math.pow(base, exponent)` | `16` |

> ✅ Both are equivalent. `**` is shorter and more modern.

---

## 8. Type Coercion – Full Rules Summary

| Expression | Result | Reason |
|------------|--------|--------|
| `3 + '10'` | `'310'` | Number + String = String |
| `"10" + 3` | `'103'` | String + Number = String |
| `1 + 3 + '4'` | `'44'` | Left-to-right: 4 + '4' = '44' |
| `"10" - 3` | `7` | `-` forces numeric conversion |
| `"10" * 3` | `30` | `*` forces numeric conversion |
| `"10" / 3` | `3.333...` | `/` forces numeric conversion |
| `"10" % 3` | `1` | `%` forces numeric conversion |
| `Number('4')` | `4` | Explicit string-to-number conversion |

> **Golden Rule:**
> - `+` with any string → **concatenation**
> - `-`, `*`, `/`, `%` with strings → **numeric coercion**

---

## 9. Quick Reference – All Operators Covered

| Operator | Name | Example | Result |
|----------|------|---------|--------|
| `+` | Addition / Concatenation | `1 + 1` | `2` |
| `-` | Subtraction | `"10" - 3` | `7` |
| `*` | Multiplication | `"10" * 3` | `30` |
| `/` | Division | `"10" / 3` | `3.333...` |
| `%` | Modulus (remainder) | `"10" % 3` | `1` |
| `**` | Exponentiation | `4 ** 2` | `16` |
| `++` | Increment (pre/post) | `x++` / `++x` | see above |
| `--` | Decrement (pre/post) | `x--` / `--x` | see above |
| `Number()` | Type conversion | `Number('34')` | `34` |
| `Math.pow()` | Power (Math method) | `Math.pow(4,2)` | `16` |