# 📝 JavaScript String Functions
> **Level:** Beginner–Intermediate | **Topic:** Built-in String Methods  
> **Source:** Live Console Practice Session

---

## 🔤 String Basics

```js
let myStr = 'Vikas Pandey';
myStr.length; // 12
```

---

## ✂️ slice()

Extracts a portion of a string. Supports **negative indices** (count from end).

```js
myStr.slice(1, 5);    // 'ikas'
myStr.slice(0, 5);    // 'Vikas'
myStr.slice(0, -5);   // 'Vikas P'   → excludes last 5 chars
myStr.slice(-6);      // 'Pandey'    → last 6 chars
myStr.slice(-6, -5);  // 'P'
myStr.slice(-1, -5);  // ''          → start > end → empty string
myStr.slice(-6, 0);   // ''          → negative to 0 = empty
```

> **Rule:** If `start >= end` (after resolving negatives), returns `''`.

---

## ✂️ substring()

Similar to `slice()` but **does NOT support negative indices** — negatives are treated as `0`.

```js
myStr.substring(0, -6);  // '' → -6 treated as 0, so substring(0,0) = ''
myStr.substring(2, -6);  // 'Vi' → -6 → 0, so substring(2,0) = substring(0,2) = 'Vi'
```

> ⚠️ **Common Mistake:** Calling `myStr.subString(...)` throws:
> ```
> TypeError: myStr.subString is not a function
> ```
> JavaScript is **case-sensitive**. Correct method name: `substring` (lowercase `s`).

---

## ✂️ substr() *(deprecated)*

`substr(startIndex, length)` — second argument is **length**, not end index.

```js
myStr.substr(2, 6);  // 'kas Pa'  → start at index 2, take 6 characters
```

> ⚠️ `substr()` is deprecated. Prefer `slice()` or `substring()`.

---

## 🔁 replace() & replaceAll()

### replace()

Replaces the **first match** only.

```js
myStr.replace('Vikas', 'Shweta');        // 'Shweta Pandey'
myStr;                                    // 'Vikas Pandey' → original unchanged!

let newStr = myStr.replace('Vikas', 'Shweta');
newStr;  // 'Shweta Pandey'
```

> ⚠️ Strings are **immutable** in JS — `replace()` returns a new string.

#### Case-insensitive replace using RegEx `/i` flag:

```js
newStr.replace(/shweTa/i, 'Vikas');  // 'Vikas Pandey' ✅
```

### replaceAll()

Replaces **all matches**. When using RegEx, **must include `/g` flag**.

```js
let str = 'Vikas Vikas Pandey';

str.replace(/vikas/i, 'Shweta');      // 'Shweta Vikas Pandey' → only first

str.replaceAll(/vikas/i, 'Shweta');
// ❌ TypeError: String.prototype.replaceAll called with a non-global RegExp argument
```

> ✅ Fix: Use `/vikas/gi` (global + case-insensitive):
> ```js
> str.replaceAll(/vikas/gi, 'Shweta');  // 'Shweta Shweta Pandey'
> ```

---

## ❌ Common Errors Explained

### 1. `TypeError: myStr.subString is not a function`
**Cause:** Wrong capitalization — JS is case-sensitive.  
**Fix:** Use `myStr.substring(...)`.

### 2. `SyntaxError: Unexpected identifier 'newStr'`
```js
ley newStr = myStr.replace('Vikas','Shweta');  // ❌ typo: 'ley' instead of 'let'
```
**Cause:** Typo in keyword. `ley` is not a valid keyword.  
**Fix:** `let newStr = ...`

### 3. `ReferenceError: str is not defined`
```js
str.replace("ShweTa", "Vikas");  // ❌ 'str' was never declared
```
**Cause:** Using a variable before declaring it.  
**Fix:** Declare with `let str = '...'` first.

### 4. `TypeError: replaceAll called with non-global RegExp`
```js
str.replaceAll(/vikas/i, 'Shweta');  // ❌ missing /g flag
```
**Fix:** `str.replaceAll(/vikas/gi, 'Shweta');`

### 5. `SyntaxError: Unexpected token '.'`
```js
let cc.slice(-4);  // ❌ can't use 'let' with method call syntax
```
**Cause:** `let` expects a variable name, not an expression.  
**Fix:** `let newCC = cc.slice(-4);`

### 6. `ReferenceError: num is not defined`
```js
num.padStart(5, '*');  // ❌ variable was declared as 'num01', not 'num'
```
**Fix:** Use the correct variable name `num01.padStart(5, '*')`.

### 7. `TypeError: str.indexOfLast / str.lastIndexOfindexOf is not a function`
**Cause:** Method name typo. The correct method is `lastIndexOf`.  
**Fix:**
```js
str.lastIndexOf('V');  // ✅ 6
```

### 8. `TypeError: str.startWith is not a function`
**Cause:** Typo — missing the `s` at the end.  
**Fix:** `str.startsWith('Vikas')` ✅

---

## 🔧 Other String Methods

### trim()
Removes **leading and trailing whitespace** only.

```js
let trim = '    hello     world!';
trim.trim();  // 'hello     world!'  → spaces INSIDE are preserved
```

### padStart() / padEnd()
Pads a string to a target length from the start or end.

```js
let num01 = '5';
num01.padStart(5, '*');  // '****5'
num01.padEnd(5, '*');    // '5****'
```

#### 💳 Real-world use case — masking credit card number:

```js
let cc = '123456789';
let newCC = cc.slice(-4);             // '6789'
newCC.padStart(cc.length, '*');       // '*****6789'
newCC.padStart(cc.length, 'X');       // 'XXXXX6789'
```

---

## 🔍 indexOf() & lastIndexOf()

```js
let str = 'Vikas Vikas Pandey';
str.indexOf('V');      // 0  → first occurrence
str.lastIndexOf('V');  // 6  → last occurrence
```

---

## ✅ startsWith() & endsWith()

```js
str.startsWith('Vikas');   // true
str.endsWith('Vikas');     // false
str.endsWith('Pandey');    // true
```

---

## 🧵 Template Literals

Use backticks `` ` `` to embed variables/expressions.

```js
let num2 = 23;
let str = `This is num ${num2}`;
str;  // 'This is num 23'
```

---

## 📊 Quick Reference Table

| Method | Description | Negative Index? |
|---|---|---|
| `slice(start, end)` | Extract substring | ✅ Yes |
| `substring(start, end)` | Extract substring | ❌ No (treated as 0) |
| `substr(start, length)` | Extract by length *(deprecated)* | ✅ (start only) |
| `replace(old, new)` | Replace first match | — |
| `replaceAll(old, new)` | Replace all matches | — |
| `trim()` | Remove leading/trailing whitespace | — |
| `padStart(len, char)` | Pad from left | — |
| `padEnd(len, char)` | Pad from right | — |
| `indexOf(str)` | First index of match | — |
| `lastIndexOf(str)` | Last index of match | — |
| `startsWith(str)` | Check start | — |
| `endsWith(str)` | Check end | — |
| `length` | String length (property) | — |

---

> 💡 **Key Takeaways**
> - Strings are **immutable** — all methods return new strings.
> - `slice()` supports negatives; `substring()` does not.
> - `replaceAll()` with RegEx **requires `/g` flag**.
> - JS is **case-sensitive** — method names must be exact.
> - Template literals use **backticks** and `${}` for interpolation.