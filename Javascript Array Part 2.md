# 📦 JavaScript Arrays – Part 2
---

## Methods Covered in This File

| # | Method | Type | Mutates Original? |
|---|--------|------|:-----------------:|
| 1 | `pop()` | Remove | ✅ Yes |
| 2 | `push()` | Add | ✅ Yes |
| 3 | `shift()` | Remove | ✅ Yes |
| 4 | `unshift()` | Add | ✅ Yes |
| 5 | `slice()` | Extract | ❌ No |
| 6 | `splice()` | Add / Remove / Replace | ✅ Yes |
| 7 | `toString()` | Convert | ❌ No |
| 8 | `values()` | Iterate | ❌ No |
| 9 | `lastIndexOf()` | Search | ❌ No |

---

## 1. `pop()`

### Definition
Removes the **last element** from an array and **returns** that removed element.

### Syntax
```js
array.pop()
```

### Example
```js
let fruits = ['Apples', 'Bananas', 'Oranges', 'Amla', 'Mango'];

fruits.pop();   // returns 'Mango'
// fruits → ['Apples', 'Bananas', 'Oranges', 'Amla']

fruits.pop();   // returns 'Amla'
// fruits → ['Apples', 'Bananas', 'Oranges']
```

> ⚠️ Modifies the original array. Length decreases by 1 each time.

### 🔷 LWC Use Case
**Removing the last item from a dynamic list** — e.g., an "Undo Last Add" button in a multi-step form where the user adds row by row.

```js
// In LWC JS controller
undoLastRow() {
    this.tableRows.pop();          // remove last added row
    this.tableRows = [...this.tableRows]; // trigger reactivity
}
```
> 💡 In LWC, always reassign the array (`[...arr]`) after mutation so the template re-renders.

---

## 2. `push()`

### Definition
Adds **one or more elements** to the **end** of an array. Returns the **new length** of the array.

### Syntax
```js
array.push(element1, element2, ...)
```

### Example
```js
let fruits = ['Apples', 'Bananas', 'Oranges'];

fruits.push('Watermelon', 'Grapes');  // returns 5
// fruits → ['Apples', 'Bananas', 'Oranges', 'Watermelon', 'Grapes']

fruits.push('Guaves', 'Citafal');     // returns 7
// fruits → ['Apples', 'Bananas', 'Oranges', 'Watermelon', 'Grapes', 'Guaves', 'Citafal']
```

### 🔷 LWC Use Case
**Adding a new record row dynamically** — e.g., when a user clicks "Add Product Line" in an order form.

```js
addProduct() {
    this.productList = [
        ...this.productList,
        { id: Date.now(), name: '', quantity: 1 }
    ];
}
```
> 💡 Spread operator `[...arr, newItem]` is preferred over `.push()` in LWC because it creates a new array reference, which triggers `@track` reactivity automatically.

---

## 3. `shift()`

### Definition
Removes the **first element** from an array and **returns** that removed element. All remaining elements shift down by one index.

### Syntax
```js
array.shift()
```

### Example
```js
let fruits = ['Apples', 'Bananas', 'Oranges', 'Watermelon'];

fruits.shift();   // returns 'Apples'
// fruits → ['Bananas', 'Oranges', 'Watermelon']
```

> ⚠️ More performance-heavy than `pop()` because every remaining element's index must be updated.

### 🔷 LWC Use Case
**Queue / notification system** — removing the oldest (first) notification after it has been displayed, like a toast message queue.

```js
dismissFirstNotification() {
    this.notifications.shift();
    this.notifications = [...this.notifications]; // trigger re-render
}
```

---

## 4. `unshift()`

### Definition
Adds **one or more elements** to the **beginning** of an array. Returns the **new length** of the array.  
Called with no arguments, it returns the current length without modifying the array.

### Syntax
```js
array.unshift(element1, element2, ...)
```

### Example
```js
let fruits = ['Bananas', 'Oranges', 'Watermelon'];

fruits.unshift('Apples', 'Apples1');  // returns 5
// fruits → ['Apples', 'Apples1', 'Bananas', 'Oranges', 'Watermelon']

fruits.unshift('--- Select Options ---');
// fruits → ['--- Select Options ---', 'Apples', 'Apples1', 'Bananas', ...]
```

### 🔷 LWC Use Case
**Prepending a default placeholder option** to a picklist/combobox options array fetched from the server.

```js
// After wire call returns picklist values
handlePicklistData({ data, error }) {
    if (data) {
        this.options = data.values.map(item => ({
            label: item.label,
            value: item.value
        }));
        this.options.unshift({ label: '-- Select --', value: '' });
    }
}
```

---

## 5. `slice()`

### Definition
Returns a **shallow copy** of a portion of an array into a **new array**, selected from `start` index up to (but **not including**) the `end` index.  
**Does NOT modify the original array.**

### Syntax
```js
array.slice(start, end)
```

| Parameter | Description |
|---|---|
| `start` | Index to begin extraction (inclusive). Default: `0` |
| `end` | Index to stop extraction (exclusive — this index is NOT included). Default: end of array |

### Example
```js
let fruits = ['--- Select Options ---', 'Apples', 'Apples1', 'Bananas', 'Oranges', 'Watermelon', 'Grapes', 'Guaves', 'Citafal'];
//                [0]                     [1]       [2]         [3]        [4]          [5]           [6]      [7]       [8]

let firstPage = fruits.slice(0, 4);
// → ['--- Select Options ---', 'Apples', 'Apples1', 'Bananas']
// Indexes 0, 1, 2, 3 → (4 is excluded)

let secondPage = fruits.slice(5, 8);
// → ['Watermelon', 'Grapes', 'Guaves']
// Indexes 5, 6, 7 → (8 is excluded)

// Original array untouched ✅
```

> 💡 **Memory trick:** `slice(start, end)` → start is IN, end is OUT.

### 🔷 LWC Use Case
**Client-side pagination** — slicing a large data array into pages to display in a `lightning-datatable`.

```js
get paginatedData() {
    const start = (this.currentPage - 1) * this.pageSize;
    const end   = this.currentPage * this.pageSize;
    return this.allRecords.slice(start, end);
}
```

---

## 6. `splice()`

### Definition
The most powerful mutating method. Used to **insert**, **delete**, or **replace** elements at any position in an array.  
Returns an array of the **removed elements** (empty `[]` if nothing was removed).

### Syntax
```js
array.splice(startIndex, deleteCount, item1, item2, ...)
```

| Parameter | Description |
|---|---|
| `startIndex` | Index at which to start changing the array |
| `deleteCount` | Number of elements to remove. Pass `0` to only insert |
| `item1, item2...` | Elements to insert at `startIndex` (optional) |

### Examples

**Insert only (deleteCount = 0):**
```js
fruits.splice(3, 0, 23, 34, 45, 56);
// Inserts 4 numbers at index 3, removes nothing
// Returns []
// fruits → [..., 23, 34, 45, 56, 'Bananas', ...]
```

**Delete only (no items passed):**
```js
fruits.splice(3, 4);
// Removes 4 elements starting at index 3
// Returns [23, 34, 45, 56]

fruits.splice(1, 1);
// Removes 1 element at index 1
// Returns ['Apples']

fruits.splice(0, 1);
// Removes first element
// Returns ['--- Select Options ---']
```

**Replace (deleteCount > 0 + items passed):**
```js
fruits.splice(1, 1, 'NewFruit');
// Removes 1 element at index 1 and inserts 'NewFruit' in its place
```

### 🔷 LWC Use Case
**Inline editing in a datatable** — replacing a specific record in the array when a user saves an edit.

```js
saveEdit(event) {
    const index = this.records.findIndex(r => r.id === event.detail.id);
    this.records.splice(index, 1, event.detail.updatedRecord); // replace 1 item
    this.records = [...this.records]; // trigger re-render
}
```

Also used for **deleting a row by index:**
```js
deleteRow(event) {
    const index = event.detail.row.index;
    this.records.splice(index, 1);
    this.records = [...this.records];
}
```

---

## 7. `toString()`

### Definition
Converts an array to a **comma-separated string**. Similar to `join()` with no argument, but cannot accept a custom separator.

### Syntax
```js
array.toString()
```

### Example
```js
let fruits = ['Apples1', 'Bananas', 'Oranges', 'Watermelon'];

fruits.toString();
// → 'Apples1,Bananas,Oranges,Watermelon'
```

> 💡 Prefer `join(',')` over `toString()` when you need control over the separator.

### 🔷 LWC Use Case
**Sending multi-select picklist values to Apex** — LWC multi-select picklist returns an array; Apex `String` field expects a semicolon-separated string.

```js
handleMultiSelect(event) {
    const selectedValues = event.detail.value; // array
    this.selectedString = selectedValues.join(';'); // 'Val1;Val2;Val3'
    // OR for simple comma-separated:
    this.selectedString = selectedValues.toString();
}
```

---

## 8. `values()`

### Definition
Returns an **Array Iterator** object that contains the **value of each element**. Used with `for...of` loop or `.next()` to iterate.

### Syntax
```js
array.values()
```

### Example
```js
let fruits = ['Apples', 'Bananas', 'Oranges'];

for (let val of fruits.values()) {
    console.log(val);
}
// → 'Apples'
// → 'Bananas'
// → 'Oranges'
```

### 🔷 LWC Use Case
**Iterating wire adapter results** — processing values from a wire-returned list before binding to template.

```js
processResults(data) {
    const result = [];
    for (let val of data.values()) {
        result.push({ label: val.Name, value: val.Id });
    }
    this.comboboxOptions = result;
}
```

> 💡 In most LWC cases, `forEach()` or `map()` is preferred over `values()`. But `values()` is useful when working with Sets or Maps converted to arrays.

---

## 9. `lastIndexOf()`

### Definition
Returns the **last index** at which a given value is found in the array, searching **backwards** from the end.  
Returns `-1` if the value is not found.

### Syntax
```js
array.lastIndexOf(valueToSearch, fromIndex)
```

| Parameter | Description |
|---|---|
| `valueToSearch` | The value to locate |
| `fromIndex` | Optional. Index to start searching backwards from. Default: last index |

### Example
```js
let fruits = ['Apples1', 'Bananas', 'Oranges', 'Watermelon', 'Grapes', 'Guaves', 'Citafal'];

fruits.lastIndexOf('Grapes');    // → 4
fruits.lastIndexOf('Apples');    // → -1  (not found)

// With duplicates:
let arr = ['A', 'B', 'A', 'C', 'A'];
arr.lastIndexOf('A');            // → 4  (last occurrence)
arr.indexOf('A');                // → 0  (first occurrence)
```

> 💡 **`indexOf()` vs `lastIndexOf()`:** `indexOf()` finds the first match from the left. `lastIndexOf()` finds the last match from the right.

### 🔷 LWC Use Case
**Detecting duplicate entries in a list** — checking if a newly added value already exists and finding its last known position.

```js
addItem(newValue) {
    const lastPos = this.selectedItems.lastIndexOf(newValue);
    if (lastPos !== -1) {
        this.showToast('Warning', `"${newValue}" already exists at position ${lastPos + 1}`, 'warning');
        return;
    }
    this.selectedItems = [...this.selectedItems, newValue];
}
```

---

## ⚡ Mutation Cheat Sheet

> In LWC, **mutating methods** (marked ✅) require array reassignment to trigger re-render:
> ```js
> this.myArray = [...this.myArray]; // after any mutation
> ```

| Method | Adds To | Removes From | Returns |
|---|---|---|---|
| `push()` | End | — | New length |
| `pop()` | — | End | Removed element |
| `unshift()` | Beginning | — | New length |
| `shift()` | — | Beginning | Removed element |
| `splice()` | Anywhere | Anywhere | Array of removed items |
| `slice()` | *(no mutation)* | *(no mutation)* | New sub-array |
| `toString()` | *(no mutation)* | *(no mutation)* | String |
| `values()` | *(no mutation)* | *(no mutation)* | Array Iterator |
| `lastIndexOf()` | *(no mutation)* | *(no mutation)* | Index or `-1` |

---

## 🔷 LWC Reactivity Golden Rule

```js
// ❌ WRONG — template will NOT re-render
this.items.push(newItem);

// ✅ CORRECT — creates new reference, triggers re-render
this.items = [...this.items, newItem];

// ✅ ALSO CORRECT — after splice/pop/shift
this.items.splice(2, 1);
this.items = [...this.items];
```

---

*Part 3 will cover: `SORT()`, `filter()`, `reduce()`, `find()`, `reduceRight()`,`sort()`*