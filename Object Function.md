# JavaScript Objects
> **Level:** Beginner–Intermediate | **Topic:** Objects, References, Map, seal/freeze  
> **Source:** Live Console Practice Session

---

## 🧱 Creating Objects

```js
let obj = {};                        // empty object literal
obj.name = 'Vikas';                  // dot notation
obj.age = 26;
obj["Profile Name"] = "Admin";       // bracket notation (for keys with spaces)

obj;  // {name: 'Vikas', age: 26, Profile Name: 'Admin'}
```

> ✅ Use **dot notation** for simple keys, **bracket notation** for keys with spaces or special characters.

---

## 🔍 typeof Operator

```js
typeof obj    // 'object'  ✅ — keyword, no parentheses needed
typeof(obj)   // 'object'  ✅ — also works (parens are optional)
```

> ⚠️ **Common Mistake:** `typeOf` or `typeOf(obj)` throws:
> ```
> ReferenceError: typeOf is not defined
> ```
> **Fix:** `typeof` — all lowercase, it's a built-in **operator**, not a function.

---

## 🔗 Reference vs Copy

### Reference (shallow assignment)

```js
let user = {isAdmin: true, Name: 'Vikas', 'Company Name': 'Deloitte'};
let admin = user;           // admin POINTS to the same object

admin.Email = 'v@gmail.com';
user;  // also has Email now! → same object in memory
```

> Both `admin` and `user` point to the **same object**. Changing one changes the other.

### Shallow Copy (spread operator)

```js
let admin1 = {...user};     // creates a NEW object with top-level properties copied

admin1.break = 'Yes';
user;  // NOT affected — admin1 is a separate object
```

> ⚠️ **Spread is shallow** — nested objects are still referenced, not deep-copied.

```js
let user = {isAdmin: true, Name: 'Vikas', adminInfo: {country: 'IN', access: ['IP','Email']}};
let adminUser = {...user};

// adminUser.adminInfo still points to the SAME nested object as user.adminInfo
```

> For deep copy, use `structuredClone(user)` or `JSON.parse(JSON.stringify(user))`.

---

## 🗺️ Map & Object.fromEntries()

```js
let myMap = new Map();
myMap.set('Email', 'info@employee.com');
myMap.set('FirstName', 'Vikaskumar');
// Map(2) {'Email' => 'info@employee.com', 'FirstName' => 'Vikaskumar'}

let map = Object.fromEntries(myMap);
map;  // {Email: 'info@employee.com', FirstName: 'Vikaskumar'}
```

> **Map → Object** conversion using `Object.fromEntries()`.

---

## 🔑 Object.keys() / values() / entries()

```js
Object.keys(map);     // ['Email', 'FirstName']
Object.values(map);   // ['info@employee.com', 'Vikaskumar']
Object.entries(map);  // [['Email', 'info@employee.com'], ['FirstName', 'Vikaskumar']]
```

> `entries()` returns an array of `[key, value]` pairs — useful for iteration.

---

## 🔍 hasOwnProperty()

```js
adminUser.hasOwnProperty('Name');   // true
adminUser.hasOwnProperty('email');  // false
```

> Checks if the property belongs **directly** to the object (not inherited from prototype).

---

## 🔒 Object.seal()

**Prevents adding or deleting properties. Existing values CAN still be updated.**

```js
Object.seal(adminUser);

adminUser.location = 'Kalyan';  // silently ignored (no new properties)
adminUser.Name = 'Rajesh';      // ✅ works — modification allowed
delete adminUser.Name;          // false — deletion blocked

adminUser;  // {isAdmin: true, Name: 'Rajesh', ...}
```

| Operation | After seal() |
|---|---|
| Add new property | ❌ Blocked |
| Update existing | ✅ Allowed |
| Delete property | ❌ Blocked |

---

## 🧊 Object.freeze()

**Prevents adding, deleting, AND updating properties. Object becomes fully immutable.**

```js
Object.freeze(adminUser);

adminUser.Name = 'Vikas';   // silently ignored
delete adminUser.Name;      // false
adminUser;  // {isAdmin: true, Name: 'Rajesh', ...} — unchanged
```

| Operation | After freeze() |
|---|---|
| Add new property | ❌ Blocked |
| Update existing | ❌ Blocked |
| Delete property | ❌ Blocked |

> ⚠️ **freeze() is also shallow** — nested objects like `adminUser.adminInfo` can still be mutated.

---

## 🔐 const Object — Reference is Locked, Contents Are Not

```js
const adminUser = {};
adminUser.Name = "Vikas";   // ✅ adding/updating properties is allowed
adminUser;                  // {Name: 'Vikas'}

adminUser = {};             // ❌ TypeError: Assignment to constant variable
```

> `const` locks the **reference** (the box), NOT the contents (what's inside the box).
> You can freely add/update/delete properties — but you cannot point `adminUser` to a new object.

---

## 🧬 Nested Object — Reference in Action

```js
let userInfor = {
  name: '',
  email: '',
  adminInfo: {
    isAdmin: false,
    access: ["IP", "sysAdmin"]
  }
}

let adminDetails = userInfor;          // same reference

adminDetails.name = 'Vikas';
adminDetails.email = 'v@gmail.com';
adminDetails.adminInfo.isAdmin = true;

userInfor;
// {name: 'Vikas', email: 'v@gmail.com', adminInfo: {isAdmin: true, access: [...]}}
// userInfor is ALSO updated — same object in memory!
```

> `adminDetails = userInfor` does NOT create a copy. Both variables point to the **exact same object** — changing one changes the other, including nested properties.

---

## 🏗️ Object.create()

Creates a new empty object. The argument passed becomes its **prototype** — NOT its own properties.

```js
let crObj = Object.create({name: 'Vikas'});
crObj;  // {}  ← looks empty!
```

> ⚠️ `{name: 'Vikas'}` is NOT copied into `crObj` as own properties.
> It is set as `crObj`'s **prototype** — accessible via `crObj.__proto__`, not directly on `crObj`.

```js
crObj.name;           // 'Vikas'  ✅ — inherited from prototype
crObj.hasOwnProperty('name');  // false — not an own property!
```

> Use `Object.create()` when you want **prototype-based inheritance**, not simple property copying.
> To create an object WITH own properties, use object literals `{}` or `Object.assign()`.

---

## ❌ Common Errors Explained

### 1. `SyntaxError: missing ) after argument list`
```js
console.log(typeOfobj    // ❌ missing closing )
```
**Fix:** `console.log(typeof obj)`

### 2. `ReferenceError: typeOf is not defined`
```js
typeOf(obj)   // ❌ wrong casing
```
**Fix:** `typeof obj` — lowercase, no parentheses needed.

### 3. `ReferenceError: Admin is not defined`
```js
obj["Profile Name"] = Admin   // ❌ Admin treated as a variable
```
**Fix:** `obj["Profile Name"] = "Admin"` — wrap string values in quotes.

### 4. `TypeError: undefined is not iterable`
```js
let map = Object.from;                 // ❌ incomplete — assigns the function reference
let map = Object.fromEntries(map);     // ❌ 'map' is undefined here
```
**Fix:** Pass the iterable (Map or array of entries) directly:
```js
let map = Object.fromEntries(myMap);  // ✅
```

### 5. `TypeError: Object.Keys is not a function`
```js
Object.Keys(map)   // ❌ capital K
```
**Fix:** `Object.keys(map)` — all lowercase.

### 6. `SyntaxError: Unexpected token ':'`
```js
adminUser['Company Name': "Deloitte"]   // ❌ object literal syntax inside brackets
```
**Fix:** Assignment syntax:
```js
adminUser['Company Name'] = 'Deloitte';  // ✅
```

### 7. `ReferenceError: adminUesr is not defined`
```js
Object.freeze(adminUesr)    // ❌ typo: 'adminUesr' vs 'adminUser'
adminUesr.Name = 'Vikas'    // ❌ same typo
```
**Fix:** Check variable name spelling — `adminUser`.

### 8. `TypeError: adminUser.Name is not a function`
```js
adminUser.Name("Vikas")   // ❌ calling Name as if it were a function
```
**Cause:** `adminUser.Name` is a property, not a method. Using `()` tries to call it as a function.  
**Fix:** Use assignment syntax:
```js
adminUser.Name = "Vikas";  // ✅
```

### 9. `ReferenceError: adminu / admi is not defined`
```js
adminu   // ❌
admi     // ❌
```
**Cause:** Incomplete variable name typed — JS looks for `adminu` as a declared variable and can't find it.  
**Fix:** Type the full correct variable name: `adminUser`.

### 10. `TypeError: Object.craete is not a function`
```js
let crObj = Object.craete({name: 'Vikas'})   // ❌ typo: 'craete'
```
**Fix:** `Object.create(...)` — correct spelling.

---

## 📊 Quick Reference Table

| Feature | Description |
|---|---|
| `obj.key` | Dot notation access |
| `obj["key"]` | Bracket notation (for space/special keys) |
| `typeof obj` | Returns `'object'` for objects |
| `let b = a` | Reference — both point to same object |
| `let b = {...a}` | Shallow copy — top-level props cloned |
| `structuredClone(a)` | Deep copy — nested objects also cloned |
| `new Map()` | Ordered key-value pairs (any key type) |
| `Object.fromEntries(map)` | Converts Map → plain Object |
| `Object.keys(obj)` | Array of keys |
| `Object.values(obj)` | Array of values |
| `Object.entries(obj)` | Array of `[key, value]` pairs |
| `obj.hasOwnProperty(k)` | Check if key belongs to obj directly |
| `Object.seal(obj)` | Block add/delete; allow update |
| `Object.freeze(obj)` | Block add/delete/update (shallow) |
| `delete obj.key` | Returns `true` (deleted) or `false` (blocked) |
| `Object.create(proto)` | New empty object with `proto` as its prototype |

---

## 🧠 seal() vs freeze() — Summary

| | `seal()` | `freeze()` |
|---|---|---|
| Add property | ❌ | ❌ |
| Update property | ✅ | ❌ |
| Delete property | ❌ | ❌ |
| Nested objects protected? | ❌ Shallow | ❌ Shallow |

---

> 💡 **Key Takeaways**
> - `typeof` is an **operator**, always lowercase — not a function.
> - Objects assigned with `=` are **references**, not copies.
> - Spread `{...obj}` creates a **shallow copy** — nested objects are still shared.
> - `Object.seal()` allows updates; `Object.freeze()` blocks everything.
> - Both `seal` and `freeze` are **shallow** — they don't protect nested objects.
> - Use bracket notation `obj["key"]` for property names containing spaces.
> - `const obj = {}` — you can mutate contents, but cannot reassign `obj` to a new object.
> - `Object.create(x)` — `x` becomes the **prototype**, NOT own properties. `crObj` will appear as `{}`.
> - `obj.prop()` with `()` tries to call `prop` as a function — use `obj.prop = value` for assignment.
