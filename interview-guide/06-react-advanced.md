# ⚛️ Advanced React Interview Questions (Hindi + English)

This section covers deep React concepts often asked in senior-level interviews.
**Ye section deep React concepts ko cover karta hai jo aksar senior-level interviews mein puche jate hain.**

## 📚 Table of Contents
1. [Reconciliation & Diffing Algorithm](#1-reconciliation--diffing-algorithm)
2. [Fiber Architecture Fundamentals](#2-fiber-architecture-fundamentals)
3. [Higher-Order Components (HOC) vs Render Props vs Hooks](#3-higher-order-components-hoc-vs-render-props-vs-hooks)
4. [Performance Optimization Deep Dive](#4-performance-optimization-deep-dive)
5. [Context vs Redux vs Recoil](#5-context-vs-redux-vs-recoil)
6. [Testing in React (Jest & RTL)](#6-testing-in-react-jest--rtl)
7. [Server-Side Rendering (SSR) vs CSR vs SSG](#7-server-side-rendering-ssr-vs-csr-vs-ssg)
8. [React Server Components (RSC)](#8-react-server-components-rsc)

---

### 1. Reconciliation & Diffing Algorithm

**Q: Explaining the Diffing Algorithm and Keys.**

**Hindi:** Reconciliation React ka wo process hai jisse wo DOM ko update karta hai. Diffing algorithm purane aur naye Virtual DOM ko compare karta hai.

**English:** Reconciliation is the process of efficiently updating the DOM. The Diffing algorithm compares the new Virtual DOM with the old one.

**Answer:**
React follows O(n) algorithm:
1.  **Elements of Different Types:** React purane tree ko destroy kar deta hai aur naya banata hai (e.g., `<div>` to `<span>`).
2.  **DOM Elements of Same Type:** Sirf changed attributes ko update karta hai.
3.  **Keys:** Keys elements ko ek stable identity dete hain taaki React unhe track kar sake (List reordering mein zaroori hai).

**Why Keys Matter:**
Bina keys ke, agar list ke start main item add karien, to React saare elements ko dobara banayega. Keys ke sath, wo purane elements ko reuse karega.

---

### 2. Fiber Architecture Fundamentals

**Q: What is React Fiber?**

**Hindi:** Fiber React 16 ka naya engine hai. Iska main maqsad **Incremental Rendering** hai (rendering ko chunks mein todna).

**English:** Fiber is the new engine in React 16. Its main goal is **Incremental Rendering** (splitting rendering work into chunks).

**Answer:**
**Features:**
-   **Pause/Resume:** Kaam ko rok ke zaroori updates (user input) ko pehle karna.
-   **Prioritization:** Updates ko priority dena (Animation > Data fetch).
-   **Concurrency:** UI ke multiple versions pe ek sath kaam karna.

**Phases:**
1.  **Render Phase:** Async, interruptible. (Plan banta hai).
2.  **Commit Phase:** Sync, uninterruptible. (DOM update hota hai).

---

### 3. Higher-Order Components (HOC) vs Render Props vs Hooks

**Q: Compare HOC, Render Props, and Hooks.**

**Hindi:** Ye teeno logic reuse karne ke patterns hain. Hooks sabse modern aur preferred tarika hain.

**English:** These are three patterns for logic reuse. Hooks are the most modern and preferred way.

**Answer:**

**1. Higher-Order Components (HOC):**
-   **Definition:** Function jo component leti hai aur naya component deti hai.
-   **Cons:** Wrapper Hell (bohot saare nested components).
```jsx
function withAuth(Component) {
  return props => isAuthenticated ? <Component {...props} /> : <Login />;
}
```

**2. Render Props:**
-   **Definition:** Prop mein function pass karna jo decide kare kya render karna hai.
-   **Cons:** Callback Hell.
```jsx
<DataProvider render={data => <Component data={data} />} />
```

**3. Hooks (Preferred):**
-   **Definition:** Functions jo state aur lifecycle methods use karne deti hain.
-   **Pros:** Cleaner code, easy to test.
```jsx
const data = useData();
return <Component data={data} />;
```

---

### 4. Performance Optimization Deep Dive

**Q: When to use `React.memo`, `useMemo`, and `useCallback`?**

**Hindi:** Inka use tab karo jab performance issue ho. Har jagah use mat karo kyunki inki apni cost hoti hai.

**English:** Use these only when there is a performance issue. Don't use them everywhere as they have their own cost.

**Answer:**
-   **`React.memo(Component)`:** Component ko re-render hone se rokta hai agar **props** same hain.
-   **`useMemo(() => value, [deps])`:** Kisi **value** (calculation result) ko cache karta hai. (Heavy calculation).
-   **`useCallback(() => fn, [deps])`:** **Function instance** ko cache karta hai. (Taaki child component unnecessary re-render na ho).

---

### 5. Context vs Redux vs Recoil

**Q: When should I use Redux over Context API?**

**Hindi:** Context API simple global updates (Theme, User) ke liye hai. Redux complex, high-frequency updates ke liye hai.

**English:** Context API is for simple global updates (Theme, User). Redux is for complex, high-frequency updates.

**Answer:**
-   **Context API:**
    -   **Use case:** Low-frequency updates (Theme, Language).
    -   **Problem:** Context update hone pe saare consumers re-render hote hain.
-   **Redux (Toolkit):**
    -   **Use case:** Complex state, frequent updates.
    -   **Pros:** DevTools (Time travel), Middlewares, only connected components re-render.
-   **Recoil/Zustand:** Modern atomic state management.

---

### 6. Testing in React (Jest & RTL)

**Q: What is the philosophy of React Testing Library (RTL)?**

**Hindi:** "Software ko waise test karo jaise user use karta hai." Implementation details pe focus mat karo.

**English:** “The more your tests resemble the way your software is used, the more confidence they can give you.” Don't focus on implementation details.

**Example:**
Is check karne ki bajaye `component.state.count === 1`, ye check karo ki screen par "Count: 1" dikh raha hai ya nahi.

```jsx
import { render, screen, fireEvent } from '@testing-library/react';
import Counter from './Counter';

test('increments counter', () => {
  render(<Counter />);
  const button = screen.getByText('Increment'); // User button dhundta hai
  fireEvent.click(button); // User click karta hai
  expect(screen.getByText('Count: 1')).toBeInTheDocument(); // User result dekhta hai
});
```

---

### 7. Server-Side Rendering (SSR) vs CSR vs SSG

**Q: Explain SSR vs CSR vs SSG.**

**Hindi:** Ye rendering strategies decide karti hain ki HTML kahan generate hoga (Server pe ya Client pe).

**English:** These rendering strategies decide where the HTML is generated (Server or Client).

**Answer:**
1.  **CSR (Client-Side Rendering):** Browser empty HTML lata hai phir JS execute karke UI banata hai (SPA).
    -   *Slow initial load, Bad SEO.*
2.  **SSR (Server-Side Rendering):** Server har request pe HTML banake bhejta hai.
    -   *Fast load, Great SEO. Server pe load zyada.*
3.  **SSG (Static Site Generation):** Build time pe HTML banta hai.
    -   *Fastest. Data stale ho sakta hai.*

---

### 8. React Server Components (RSC)

**Q: What are React Server Components (RSC)?**

**Hindi:** RSC wo components hain jo **sirf server** par render hote hain. Inka code client pe nahi jata.

**English:** RSC are components that render **exclusively on the server**. Their code is never sent to the client.

**Answer:**
-   **Zero Bundle Size:** Server component ka code (aur bhaari libraries) browser download nahi karta.
-   **Direct Backend Access:** DB queries directly component mein likh sakte hain.
-   **Hybrid:** Server Components client components ko render kar sakte hain, lekin client components server components ko import nahi kar sakte.
