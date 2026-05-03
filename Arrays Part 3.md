# 📦 JavaScript Arrays – Part 3
---

## Methods Covered in This File

| # | Method | Returns | Mutates Original? |
|---|--------|---------|:-----------------:|
| 1 | `filter()` | New filtered array | ❌ No |
| 2 | `find()` | First matched element or `undefined` | ❌ No |
| 3 | `reduce()` | Single accumulated value | ❌ No |
| 4 | `reduceRight()` | Single accumulated value (RTL) | ❌ No |
| 5 | `sort()` — numbers | Sorted array (in-place) | ✅ Yes |
| 6 | `sort()` — objects by number | Sorted array (in-place) | ✅ Yes |
| 7 | `sort()` — objects by string | Sorted array (in-place) | ✅ Yes |

---

## 1. `filter()`

### Definition
Creates a **new array** containing only the elements that **pass a given condition** (callback returns `true`).  
Does NOT modify the original array.

### Syntax
```js
array.filter(function(currentVal, index, originalArray) {
    return condition; // keep element if true
});
```

| Parameter | Description |
|---|---|
| `currentVal` | Current element being tested |
| `index` | Index of current element |
| `originalArray` | The original array |

### Example
```js
let number = [23, 34, 45, 56, 67, 78, 89, 2, 3];

let newNumber = number.filter(function(currentVal) {
    return currentVal > 10;
});

// newNumber → [23, 34, 45, 56, 67, 78, 89]
// Original number array → unchanged ✅
```

> 💡 Elements where the callback returns `false` (or falsy) are **excluded** from the new array.

### 🔷 LWC Use Case
**Filtering a record list based on user search input or a picklist selection.**

```js
// Filter contacts by selected Account
handleAccountChange(event) {
    const selectedAccount = event.detail.value;
    this.filteredContacts = this.allContacts.filter(function(contact) {
        return contact.AccountId === selectedAccount;
    });
}
```

Also used for **removing a deleted row** from a displayed list without calling the server again:

```js
handleDelete(event) {
    const deletedId = event.detail.row.Id;
    this.records = this.records.filter(function(record) {
        return record.Id !== deletedId;
    });
}
```

---

## 2. `find()`

### Definition
Returns the **first element** in the array that satisfies the provided condition.  
Returns `undefined` if no element passes the condition.  
Stops searching as soon as the first match is found.

### Syntax
```js
array.find(function(currentVal, index, originalArray) {
    return condition;
});
```

### Example
```js
// Finding a string value
let fruits = ['Apples1', 'Bananas', 'Oranges', 'Watermelon'];

let fruitName = fruits.find(function(currentVal) {
    return currentVal === 'Oranges';
});
// fruitName → 'Oranges'

// Finding an object by Id
let selectRecordId = '002uiu34';
let contactList = []; // empty array

let contactRecord = contactList.find(function(currentVal) {
    return currentVal.Id === selectRecordId;
});
// contactRecord → undefined  (because contactList is empty)
```

> ⚠️ `find()` returns the **element itself**, not its index. Use `findIndex()` if you need the position.

### 🔷 LWC Use Case
**Finding a specific record from a locally cached list** — avoids an extra Apex call just to get one record's details.

```js
// User clicks a row — find that record object from the already-loaded list
handleRowAction(event) {
    const recordId = event.detail.row.Id;
    this.selectedRecord = this.allRecords.find(function(rec) {
        return rec.Id === recordId;
    });
    this.showModal = true;
}
```

---

## 3. `reduce()`

### Definition
Executes a **reducer function** on each element of the array (left to right), producing a **single output value** (accumulator).  
The accumulator carries the running result from one iteration to the next.

### Syntax
```js
array.reduce(function(accumulator, currentVal, index, originalArray) {
    return updatedAccumulator;
}, initialValue);
```

| Parameter | Description |
|---|---|
| `accumulator` | Carries the running result. Starts as `initialValue` on first call |
| `currentVal` | Current element being processed |
| `index` | Index of current element |
| `originalArray` | The original array |
| `initialValue` | Starting value of the accumulator (recommended — always pass this) |

### Example
```js
let number = [23, 34, 45, 56, 67, 78, 89, 2, 3];

let sum = number.reduce(function(accumulator, currentVal) {
    return accumulator + currentVal;
}, 0);

// sum → 397
```

**Step-by-step trace (from console output):**

| Step | Accumulator (before) | currentVal | Result |
|------|----------------------|------------|--------|
| 1 | 0 | 23 | 23 |
| 2 | 23 | 34 | 57 |
| 3 | 57 | 45 | 102 |
| 4 | 102 | 56 | 158 |
| 5 | 158 | 67 | 225 |
| 6 | 225 | 78 | 303 |
| 7 | 303 | 89 | 392 |
| 8 | 392 | 2 | 394 |
| 9 | 394 | 3 | **397** |

> 💡 Always pass `initialValue` (the `0` after the function). Without it, the first element becomes the accumulator and is skipped — can cause bugs with empty arrays.

### 🔷 LWC Use Case
**Calculating totals in an order/invoice component** — summing up line item amounts.

```js
get orderTotal() {
    return this.lineItems.reduce(function(total, item) {
        return total + (item.quantity * item.unitPrice);
    }, 0);
}
```

Also useful for **building a map/object from an array** (grouping records):

```js
// Group contacts by AccountId
let groupedByAccount = this.contacts.reduce(function(acc, contact) {
    let key = contact.AccountId;
    if (!acc[key]) acc[key] = [];
    acc[key].push(contact);
    return acc;
}, {});
```

---

## 4. `reduceRight()`

### Definition
Same as `reduce()` but processes the array **from right to left** (last element to first).  
Produces the same sum for addition, but order matters for string concatenation, nested structures, or operations where sequence is significant.

### Syntax
```js
array.reduceRight(function(accumulator, currentVal, index, originalArray) {
    return updatedAccumulator;
}, initialValue);
```

### Example
```js
let number = [23, 34, 45, 56, 67, 78, 89, 2, 3];

let sum = number.reduceRight(function(accumulator, currentVal) {
    return accumulator + currentVal;
}, 0);

// sum → 397 (same result for addition, but processes 3 → 2 → 89 → 78 → ...)
```

**Step-by-step trace (from console output):**

| Step | Accumulator (before) | currentVal | Result |
|------|----------------------|------------|--------|
| 1 | 0 | 3 | 3 |
| 2 | 3 | 2 | 5 |
| 3 | 5 | 89 | 94 |
| 4 | 94 | 78 | 172 |
| ... | ... | ... | ... |
| 9 | 374 | 23 | **397** |

> 💡 `reduce()` goes **left → right**. `reduceRight()` goes **right → left**. For addition both give the same result. For string building or nested logic, the direction matters.

### 🔷 LWC Use Case
**Processing breadcrumb / navigation path in reverse** — building a path string from the deepest child up to the root.

```js
let breadcrumbs = ['Home', 'Accounts', 'Acme Corp', 'Contacts'];

let reversePath = breadcrumbs.reduceRight(function(acc, crumb) {
    return acc ? acc + ' > ' + crumb : crumb;
}, '');
// → 'Contacts > Acme Corp > Accounts > Home'
```

---

## 5. `sort()` — Numbers

### Definition
Sorts the elements of an array **in-place** (modifies the original array) and returns the sorted array.

For **numbers**, you must pass a comparator function. Without it, numbers are sorted as strings (`[2, 10, 3]` → `[10, 2, 3]` — wrong!).

### Syntax
```js
// Ascending (smallest first)
array.sort(function(a, b) {
    return a - b;
});

// Descending (largest first)
array.sort(function(a, b) {
    return b - a;
});
```

### How the Comparator Works

| Return value | Meaning |
|---|---|
| Negative (`a - b < 0`) | `a` comes **before** `b` |
| Positive (`a - b > 0`) | `a` comes **after** `b` |
| `0` | Order stays the same |

### Example
```js
let number = [23, 34, 45, 56, 67, 78, 89, 2, 3];

// Ascending
number.sort(function(a, b) { return a - b; });
// → [2, 3, 23, 34, 45, 56, 67, 78, 89]

// Descending
number.sort(function(a, b) { return b - a; });
// → [89, 78, 67, 56, 45, 34, 23, 3, 2]
```

> ⚠️ `sort()` mutates the original array. Clone first if you need the original: `[...number].sort(...)`

### 🔷 LWC Use Case
**Sorting a datatable column client-side** when the user clicks a column header.

```js
handleSort(event) {
    const { fieldName, sortDirection } = event.detail;
    this.sortedBy = fieldName;
    this.sortDirection = sortDirection;

    this.records = [...this.records].sort(function(a, b) {
        return sortDirection === 'asc'
            ? a[fieldName] - b[fieldName]
            : b[fieldName] - a[fieldName];
    });
}
```

---

## 6. `sort()` — Objects by Number Property

### Definition
To sort an array of **objects** by a numeric property, subtract the property values in the comparator.

### Syntax
```js
array.sort(function(a, b) {
    return a.propertyName - b.propertyName;  // ascending
    // OR
    return b.propertyName - a.propertyName;  // descending
});
```

### Example
```js
let items = [
    { name: 'Edward', value: 21 },
    { name: 'Edwar',  value: 22 },
    { name: 'Edwad',  value: 213 },
    { name: 'Edwd',   value: 25 }
];

items.sort(function(a, b) {
    return a.value - b.value;  // ascending by value
});
// → Edward(21), Edwar(22), Edwd(25), Edwad(213)
```

> ❌ **Why `a.name - b.name` doesn't work for strings:**  
> The `-` operator tries to convert strings to numbers. `'Edward' - 'Edwar'` → `NaN`. The sort stays unchanged.  
> For strings, use the `toUpperCase()` comparison pattern (see Method 7 below).

### 🔷 LWC Use Case
**Sorting Opportunity records by Amount or CloseDate value** in a pipeline view.

```js
this.opportunities = [...this.opportunities].sort(function(a, b) {
    return b.Amount - a.Amount; // highest amount first
});
```

---

## 7. `sort()` — Objects by String Property

### Definition
To sort objects by a **string property**, convert both values to the same case and use comparison operators (`<`, `>`).  
Returns `-1`, `1`, or `0` to tell the sort engine the order.

### Syntax
```js
array.sort(function(a, b) {
    let valA = a.propertyName.toUpperCase();
    let valB = b.propertyName.toUpperCase();

    if (valA < valB) return -1;  // a before b
    if (valA > valB) return  1;  // b before a
    return 0;                    // equal
});
```

### Example
```js
let names = [
    { name: 'Bheem',   value: 21 },
    { name: 'Edwar',   value: 22 },
    { name: 'Vikas',   value: 27 },
    { name: 'Vaibhav', value: 15 }
];

names.sort(function(a, b) {
    let nameA = a.name.toUpperCase();
    let nameB = b.name.toUpperCase();

    if (nameA < nameB) return -1;
    if (nameA > nameB) return  1;
    return 0;
});

// → Bheem, Edwar, Vaibhav, Vikas  (alphabetical A→Z)
```

> 💡 `.toUpperCase()` is used so that `'apple'` and `'Apple'` are treated as equal — otherwise lowercase letters sort after ALL uppercase letters in ASCII.

### 🔷 LWC Use Case
**Alphabetically sorting Contact/Account names** in a combobox or list view.

```js
this.contacts = [...this.contacts].sort(function(a, b) {
    let nameA = a.Name.toUpperCase();
    let nameB = b.Name.toUpperCase();
    if (nameA < nameB) return -1;
    if (nameA > nameB) return  1;
    return 0;
});
```

---

## ⚠️ Common Mistakes from Practice

| Mistake | What Happened | Fix |
|---|---|---|
| `a.name - b.name` for strings | Returns `NaN`, sort order unchanged | Use `toUpperCase()` + `<` `>` comparison |
| `bname.toUpperCase()` | `ReferenceError: bname is not defined` | Should be `b.name.toUpperCase()` |
| Missing `'` in object string values | `SyntaxError: Invalid or unexpected token` | Close all string literals properly |
| No `initialValue` in `reduce()` | Crashes on empty array | Always pass `0` or `{}` as initial value |
| Mutating array in `sort()` without clone | Original array lost | Use `[...array].sort(...)` to preserve original |

---

## 📊 Quick Method Summary

| Method | Input | Output | Stops Early? | Mutates? |
|---|---|---|---|---|
| `filter()` | Condition | New array (matching items) | ❌ | ❌ |
| `find()` | Condition | First match or `undefined` | ✅ (on first match) | ❌ |
| `reduce()` | Reducer + initial value | Single value (L→R) | ❌ | ❌ |
| `reduceRight()` | Reducer + initial value | Single value (R→L) | ❌ | ❌ |
| `sort()` numbers | `a - b` comparator | Sorted array | ❌ | ✅ |
| `sort()` strings | `<` `>` comparator | Sorted array | ❌ | ✅ |

---

## 🔷 LWC Cheat Sheet — Which Method to Use When

| Scenario | Method |
|---|---|
| Show only active records | `filter()` |
| Find a record by Id from cached list | `find()` |
| Calculate total / sum of line items | `reduce()` |
| Build a grouped map from a flat list | `reduce()` |
| Sort datatable column (numbers) | `sort()` with `a - b` |
| Sort combobox options (strings) | `sort()` with `toUpperCase()` |
| Remove a deleted row from displayed list | `filter()` |

---
