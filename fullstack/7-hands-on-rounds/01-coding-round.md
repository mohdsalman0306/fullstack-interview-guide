# Coding Round

> **Time needed:** about 4 hours · **Questions:** 24
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.
> Best practice: close this file, type each solution from memory in a blank file, run it with Node, then compare.

## Progress checklist
- [ ] 1. How to behave in a coding round
- [ ] 2. Implement `debounce`
- [ ] 3. Implement `throttle`
- [ ] 4. Run async tasks with a concurrency limit
- [ ] 5. Implement `Promise.all`
- [ ] 6. Retry with exponential backoff and jitter
- [ ] 7. Implement `memoize`
- [ ] 8. Implement an LRU cache
- [ ] 9. Deep clone
- [ ] 10. Flatten a nested array
- [ ] 11. Group objects by a key
- [ ] 12. Implement an event emitter
- [ ] 13. Implement `curry`
- [ ] 14. "What does this print?" puzzles (5 puzzles)
- [ ] 15. The patterns that solve most problems
- [ ] 16. Two Sum
- [ ] 17. Valid parentheses
- [ ] 18. Longest substring without repeating characters
- [ ] 19. Group anagrams
- [ ] 20. First non-repeating character
- [ ] 21. Binary search
- [ ] 22. Top K frequent elements
- [ ] 23. Merge overlapping intervals
- [ ] 24. Big-O cheat sheet

---

## 1. How should you behave in a coding round?

**In simple words**
The interviewer scores how you **talk** as much as your code. A correct answer typed in silence scores lower than a slightly slower one that you explain clearly. So think out loud the whole time.

**The 7 steps**
1. **Repeat the problem** in your own words. "So I get an array of numbers and a target, and I return the two indexes that add up to it?"
2. **Ask about edge cases.** Empty input? Duplicates? Negative numbers? Very large input? Can I change the input?
3. **Say the simple solution first**, with its cost. "The brute force is two loops, O(n²)."
4. **Improve it out loud.** "I can use more memory to get more speed. With a hash map I get O(n)."
5. **Code with clear names**, and talk while you type.
6. **Test it yourself.** Walk through a normal case by hand, then an edge case.
7. **Say the final cost**: time and space (memory).

**🗣️ If you get stuck, say this**
> "Let me think out loud. I know I need ___. What if I ___?"

Interviewers often give hints when they can hear your thinking.

**⚠️ Avoid saying**
Nothing. Silence for two minutes is the worst thing you can do. Say what you are thinking, even if it is not finished.

---

# Part 1: JavaScript utility questions (very common for full stack)

## 2. Implement `debounce`

**The task:** Make a function wait until the calls **stop** for `wait` ms, then run once. (Example: search box. Only search when the user stops typing.)

**The idea:** The trick is one timer kept in a **closure** (a function remembers the variables around it). Every new call cancels the old timer and starts a new one.

```javascript
function debounce(fn, wait) {
  let timer; // lives in the closure, shared by all calls
  function debounced(...args) {
    clearTimeout(timer); // cancel the previous waiting call
    timer = setTimeout(() => fn.apply(this, args), wait); // start a fresh wait
  }
  debounced.cancel = () => clearTimeout(timer); // lets a component clean up
  return debounced;
}

// usage
const onSearch = debounce((q) => fetchResults(q), 300);
```

**🗣️ Say this while coding**
> "I keep one timer in a closure. Every call cancels the previous timer and starts a new one. So the function only runs once the calls stop for `wait` milliseconds. I use `apply` so `this` and the arguments pass through correctly. I also added `cancel`, so a React component can clean up when it unmounts."

**Cost:** O(1) per call. Each call just clears and sets one timer.

**They may also ask**
- *Debounce vs throttle?* Debounce waits until things go quiet. Throttle runs at most once every X ms while things keep happening.

---

## 3. Implement `throttle`

**The task:** Let a function run **at most once** every `interval` ms, no matter how often it is called. (Example: scroll or resize events.)

**The idea:** The trick is to remember **when it last ran**. If enough time has passed, run it. If not, ignore the call.

```javascript
function throttle(fn, interval) {
  let last = 0; // time of the last real run
  return function (...args) {
    const now = Date.now();
    if (now - last >= interval) { // enough time has passed
      last = now;
      fn.apply(this, args);
    }
    // otherwise: ignore this call
  };
}
```

**🗣️ Say this while coding**
> "I remember when it last ran. If enough time has passed, it runs. Otherwise the call is ignored. This is a **leading-edge** throttle, it runs at the start of the interval. A **trailing** version would also schedule one last call at the end of the interval, so the final event is not lost."

**Cost:** O(1) per call. Just one time check.

---

## 4. Run async tasks with a concurrency limit

**The task:** Run an async job for every item, but never more than `limit` at the same time. Return results in the same order as the input.

**The idea:** The trick is to start `limit` small "workers". Each worker keeps grabbing the next item until the list is empty. Each result goes into its original slot, so the order stays correct.

```javascript
async function mapWithLimit(items, limit, worker) {
  const results = new Array(items.length);
  let next = 0; // index of the next item to take

  async function runner() {
    while (next < items.length) {
      const i = next++;                 // safe: JS runs one thing at a time between awaits
      results[i] = await worker(items[i], i);
    }
  }

  // start `limit` runners (or fewer, if there are fewer items)
  const runners = Array.from({ length: Math.min(limit, items.length) }, runner);
  await Promise.all(runners);
  return results;                       // same order as input
}

// usage: resize 500 images, max 5 at a time
await mapWithLimit(images, 5, resizeImage);
```

**🗣️ Say this while coding**
> "I start `limit` workers. Each one keeps taking the next index until the list is empty. `next++` is safe because JavaScript is single-threaded, nothing else runs between reading and increasing it. Results go into their original position, so the order is kept. Failure rule: as written, the first error rejects everything, like `Promise.all`. If I wanted every result, I'd catch the error per item and store `{ status, value }` or `{ status, reason }`, like `allSettled`."

**Cost:** O(n) work in total. Each item is handled once. Extra memory is O(n) for the results.

**They may also ask**
- *Why not just `Promise.all(items.map(worker))`?* That starts all 500 at once. It can overload the CPU, memory, a database, or an API's rate limit.

---

## 5. Implement `Promise.all`

**The task:** Write your own `Promise.all`: wait for all values, return their results in order, and fail as soon as one fails.

**The idea:** The trick is a counter of how many are still waiting. Each result is stored at its **input index**, not in finish order. When the counter hits zero, resolve.

```javascript
function promiseAll(values) {
  return new Promise((resolve, reject) => {
    const results = new Array(values.length);
    let remaining = values.length;
    if (remaining === 0) return resolve(results); // empty input: done right away

    values.forEach((value, i) => {
      Promise.resolve(value).then((v) => { // wrap plain values too
        results[i] = v;                    // keep input order
        if (--remaining === 0) resolve(results);
      }, reject);                          // first rejection wins
    });
  });
}
```

**🗣️ Say this while coding**
> "Key details. Results keep the **input order**, not the order they finish. Plain values that are not promises get wrapped with `Promise.resolve`. An empty array resolves immediately. And the first rejection rejects the whole thing."

**Cost:** O(n). We go over the list once.

**They may also ask**
- *How are `allSettled`, `race` and `any` different?* `allSettled` waits for all and never rejects. `race` gives the first one to finish, success or fail. `any` gives the first success.

---

## 6. Retry with exponential backoff and jitter

**The task:** Call a function. If it fails with a temporary error, wait a bit and try again, a few times.

**The idea:** The trick is three parts. **Exponential backoff**: wait longer each time (200, 400, 800 ms). **Jitter**: add randomness to the wait, so many clients don't retry at the same moment. **shouldRetry**: only retry errors that might go away.

```javascript
const sleep = (ms) => new Promise((r) => setTimeout(r, ms));

async function retry(fn, { retries = 3, baseMs = 200, shouldRetry = () => true } = {}) {
  for (let attempt = 0; ; attempt++) {
    try {
      return await fn();
    } catch (err) {
      // give up if we are out of retries, or this error is permanent
      if (attempt >= retries || !shouldRetry(err)) throw err;
      const delay = baseMs * 2 ** attempt;        // 200, 400, 800...
      await sleep(Math.random() * delay);         // "full jitter": random wait from 0 to delay
    }
  }
}

// usage: only retry temporary errors
await retry(() => callPaymentStatus(id), {
  shouldRetry: (e) => e.status >= 500 || e.code === 'ETIMEDOUT',
});
```

**🗣️ Say this while coding**
> "Exponential, so we back off and give a struggling service room to recover. Jitter, so a thousand clients don't all retry at the same instant. And `shouldRetry`, so we never retry permanent errors like a 400 Bad Request. With `retries = 3` that's one first try plus up to three retries."

**Cost:** At most `retries + 1` calls. Memory O(1).

**They may also ask**
- *Is it safe to retry a payment?* Only if the call is **idempotent** (doing it twice has the same effect as once). For writes like "create payment", send an **idempotency key**. Checking a status is a read, so it is safe.

---

## 7. Implement `memoize`

**The task:** Wrap a function so it remembers results. Same input again? Return the saved answer instead of computing it again.

**The idea:** The trick is a `Map` used as a cache. Turn the arguments into a key. If the key is in the cache, return the saved value.

```javascript
function memoize(fn, keyFn = (...args) => JSON.stringify(args)) {
  const cache = new Map();
  return function (...args) {
    const key = keyFn(...args);                       // e.g. "[1,2]"
    if (!cache.has(key)) cache.set(key, fn.apply(this, args)); // compute only once
    return cache.get(key);
  };
}
```

**🗣️ Say this while coding**
> "Some caveats I'd mention. The cache grows forever, so in production I'd cap it with an LRU. JSON keys don't work for functions or circular objects. For async functions, this caches the **promise**. That's actually nice, because two calls at the same time share one request. But a rejected promise would be cached too, so I'd delete the key on failure."

**Cost:** A cache hit is O(1) after building the key. Memory grows with every different input.

---

## 8. Implement an LRU cache

**The task:** Build a cache with a max size. When it's full, throw out the **Least Recently Used** item (the one nobody touched for the longest time).

**The idea:** The trick is that a JavaScript `Map` remembers insertion order. So the first key is always the oldest. On every use, delete the key and add it again, which moves it to the "newest" end.

```javascript
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.map = new Map();               // Map remembers insertion order
  }

  get(key) {
    if (!this.map.has(key)) return undefined;
    const value = this.map.get(key);
    this.map.delete(key);               // move to the "most recent" end
    this.map.set(key, value);
    return value;
  }

  set(key, value) {
    if (this.map.has(key)) this.map.delete(key);
    this.map.set(key, value);
    if (this.map.size > this.capacity) {
      const oldestKey = this.map.keys().next().value; // first key = least recently used
      this.map.delete(oldestKey);
    }
  }
}
```

**🗣️ Say this while coding**
> "A JavaScript `Map` keeps insertion order, so the first key is always the least recently used. On every access I delete and re-insert the key to move it to the end. Both `get` and `set` are O(1). In other languages you'd use a hash map plus a doubly linked list to get the same effect."

**Cost:** O(1) time for `get` and `set`. O(capacity) memory.

---

## 9. Deep clone

**The task:** Make a full copy of an object, including all nested objects, so changing the copy never changes the original.

**The idea:** The trick is recursion (the function calls itself for each nested value), plus a `WeakMap` that remembers objects we already copied. That stops infinite loops when an object points back to itself.

```javascript
function deepClone(value, seen = new WeakMap()) {
  if (value === null || typeof value !== 'object') return value;  // primitives: return as is
  if (seen.has(value)) return seen.get(value);                    // circular reference: reuse the copy
  if (value instanceof Date) return new Date(value);
  if (value instanceof Map) {
    const copy = new Map();
    seen.set(value, copy);
    value.forEach((v, k) => copy.set(k, deepClone(v, seen)));
    return copy;
  }
  const copy = Array.isArray(value) ? [] : {};
  seen.set(value, copy);  // remember BEFORE going deeper
  for (const key of Object.keys(value)) copy[key] = deepClone(value[key], seen);
  return copy;
}
```

**🗣️ Say this while coding**
> "In real code I'd just use `structuredClone`. But writing it shows I know the traps. Circular references, which I handle with a WeakMap of objects I already copied. Dates. Maps. And arrays versus plain objects."

**Cost:** O(n), where n is the number of values inside. Each one is copied once.

**They may also ask**
- *Why not `JSON.parse(JSON.stringify(obj))`?* It breaks Dates (they become strings), drops `undefined` and functions, loses Maps and Sets, and throws on circular references.

---

## 10. Flatten a nested array

**The task:** Turn `[1, [2, [3, 4]], 5]` into `[1, 2, 3, 4, 5]`.

**The idea:** The trick is: if an item is an array, flatten it too (recursion). If not, just add it. The second version uses a **stack** instead, so very deep arrays can't cause a "too much recursion" error.

```javascript
function flatten(arr) {
  const out = [];
  for (const item of arr) {
    if (Array.isArray(item)) out.push(...flatten(item)); // go inside nested arrays
    else out.push(item);
  }
  return out;
}

// Iterative version (no recursion depth limit):
function flattenIterative(arr) {
  const stack = [...arr], out = [];
  while (stack.length) {
    const item = stack.pop();                 // take from the end
    if (Array.isArray(item)) stack.push(...item); // unpack it back onto the stack
    else out.push(item);
  }
  return out.reverse();                       // we worked from the end, so reverse
}
// Built-in: arr.flat(Infinity)
```

**🗣️ Say this while coding**
> "The recursive version is the easiest to read. If the nesting could be very deep, I'd use the iterative version with a stack, because it can't run out of call stack. Because I pop from the end, the output comes out backwards, so I reverse it at the end. In real code I'd just use `arr.flat(Infinity)`."

**Cost:** O(n), where n is the total number of items at all levels.

---

## 11. Group objects by a key

**The task:** Turn a list of orders into `{ paid: [...], pending: [...] }`.

**The idea:** The trick is one loop. For each item, work out its key, create an empty list for that key if needed, then push the item.

```javascript
function groupBy(items, keyFn) {
  const groups = {};
  for (const item of items) {
    const key = keyFn(item);
    (groups[key] ??= []).push(item); // create the list the first time, then add
  }
  return groups;
}

groupBy(orders, (o) => o.status); // { paid: [...], pending: [...] }
// Modern built-in: Object.groupBy(orders, (o) => o.status)
```

**🗣️ Say this while coding**
> "One pass. `??=` creates the empty array only the first time we see a key. Today there's also a built-in, `Object.groupBy`."

**Cost:** O(n). Each item is visited once.

---

## 12. Implement an event emitter

**The task:** Build a small class with `on`, `off`, `once` and `emit`, like Node's `EventEmitter`.

**The idea:** The trick is a `Map` from event name to a `Set` of listener functions. `emit` calls each one. `on` returns an "unsubscribe" function for easy cleanup.

```javascript
class Emitter {
  #listeners = new Map(); // private field: event name -> Set of functions

  on(event, fn) {
    if (!this.#listeners.has(event)) this.#listeners.set(event, new Set());
    this.#listeners.get(event).add(fn);
    return () => this.off(event, fn);   // handy unsubscribe function
  }

  off(event, fn) {
    this.#listeners.get(event)?.delete(fn);
  }

  once(event, fn) {
    // wrap fn so it removes itself before it runs
    const off = this.on(event, (...args) => { off(); fn(...args); });
    return off;
  }

  emit(event, ...args) {
    // copy first, so listeners removing themselves (once) don't break the loop
    for (const fn of [...(this.#listeners.get(event) ?? [])]) fn(...args);
  }
}
```

**🗣️ Say this while coding**
> "A Map of event names to a Set of listeners. A Set stops the same function being added twice. `on` returns an unsubscribe function, which is nice for React cleanup. `once` wraps the listener so it removes itself first. In `emit` I copy the list before looping, because a `once` listener deletes itself while we are looping."

**Cost:** `on` and `off` are O(1). `emit` is O(k), where k is the number of listeners for that event.

---

## 13. Implement `curry`

**The task:** Turn `f(a, b, c)` into something you can call as `f(a)(b)(c)` or `f(a, b)(c)`.

**The idea:** The trick is to collect arguments. If we have enough (`fn.length` tells how many the function expects), call it. If not, return a new function that waits for more.

```javascript
function curry(fn) {
  return function curried(...args) {
    return args.length >= fn.length
      ? fn.apply(this, args)                                   // enough args: call it
      : (...more) => curried.apply(this, [...args, ...more]); // not enough: wait for more
  };
}

const add3 = curry((a, b, c) => a + b + c);
add3(1)(2)(3); // 6
add3(1, 2)(3); // 6
```

**🗣️ Say this while coding**
> "`fn.length` tells me how many arguments the function expects. If I have enough, I call it. If not, I return a function that remembers the arguments so far, through a closure, and asks for more."

**Cost:** O(1) work per call, plus copying the collected arguments.

**They may also ask**
- *Any limit?* `fn.length` does not count default parameters or rest parameters (`...args`). So curry doesn't work well with those.

---

## 14. "What does this print?" puzzles

These test if you really understand JavaScript. Always explain **why**, step by step.

### Puzzle 1: `var` vs `let` in a loop

```javascript
for (var i = 0; i < 3; i++) setTimeout(() => console.log(i));  // 3 3 3
for (let j = 0; j < 3; j++) setTimeout(() => console.log(j));  // 0 1 2
```

**Why, step by step**
1. The loop finishes first. The timers only run later.
2. `var` makes **one** shared `i` for the whole loop. When the timers run, `i` is already 3. So all print 3.
3. `let` makes a **new** `j` for each loop round. Each callback remembers its own `j` (closure). So they print 0, 1, 2.

**🗣️ Say this**
> "`var` has one shared variable, so by the time the timers run, it's 3. `let` creates a new binding for each iteration, so each callback closes over its own value."

### Puzzle 2: Event loop order

```javascript
console.log('start');
setTimeout(() => console.log('timeout'), 0);
Promise.resolve().then(() => console.log('promise'));
(async () => { console.log('async start'); await null; console.log('async end'); })();
console.log('end');
// start, async start, end, promise, async end, timeout
```

**Why, step by step**
1. Normal (synchronous) code runs first, top to bottom: `start`.
2. `setTimeout` puts `timeout` in the **macrotask** queue (the "later" line).
3. `.then` puts `promise` in the **microtask** queue (the "very soon" line).
4. The async function runs right away until its first `await`: prints `async start`. The rest (`async end`) goes into the microtask queue, after `promise`.
5. `end` prints. Synchronous code is done.
6. Now all microtasks run, in order: `promise`, then `async end`.
7. Only then the timer runs: `timeout`.

**🗣️ Say this**
> "Synchronous code first, including the async function up to its first `await`. Then microtasks in order: 'promise', then the rest of the async function. Then the timer, which is a macrotask."

### Puzzle 3: Losing `this`

```javascript
class User {
  constructor(name) { this.name = name; }
  greet() { return `Hi ${this.name}`; }
}
const u = new User('Salman');
const greet = u.greet;
greet(); // TypeError: Cannot read properties of undefined
```

**Why, step by step**
1. `this` depends on **how** a function is called, not where it is written.
2. `greet()` is called alone, with no object before the dot.
3. Code inside a class always runs in strict mode. In strict mode, `this` is then `undefined`.
4. `undefined.name` throws a TypeError.

**🗣️ Say this**
> "Class bodies are strict mode, and `greet` is called without an object, so `this` is `undefined`. The fix is `u.greet.bind(u)`, or an arrow function class field."

### Puzzle 4: The `parseInt` trap

```javascript
['1', '2', '3'].map(parseInt); // [1, NaN, NaN]
```

**Why, step by step**
1. `map` calls the function with `(value, index, array)`.
2. `parseInt` takes a second argument: the **radix** (number base).
3. So the calls are `parseInt('1', 0)`, `parseInt('2', 1)`, `parseInt('3', 2)`.
4. Radix 0 means "use the default", base 10, so we get `1`.
5. Radix 1 is not a valid base, so `NaN`.
6. Radix 2 is binary. Binary only has digits 0 and 1, so `'3'` gives `NaN`.

**🗣️ Say this**
> "`map` passes the value and the index, so the index becomes the radix. That gives `parseInt('2', 1)`, which is an invalid base, and `parseInt('3', 2)`, where 3 is not a binary digit. Use `.map(Number)` or `.map(s => parseInt(s, 10))`."

### Puzzle 5: Floating point

```javascript
0.1 + 0.2 === 0.3; // false (0.30000000000000004)
```

**Why, step by step**
1. Computers store numbers in binary (base 2).
2. 0.1 can't be stored exactly in binary, like 1/3 can't be written exactly in decimal (0.333...).
3. So tiny errors add up, and the sum is `0.30000000000000004`.

**🗣️ Say this**
> "Binary floating point can't represent 0.1 exactly. For **money**, I store **integer minor units**, like cents or fils, or use a decimal library. Never floats."

---

# Part 2: Algorithm patterns

## 15. What patterns solve most interview problems?

**In simple words**
Most interview problems are one of about ten patterns. Learn to spot the "clue words" in the question, and you know where to start.

| Pattern | Use when you see… | Typical cost |
|---|---|---|
| **Hash map / counting** | "count", "duplicates", "anagram", "first unique", "two sum" | O(n) time, O(n) space |
| **Hash set** | "seen before?", "contains duplicate" | O(n) |
| **Two pointers** | Sorted array, pairs, palindromes, "remove in place" | O(n) |
| **Sliding window** | "longest/shortest substring or subarray with…" | O(n) |
| **Stack** | Brackets, "next greater element", undo, parsing | O(n) |
| **Queue / BFS** (breadth-first, level by level) | Shortest path when every step costs the same, level-by-level tree | O(V + E) |
| **DFS / recursion** (depth-first, go deep first) | Trees, graphs, all combinations, permutations | O(V + E) or exponential |
| **Binary search** | Sorted data, or "find the smallest X that works" | O(log n) |
| **Sort first** | Intervals, grouping, closest pairs | O(n log n) |
| **Heap** (always gives you the smallest/largest fast) | "Top K", "K-th largest", merge K sorted lists | O(n log k) |

V = number of nodes (points), E = number of edges (links between them).

**🗣️ Say this in the interview**
> "This looks like a counting problem, so my first idea is a hash map. That usually gets it down to one pass, O(n) time, using O(n) extra memory."

---

## 16. Two Sum

**The task:** Given an array and a target, return the indexes of the two numbers that add up to the target.

**The idea:** The trick is a hash map of numbers we've already seen. For each number, ask: "Have I already seen the number I need (`target - current`)?"

```javascript
function twoSum(nums, target) {
  const seen = new Map();                 // value -> index
  for (let i = 0; i < nums.length; i++) {
    const need = target - nums[i];        // the partner we are looking for
    if (seen.has(need)) return [seen.get(need), i];
    seen.set(nums[i], i);                 // remember this number for later
  }
  return null;                            // no pair found
}
```

**🗣️ Say this while coding**
> "Brute force is two loops, O(n²). With a map of values I've already seen, each number checks for its partner in O(1). So **O(n) time, O(n) space.** I add the number to the map *after* checking, so a number can't pair with itself."

**Cost:** O(n) time, we look at each number once. O(n) space for the map.

---

## 17. Valid parentheses

**The task:** Check if a string like `"({[]})"` has correctly matched and closed brackets.

**The idea:** The trick is a **stack** (like a pile of plates: last in, first out). Push every opening bracket. Every closing bracket must match the top of the pile.

```javascript
function isValid(s) {
  const pairs = { ')': '(', ']': '[', '}': '{' }; // closer -> its opener
  const stack = [];
  for (const ch of s) {
    if (ch === '(' || ch === '[' || ch === '{') stack.push(ch);
    else if (ch in pairs && stack.pop() !== pairs[ch]) return false; // wrong match
  }
  return stack.length === 0; // anything left open = invalid
}
```

**🗣️ Say this while coding**
> "Every opener goes on the stack. Every closer must match the most recent opener. At the end the stack must be empty, otherwise something was never closed. **O(n).**"

**Cost:** O(n) time, one pass. O(n) space in the worst case, if everything is an opener.

---

## 18. Longest substring without repeating characters

**The task:** Find the length of the longest part of a string with no repeated letters. (`"abcabcbb"` → 3, for `"abc"`.)

**The idea:** The trick is a **sliding window**: a range `[start, i]` that always holds unique letters. When a letter repeats inside the window, jump `start` just past its last position.

```javascript
function lengthOfLongestSubstring(s) {
  const lastSeen = new Map(); // letter -> last index where we saw it
  let start = 0, best = 0;
  for (let i = 0; i < s.length; i++) {
    const ch = s[i];
    if (lastSeen.has(ch) && lastSeen.get(ch) >= start) {
      start = lastSeen.get(ch) + 1;       // jump the window past the duplicate
    }
    lastSeen.set(ch, i);
    best = Math.max(best, i - start + 1); // current window size
  }
  return best;
}
```

**🗣️ Say this while coding**
> "The window from `start` to `i` always has unique characters. When I see a repeat inside the window, I move `start` just past its previous position. The `>= start` check matters: a repeat that is already outside the window doesn't count. Each character is processed once. **O(n).**"

**Cost:** O(n) time, one pass. Space is O(k), where k is the number of different letters.

---

## 19. Group anagrams

**The task:** Group words that use the same letters. `["eat", "tea", "tan", "nat"]` → `[["eat","tea"], ["tan","nat"]]`.

**The idea:** The trick is to give every word a "signature": its letters sorted. Anagrams get the same signature, so use it as a map key.

```javascript
function groupAnagrams(words) {
  const groups = new Map();
  for (const w of words) {
    const key = [...w].sort().join('');   // "eat", "tea" -> "aet"
    if (!groups.has(key)) groups.set(key, []);
    groups.get(key).push(w);
  }
  return [...groups.values()];
}
```

**🗣️ Say this while coding**
> "Anagrams have the same letters, so if I sort the letters, they get the same key. I group by that key in a map. Cost is n words times sorting each word."

**Cost:** O(n · k log k), where n is the number of words and k is the length of the longest word (we sort each word). O(n · k) space.

**They may also ask**
- *Can you avoid the sort?* Yes. Count the 26 letters and use the counts as the key. That makes it O(n · k).

---

## 20. First non-repeating character

**The task:** Return the first letter in a string that appears only once. (`"swiss"` → `"w"`.)

**The idea:** The trick is two passes. First count every letter. Then go through the string again, in the original order, and return the first letter with count 1.

```javascript
function firstUnique(str) {
  const count = new Map();
  for (const ch of str) count.set(ch, (count.get(ch) ?? 0) + 1); // pass 1: count
  for (const ch of str) if (count.get(ch) === 1) return ch;      // pass 2: first with count 1
  return null;
}
```

**🗣️ Say this while coding**
> "Two passes. Count everything, then return the first character with count 1 in the original order. Two passes is still **O(n)**."

**Cost:** O(n) time, two loops over the string. Space is O(k) for the different letters.

---

## 21. Binary search

**The task:** Find the index of a value in a **sorted** array, or return -1.

**The idea:** The trick is to look at the middle. If the target is bigger, throw away the left half. If smaller, throw away the right half. Repeat. Like finding a word in a dictionary.

```javascript
function binarySearch(arr, target) {
  let lo = 0, hi = arr.length - 1;
  while (lo <= hi) {                       // "<=" so a 1-item range is still checked
    const mid = Math.floor((lo + hi) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) lo = mid + 1;   // target is in the right half
    else hi = mid - 1;                     // target is in the left half
  }
  return -1;
}
```

**🗣️ Say this while coding**
> "I cut the search space in half each step, so it's **O(log n)**. The classic bugs are off-by-one errors in `lo <= hi` and the `mid + 1` / `mid - 1` updates. So I test with 1-element and 2-element arrays."

**Cost:** O(log n) time. A million items need only about 20 steps. O(1) space.

---

## 22. Top K frequent elements

**The task:** Return the `k` numbers that appear most often.

**The idea:** The trick is to count with a map, then sort by count and take the first `k`.

```javascript
function topKFrequent(nums, k) {
  const count = new Map();
  for (const n of nums) count.set(n, (count.get(n) ?? 0) + 1); // count each number
  return [...count.entries()]          // [[number, count], ...]
    .sort((a, b) => b[1] - a[1])       // most frequent first
    .slice(0, k)
    .map(([n]) => n);                  // keep only the numbers
}
```

**🗣️ Say this while coding**
> "Count, then sort by frequency: O(n log n). If they want better, a **min-heap of size k** gives O(n log k). Or **bucket sort** by frequency gives O(n), because a count can never be bigger than n."

**Cost:** O(n log n) time because of the sort. O(n) space for the counts.

---

## 23. Merge overlapping intervals

**The task:** Merge time ranges that overlap. `[[1,3],[2,6],[8,10]]` → `[[1,6],[8,10]]`.

**The idea:** The trick is to **sort by start time first**. Then each interval either overlaps the last merged one (so stretch it), or it doesn't (so start a new one).

```javascript
function mergeIntervals(intervals) {
  const sorted = [...intervals].sort((a, b) => a[0] - b[0]); // copy, then sort by start
  const out = [];
  for (const [start, end] of sorted) {
    const last = out[out.length - 1];
    if (last && start <= last[1]) last[1] = Math.max(last[1], end); // overlap: stretch it
    else out.push([start, end]);                                    // no overlap: new interval
  }
  return out;
}
// [[1,3],[2,6],[8,10]] -> [[1,6],[8,10]]
```

**🗣️ Say this while coding**
> "Sort by start time, then either extend the last interval or start a new one. I use `Math.max` because one interval can sit fully inside another. It's **O(n log n)** for the sort. A real-world use: merging store opening hours or booking slots."

**Cost:** O(n log n) time for the sort, then one O(n) pass. O(n) space for the output.

---

## 24. Big-O cheat sheet

**In simple words**
Big-O tells you how the work grows when the input grows. It's not about exact seconds. It's about "if the data is 10 times bigger, how much slower is it?"

| Cost | Name | Example |
|---|---|---|
| O(1) | Constant: same time, any size | Map/Set lookup, array index |
| O(log n) | Logarithmic: halves the problem each step | Binary search, database B-tree index lookup |
| O(n) | Linear: one look at each item | One loop over the data |
| O(n log n) | "n log n" | Good sorting |
| O(n²) | Quadratic: every item against every item | Nested loops over the same data |
| O(2ⁿ) | Exponential: doubles with each extra item | All subsets, naive recursion |

**🗣️ Say this in the interview**
> "This is O(n) time, because we look at each item once, and O(n) space for the map. I always state **both** time and space, because interviewers often ask, 'Can you do it with less memory?'"

**⚠️ Avoid saying**
Only the time cost. Always add the space (memory) cost too.

---

## Remember these 10 things
1. Talk the whole time. Restate, ask edge cases, brute force first, then improve.
2. Always finish with time **and** space cost, and test by hand with an edge case.
3. Debounce = wait until calls stop. Throttle = at most once per interval.
4. Use `fn.apply(this, args)` in wrappers so `this` and arguments pass through.
5. Concurrency limit: start `limit` workers that each take the next index; store results by index.
6. Retry: exponential backoff + jitter + only retry temporary errors (5xx, timeouts), never a 400.
7. LRU in JS: a `Map` keeps insertion order. Delete + re-insert on use; evict the first key.
8. Microtasks (promises, code after `await`) run before macrotasks (`setTimeout`).
9. Money: never floats. Store integer cents/fils.
10. Clue words → pattern: "count/duplicate" = hash map, "brackets" = stack, "longest substring" = sliding window, "sorted" = binary search, "intervals" = sort first, "top K" = heap.
