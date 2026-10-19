# JavaScript Fundamentals

> **Time needed:** about 2.5 hours · **Questions:** 18 + quick questions
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

## Progress checklist
- [ ] 1. How does JavaScript run your code?
- [ ] 2. Scope and closures
- [ ] 3. Hoisting and the TDZ
- [ ] 4. `var` vs `let` vs `const`
- [ ] 5. How `this` works
- [ ] 6. `call` vs `apply` vs `bind`
- [ ] 7. Prototypes and classes
- [ ] 8. Promises and async/await
- [ ] 9. Microtasks vs macrotasks
- [ ] 10. Error handling in async code
- [ ] 11. `==` vs `===`
- [ ] 12. Primitive vs reference types
- [ ] 13. Shallow copy vs deep copy
- [ ] 14. Debounce vs throttle
- [ ] 15. `map`, `filter`, `reduce`
- [ ] 16. CommonJS vs ES Modules
- [ ] 17. Garbage collection and memory leaks
- [ ] 18. Generators and iterators
- [ ] Quick questions
- [ ] Remember these 10 things

---

## 1. How does JavaScript run your code? (call stack, heap, event loop)

**In simple words**
Your code runs on one thread, one thing at a time. Slow work happens in the background, and its callback waits in a queue.

**Picture it**
One chef who never stands watching the oven. They put the dish in, cook the next order, and come back when the timer rings.

**🗣️ Say this in the interview**
> "JavaScript runs my code on one thread with one **call stack**. The call stack is a list of the functions running right now. A function goes on when it's called and comes off when it returns. Objects live in a separate memory area called the **heap**.
> When my code starts something slow, like a network call or a timer, it doesn't wait. The browser or Node does that work in the background. When it's done, its callback goes into a queue. The **event loop** has one simple job: when the stack is empty, take the next callback from the queue and run it. That's how one thread can handle thousands of requests."

**They may also ask**
- *What is an execution context?* The "environment" code runs in: its variables, its `this`, and its outer scope. A global one is created first, then a new one for every function call.
- *What causes a stack overflow?* Too many nested calls, usually recursion with no stop condition. The stack has a fixed size.
- *Stack vs heap?* The stack holds function calls. It is fast but small. The heap holds objects. It is big and the garbage collector cleans it.

**⚠️ Avoid saying**
"JavaScript is multi-threaded." Say instead: *my* code runs on one thread, but the runtime (the browser, or libuv in Node) uses other threads for I/O.

---

## 2. What are scope, lexical scope, and closures?

**In simple words**
A closure is a function that remembers the variables around it, even after the outer function has finished.

**Picture it**
A backpack. The inner function carries the variables it needs, wherever it goes.

**🗣️ Say this in the interview**
> "Scope is where a variable is visible. JavaScript uses **lexical scope**, so visibility depends on where the code is written, not where it's called.
> A **closure** is a function that remembers the variables from where it was created, even after the outer function returned.
> I use closures all the time. Debounce keeps its timer in a closure. React hooks depend on them. And they are the easiest way to keep private state. The downside: if a closure holds a big object, that object stays in memory."

```javascript
function createCounter() {
  let count = 0;              // private: nothing outside can touch it
  return () => ++count;       // this arrow function remembers `count`
}
const next = createCounter();
next(); // 1
next(); // 2
```

**They may also ask**
- *Practical uses?* Private state, function factories, memoization (caching results), debounce/throttle, event handlers, partial application.
- *Can closures leak memory?* Yes. A long-lived callback (an event listener or interval) that holds a big object keeps it alive. Remove the listener, or don't capture what you don't need.
- *The classic loop bug?* With `var` in a `for` loop plus `setTimeout`, all callbacks see the last value, because they share one variable. `let` creates a fresh variable for each loop round. (See the Coding Round file, "What does this print?" puzzles.)

**⚠️ Avoid saying**
"A closure is a function inside a function." Say instead: it's a function that *remembers* its outer variables after the outer function has returned.

---

## 3. What is hoisting and the temporal dead zone (TDZ)?

**In simple words**
JavaScript notes all declarations before running code. But `let` and `const` can't be used before their line.

**🗣️ Say this in the interview**
> "Before running code, JavaScript registers all declarations in their scope. That's **hoisting**.
> **Function declarations** are fully hoisted, so I can call them before they appear. **`var`** is hoisted and starts as `undefined`. **`let`, `const` and classes** are hoisted too, but I can't use them until the line where they're declared. That gap is the **temporal dead zone**. Using them there throws a `ReferenceError`."

```javascript
console.log(a); // undefined  (var is hoisted as undefined)
console.log(b); // ReferenceError  (b is in the TDZ)
var a = 1;
let b = 2;
```

**They may also ask**
- *Why does `typeof x` throw for a `let` variable?* If `x` is in its TDZ, even `typeof` throws. For a variable that was never declared, `typeof` safely returns `"undefined"`.
- *Function declaration vs expression?* `function foo() {}` is hoisted with its body. `const foo = () => {}` follows `const` rules, so it's in the TDZ until that line.

**⚠️ Avoid saying**
"`let` and `const` are not hoisted." They are hoisted, they just can't be used before their line (the TDZ).

---

## 4. What is the difference between `var`, `let` and `const`?

**In simple words**
`var` is the old way and leaks out of blocks. `let` and `const` live only inside their `{ }`.

**🗣️ Say this in the interview**
> "`var` is **function-scoped**, can be declared twice, and starts as `undefined`. `let` and `const` are **block-scoped**, so they only live inside their `{ }`, and they have a TDZ. `const` means I can't **reassign** the variable. But if it holds an object, I can still change what's inside. In modern code I use `const` by default, `let` when I need to reassign, and never `var`."

**They may also ask**
- *Can a `const` object be changed?* Yes. `const user = {}; user.name = 'A'` works. `Object.freeze` stops that, but only at the top level (shallow).
- *Why avoid `var`?* It leaks out of blocks, allows silent redeclaration, and causes the loop-closure bug.

**⚠️ Avoid saying**
"`const` makes a value immutable." Say instead: `const` stops reassignment; the object inside can still change.

---

## 5. How does `this` work?

**In simple words**
`this` is decided by **how** a function is called. Arrow functions borrow `this` from the code around them.

**🗣️ Say this in the interview**
> "`this` depends on how a function is called. There are four rules:
> - Called as a method, `obj.fn()`: `this` is `obj`.
> - Called with `new`: `this` is the new object.
> - Called with `call`, `apply` or `bind`: `this` is whatever I pass.
> - Called plainly, `fn()`: `this` is `undefined` in strict mode.
>
> **Arrow functions** don't have their own `this`. They take it from the surrounding code. That's why they're great for callbacks and bad as object methods."

```javascript
// Inside an ES module (strict mode is on by default)
const user = {
  name: 'Salman',
  regular() { return this.name; },  // user.regular() -> 'Salman'
  arrow: () => this?.name,          // user.arrow() -> undefined: `this` comes from outside the object
};
const fn = user.regular;
fn(); // TypeError: `this` is undefined, because we lost the object
```

**They may also ask**
- *Arrow function as an object method?* Usually a bug, because `this` won't be the object.
- *Why do we lose `this` when passing a method as a callback?* It gets called later as a plain function. Fix it with `.bind(obj)` or an arrow wrapper like `() => obj.fn()`.

**⚠️ Avoid saying**
"`this` is the object where the function is written." Say instead: it depends on how the function is *called*.

---

## 6. What is the difference between `call`, `apply` and `bind`?

**In simple words**
All three choose `this`. Two run now, one runs later.

**🗣️ Say this in the interview**
> "All three set `this`. `call` runs the function now, with arguments one by one. `apply` runs it now, with arguments as an **array**. `bind` doesn't run it. It **returns a new function** with `this` locked in. My memory trick: **A**pply = **A**rray, **B**ind = **B**ack later."

```javascript
greet.call(user, 'Hi', '!');               // runs now
greet.apply(user, ['Hi', '!']);            // runs now, args in an array
const later = greet.bind(user, 'Hi');      // runs later
later('!');
```

**⚠️ Avoid saying**
"`bind` calls the function." It doesn't; it returns a new function.

---

## 7. How do prototypes and classes work?

**In simple words**
Objects inherit from other objects through a hidden link. Classes are just nicer syntax for this.

**🗣️ Say this in the interview**
> "Every object has a hidden link to another object, its **prototype**. When I read a property that isn't on the object, JavaScript walks up this chain until it finds it or reaches `null`.
> **Classes are just nicer syntax** over prototypes. Methods I write in a class live once on the prototype and are shared by all instances. They are not copied into each object."

**They may also ask**
- *`prototype` vs `__proto__`?* `Foo.prototype` is the object that `new Foo()` instances will link to. `obj.__proto__` (properly `Object.getPrototypeOf(obj)`) is the link itself.
- *Own vs inherited property?* `Object.hasOwn(obj, 'x')` checks only the object. `'x' in obj` also checks the chain.
- *Private fields?* `#field` in a class is truly private. The language enforces it; it's not just a naming habit like `_field`.

**⚠️ Avoid saying**
"JavaScript classes work like Java classes." Say instead: they are syntax over prototypes.

---

## 8. Explain Promises and async/await.

**In simple words**
A Promise is a value that arrives later. The big mistake is awaiting things one by one when they could run together.

**Picture it**
A Promise is the buzzer a restaurant gives you. You can do other things. It buzzes when your food is ready (or when they tell you it's sold out).

**🗣️ Say this in the interview**
> "A **Promise** is a placeholder for a value that arrives later. It starts **pending**, then becomes **fulfilled** or **rejected**. **async/await** is cleaner syntax over Promises. `await` pauses only that one async function; everything else keeps running.
> The most common mistake I see is awaiting calls **one by one** when they don't depend on each other. If two calls are independent, I start both and use `Promise.all`."

```javascript
// ❌ One after another: about 2x slower
const user = await getUser(id);
const orders = await getOrders(id);

// ✅ Both at the same time
const [user, orders] = await Promise.all([getUser(id), getOrders(id)]);
```

**The four combinators (know these well)**

| Method | Resolves when | Rejects when | Use it for |
|---|---|---|---|
| `Promise.all` | **all** succeed | **any one** fails (fails fast) | Independent calls you need all of |
| `Promise.allSettled` | **all** finish (either way) | never | Batch jobs where you want every result |
| `Promise.race` | the **first** one settles | the first one to settle is an error | Timeouts |
| `Promise.any` | the **first success** | **all** fail | Fastest of several mirrors |

**They may also ask**
- *Does `Promise.all` cancel the others when one fails?* No. The others keep running; you just stop waiting. Use `AbortController` for real cancelling.
- *What happens when an async function throws?* It returns a rejected Promise. If nobody catches it, you get an `unhandledRejection`, which crashes Node by default since v15.
- *How do you limit concurrency (how many run at once)?* A small worker pool (see the Coding Round file, "concurrency limit"), or a library like `p-limit`.
- *Timeout pattern?* `Promise.race([work, timeout(5000)])`, or better, pass `AbortSignal.timeout(5000)` to `fetch`.

**⚠️ Avoid saying**
"`await` blocks the thread." It only pauses that one async function.

---

## 9. What is the difference between microtasks and macrotasks?

**In simple words**
Promise callbacks (microtasks) always jump ahead of timers (macrotasks). Interviewers love output puzzles on this.

**Picture it**
A bank counter. After serving each normal customer, the clerk serves *everyone* in the VIP line before calling the next normal customer.

**🗣️ Say this in the interview**
> "There are two main queues. **Microtasks** are Promise callbacks and `queueMicrotask`. **Macrotasks** are things like `setTimeout`, I/O and events.
> The rule: after each macrotask, the engine **empties the whole microtask queue** before moving on. So Promise callbacks always run before the next timer. In Node there's an even earlier queue, `process.nextTick`, which runs before Promise callbacks."

**Output puzzle (very common):**

```javascript
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
process.nextTick(() => console.log('4'));
console.log('5');
// Node (CommonJS): 1, 5, 4, 3, 2
```

How to explain it: "1 and 5 are normal code, so they run first. Then the stack is empty. Node runs nextTick (4), then Promise microtasks (3), then the timer (2)."

**They may also ask**
- *Can microtasks starve the event loop?* Yes. A Promise chain that keeps adding new microtasks blocks timers and I/O forever.
- *Does it change in an ES module?* Yes. In an ES module the output is `1, 5, 3, 4, 2`, because module code itself runs inside a Promise job. Mention this only if they push. It's a strong senior signal.

**⚠️ Avoid saying**
"`setTimeout(fn, 0)` runs immediately." It runs only after the current code and all microtasks finish.

---

## 10. How do you handle errors in async code?

**In simple words**
Use `try/catch` with `await`, and never leave a Promise unhandled.

**🗣️ Say this in the interview**
> "With async/await I use normal `try/catch`. The key rule: every Promise must be awaited or have a `.catch`. Otherwise errors disappear, or they crash the process. I catch errors at the boundary where I can actually *do* something, like return an HTTP error or retry. Not at every line."

```javascript
async function load() {
  try {
    return await fetchData(); // with `await`, the catch below handles a rejection
    // `return fetchData();` would skip this catch
  } catch (err) {
    log(err);
    return null;
  }
}
```

**They may also ask**
- *`return await` vs `return`?* Inside a `try` block, `return await promise` lets your `catch` handle a rejection. A plain `return promise` skips your catch.
- *Fire-and-forget?* Only with an explicit `.catch(log)`. Otherwise it's a silent failure waiting to happen.

**⚠️ Avoid saying**
"I wrap every line in try/catch." Say instead: I catch at the boundary where I can respond or retry.

---

## 11. What is the difference between `==` and `===`? What is type coercion?

**In simple words**
`==` converts types first (**coercion**), which gives strange results. Use `===`.

**🗣️ Say this in the interview**
> "`===` compares **value and type** with no conversion. `==` converts types first, which gives weird results like `'' == 0` being `true`. I always use `===`. The one accepted exception is `x == null`, which checks for both `null` and `undefined` in one go."

**They may also ask**
- *`NaN === NaN`?* `false`. Use `Number.isNaN()`.
- *`typeof null`?* `"object"`. It's an old bug in the language.
- *`null` vs `undefined`?* `undefined` means "not set yet". `null` means "empty on purpose".

**⚠️ Avoid saying**
"`==` is always wrong." Mention the `x == null` exception.

---

## 12. What is the difference between primitive and reference types?

**In simple words**
Simple values are copied. Objects are shared, so a change in one place shows everywhere.

**Picture it**
A primitive is a photocopy. An object is a shared Google Doc link: everyone with the link sees your edits.

**🗣️ Say this in the interview**
> "Primitives (string, number, boolean, null, undefined, symbol, bigint) are copied **by value**. Objects, arrays and functions are passed as a **reference to the same object**. So if I pass an object into a function and change it, the caller sees the change. That's why I prefer returning new objects instead of changing inputs, especially in React."

```javascript
function rename(u) { u.name = 'B'; }
const user = { name: 'A' };
rename(user);
user.name; // 'B': the function changed the caller's object
```

**⚠️ Avoid saying**
"Objects are copied when you pass them." They're not; both sides point to the same object.

---

## 13. What is the difference between a shallow copy and a deep copy?

**In simple words**
A shallow copy shares nested objects. A deep copy copies everything.

**🗣️ Say this in the interview**
> "A **shallow** copy, like spread `{...obj}`, copies only the top level. Nested objects are still shared. A **deep** copy duplicates everything. Today I use **`structuredClone`**. It's built in and handles Dates, Maps, Sets and circular references. `JSON.parse(JSON.stringify())` is the old trick, but it loses Dates, `undefined` and functions, and it breaks on circular references."

```javascript
const a = { info: { city: 'Dubai' } };
const shallow = { ...a };
shallow.info.city = 'Riyadh';
a.info.city; // 'Riyadh': nested object is shared

const deep = structuredClone(a); // fully separate copy
```

**They may also ask**
- *Why does it matter in React?* React compares by reference. If you change a nested object in place, the reference stays the same, and React may not re-render.
- *What can't `structuredClone` copy?* Functions, DOM nodes, and class prototypes (you get a plain object back).

**⚠️ Avoid saying**
"Spread makes a full copy." It's only shallow.

---

## 14. What is the difference between debounce and throttle?

**In simple words**
Both limit how often a function runs, in different ways.

**Picture it**
Debounce is a lift door that waits until people stop walking in. Throttle is a bus that leaves every 10 minutes no matter what.

**🗣️ Say this in the interview**
> "**Debounce** waits until the calls **stop** for a while, then runs once. It's perfect for a search box: search only after the user stops typing for 300 ms. **Throttle** runs **at most once per interval**, no matter how many calls come in. That's for scroll, resize, or tracking events."

Code for both: see the Coding Round file (`debounce` and `throttle` questions).

**They may also ask**
- *Leading vs trailing?* Trailing runs after the pause (the usual case). Leading runs right away on the first call, then ignores calls until the pause.
- *How do you cancel a debounce?* Add a `cancel()` that calls `clearTimeout`, for example when a React component unmounts.

**⚠️ Avoid saying**
Mixing them up. Search box = debounce. Scroll = throttle.

---

## 15. Explain `map`, `filter`, `reduce`, and immutability.

**In simple words**
Three array tools that return new values and don't change the original.

**🗣️ Say this in the interview**
> "`map` transforms every item, `filter` keeps some items, and `reduce` boils a list down to one value. None of them change the original array. I use `reduce` for simple sums and grouping. But if a reduce gets complicated, a plain `for…of` loop is easier for the next person to read."

**They may also ask**
- *Which array methods change the original?* `push`, `pop`, `splice`, `sort`, `reverse`, `fill`. The newer `toSorted`, `toReversed`, `toSpliced` and `with` return copies instead.
- *Does chaining cost anything?* Each step creates a new array. Fine for normal sizes. For very large arrays, a single loop is faster.

**⚠️ Avoid saying**
"`sort` returns a new array." It sorts in place. Use `toSorted` for a copy.

---

## 16. What is the difference between CommonJS and ES Modules?

**In simple words**
Two module systems: Node's old `require`, and the standard `import`/`export`.

**🗣️ Say this in the interview**
> "**CommonJS** is Node's original system: `require` and `module.exports`, loaded at runtime, one by one. **ES Modules** are the standard: `import` and `export`. Imports are static, so tools can read them before running the code and **tree-shake** them, which means remove unused code from the bundle. I use ESM for new code.
> Node decides which one a file is from the extension (`.mjs` or `.cjs`) or the `"type"` field in package.json."

**They may also ask**
- *Default vs named exports?* I prefer named exports. They're easier to search, refactor and auto-import.
- *`__dirname` in ESM?* It doesn't exist. Use `import.meta.dirname` (recent Node) or build it from `import.meta.url`.
- *Can CommonJS load ESM?* In the past, only through dynamic `import()`. Recent Node versions can `require()` most ES modules directly.

**⚠️ Avoid saying**
"They're the same, just different syntax." ESM is static, which is what makes tree-shaking possible.

---

## 17. How does garbage collection work? What causes memory leaks?

**In simple words**
JavaScript frees what nothing points to. A leak means something still points to data you don't need.

**🗣️ Say this in the interview**
> "V8 automatically frees objects that nothing can reach anymore. So a **memory leak** in JavaScript means something is still *holding a reference* to data I don't need. The usual causes are a cache with no size limit, a global array that keeps growing, event listeners that are never removed, timers that are never cleared, or closures holding big objects."

**They may also ask**
- *WeakMap use case?* Attaching extra data to an object without keeping the object alive. For example, caching metadata per request object. When the object is gone, the entry disappears by itself.
- *How do you find a leak?* Take heap snapshots, compare them, and follow the retainer path (what is holding the object). See the Node.js file and the Production Troubleshooting file.

**⚠️ Avoid saying**
"JavaScript has a garbage collector, so it can't leak." It can, if you keep references you don't need.

---

## 18. What are generators and iterators?

**In simple words**
A generator hands out items one at a time, pausing in between. Good for big data.

**🗣️ Say this in the interview**
> "An **iterator** is anything with a `next()` method. That's what `for…of` uses. A **generator**, `function*`, is an easy way to write one: it pauses at each `yield`. They're useful for lazy sequences, like processing a large dataset page by page without loading it all. **Async generators** with `for await` are great for streaming paginated API results."

```javascript
async function* allOrders(api) {
  let cursor;
  do {
    const page = await api.list({ cursor }); // fetch one page
    yield* page.items;                        // hand out its items one by one
    cursor = page.nextCursor;
  } while (cursor);                           // stop when there are no more pages
}
for await (const order of allOrders(api)) process(order);
```

**⚠️ Avoid saying**
"Generators run in parallel." They don't; they just pause and resume.

---

## Quick questions
- **`?.` vs `??`?** `?.` (optional chaining) stops if the value is null/undefined. `??` (nullish coalescing) gives a default only for null/undefined, not for `0` or `''` (unlike `||`).
- **Arrow vs normal function?** Arrows have no own `this`, `arguments` or `prototype`, and can't be used with `new`.
- **`Object.freeze`?** Makes an object read-only, but only the top level (shallow).
- **Set vs Array?** Set keeps unique values and has a fast `has()`. Array keeps order and allows duplicates.
- **Map vs Object?** Map allows any key type, keeps insertion order, has `size`, and is better for frequent add/remove.
- **Pure function?** Same input gives the same output, with no side effects. Easy to test.
- **Currying?** Turning `f(a, b)` into `f(a)(b)`. Useful for partial application (fixing some arguments early).
- **IIFE?** A function that runs immediately: `(() => {...})()`. Used to create scope before modules existed.
- **Symbol?** A unique primitive value. Used for hidden object keys that never clash.
- **`Object.groupBy`?** Built-in grouping (ES2024): `Object.groupBy(items, i => i.type)`.
- **Event loop in one sentence?** "Run sync code, empty the microtasks, take the next macrotask, repeat."
- **Truthy/falsy?** Falsy: `false, 0, -0, 0n, '', null, undefined, NaN`. Everything else is truthy, including `[]` and `{}`.

## Remember these 10 things
1. Your code runs on one thread; the event loop runs queued callbacks when the stack is empty.
2. A closure is a function that remembers its outer variables after the outer function returns.
3. `let`/`const` are hoisted but in the TDZ; `var` is hoisted as `undefined`.
4. `const` stops reassignment, not changes inside an object.
5. `this` depends on how a function is called; arrow functions borrow `this` from outside.
6. Apply = Array, Bind = Back later.
7. Independent calls: use `Promise.all`, not one `await` after another.
8. Order: sync code → `process.nextTick` → Promise microtasks → timers (in Node CommonJS).
9. Spread is a shallow copy; use `structuredClone` for deep copies.
10. A JS memory leak = something still holds a reference (unbounded cache, listeners, timers, closures).
