# 🟡 JavaScript Core (Basic to Intermediate)

This guide covers the **absolute essentials** of JavaScript. Master these before moving to advanced topics.

## 📚 Table of Contents
1.  [Variables & Data Types (`var`, `let`, `const`)](#1-variables--data-types)
2.  [Operators & Coercion (`==` vs `===`)](#2-operators--coercion)
3.  [Functions & `this` Keyword](#3-functions--this-keyword)
4.  [Closures (Lexical Scope)](#4-closures-lexical-scope)
5.  [Objects & References (Value vs Reference)](#5-objects--references)
6.  [Arrays & Methods (`map`, `filter`, `reduce`)](#6-arrays--methods)
7.  [Asynchronous JS (Callbacks, Promises, Async/Await)](#7-asynchronous-js)
8.  [DOM Manipulation & Events](#8-dom-manipulation--events)
9.  [Storage (Local vs Session vs Cookies)](#9-storage)

---

### 1. Variables & Data Types

**Q1: Difference between `var`, `let`, and `const`?**

**Hindi:** `var` function scope hota hai (purana tarika). `let` aur `const` block scope hote hain (naya tarika). `const` ko change nahi kar sakte.

**English:** `var` is function-scoped (old). `let` and `const` are block-scoped (modern). `const` cannot be reassigned.

| Feature | var | let | const |
| :--- | :--- | :--- | :--- |
| **Scope** | Function | Block `{}` | Block `{}` |
| **Reassign** | ✅ Yes | ✅ Yes | ❌ No |
| **Hoisting** | `undefined` | Error (TDZ) | Error (TDZ) |

**Q2: What is "Hoisting"?**

**Hindi:** Hoisting ka matlab hai JS variables aur functions ko code run hone se pehle **top** par le jata hai.

**English:** Hoisting moves variable and function declarations to the top of their scope before execution.

```javascript
console.log(a); // undefined (var hoisted)
var a = 5;

console.log(b); // Error! (let not initialized)
let b = 10;
```

**Q3: `null` vs `undefined`?**

**Hindi:** `undefined` matlab variable bana hai par value nahi di. `null` matlab humne jaan-bujh ke "empty" value di hai.

**English:** `undefined` means variable is declared but not assigned. `null` is an assignment value representing "no value".

---

### 2. Operators & Coercion

**Q4: Difference between `==` and `===`?**

**Hindi:** `==` (Loose Equality) value check karta hai aur type conversion karta hai. `===` (Strict Equality) value aur type dono check karta hai (Type conversion nahi karta).

**English:**
- `==`: Checks value, performs type coercion (e.g., `5 == "5"` is `true`).
- `===`: Checks value and type (e.g., `5 === "5"` is `false`). **Always use this.**

**Q5: What are Falsy values?**
`false`, `0`, `""` (empty string), `null`, `undefined`, `NaN`.

---

### 3. Functions & `this` Keyword

**Q6: Difference between Regular and Arrow functions?**

**Hindi:** Arrow functions syntax mein chote hote hain aur unka apna `this` nahi hota.

**English:** Arrow functions have shorter syntax and do not have their own `this` context.

```javascript
const obj = {
  name: "User",
  regular: function() { console.log(this.name) }, // "User"
  arrow: () => { console.log(this.name) } // undefined (Global 'this')
};
```

**Q7: Explain `this` keyword basics.**

**Hindi:**
- **Global:** Window object refer karta hai.
- **Object Method:** Object khud refer karta hai.
- **Arrow Function:** Apne parent scope ka `this` leta hai (Lexical `this`).

**English:**
- **Global:** Refers to `window`.
- **Object Method:** Refers to the object calling the method.
- **Arrow Function:** Does not have its own `this`; inherits from the surrounding scope.

**Q8: What is an IIFE?**

**Hindi:** IIFE (Immediately Invoked Function Expression) wo function hai jo banne ke turant baad run ho jata hai.

**English:** IIFE is a function that runs as soon as it is defined.

```javascript
(function() {
  console.log("I run immediately!");
})();
```

---

### 4. Closures (Lexical Scope)

**Q9: What is a Closure?**

**Hindi:** Closure wo function hai jo apne parent function ke variables ko yaad rakhta hai, bhale hi parent function execute hoke khatam ho gaya ho.

**English:** A closure is a function that remembers its outer variables and can access them even after the outer function has finished executing.

```javascript
function outer() {
  let count = 0;
  return function inner() {
    count++;
    console.log(count);
  };
}

const counter = outer();
counter(); // 1
counter(); // 2
```

**Q10: What is Lexical Scope?**

**Hindi:** Lexical scope ka matlab hai ki inner function apne outer function ke variables ko access kar sakta hai (jahan wo likha gaya tha).

**English:** A function's ability to access variables from its parent scope based on where it is defined in the source code.

---

### 5. Objects & References

**Q11: Value Types vs Reference Types?**

**Hindi:**
- **Primitives (Value):** Number, String. Copy karne par value copy hoti hai.
- **Objects (Reference):** Array, Object. Copy karne par address (reference) copy hota hai.

**English:**
- **Primitives:** Stored by value. Independent copies.
- **Reference:** Stored by reference (memory address). Modifying one affects the other.

```javascript
// Reference Logic
let a = { name: "Raj" };
let b = a;
b.name = "Simran";
console.log(a.name); // "Simran" (Because 'b' points to same memory as 'a')
```

---

### 6. Arrays & Methods

**Q12: What is Destructuring?**

**Hindi:** Array ya Object se values ko nikal kar variables mein store karne ka aasan tarika.

**English:** A syntax to unpack values from arrays or properties from objects into distinct variables.

```javascript
// Array
const [x, y] = [10, 20];

// Object
const user = { name: "Raj", age: 25 };
const { name, age } = user; // name="Raj", age=25
```

**Q13: Usage of Spread Operator (`...`)?**

**Hindi:** Spread operator array ya object ko "khol" deta hai (copy ya merge karne ke liye).

**English:** Expands an array/object into individual elements. Used for copying or merging.

```javascript
const arr1 = [1, 2];
const arr2 = [...arr1, 3, 4]; // [1, 2, 3, 4]

const obj1 = { a: 1 };
const obj2 = { ...obj1, b: 2 }; // { a: 1, b: 2 }
```

---

### 6. Arrays & Methods

**Q14: Detailed explanation of `map`, `filter`, and `reduce`?**

**1. `map()` - Transform**
**Hindi:** Har element ko transform karke **naya array** return karta hai. Length same rehti hai.
**English:** Creates a new array by applying a function to every element. Same length as original.

```javascript
const nums = [1, 2, 3];
const doubled = nums.map(num => num * 2); 
// Output: [2, 4, 6]
```

**2. `filter()` - Select**
**Hindi:** Condition check karta hai using `true/false`. Jo elements condition pass karte hain unka **naya array** banata hai.
**English:** Creates a new array with all elements that pass the test implemented by the function.

```javascript
const nums = [1, 2, 3, 4];
const evens = nums.filter(num => num % 2 === 0);
// Output: [2, 4]
```

**3. `reduce()` - Accumulate (Jodna)**
**Hindi:** Saare elements ko process karke **single value** (number, object, ya array) return karta hai. Isme `accumulator` hota hai jo result store karta hai.
**English:** Executes a reducer function on each element, resulting in a **single output value**.

**Syntax:** `arr.reduce(callback(accumulator, currentValue), initialValue)`

```javascript
const nums = [1, 2, 3, 4];

const sum = nums.reduce((acc, curr) => {
  return acc + curr;
}, 0); // 0 is initial value

// Iteration 1: acc=0, curr=1 => returns 1
// Iteration 2: acc=1, curr=2 => returns 3
// Iteration 3: acc=3, curr=3 => returns 6
// Iteration 4: acc=6, curr=4 => returns 10
```

**Q15: `map` vs `forEach`?**
- **Return:** `map` returns new array, `forEach` returns `undefined`.
- **Chaining:** `map` can be chained (`.map().filter()`), `forEach` cannot.

**Q16: `slice` vs `splice`?**

**Hindi:** `slice` naya array return karta hai (copy). `splice` original array ko modify karta hai (cut/add).

**English:** `slice` returns a shallow copy of a portion of an array. `splice` changes the contents of an array by removing or replacing elements.

```javascript
// Slice (Non-destructive)
const arr = [1, 2, 3, 4];
const sliced = arr.slice(1, 3); // [2, 3]
console.log(arr); // [1, 2, 3, 4] (Unchanged)

// Splice (Destructive)
const arr2 = [1, 2, 3, 4];
const spliced = arr2.splice(1, 2); // [2, 3] (Removed)
console.log(arr2); // [1, 4] (Modified)
```

---

### 7. Asynchronous JS

**Q17: What is a Callback function?**

**Hindi:** Ek function jo dusre function ke complete hone ke baad chalta hai.

**English:** A function passed into another function as an argument, executed later.

**Q18: Promises (States: Pending, Fulfilled, Rejected)**

**Hindi:** Promise ek wada hai ki future mein value milegi (ya error aayega). Isse callback hell se bacha jata hai.

**English:** Represents the eventual completion (or failure) of an async operation. Helps avoid callback hell.

**Q19: Why use `async/await`?**

**Hindi:** `async/await` promises ko likhne ka saaf (clean) tarika hai. Code synchronous lagta hai.

**English:** Syntactic sugar over Promises. Makes async code look and behave like synchronous code.

```javascript
async function getData() {
  try {
    const res = await fetch(url);
    const data = await res.json();
    console.log(data);
  } catch (error) {
    console.log(error);
  }
}
```

---

### 8. DOM Manipulation & Events

**Q20: Event Bubbling vs Capturing?**

**Hindi:**
- **Bubbling:** Event neeche se upar jata hai (Child -> Parent). (Default)
- **Capturing:** Event upar se neeche aata hai (Parent -> Child).

**English:**
- **Bubbling:** Event propagates from target element up to the root.
- **Capturing:** Event propagates from root down to the target.

**Q21: How to stop event propagation?**

**Answer:** `event.stopPropagation()`

**Q22: What is Event Delegation?**

**Hindi:** Parent element par listener lagana, taaki wo apne sabhi children ke events handle kare (Bubbling ki wajah se). Memory bachta hai.

**English:** Attaching a single event listener to a parent element to manage events for all its children using event bubbling. Use `e.target` to match the child.

```javascript
document.querySelector('#parent').addEventListener('click', (e) => {
  if(e.target.tagName === 'BUTTON') {
    console.log('Button clicked:', e.target.textContent);
  }
});
```

---

### 9. Storage

**Q23: LocalStorage vs SessionStorage vs Cookies?**

**Hindi:**
- **LocalStorage:** Data kabhi delete nahi hota (jab tak manually na karein).
- **SessionStorage:** Tab band karne par data udd jata hai.
- **Cookies:** Server par data bhejne ke liye use hota hai (expiration time ke sath).

**English:**
- **LocalStorage:** Persists even after browser calls. 5-10MB.
- **SessionStorage:** Cleared when tab/session closes. 5MB.
- **Cookies:** Sent with HTTP requests. Small capacity (4KB).
