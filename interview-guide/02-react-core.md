# ⚛️ React Core (Basic to Intermediate)

This guide covers the **foundations** of React. Master these concepts before moving to advanced patterns.

## 📚 Table of Contents
1.  [Components (Function vs Class)](#1-components-function-vs-class)
2.  [JSX & Virtual DOM](#2-jsx--virtual-dom)
3.  [Props & State (`useState`)](#3-props--state)
4.  [Effects & Lifecycle (`useEffect`)](#4-effects--lifecycle)
5.  [Hooks Essentials](#5-hooks-essentials)
6.  [Props Drilling & Lifting State Up](#6-props-drilling--lifting-state-up)
7.  [Forms (Controlled vs Uncontrolled)](#7-forms)
8.  [Lists & Keys (Crucial)](#8-lists--keys-crucial)
9.  [Context API & Custom Hooks](#9-context-api--custom-hooks)
10. [Fragments & Portals](#10-fragments--portals)
9.  [Context API & Custom Hooks](#9-context-api--custom-hooks)
10. [Fragments & Portals](#10-fragments--portals)

---

### 1. Components (Function vs Class)

**Q1: What is a Component?**

**Hindi:** Component React app ka ek chota hissa hai (jaise Button, Header). Ye reusable hota hai.

**English:** Components are independent, reusable pieces of UI. They accept inputs (props) and return React elements.

**Q2: Functional vs Class Component?**

**Hindi:**
- **Functional:** Functions hote hain. Hooks use karte hain (`useState`, `useEffect`). (Current Standard).
- **Class:** Classes hoti hain. `this.state` aur `lifecycle methods` use karte hain. (Legacy).

**English:** Functional components are simpler and use Hooks. Class components are older and maintain state via `this`.

---

### 2. JSX & Virtual DOM

**Q3: What is JSX?**

**Hindi:** JSX JavaScript mein HTML jaisa code likhne deta hai. Ye browser mein `React.createElement` mein badal jata hai.

**English:** Syntax extension for JavaScript. Looks like HTML but compiles into JS objects (`React.createElement`).

```javascript
const element = <h1>Hello</h1>;
// Compiles to:
React.createElement('h1', null, 'Hello');
```

**Q4: Why use Virtual DOM?**

**Hindi:** Real DOM slow hota hai. Virtual DOM mein changes dekhna fast hai. Phir React sirf zaroori parts ko update karta hai (Reconciliation).

**English:** It's a lightweight copy of the Real DOM. React updates Virtual DOM first, compares with previous version, and only updates changed parts in Real DOM (Efficient).

---

### 3. Props & State

**Q5: Diff between Props and State?**

**Hindi:**
- **Props:** Data jo parent se milta hai. Read-only (Immutable).
- **State:** Component ka apna data. Mutable (Change ho sakta hai).

**English:**
- **Props:** Passed from parent. Immutable.
- **State:** Managed within component. Mutable.

```javascript
function Button(props) {
  // props.label aa raha hai parent se
  const [clicked, setClicked] = useState(false); // state yahan hai
  return <button>{props.label}</button>;
}
```

**Q6: Why we cannot change `state` directly?**

**Answer:** Direct mutation (`this.state.count = 2`) React ko pata nahi chalne deta. Humesha `setState` ya `useState` setter use karo taaki re-render trigger ho.

---

### 4. Effects & Lifecycle

**Q7: `useEffect` Dependency Array?**

**Hindi:** `useEffect` kab chalega ye dependency array batata hai.
- `[]`: Sirf ek baar (Mount).
- `[prop]`: Jab wo prop change ho.
- No array: Har render par.

**English:** Controls when the effect runs.
- `[]`: Once on mount.
- `[prop]`: Whenever `prop` changes.
- Omitted: Every render.

**Q8: Lifecycle Methods vs Hooks equivalent?**

| Lifecycle | Hook Equivalent |
| :--- | :--- |
| `componentDidMount` | `useEffect(() => {}, [])` |
| `componentDidUpdate` | `useEffect(() => {}, [dep])` |
| `componentWillUnmount` | `useEffect(() => { return cleanup }, [])` |

---

### 5. Hooks Essentials

**Q9: Rules of Hooks?**

**Answer:**
1.  **Top Level Only:** Loops, conditions, ya nested functions mein call mat karo.
2.  **React Functions Only:** Sirf Components ya Custom Hooks mein use karo.

**Q10: `useRef` use cases?**

**Hindi:** DOM element ko access karne ke liye (focus input) ya mutable value store karne ke liye jo re-render na karwaye.

**English:** Accessing DOM nodes directly or storing mutable values that persist across renders without causing re-renders.

---

### 6. Props Drilling & Lifting State Up

**Q11: What is Prop Drilling?**

**Hindi:** Jab data ko parent se bohot neeche wale child tak bhejna ho, to beech wale components ko bhi prop pass karna padta hai. Isse Prop Drilling kehte hain.

**English:** Passing props through multiple levels of components that don’t need the data themselves.

**Solution:** Context API or Redux.

**Q12: What is Lifting State Up?**

**Hindi:** Jab do siblings ko same state chahiye, to unke common parent mein state rakhte hain aur props se pass karte hain.

**English:** Sharing state between components by moving it to their closest common ancestor.

---

### 7. Forms

**Q13: Controlled vs Uncontrolled Components?**

**Hindi:**
- **Controlled:** Input ki value React State se control hoti hai (`value={state}`).
- **Uncontrolled:** DOM khud value sambhalta hai (`ref` use karte hain).

**English:**
- **Controlled:** Form data is handled by React component state.
- **Uncontrolled:** Form data is handled by the DOM itself.

```javascript
// Controlled
<input value={name} onChange={e => setName(e.target.value)} />

// Uncontrolled
<input ref={inputRef} />
```

---

### 8. Lists & Keys (Crucial)

**Q14: Why do we need `key` in Lists?**

**Hindi:** `key` React ko batata hai ki kaunsa item change, add, ya remove hua hai.
- **Rules:** Unique hona chahiye (siblings main). `index` use karna avoid karo agar list reorder ho sakti hai.

**English:** Keys give elements a stable identity, helping React identify which items have changed, are added, or are removed.
- **Rules:** Must be unique among siblings. Avoid using `index` if list order can change.

```javascript
// Good
items.map(item => <li key={item.id}>{item.name}</li>)

// Bad (if list reorders)
items.map((item, index) => <li key={index}>{item.name}</li>)
```

**Q15: What is Conditional Rendering?**

**Hindi:** Conditions ke base par UI dikhana.
1. `if-else` (Component return se pehle).
2. Ternary `? :` (JSX ke andar).
3. Logical `&&` (Sirf true hone par dikhana).

**English:** Rendering components based on conditions.
1. `if-else` (Before return).
2. Ternary `? :` (Inline).
3. Logical `&&` (Short-circuit).

```javascript
return (
  <div>
    {isLoggedIn ? <User /> : <Login />}
    {isAdmin && <AdminPanel />}
  </div>
);
```

---

### 9. Context API & Custom Hooks

**Q16: What is Context API?**

**Hindi:** Prop drilling se bachne ka tarika. Ye state ko globally (ya specific tree section mein) share karne deta hai without passing props manually.

**English:** A way to share values like these between components without having to explicitly pass a prop through every level of the tree.
- `createContext`: Creates a context.
- `Provider`: Provides the value.
- `useContext`: Consumes the value.

**Q17: What are Custom Hooks?**

**Hindi:** Apna khud ka hook banana taaki logic ko reuse kar sakein (e.g., fetching data, toggle boolean). "use" se start hona chahiye.

**English:** A JavaScript function that starts with "use" and can call other Hooks. Allows logic reuse across components.

```javascript
// useToggle.js
function useToggle(initial = false) {
  const [state, setState] = useState(initial);
  const toggle = () => setState(!state);
  return [state, toggle];
}
```

---

### 10. Fragments & Portals

**Q18: What are Fragments (`<>...</>`)?**

**Hindi:** React mein multiple elements ko bina extra DOM node (jaise `div`) ke return karne ka tarika.

**English:** A common pattern to return multiple elements. Fragments let you group a list of children without adding extra nodes to the DOM.

**Q19: What are Portals?**

**Hindi:** Child component ko apne parent DOM hierarchy se bahar render karne ka tarika (e.g., Modals, Tooltips).

**English:** Way to render children into a DOM node that exists outside the DOM hierarchy of the parent component.
