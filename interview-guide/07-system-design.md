# 🏗️ Frontend System Design (Hindi + English)

This section covers how to approach large-scale frontend system design interviews.
**Ye section large-scale frontend system design interviews ko approach karne ke liye hai.**

## 📚 Table of Contents
1. [The RADIO Framework](#1-the-radio-framework)
2. [Example: Design a News Feed (Facebook/Twitter)](#2-example-design-a-news-feed-facebook-twitter)
3. [Example: Design a Real-time Chat Application](#3-example-design-a-real-time-chat-application)
4. [Performance Considerations](#4-performance-considerations)
5. [Accessibility (A11y) & Internationalization (i18n)](#5-accessibility-a11y--internationalization-i18n)

---

### 1. The RADIO Framework

**Hindi:** Interview mein koi bhi design problem solve karne ke liye ye framework use karo.

**English:** Use this framework to tackle any design problem in an interview.

**R - Requirements:**
-   **Functional:** User kya kar sakta hai? (Post dekhna, like karna).
-   **Non-functional:** Performance (Kitna fast ho?), Offline support?

**A - Architecture / High Level Design:**
-   Client-Server ya P2P? (Kaise connect hoga).
-   **State Management:** Local ya Global (Redux)?
-   **Rendering Strategy:** CSR, SSR ya SSG?

**D - Data Model:**
-   **Entities:** `User`, `Post`, `Comment` (Kya data store hoga).
-   **API Contract:** `GET /feed`, `POST /message` (Backend se kaise baat hogi).

**I - Interface / Component Hierarchy:**
-   UI ko chote-chote components mein todo.
    -   `FeedContainer` -> `PostList` -> `PostItem`

**O - Optimizations & Operations:**
-   **Performance:** Code splitting, lazy loading.
-   **UX:** Loading skeletons, Error handling.

---

### 2. Example: Design a News Feed (Facebook/Twitter)

**1. Requirements:**
-   Users continuously scroll kar sakein (Infinite Scroll).
-   Naye posts real-time ya button click pe aayein.
-   Images/Videos support.

**2. Architecture:**
-   **Rendering:** CSR (Interactive) ya SSR (Initial load fast).
-   **Data Fetching:** GraphQL (sirf zaroori data mangwana) ya REST.
-   **Pagination:** Cursor-based (Next page ke liye token bhejna, kyunki naye posts aane se page number badal sakta hai).

**3. Data Model:**
```typescript
interface Post {
  id: string;
  author: User;
  content: string;
  media: Media[];
  likesCount: number;
  createdAt: string;
}
```

**4. Optimizations:**
-   **Virtualization:** `react-window` use karo taaki sirf screen pe dikhne wale posts hi DOM mein hon. (Performance ke liye zaroori).
-   **Optimistic UI:** Like count *turant* badha do, API call background mein chalne do.

---

### 3. Example: Design a Real-time Chat Application

**1. Requirements:**
-   1-on-1 aur Group chats.
-   Read receipts (Blue ticks).
-   Typing indicators ("User is typing...").
-   Offline support (Net jane par bhi messsage dikhe).

**2. Architecture:**
-   **Communication:** WebSockets (Socket.io) real-time data ke liye. HTTP/2 one-way updates ke liye.
-   **Storage:** IndexedDB (Browser database) messages save karne ke liye.

**3. State Management:**
-   Complex state hoti hai (active chat, unread counts). Redux ya Zustand use karo.
-   Data Normalization: Messages ko ID se map karke store karo, array mein nahi.

**4. Optimizations:**
-   **Message Batching:** Har ek message aane par list re-render mat karo, thoda wait karke ek sath update karo.
-   **Throttling:** Typing indicator har keystroke pe server mat bhejo (500ms ka gap rakho).

---

### 4. Performance Considerations

**Key Metrics (Core Web Vitals):**
1.  **LCP (Largest Contentful Paint):** Main content kab dikha (< 2.5s).
2.  **FID (First Input Delay):** Site ne click pe kab react kiya (< 100ms).
3.  **CLS (Cumulative Layout Shift):** Layout kitna hila (< 0.1).

**Techniques:**
-   **Code Splitting:** JS bundle ko chota karna (`React.lazy`).
-   **Tree Shaking:** Unused code ko hatana.
-   **Asset Optimization:** Images compress karna (WebP).
-   **Caching:** Browser cache, CDN use karna.

---

### 5. Accessibility (A11y) & Internationalization (i18n)

**Accessibility (Sabke liye usable banana):**
-   Semantic HTML (`<button>` use karo `<div>` nahi).
-   `aria-labels` icons ke liye.
-   Keyboard navigation (Tab se chalna chahiye).
-   Color contrast (Text readable hona chahiye).

**Internationalization (Languages support):**
-   `react-i18next` library use karo.
-   Hardcoded strings mat likho.
-   RTL support (Urdu/Arabic ke liye right-to-left layout).
