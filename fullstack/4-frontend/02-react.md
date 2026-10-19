# React

> **Time needed:** about 2 hours · **Questions:** 16 + 10 quick questions
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

## Progress checklist
- [ ] 1. Virtual DOM, reconciliation and keys
- [ ] 2. What causes a re-render?
- [ ] 3. useState, batching and immutability
- [ ] 4. useEffect: when to use it, and when not
- [ ] 5. useRef
- [ ] 6. useMemo, useCallback, React.memo, React Compiler
- [ ] 7. Controlled vs uncontrolled inputs
- [ ] 8. Choosing state management
- [ ] 9. Context and performance
- [ ] 10. Custom hooks
- [ ] 11. Error boundaries
- [ ] 12. Code splitting, lazy and Suspense
- [ ] 13. Optimizing React performance
- [ ] 14. What's new in React 19
- [ ] 15. Data fetching on the client
- [ ] 16. Testing React
- [ ] Quick questions
- [ ] Remember these 10 things

---

## 1. How does the virtual DOM work, and why do keys matter?

**In simple words**
When state changes, React runs your components again. It gets a new picture of the UI as plain JavaScript objects. This is often called the **virtual DOM**. React compares the new picture with the old one and changes only the real page parts that are different.

**🗣️ Say this in the interview**
> "When state changes, React calls my components again and gets a new description of the UI, a tree of plain objects. People call this the virtual DOM. React compares it with the previous tree. That comparing step is called **reconciliation**. Then it updates only the real DOM nodes that changed, because touching the real DOM is the slow part.
> **Keys** tell React which list item is which between renders. With a stable key like a product ID, React can reorder items correctly. If I use the array index as the key and the list is sorted or filtered, React can give the wrong state to the wrong item. For example, a checkbox stays ticked on the wrong row."

**⚠️ Avoid saying**
"The virtual DOM makes React fast." Say instead: it makes updates predictable and fast enough. Real speed comes from avoiding extra renders and heavy work.

---

## 2. What causes a component to re-render?

**In simple words**
A component renders again for four reasons. A re-render is not always a problem. It is a problem only when it is slow or happens very often.

**🗣️ Say this in the interview**
> "Four things: its **state** changes, its **parent re-renders**, a **context** it uses changes, or an external store it listens to changes.
> Props changing is not really the trigger. The parent re-rendering is. By default, when a parent renders, all its children render too, even if their props are the same.
> A re-render is not automatically bad. React only touches the DOM if the output changed. It's a problem when the render itself is expensive, or happens hundreds of times, like on every keystroke in a big form."

**⚠️ Avoid saying**
"A component re-renders when its props change." Say: when its parent re-renders, children render too, whatever the props.

---

## 3. How do state updates work? What about batching and immutability?

**In simple words**
`setState` does not change the value right away. React collects several updates and does one render. Also, you must never change (mutate) state objects directly. Always make a new one.

**🗣️ Say this in the interview**
> "State updates are **asynchronous and batched**. **Batching** means React groups several `setState` calls into one re-render. Since React 18 it does this everywhere, also inside timeouts and promises. So if the new value depends on the old value, I use the **functional form**: `setCount(c => c + 1)`.
> I treat state as **immutable**, meaning I never change it in place. I create a new object or array, because React compares by **reference** (is it the same object in memory?). If I mutate the object and set the same one back, React thinks nothing changed."

```javascript
// ❌ Mutating: same object, React may not re-render
cart.items.push(item); setCart(cart);
// ✅ New objects, so React sees the change
setCart(prev => ({ ...prev, items: [...prev.items, item] }));
```

**⚠️ Avoid saying**
"`setState` updates the value immediately." Say: it schedules an update, and the new value shows on the next render.

---

## 4. What is `useEffect` for, and when should you NOT use it?

**In simple words**
`useEffect` is for talking to things outside React: timers, WebSockets, browser APIs, analytics. Many people use it for things that don't need it. That causes extra renders and bugs.

**🗣️ Say this in the interview**
> "`useEffect` is for **syncing with something outside React**: subscriptions, timers, WebSockets, browser APIs, analytics, or a non-React widget. It runs **after** React has put the render on the screen. The **cleanup** function runs before the next effect and when the component is removed.
> What I don't use it for:
> - **Calculating values from props or state.** I just calculate them during render. An effect that sets state causes an extra render.
> - **Reacting to a user event.** That logic goes in the event handler.
> - **Resetting state when a prop changes.** I give the component a `key` instead.
> - **Fetching data**, in most cases. I use a library like TanStack Query, or Server Components in Next.js."

**Misuse vs the better way:**

```jsx
// ❌ Extra state + effect: renders twice, can get out of sync
const [fullName, setFullName] = useState('');
useEffect(() => {
  setFullName(`${firstName} ${lastName}`);
}, [firstName, lastName]);

// ✅ Just calculate it during render
const fullName = `${firstName} ${lastName}`;
```

**Fetching with a race condition fix:**

```javascript
useEffect(() => {
  const controller = new AbortController();
  fetch(`/api/search?q=${query}`, { signal: controller.signal })
    .then(r => r.json())
    .then(setResults)
    .catch(err => { if (err.name !== 'AbortError') setError(err); });
  // When query changes, cancel the old request
  return () => controller.abort();
}, [query]);
```

> "Without the cleanup, if the user types 'ta' then 'tap', the 'ta' answer could arrive **last** and overwrite the correct results. That's a **race condition**: two requests racing, and the wrong one wins."

**They may also ask**
- *Why does my effect run twice in development?* **Strict Mode** mounts, unmounts and mounts again in development only, to find missing cleanups. It doesn't happen in production. If running twice breaks something, the cleanup is wrong.
- *What goes in the dependency array?* Every reactive value the effect uses (props, state, things made from them). If that causes a loop, restructure the code (move the function inside the effect, or use an updater function). Don't hide it from the linter.
- *What is `useLayoutEffect`?* It runs **before** the browser paints the screen. Use it only to measure the DOM and avoid a visible flicker.

**⚠️ Avoid saying**
"I use `useEffect` to update state when props change." Say: I calculate it during render, or use a `key` to reset.

---

## 5. What is `useRef` for?

**In simple words**
`useRef` is a box that keeps a value between renders. Changing it does NOT cause a re-render.

**🗣️ Say this in the interview**
> "`useRef` gives me a box whose value **survives between renders but doesn't trigger a re-render** when it changes. I use it two ways. First, to hold a **DOM element**, for example to focus an input or measure its size. Second, to hold **changing values** that the UI doesn't show, like a timer ID, the previous value, or an AbortController."

**⚠️ Avoid saying**
"I use a ref instead of state to show values." If the screen must update, it should be state.

---

## 6. What do `useMemo`, `useCallback` and `React.memo` do? What about the React Compiler?

**In simple words**
These tools help React skip work. **Memoization** means remembering a result so you don't calculate it again. But they have a cost, so use them only when you measured a real problem.

**🗣️ Say this in the interview**
> "`useMemo` remembers a **calculated value**. `useCallback` remembers a **function**, so it's the same function between renders. `React.memo` lets a child component **skip re-rendering** when its props didn't change.
> They work as a team. `React.memo` on a child is useless if the parent passes a new function or object every render. So I wrap those props in `useCallback` or `useMemo`.
> But I add them only **after measuring** with the React Profiler. They cost memory and comparisons, and make code harder to read.
> In newer projects, the **React Compiler** does this memoization automatically at build time. So manual `useMemo` and `useCallback` are needed much less."

**⚠️ Avoid saying**
"I wrap everything in `useMemo` for performance." Say: I measure first, then memoize only where it costs.

---

## 7. Controlled vs uncontrolled components: what's the difference?

**In simple words**
A **controlled** input keeps its value in React state. An **uncontrolled** input keeps its value in the browser's DOM, and you read it when you need it.

**🗣️ Say this in the interview**
> "A **controlled** input has its value in React state. Every keystroke calls `setState`. That makes validation and dependent fields easy. An **uncontrolled** input keeps its value in the **DOM**, and I read it with a ref or from the form on submit. That means fewer re-renders, which is good for big forms.
> In practice I use **React Hook Form**, which is mostly uncontrolled for speed, together with **Zod** for validation. Often it's the same Zod schema as the backend."

**⚠️ Avoid saying**
"Controlled is always better." Say: controlled is easier for live validation, uncontrolled is faster for big forms.

---

## 8. How do you choose a state management approach?

**In simple words**
First ask: what kind of state is it? Different kinds need different tools. Most apps don't need Redux.

**🗣️ Say this in the interview**
> "I first split state into **types**, because each type has a best tool:
> - **Server state**, like products, orders, the user profile. This data lives on the server. I use **TanStack Query** or SWR, or Server Components. They handle caching, refetching, loading states and removing duplicate requests. I don't copy it into Redux.
> - **Local UI state**, like 'is the modal open' or an input value. That's **`useState`** in the component that needs it.
> - **Shared UI state**, like the theme, the logged-in user, the cart drawer. I use **Context** if it changes rarely, or a small store like **Zustand** if it changes often.
> - **URL state**, like filters, sort and page number. I **keep it in the URL**, so it's shareable and survives a refresh.
> - **Complex client workflows**: **Redux Toolkit**, if the team needs strict structure and devtools.
>
> Most apps that 'need Redux' really just need a server-state library."

**⚠️ Avoid saying**
"I put everything in Redux." Say: server data goes in a server-state library, not a global store.

---

## 9. How does Context affect performance?

**In simple words**
When a context value changes, every component that uses it renders again. If you build it carelessly, a lot of the page re-renders for no reason.

**🗣️ Say this in the interview**
> "When a context **value changes**, every component using that context re-renders. Two common mistakes. One: passing a **new object every render**, like `value={{ user, setUser }}`. Then all users of the context re-render every time the parent renders. Two: putting fast-changing data in one big context together with slow-changing data.
> Fixes: memoize the provider value, **split contexts** by topic, keep fast-changing state local, or use a store with **selectors** like Zustand. A selector means a component subscribes only to the small piece of data it uses."

**⚠️ Avoid saying**
"Context is a replacement for Redux for everything." Say: Context is good for rarely changing values; for fast-changing shared state, use a store with selectors.

---

## 10. What is a custom hook?

**In simple words**
A custom hook is a normal function whose name starts with `use`. It lets you reuse logic that uses other hooks. Each component that calls it gets its own separate state.

**🗣️ Say this in the interview**
> "A custom hook is just a function starting with `use` that **reuses stateful logic** between components. For example `useDebounce`, `useCart`, `useMediaQuery`. It shares **logic**, not **state**. Each component that calls it gets its own state. Hooks replaced higher-order components and render props for most use cases."

```javascript
// Returns the value only after the user stops typing for `delay` ms
function useDebouncedValue(value, delay = 300) {
  const [debounced, setDebounced] = useState(value);
  useEffect(() => {
    const t = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(t); // new keystroke cancels the old timer
  }, [value, delay]);
  return debounced;
}

// Usage: search only after typing stops
const debouncedQuery = useDebouncedValue(query, 300);
```

**⚠️ Avoid saying**
"Two components using the same custom hook share the same data." They don't. Each call has its own state.

---

## 11. What are error boundaries?

**In simple words**
An error boundary catches errors while its child components render. It shows a fallback message instead of a blank white page.

**Picture it**
A fuse box in a house. One room loses power, but the rest of the house still has lights.

**🗣️ Say this in the interview**
> "An error boundary catches errors **during rendering** in its child tree, and shows a fallback UI instead of a blank white screen. I put them around **independent sections**, like the reviews widget or recommendations. Then one broken section doesn't take down the whole product page. I also log the error to monitoring.
> They do **not** catch errors in **event handlers**, async code, or on the server. Those need normal try/catch."

**⚠️ Avoid saying**
"An error boundary catches all errors." It doesn't catch event handler or async errors.

---

## 12. What is code splitting? How do `lazy` and Suspense help?

**In simple words**
Code splitting means you don't send all the JavaScript at once. Heavy parts load only when they are needed.

**🗣️ Say this in the interview**
> "Code splitting means not shipping all the JavaScript up front. `React.lazy(() => import('./HeavyChart'))` loads a component only when it's rendered. `<Suspense fallback={...}>` shows a placeholder while it loads. I split at **routes**, and around **heavy, rarely used components**: rich text editors, charts, maps, modals.
> Suspense is also how **streaming** works in Next.js. Slow parts of the page arrive later, while the rest shows immediately."

**⚠️ Avoid saying**
"I lazy-load every component." Split where it helps: routes and heavy widgets.

---

## 13. How do you optimize React performance?

**In simple words**
Measure first. Then fix the big things before the small things. Sending less JavaScript and less data usually helps more than memoization.

**🗣️ Say this in the interview**
> "I **measure first**: the React Profiler to see what renders and why, plus Web Vitals and Lighthouse for what the user feels. Then, in order of how often they really help:
> 1. **Ship less JavaScript**: Server Components, code splitting, removing heavy libraries.
> 2. **Move state down**, closer to where it's used, so fewer components re-render.
> 3. **Virtualize long lists** with TanStack Virtual. That means rendering only the rows you can see.
> 4. **Fix data fetching**: no waterfalls (requests waiting on each other for no reason), and cached server data.
> 5. **Optimize images and fonts.**
> 6. **Memoize**, only where the profiler shows a real cost.
> 7. **Debounce** expensive input handlers, and use `useTransition` or `useDeferredValue` for updates that are not urgent."

**From my work**
"My biggest React/Next.js performance win wasn't memoization. We were sending 4 MB of props to a client component. **What you send** often matters more than **how you render**." Use Story S1 — see `1-interview-basics/02-your-story.md`.

**⚠️ Avoid saying**
"I use `useMemo` and `useCallback` everywhere." Say: I measure first, and the big wins are usually less JavaScript and less data.

---

## 14. What's new in React 19?

**In simple words**
React 19 made forms and server work much easier. It also removed some old boilerplate.

**🗣️ Say this in the interview**
> "The main things:
> - **Actions**: I can pass an async function to a form's `action`, and React handles the pending state, errors and form reset for me.
> - **`useActionState`**, **`useFormStatus`** and **`useOptimistic`** for form state, loading UI and **optimistic updates**. Optimistic means showing the result before the server confirms, like showing the item in the cart right away while the request runs.
> - **`use()`** to read a Promise or a context during render, together with Suspense.
> - **`ref` as a normal prop**, so `forwardRef` is no longer needed.
> - **Server Components and Server Actions** are stable.
> - Separately, the **React Compiler** handles memoization automatically."

**⚠️ Avoid saying**
"The React Compiler is part of React 19." It's a separate tool; mention it as separate.

---

## 15. How do you fetch data on the client?

**In simple words**
Don't write your own fetching with `useEffect` every time. A library like TanStack Query handles the hard parts for you.

**🗣️ Say this in the interview**
> "For client-side fetching I use **TanStack Query**, not hand-written `useEffect` fetching. It gives me caching, deduping (two components asking for the same data get one request), background refetching, retries, pagination helpers and request cancellation. It also fixes race conditions for free.
> In the Next.js App Router, most fetching moves to **Server Components**. The client only fetches for truly interactive things, like search-as-you-type."

**⚠️ Avoid saying**
"I always fetch in `useEffect`." Say: I use a data library or Server Components.

---

## 16. How do you test React components?

**In simple words**
Test what the user sees and does. Don't test the inside of the component.

**🗣️ Say this in the interview**
> "I test **behaviour, not implementation**. With **React Testing Library**, I find elements the way a user would, like `getByRole('button', { name: 'Add to cart' })`. I click or type, and then check what the user sees. I don't test internal state or which hooks ran.
> I mock the **network** with **MSW** (Mock Service Worker, it fakes API responses), not my own components. Pure logic gets plain unit tests. The most important journeys, like search → add to cart → checkout, get **Playwright** end-to-end tests."

**⚠️ Avoid saying**
"I check the component's state in my tests." Say: I check what appears on the screen.

---

## Quick questions
- **Props vs state?** Props come from the parent and are read-only. State belongs to the component and changes over time.
- **Lifting state up?** Moving state to the nearest common parent, so sibling components can share it.
- **Prop drilling?** Passing props down through many layers. Fix with composition (`children`), context, or a store.
- **Fragment?** `<>…</>` groups children without adding an extra DOM node.
- **Portal?** Renders children into a different DOM node (modals, tooltips), but keeps React context working.
- **Why can't hooks be inside conditions?** React tracks hooks by the **order they are called**. A condition can change that order and break it.
- **HOC vs hook?** A HOC (higher-order component) wraps a component. Hooks share logic more simply. Hooks are preferred today.
- **`useTransition`?** Marks an update as not urgent, so typing stays smooth while a heavy list updates.
- **Using `key` to reset a component?** Changing a component's `key` removes it and mounts a fresh one with new state.
- **Class vs function components?** Function components with hooks are the standard. Classes are only needed for error boundaries (or you use a library for that).

## Remember these 10 things
1. React compares the new UI tree with the old one (reconciliation) and changes only what's different.
2. Use stable IDs as keys, not the array index.
3. A parent re-render renders all its children too, even with the same props.
4. Never mutate state. Make new objects. Use `setX(prev => ...)` when the new value depends on the old.
5. `useEffect` is for syncing with the outside world. Calculate derived values during render.
6. Always clean up effects. Abort old fetches to avoid race conditions.
7. Measure with the Profiler before adding `useMemo`/`useCallback`. The React Compiler does much of it now.
8. Split state by type: server state in TanStack Query, UI state local, filters in the URL.
9. Error boundaries catch render errors only, not event handler or async errors.
10. My S1 lesson: what you send (4 MB of props) matters more than how you render.
