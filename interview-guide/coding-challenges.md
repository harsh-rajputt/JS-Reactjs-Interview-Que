# 💻 Frontend Coding Challenges (Hindi + English)

Practicing these tiny challenges will prepare you for live coding rounds.
**Ye chote challenges apko live coding rounds ke liye taiyar karenge.**

## 📚 Table of Contents
1. [Debounced Search (Performance)](#1-debounced-search-performance)
2. [Custom Hook: useFetch (Data Fetching)](#2-custom-hook-usefetch-data-fetching)
3. [Infinite Scroll (UX/Performance)](#3-infinite-scroll-uxperformance)
4. [Polyfill: Array.map (Core JS)](#4-polyfill-arraymap-core-js)
5. [Detect Outside Click (Hooks)](#5-detect-outside-click-hooks)

---

### 1. Debounced Search (Performance)

**Problem:** Create a search input that only triggers an API call 500ms after the user stops typing.

**Hindi:** Ek aisa search box banao jo tabhi API call kare jab user 500ms tak type karna rok de. Har letter pe call nahi honi chahiye.

**English:** Create a search input that only triggers an API call 500ms after the user stops typing.

**Solution:**
```javascript
import { useState, useEffect } from 'react';

// Custom Hook for Debouncing
function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    // 1. Timer set karo
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    // 2. Cleanup: Agar user ne pehle hi dobara type kiya, to purana timer cancel kar do
    return () => {
      clearTimeout(handler);
    };
  }, [value, delay]);

  return debouncedValue;
}

// Usage in Component
function SearchComponent() {
  const [searchTerm, setSearchTerm] = useState('');
  // 500ms tak wait karega
  const debouncedSearchTerm = useDebounce(searchTerm, 500);

  useEffect(() => {
    if (debouncedSearchTerm) {
      console.log('API Call with:', debouncedSearchTerm);
      // fetchApi(debouncedSearchTerm);
    }
  }, [debouncedSearchTerm]);

  return <input onChange={(e) => setSearchTerm(e.target.value)} />;
}
```

---

### 2. Custom Hook: useFetch (Data Fetching)

**Problem:** Create a reusable hook to fetch data with loading/error states.

**Hindi:** Ek aisa custom hook banao jo kisi bhi URL se data fetch kare aur loading/error state bhi return kare.

**English:** Create a reusable hook to fetch data that also returns loading/error states.

**Solution:**
```javascript
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    // Request cancel karne ke liye controller
    const abortController = new AbortController();
    setLoading(true);

    fetch(url, { signal: abortController.signal })
      .then((res) => {
        if (!res.ok) throw new Error('Network response was not ok');
        return res.json();
      })
      .then((data) => {
        setData(data);
        setLoading(false);
        setError(null);
      })
      .catch((err) => {
        if (err.name === 'AbortError') {
          console.log('Fetch aborted - Component unmounted or URL changed');
        } else {
          setError(err.message);
          setLoading(false);
        }
      });

    // Cleanup: Component hatne par request cancel ho jayegi
    return () => abortController.abort();
  }, [url]);

  return { data, loading, error };
}
```

---

### 3. Infinite Scroll (UX/Performance)

**Problem:** Load more items as the user scrolls to the bottom of the page.

**Hindi:** Jaise hi user page ke neeche pahuche, naye items load karo (Jaise Instagram feed).

**English:** As soon as the user reaches the bottom of page, load new items (Like Instagram feed).

**Solution (Intersection Observer):**
```javascript
import { useState, useEffect, useRef, useCallback } from 'react';

function InfiniteScrollList() {
  const [items, setItems] = useState([]);
  const [page, setPage] = useState(1);
  const [loading, setLoading] = useState(false);
  const observer = useRef();

  // Last element ka reference
  const lastElementRef = useCallback(node => {
    if (loading) return;
    if (observer.current) observer.current.disconnect();

    // Jab last element dikhne lagay (intersect kare)
    observer.current = new IntersectionObserver(entries => {
      if (entries[0].isIntersecting) {
        setPage(prev => prev + 1); // Next page load karo
      }
    });

    if (node) observer.current.observe(node);
  }, [loading]);

  useEffect(() => {
    setLoading(true);
    // Fake API call
    setTimeout(() => {
      setItems(prev => [...prev, ...Array.from({ length: 10 }, (_, i) => `Item ${prev.length + i + 1}`)]);
      setLoading(false);
    }, 1000);
  }, [page]);

  return (
    <ul>
      {items.map((item, index) => {
        if (items.length === index + 1) {
          // Last element pe ref laga diya
          return <li ref={lastElementRef} key={item}>{item}</li>;
        } else {
          return <li key={item}>{item}</li>;
        }
      })}
      {loading && <p>Loading...</p>}
    </ul>
  );
}
```

---

### 4. Polyfill: Array.map (Core JS)

**Problem:** Implement `Array.prototype.myMap`.

**Hindi:** Apna khud ka `map` function banao agar browser mein built-in na ho.

**English:** Implement your own version of `Array.map` function.

**Solution:**
```javascript
Array.prototype.myMap = function(callback) {
  const newArray = [];
  // 'this' wo array hai jisne map call kiya
  for (let i = 0; i < this.length; i++) {
    // Har item ke liye callback run karo aur result push karo
    newArray.push(callback(this[i], i, this));
  }
  return newArray;
};

// Test
const arr = [1, 2, 3];
const double = arr.myMap(num => num * 2); 
console.log(double); // [2, 4, 6]
```

---

### 5. Detect Outside Click (Hooks)

**Problem:** Close a modal/dropdown when clicking outside of it.

**Hindi:** Jab user modal ke bahar click kare to modal band ho jana chahiye.

**English:** Close the modal when the user clicks anywhere outside of it.

**Solution:**
```javascript
import { useState, useEffect, useRef } from 'react';

function useOnClickOutside(ref, handler) {
  useEffect(() => {
    const listener = (event) => {
      // Agar click ref (modal) ke andar hua hai, to kuch mat karo
      if (!ref.current || ref.current.contains(event.target)) {
        return;
      }
      // Agar bahar click hua hai, to handler chalao (close karo)
      handler(event);
    };

    document.addEventListener('mousedown', listener);
    document.addEventListener('touchstart', listener);

    return () => {
      document.removeEventListener('mousedown', listener);
      document.removeEventListener('touchstart', listener);
    };
  }, [ref, handler]);
}

// Usage
function Modal({ onClose }) {
  const ref = useRef();
  // Ref batata hai ki modal kaunsa element hai
  useOnClickOutside(ref, onClose);

  return (
    <div className="modal-overlay">
      <div ref={ref} className="modal-content">
        <h1>Title</h1>
        <p>Click outside to close me</p>
      </div>
    </div>
  );
}
```
