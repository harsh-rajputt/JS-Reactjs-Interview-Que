# 🚀 Advanced JavaScript Interview Questions (Hindi + English)

This section covers complex JavaScript concepts often asked in senior-level interviews.
**Ye section complex JavaScript concepts ko cover karta hai jo aksar senior-level interviews mein puche jate hain.**

## 📚 Table of Contents
1. [Execution Context & Call Stack](#1-execution-context--call-stack)
2. [Event Loop: Microtasks vs Macrotasks](#2-event-loop-microtasks-vs-macrotasks)
3. [Prototypal Inheritance Deep Dive](#3-prototypal-inheritance-deep-dive)
4. [Closures: Advanced Scenarios](#4-closures-advanced-scenarios)
5. [This Keyword: Hard Binding](#5-this-keyword-hard-binding)
6. [Memory Management & Garbage Collection](#6-memory-management--garbage-collection)
7. [Iterators & Generators](#7-iterators--generators)
8. [Web Workers & Multithreading](#8-web-workers--multithreading)

---

### 1. Execution Context & Call Stack

**Q: What is the Execution Context regarding the Call Stack?**

**Hindi:** Execution Context wo environment hai jahan JavaScript code evaluate aur execute hota hai. Call Stack isko manage karta hai (LIFO order mein).

**English:** Execution Context is the environment where JavaScript code is evaluated and executed. The Call Stack manages these contexts.

**Answer:**
There are three types:
1.  **Global Execution Context (GEC):** Default context. Browser mein ye `window` object create karta hai aur `this` ko `window` se bind karta hai.
2.  **Functional Execution Context (FEC):** Jab bhi koi function invoke (call) hota hai, uska apna execution context banta hai.
3.  **Eval:** Code inside `eval()` function.

**Creation Phase:**
-   **Variable Object (VO):** Variables aur functions ko memory milti hai (Hoisting).
-   **Scope Chain:** Parent scopes ka reference create hota hai.
-   **This Binding:** `this` ki value decide hoti hai.

**Execution Phase:**
-   Code line-by-line execute hota hai aur variables ko values assign hoti hain.

---

### 2. Event Loop: Microtasks vs Macrotasks

**Q: Explain the difference between Microtasks and Macrotasks.**

**Hindi:** Event Loop do tarah ki queues ko handle karta hai. Microtasks ki priority Macrotasks se high hoti hai.

**English:** The Event Loop processes tasks from two queues. Microtasks have higher priority than Macrotasks.

**Answer:**

**1. Macrotasks (Task Queue):**
-   Examples: `setTimeout`, `setInterval`, `setImmediate`, I/O.
-   **Priority:** Lower. Har loop iteration mein ek task process hota hai.

**2. Microtasks (Microtask Queue):**
-   Examples: `Promise.then`, `queueMicrotask`, `MutationObserver`.
-   **Priority:** Higher. **Saare** microtasks current script ke turant baad aur next macrotask se pehle execute hote hain.

**Example:**
```javascript
console.log('Start'); // 1. Synchronous

setTimeout(() => {
  console.log('Timeout'); // 5. Macrotask
}, 0);

Promise.resolve().then(() => {
  console.log('Promise'); // 3. Microtask
});

queueMicrotask(() => {
  console.log('Microtask'); // 4. Microtask (after Promise)
});

console.log('End'); // 2. Synchronous
```

**Output:**
```
Start
End
Promise
Microtask
Timeout
```

---

### 3. Prototypal Inheritance Deep Dive

**Q: How does the Prototype Chain work exactly?**

**Hindi:** Har object ka ek link hota hai doosre object se jise `__proto__` kehte hain. Jab hum koi property access karte hain jo object pe nahi hai, to JS use prototype chain mein dhundta hai.

**English:** Every object in JavaScript has a internal link to another object called its prototype. When accessing a property not found on the object, JS looks up the prototype chain.

**Answer:**
1.  JS pehle object khud check karta hai.
2.  Agar nahi mila, to `__proto__` check karta hai.
3.  Ye tab tak chalta hai jab tak `null` nahi aa jata (Chain end).

**Code:**
```javascript
const animal = {
  eats: true,
  walk() { console.log("Animal walks"); }
};

const rabbit = {
  jumps: true,
  __proto__: animal // Inherits from animal
};

// Prototype Methods
rabbit.walk(); // "Animal walks" (prototype se aaya)
console.log(rabbit.eats); // true

// Overriding
rabbit.walk = function() { console.log("Rabbit hops"); };
rabbit.walk(); // "Rabbit hops" (Own property execute hoga)
```

---

### 4. Closures: Advanced Scenarios

**Q: What is the "Stale Closure" problem in React/hooks?**

**Hindi:** Stale closure tab hota hai jab ek function purane variables (state) ko yaad rakhta hai kyunki wo recreate nahi hua.

**English:** A stale closure occurs when a closure captures an outdated version of a variable because dependencies weren't updated.

**Example (React):**
```javascript
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      console.log(count); // Humesha 0 print karega (Old closure)
    }, 1000);
    return () => clearInterval(id);
  }, []); // Empty dependency array -> Stale Closure
  
  // Fix: Add [count] to dependencies OR use setCount(c => c + 1)
}
```

---

### 5. This Keyword: Hard Binding

**Q: What is the difference between explicit binding (call/apply) and hard binding (bind)?**

**Hindi:** `call/apply` function ko turant run karte hain `this` ke sath. `bind` naya function return karta hai jiska `this` fixed (locked) hota hai.

**English:** `call/apply` invoke the function immediately. `bind` returns a new function with `this` permanently locked.

**Answer:**
-   **Explicit Binding:** `call` aur `apply` function ko manually `this` dete hain.
-   **Hard Binding:** `bind` se jo function banta hai, uska `this` baad mein kabhi change nahi ho sakta.

```javascript
function foo() {
  console.log(this.a);
}

const obj = { a: 2 };
const bar = foo.bind(obj); // Hard binding

bar(); // 2
bar.call({ a: 3 }); // 2 // 'this' ignore ho jayega!
```

---

### 6. Memory Management & Garbage Collection

**Q: How does JavaScript handle Garbage Collection (GC)?**

**Hindi:** JavaScript "Mark-and-Sweep" algorithm use karta hai unused memory ko free karne ke liye.

**English:** JavaScript uses the "Mark-and-Sweep" algorithm to reclaim memory.

**Answer:**
1.  **Marking:** GC "roots" (global object) se start karta hai aur reachable objects ko mark karta hai.
2.  **Sweeping:** Jo objects mark nahi hue (unreachable), unhe memory se hata diya jata hai.

**Common Context:**
-   **Global Variables:** Agar galti se global variable bana diya.
-   **Event Listeners:** Component unmount hone par remove na karna.
-   **Timers:** `setInterval` ko clear na karna.

---

### 7. Iterators & Generators

**Q: Why use Generators (`function*`)?**

**Hindi:** Generators functions ko pause (`yield`) aur resume karne ki taqat dete hain.

**English:** Generators allow functions to pause (`yield`) and resume execution.

**Answer:**
Useful for:
-   **Lazy Evaluation:** Values tabhi generate hoti hain jab chahiye (Infinite streams).
-   **Async Flow:** `async/await` ka basic concept yahi hai.

```javascript
function* idMaker() {
  let index = 0;
  while (true) {
    yield index++; // Yahan ruk jayega
  }
}

const gen = idMaker();
console.log(gen.next().value); // 0
console.log(gen.next().value); // 1
```

---

### 8. Web Workers & Multithreading

**Q: How can we achieve multithreading in JavaScript?**

**Hindi:** JavaScript single-threaded hai, lekin **Web Workers** use karke hum background thread mein heavy code run kar sakte hain.

**English:** JavaScript is single-threaded, but **Web Workers** allow running scripts in background threads.

**Answer:**
-   Main thread UI block nahi karta.
-   **Communication:** `postMessage` aur `onmessage` se data pass hota hai.

```javascript
// main.js
const worker = new Worker('worker.js');
worker.postMessage('Hello'); // Worker ko data bheja

// worker.js
onmessage = (e) => {
  postMessage('Worker received: ' + e.data); // Main thread ko wapas bheja
};
```
