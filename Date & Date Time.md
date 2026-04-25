# JavaScript `Date` Object – Notes

---

## 1. Creating a Date

```js
// ❌ Wrong – 'date' is case-sensitive, must be capitalized
new date();  // ReferenceError: date is not defined

// ✅ Correct – current date & time
new Date();  // Mon Apr 20 2026 18:12:04 GMT+0530 (India Standard Time)

// ✅ From a date string – must end with semicolon, not colon
new Date("2014-03-21");  // Fri Mar 21 2014 05:30:00 GMT+0530
// ❌ new Date("2014-03-21"):  → SyntaxError (colon instead of semicolon)
```

---
h

```js
todaysDate.getMonths();  // ❌ TypeError: todaysDate.getMonths is not a function
todaysDate.getMonth();   // ✅ Correct (no 's')
```

---

## 4. Setter Methods

```js
todaysDate.setDate(10);        // Sets day-of-month to 10 → returns new timestamp (ms)
todaysDate.setFullYear(2308);  // Sets year to 2308 → returns new timestamp (ms)

// After both setters:
todaysDate  // Fri Apr 10 2308 18:13:09 GMT+0530
```

> Setter methods return the **timestamp in milliseconds** (not the Date object itself), but they **mutate** the original Date object.

---

## 5. Formatting with `toLocaleDateString()`

Correct method name: **`toLocaleDateString()`**

```js
// ❌ Common typos
todaysDate.toLocalDateString('en-US')   // TypeError – missing 'e' in 'Locale'
todaysDate.toLocalEDateString('en-US')  // TypeError – extra 'E'

// ✅ Correct
todaysDate.toLocaleDateString('en-US')  // '4/10/2308'  → M/D/YYYY
todaysDate.toLocaleDateString('fr')     // '10/04/2308' → D/M/YYYY
todaysDate.toLocaleDateString('en-GB')  // '10/04/2308' → D/M/YYYY
todaysDate.toLocaleDateString('fr-CA')  // '2308-04-10' → YYYY-MM-DD
```

### ⚠️ Locale Tag Format: use hyphens, not underscores

```js
todaysDate.toLocaleDateString('en_US')  // ❌ RangeError: Invalid language tag
todaysDate.toLocaleDateString('en-US')  // ✅ Correct
```

### Locale Format Summary

| Locale | Format | Example |
|--------|--------|---------|
| `en-US` | M/D/YYYY | `4/10/2308` |
| `fr` | D/M/YYYY | `10/04/2308` |
| `en-GB` | D/M/YYYY | `10/04/2308` |
| `fr-CA` | YYYY-MM-DD | `2308-04-10` |

---

## 6. Error Reference

| Error | Cause | Fix |
|-------|-------|-----|
| `ReferenceError: date is not defined` | Used lowercase `date` | Use `Date` (capital D) |
| `SyntaxError: Unexpected token ':'` | Used `:` instead of `;` at end of statement | End with `;` |
| `TypeError: getMonths is not a function` | Method doesn't exist | Use `getMonth()` |
| `TypeError: toLocalDateString is not a function` | Typo in method name | Use `toLocaleDateString()` |
| `RangeError: Invalid language tag` | Used underscore in locale (`en_US`) | Use hyphen: `en-US` |

---

## 7. Quick Reference – Common Date Methods

| Method | Returns | Example Output |
|--------|---------|---------------|
| `getDate()` | Day of month (1–31) | `20` |
| `getDay()` | Day of week (0–6) | `1` (Monday) |
| `getMonth()` | Month (0–11) | `3` (April) |
| `getFullYear()` | 4-digit year | `2026` |
| `setDate(n)` | Timestamp (ms) | Mutates date |
| `setFullYear(n)` | Timestamp (ms) | Mutates date |
| `toLocaleDateString(locale)` | Formatted date string | `'4/20/2026'` |