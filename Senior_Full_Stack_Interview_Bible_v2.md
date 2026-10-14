# Senior Full Stack Engineer Interview Bible

**Prepared for:** Mohammad Salman
**Stack:** JavaScript · TypeScript · Node.js · Express · REST · GraphQL · React · Next.js · SQL · MongoDB · Redis · Docker · Kubernetes · AWS / Azure · System Design
**Version:** 2.0 (October 2026)

> Interviewers don't score you on how much you know. They score you on how **clearly** you explain it and whether they'd trust you with their production system. This version is written so you can **say** the answers out loud instead of reciting a textbook.

---

## How this guide works

Every question follows the same layout. Not every question uses every part.

| Label | What it's for |
|---|---|
| 🗣️ **Say it** | The answer in plain spoken English, about 30–45 seconds out loud. Learn the *flow*, not the exact words. |
| 💡 **Picture it** | A simple analogy. Use it to make an idea stick, or when the interviewer looks unsure. |
| 🔍 **Go deeper** | Extra detail. Use it only when they say "can you elaborate?" |
| ❓ **Follow-ups** | The next questions they're likely to ask, **with short answers**. |
| 🏢 **From my work** | Where you've used it. Links to your Story Bank in Chapter 1. |
| ⚠️ **Trap** | A common answer that makes a senior sound junior. |

### Study plan

| When | What to do |
|---|---|
| 7+ days before | One or two chapters a day. Say every 🗣️ answer **out loud** once. Reading silently is not practice. |
| 3 days before | Practise the Story Bank (Ch. 1) and Behavioral (Ch. 20) out loud, with a timer. |
| Day before | Top 30 list (below), Production Scenarios (Ch. 17), Cheat Sheet (Ch. 23). |
| 1 hour before | Cheat Sheet and your "Tell me about yourself", twice. |

---

## Table of Contents

**Part A: How to interview**
0. How to Answer (formula, golden rules, phrase bank, Top 30)
1. Your Story ("Tell me about yourself", Story Bank)

**Part B: Technical**
2. JavaScript Fundamentals
3. TypeScript
4. Node.js Core and Production
5. Express and REST API Design
6. GraphQL
7. Authentication and Security
8. Web and Browser Fundamentals *(new)*
9. React
10. Next.js
11. Databases and Query Optimization
12. Redis, Queues and Event-Driven Systems
13. Docker, Kubernetes and CI/CD
14. Cloud: AWS and Azure *(new)*
15. System Design
16. Testing and Playwright
17. Production Troubleshooting Scenarios
18. Your PHP and Magento Background *(new)*

**Part C: Senior and people questions**
19. Engineering Leadership and Practices *(new)*
20. Behavioral and HR

**Part D: Hands-on rounds**
21. Coding Round (with working code)
22. SQL Round (with working queries)

**Part E**
23. Last-Minute Cheat Sheet

---

# 0. How to Answer

## 0.1 The 4-step answer: What → How → Example → Trade-off

This is the single most important page in the guide. Every technical answer should follow it.

| Step | What you say | Length |
|---|---|---|
| **What** | One plain sentence. No jargon. | 1 sentence |
| **How** | How it works under the hood. | 2–3 sentences |
| **Example** | Where you used it, or a concrete case. | 1–2 sentences |
| **Trade-off** | The downside, or when *not* to use it. | 1 sentence |

Then **stop**. Let them steer. A short answer that invites a follow-up beats a three-minute lecture.

### Textbook vs spoken: the same answer, two ways

**Question: "What is a closure?"**

❌ **Textbook (what the old guide sounded like):**
"A closure is the combination of a function bundled together with references to its surrounding state, the lexical environment. Closures are created every time a function is created, at function creation time."

✅ **Spoken (what you should sound like):**
> "A closure is when a function **remembers the variables from the place it was created**. *(What)*
> So even after the outer function has finished running, the inner function can still read and update those variables. *(How)*
> I use it all the time without thinking about it. Debounce is a closure, because it keeps the timer between calls. React hooks rely on it too. *(Example)*
> The one thing to watch is memory: if a closure holds onto a big object, that object can't be garbage-collected. *(Trade-off)*"

That took about 25 seconds, and the interviewer now has three things to ask about.

## 0.2 Golden rules

1. **Answer first, explain second.** The first sentence should already answer the question.
2. **Keep the first answer under 45 seconds.** Then pause. Silence after your answer is fine; it's their turn.
3. **Say "I" for your own work.** "We" hides what you did.
4. **Only use numbers you can defend.** If they ask "how did you measure that?", you need an answer.
5. **Always mention a trade-off.** Seniors say "it depends on X; in our case it was Y, so I chose Z."
6. **If you don't know, say so, then reason out loud.** Bluffing is the fastest way to lose trust.
7. **For scenario questions, never guess the cause.** Measure → isolate → fix → verify.
8. **In coding and design rounds, think out loud.** A silent candidate is impossible to score.

## 0.3 Phrase bank

**Starting an answer**
- "In one line: ___. The reason it matters is ___."
- "There are two parts to this, ___ and ___. Let me start with ___."
- "The short answer is ___. Want me to go into how it works?"

**Buying a few seconds**
- "Good question. Let me think about that for a second."
- "Just to make sure I answer the right thing, are you asking about ___ or ___?"

**When you don't know**
- "I haven't used that directly, but here's how I'd reason about it…"
- "I don't remember the exact API, but the idea is ___."
- "I don't know that one. If it came up at work, I'd start by checking ___."

**Sounding senior**
- "It depends on the access pattern. If it's read-heavy, ___. If it's write-heavy, ___."
- "Before changing anything, I'd want to measure ___."
- "The simplest thing that works is ___. I'd only add ___ if we actually hit ___."
- "The risk with that approach is ___, so I'd add ___ to protect against it."

**Disagreeing politely** (design rounds, code review questions)
- "That would work. One thing I'd watch out for is ___."
- "I'd lean the other way, because ___. But if ___ matters more, your approach is better."

**Wrapping up**
- "So to summarize: ___. Happy to go deeper on any part."
- "Does that answer it, or would you like me to go into ___?"

## 0.4 Red flags that make a senior sound junior

- Giving a definition but never saying **when you'd use it**.
- Saying "X is always better." There are no trade-offs in that sentence.
- Jumping straight to a fix in a scenario without saying how you'd **confirm the cause**.
- Criticizing your current or past employer.
- Bluffing an API detail. Interviewers usually know.
- Saying "we" for everything, so they can't tell what *you* did.
- A three-minute answer to a 20-second question.
- Never asking a clarifying question in system design.

## 0.5 Top 30 questions to prepare first

If you only have one evening, prepare these.

| # | Question | Where |
|---|---|---|
| 1 | Tell me about yourself | 1.1 |
| 2 | Your biggest technical challenge / performance win | Story Bank S1, S2 |
| 3 | Explain the event loop (JS and Node) | 2.1, 4.2 |
| 4 | Closures, and a practical use | 2.2 |
| 5 | Promises, async/await, `Promise.all` vs `allSettled` | 2.8 |
| 6 | Microtask vs macrotask, plus an output puzzle | 2.9 |
| 7 | How do you scale a Node.js API? | 4.11 |
| 8 | Streams and backpressure | 4.6 |
| 9 | REST vs GraphQL, and when to pick each | 6.3 |
| 10 | The N+1 problem and DataLoader | 6.4 |
| 11 | JWT, refresh tokens, and where to store them | 7.3–7.7 |
| 12 | XSS vs CSRF | 7.8, 7.9 |
| 13 | What causes a React re-render? How do you stop extra ones? | 9.2, 9.6 |
| 14 | When should you *not* use `useEffect`? | 9.4 |
| 15 | SSR vs SSG vs ISR vs CSR; Server vs Client Components | 10.1, 10.3 |
| 16 | How do you optimize a slow SQL query? | 11.4 |
| 17 | Indexes: composite, covering, when they don't help | 11.1–11.3 |
| 18 | Transactions and isolation levels | 11.5, 11.6 |
| 19 | Caching strategy, invalidation, stampede | 12.2–12.4 |
| 20 | Queues, at-least-once delivery, idempotent consumers, DLQ | 12.10–12.12 |
| 21 | Idempotency keys for payments/orders | 5.6 |
| 22 | Docker best practices; liveness vs readiness | 13.2, 13.8 |
| 23 | Zero-downtime deploys and DB migrations | 13.10, 11.17 |
| 24 | Design an e-commerce checkout / flash sale | 15.7 |
| 25 | Design a URL shortener or rate limiter | 15.4, 15.5 |
| 26 | "API latency jumped from 200 ms to 8 s." What do you do? | 17.2 |
| 27 | "Memory keeps growing in production." | 17.4 |
| 28 | Conflict with a teammate | 20.6 |
| 29 | A mistake you made | 20.7 |
| 30 | Why are you leaving? Salary? Questions for us? | 20.2, 20.13, 20.15 |


---

# 1. Your Story

## 1.1 "Tell me about yourself"

**Structure:** Present → Past → Proof → Future. About 60–90 seconds. This is not your resume read aloud. It's a trailer that makes them want to ask more.

> 🗣️ **Full version (~75 seconds)**
>
> "I'm a Senior Full Stack Engineer with about ten years of experience, currently based in Dubai.
>
> I started my career in PHP and enterprise e-commerce, mostly Magento. That gave me a really strong foundation in backend design, databases, and how commerce systems behave under real traffic. Over the last several years my focus has moved to **Node.js and TypeScript** on the backend and **React and Next.js** on the frontend.
>
> Right now I work on a multi-brand, multi-country e-commerce platform. A lot of my work is on the headless side: GraphQL APIs on the backend, a Next.js storefront on the front. I also work on integrations, asynchronous processing with Azure Service Bus and AWS SNS/SQS, and performance. For example, recently I cut our product page payload from about **4 MB to under 300 KB**, and took a product listing from over **1,800 database queries down to about 240** by batching resolvers.
>
> What I enjoy most is owning a feature end to end, from the data model and the API to the UI and how it behaves in production. I'm looking for a role where I can do that at a bigger scale and keep growing toward architecture and technical leadership."

> 🗣️ **Short version (~30 seconds)**, for when they say "briefly":
>
> "I'm a full stack engineer with about ten years in e-commerce. I started in PHP and Magento and now work mainly with Node.js, TypeScript, GraphQL, React and Next.js. Day to day I build headless commerce APIs and storefronts and do a lot of performance work. Recently I took a product page from 4 MB down to under 300 KB. I'm looking for a bigger-scale role where I can own systems end to end."

**Tips**
- End on **why this role**. It hands them an easy next question.
- Mention **one or two numbers** at most. They're hooks for follow-ups, so be ready to tell the full story.
- Don't list every technology. Name the ones in *their* job description.
- Tweak the last sentence for each company: "…which is why [their product/scale/domain] interests me."

---

## 1.2 Your Story Bank

Prepare **8 stories**. Each one can answer 5+ different questions. The interviewer asks "tell me about a time when…" and you pick the closest story.

> ⚠️ **Before you use these:** The details below come from your own recent work, with numbers that were measured. Re-read each one, check the numbers, and **only say what you can explain in depth**. Several measurements were taken on a local or test environment, so say "in our tests" rather than "in production" unless you've confirmed production numbers. Remove internal names (SKUs, module names) if you're unsure about confidentiality.

### S1: The 4 MB product page → 280 KB
**Use it for:** biggest challenge · performance win · React/Next.js deep dive · trade-offs · "tell me about something you're proud of"

- **Situation:** Our headless storefront is a Next.js App Router frontend on Magento GraphQL. For products with many finishes (one had 55 variants), the product page HTML was **3.96 MB** and loaded extremely slowly in testing.
- **Task:** Make it fast **without** breaking instant swatch switching. Clicking a colour had no network call, and the business liked that.
- **Action:**
  - Measured what was in the document instead of guessing. 3.83 MB of it was the **React Server Components payload** embedded in the HTML. The server page passed the full product, with every variant, as props into a Client Component, so all of it was serialized into the page.
  - Found that **94%** of the payload was `related_products`, which was repeated for **every** variant (about 50 rows × 55 variants).
  - Kept the variants, since they power instant swatch clicks, but removed related products from the initial query. Related products now load **after first paint**, only for the selected variant, and are cached per SKU.
  - On the backend, added an optional `skus` argument to the GraphQL field so the frontend can ask for only the children it needs. It's backward compatible.
  - **Deployed the backend before the frontend**, because a query using an unknown argument fails completely.
- **Result:** **3.96 MB → 0.28 MB, about 14× smaller.** Swatch clicks were still instant.
- **Lesson:** "In the App Router, anything you pass from a Server Component to a Client Component gets serialized into the page. You have to look at the payload, not only the JS bundle."

> 🗣️ "Our product pages were coming out at almost 4 MB of HTML for products with lots of finishes. Instead of guessing, I measured what was inside the document, and almost all of it was the React Server Components payload. We were passing the whole product with all 55 variants into a client component, and 94% of that was related products repeated for every variant. I kept the variants, because they made colour switching instant, but moved related products to load after first paint for just the selected variant. Page size went from about 4 MB to 280 KB, roughly 14 times smaller, and the swatches still switch instantly."

**Likely follow-ups:** Why not just paginate the variants? (Variants double as the swatch-to-SKU map, so removing them breaks instant switching.) How did you measure it? (Document size and the flight payload in the network tab, broken down per field.) What would you do differently? (Add a payload-size budget check in CI.)

---

### S2: 1,811 database queries → 238 (the N+1 fix)
**Use it for:** performance · GraphQL N+1 · database optimization · "how do you make sure you don't break things?"

- **Situation:** A GraphQL product listing (24 products) was slow. We had several custom resolvers (brand details, lifestyle images, downloads, swatches, variant options) and each one loaded its data **per product**.
- **Action:**
  - Counted the SQL queries per request: **1,811 SELECTs** for 24 products. That's a classic N+1, multiplied across several resolvers.
  - Replaced per-product loading with **batch loaders**. Each resolver registers the IDs it needs, then one query fetches them all for the request. This is the same idea as DataLoader in Node.
  - Verified the response was **byte-for-byte identical** before and after, so the optimization changed nothing functionally.
- **Result:** **1,811 → 238 queries**, warm response **~1.6 s → ~0.65 s** (measured in our test environment).
- **Lesson:** "With GraphQL, a resolver that looks innocent can run once per item. You have to look at query counts per request, not only slow queries."

> 🗣️ "We had a product listing endpoint in GraphQL that felt slow. I counted the database queries for one request with 24 products and it was over 1,800. Several custom resolvers were each loading their data product by product, which is a classic N+1. I rewrote them to batch: collect all the IDs first, then load them in one query per resolver, the DataLoader pattern. To make sure I didn't break anything, I compared the full response before and after, and it was byte-for-byte identical. Queries went from 1,811 to 238, and response time from about 1.6 seconds to 0.65."

---

### S3: Stores showing "open" on Dubai's clock
**Use it for:** attention to detail · debugging · API design · "a bug that was hard to spot"

- **Situation:** Our store locator API returns whether each physical store is open right now. There are 136 locations across many countries.
- **Problem:** It calculated "open now" using the **website's** timezone. So **26 of 136** locations were wrong at any given moment. For example, UK stores showed OPEN based on Dubai time while London was still closed.
- **Action:**
  - Changed the calculation to use **each location's own timezone**, derived from its country code via the standard timezone database, so there's no lookup table to maintain. Added an override hook for countries that span several timezones.
  - Replaced a boolean `is_open_now` with an enum: `OPEN / BREAK / CLOSED`. A boolean couldn't express "on a lunch break".
  - Verified across 133 locations in 22 countries.
  - Found one store whose break was entered as 00:00–16:00, which was clearly a data entry error. **Flagged it to the business** instead of coding around it.
- **Result:** The answer is now the same whichever site asks, and it's correct for every country.

> 🗣️ "Our store locator showed whether each store was open right now, but it used the website's timezone for the calculation. So on the Dubai site, UK stores showed as open hours before they actually opened. About 26 of 136 stores were wrong at any moment. I changed it to compute in each store's own timezone, derived from its country, and changed the field from a true/false to an OPEN, BREAK or CLOSED enum, because a boolean couldn't express a lunch break. I also found a store with a clearly mis-entered break window, and I sent that back to the business rather than hiding it in code."

---

### S4: Fixing the cause, not the symptom (missing product URLs)
**Use it for:** root-cause analysis · pushing back on the wrong fix · data quality · ownership

- **Situation:** About **128 products** had no SEO-friendly URL on some store views. The storefront fell back to ugly `/catalog/product/view/id/...` links and our headless frontend couldn't resolve them. The existing approach was a **cron job that regenerated URLs**.
- **Action:**
  - Ran a dry run first. **125 of the 128 could not be generated at all**, so the cron could never fix them. It was only ever fixing about 2%.
  - Root cause: those products had an **empty URL key**, so the platform derived the URL from the product **name**. A sibling product with the same name already owned that URL. The unique constraint failed, the transaction rolled back, and the product ended up with **zero** URLs. No visible error.
  - Built CLI tools: `report`, `diagnose`, and `fix-url-keys`, which backfills unique URL keys, all with a `--dry-run` mode. **Removed the cron**, which was wasted work.
  - Recommended the real upstream fix: the product import should always set a unique URL key.
- **Result:** **128 → 3** broken products, and the remaining 3 just needed a normal regeneration.

> 🗣️ "We had about 128 products with no proper URL, and the existing fix was a cron that kept regenerating URLs. Before changing anything I did a dry run, and 125 of the 128 couldn't be generated at all, so the cron was never going to work. The real cause was that these products had no URL key, so the system built the URL from the product name, and another product with the same name already had it. The insert failed silently. I built a small CLI to diagnose and backfill unique URL keys, with a dry-run mode, removed the cron, and recommended fixing it at the import. We went from 128 broken products to 3."

---

### S5: Bad upstream data crashing the API
**Use it for:** working with other teams · defensive design · "something outside your control"

- **Situation:** About **311 child products across ~144 configurable products** came from the import/PIM feed missing required attribute values, like size. Those null values **crashed our GraphQL resolvers**. The admin screen also silently hid these products, so the business thought they weren't linked at all.
- **Action:** Two tracks at once:
  1. **Protect the API:** a completeness check that treats an incomplete child as "not linked", filtering it out of variants and options. Healthy products pass through untouched, and it runs one query per request.
  2. **Fix the source:** produced an exact list of affected SKUs and handed it to the catalog/import team.
- **Result:** Product pages stopped crashing immediately, and the data team had a precise list to fix.

> 🗣️ "Our product pages started crashing for certain products. The root cause was in data we didn't own: about 300 child products came in from the import feed missing required attributes, and the nulls broke our resolvers. I did two things in parallel. First, I hardened the API so it treats an incomplete product as not linked, which means the page still works and just doesn't show that option. Second, I generated the exact list of affected products and gave it to the catalog team, because the real fix had to happen at the source. I didn't want code quietly hiding a data problem forever."

---

### S6: One character that broke every GraphQL query
**Use it for:** debugging · a mistake and what you learned · "a bug where the error was misleading"

- **Situation:** After adding documentation to a GraphQL schema file, **every** query on the endpoint failed with "Unterminated string". That included trivial unrelated queries.
- **Action:** The file parsed fine on its own, so I went one layer down into the framework's schema reader. It splits type definitions with a regular expression that **isn't aware of strings**, so a `{` inside a description cut the type definition short. Fixed by removing the brace, and wrote down the rule for the team.
- **Related lesson:** Another time, real errors were being **masked** by a library bug. Multibyte characters (decorative `──` lines) in query comments crashed the error formatter, so you saw a confusing `str_repeat()` error instead of the real one. Rule: if the error makes no sense, strip non-ASCII from the query and re-run.
- **Lesson:** "When a small change breaks everything and the error makes no sense, don't trust the error message. Bisect the change and check assumptions one layer down."

> ⚠️ Be honest about the impact. Was this caught locally or on staging, or did it reach production? Say exactly that.

---

### S7: Changing an API without breaking anyone
**Use it for:** API versioning · backward compatibility · release safety · security thinking

- **Situation:** The frontend needed details for one specific child product, but the existing GraphQL field returned **all** children.
- **Action:**
  - Added an **optional** `skus` argument. If omitted, the field behaves exactly as before, so existing clients are unaffected.
  - Restricted it to the parent's own children, so it **can't be abused** to fetch arbitrary hidden products.
  - Planned the release order: **backend first, then frontend**, because a client sending an unknown argument gets the whole query rejected.
- **Result:** Rolled out with no downtime and no client changes required.

> 🗣️ "When I need to change an API, I try to make it additive. Here, instead of a new endpoint or a version bump, I added an optional argument. Old clients don't send it and get exactly the same result. I also restricted it to the product's own children so nobody could use it to fetch hidden products. And I made sure the backend shipped before the frontend, because in GraphQL an unknown argument fails the whole query."

---

### S8: Silent wrong-page links (data drift)
**Use it for:** finding bugs nobody reported · data quality · SEO/e-commerce impact

- **Situation:** Category links were built from a stored `url_path` field.
- **Found:** **943 store/category pairs** where that stored path didn't match the real live URL. Worse, the stale path often still worked but pointed to a **different** category, so shoppers silently landed on the wrong page. No 404, no log, so nobody had reported it.
- **Action:** Switched to the URL rewrite table as the single source of truth, with a safe fallback, and compared per store to get an accurate count.

> 🗣️ "The scary bugs are the ones that don't throw errors. Our category links used a stored path that had drifted from the real URLs in 943 cases, and many of those old paths still worked but went to a different category. No 404, no log, nobody reported it. I switched to the actual rewrite table as the source of truth."

---

### Stories to prepare yourself (templates)

I don't have the details for these, so fill them in from your own experience. Interviewers *will* ask them.

**S9: Leadership / mentoring.** A time you guided a junior, led a feature, or set a standard.
- Who was it, and what were they struggling with? What did you do (pairing, reviews, documentation)? What changed (they shipped X alone, review time dropped)?

**S10: Conflict / disagreement.** A *technical* disagreement, never a personality clash.
- What was the disagreement? How did you get evidence (spike, benchmark, prototype)? What was decided? Did you support it even if it wasn't your idea?

**S11: Tight deadline / trade-off.** Something shipped under pressure.
- What did you cut, and why? What did you refuse to cut (tests, security)? How did you communicate the risk? What did you follow up on later?

**S12: Messaging / async system.** Your Azure Service Bus or AWS SNS/SQS work.
- What was decoupled, and why? How did you handle retries, duplicates, and dead letters? What went wrong once, and what did you change?

**S13: Production incident.** An outage or a serious bug in production.
- How was it detected? What did you do first to stop the damage? How did you find the cause? What did you add so it can't happen again?

---

## 1.3 Which story for which question?

| If they ask… | Use |
|---|---|
| Biggest technical challenge | S1 or S2 |
| A performance improvement | S1 (frontend) / S2 (backend) |
| A hard bug | S3 or S6 |
| Root-cause analysis / you questioned the obvious fix | S4 |
| Working with another team / something outside your control | S5 |
| A mistake you made | S6 (if it was your change), else S13 |
| Backward compatibility / API design / safe releases | S7 |
| Something you found that nobody asked you to find | S8 or S4 |
| Proud of / impact | S1 |
| Leadership / mentoring | S9 |
| Conflict | S10 |
| Pressure / deadline | S11 |
| Distributed systems / queues | S12 |
| Production incident / on-call | S13 or S6 |

---

# 2. JavaScript Fundamentals

### 2.1 How does JavaScript run your code? (call stack, heap, event loop)

> 🗣️ "JavaScript runs your code on **one thread with one call stack**, so it does one thing at a time. Each function call goes onto the stack, and comes off when it returns. Objects live in a separate memory area called the heap.
> When it hits something slow, like a network call or a timer, it doesn't sit and wait. The browser or Node handles that in the background. When it's done, its callback goes into a queue. The **event loop**'s job is simple: when the stack is empty, take the next callback from the queue and run it. That's how a single thread can handle thousands of requests."

💡 **Picture it:** One chef who never stands watching the oven. They put the dish in, cook the next order, and come back when the timer rings.

❓ **Follow-ups**
- *What is an execution context?* The "environment" a piece of code runs in: its variables, its `this`, and its outer scope. A global context is created first, then a new one for every function call.
- *What causes a stack overflow?* Too many nested calls, usually recursion with no base case. The stack has a fixed size.
- *Stack vs heap?* The stack holds function frames and is fast but small. The heap holds objects, is large, and is managed by the garbage collector.

⚠️ **Trap:** Saying "JavaScript is multi-threaded." *Your* code runs on one thread. The runtime (browser or libuv in Node) uses other threads for I/O.

---

### 2.2 Scope, lexical scope, and closures

> 🗣️ "**Scope** is where a variable is visible. JavaScript uses **lexical scope**, which means visibility is decided by where the code is *written*, not where it's called from.
> A **closure** is a function that remembers the variables from where it was created, even after the outer function has returned.
> I use closures constantly: debounce keeps its timer in a closure, React hooks depend on them, and they're the easiest way to keep private state. The downside is that a closure holding a big object keeps it alive in memory."

```javascript
function createCounter() {
  let count = 0;              // private, no one outside can touch it
  return () => ++count;       // this arrow function "closes over" count
}
const next = createCounter();
next(); // 1
next(); // 2
```

❓ **Follow-ups**
- *Practical uses?* Private state, function factories, memoization, debounce/throttle, event handlers, partial application.
- *Can closures leak memory?* Yes. A long-lived callback (an event listener or interval) that closes over a large object keeps that object reachable. Fix it by removing the listener or not capturing what you don't need.
- *Classic loop bug?* With `var` in a `for` loop plus `setTimeout`, all callbacks see the final value, because they share one variable. `let` creates a new binding per iteration. (See 21.13.)

---

### 2.3 Hoisting and the temporal dead zone (TDZ)

> 🗣️ "Before running code, JavaScript registers all declarations in their scope. That's hoisting.
> **Function declarations** are fully hoisted, so you can call them before they appear. **`var`** is hoisted and set to `undefined`. **`let`, `const` and classes** are hoisted too, but you can't touch them until the line where they're declared. That gap is the **temporal dead zone**, and accessing them there throws a `ReferenceError`."

❓ **Follow-ups**
- *Why does `typeof x` throw for a `let` variable?* If `x` is in its TDZ, even `typeof` throws. For a variable that was never declared at all, `typeof` safely returns `"undefined"`.
- *Function declaration vs expression?* `function foo() {}` is hoisted with its body. `const foo = () => {}` follows `const` rules, so it's in the TDZ until that line.

---

### 2.4 `var` vs `let` vs `const`

> 🗣️ "`var` is **function-scoped**, can be redeclared, and starts as `undefined`. `let` and `const` are **block-scoped** and have a TDZ. `const` means you can't **reassign** the variable, but if it holds an object, you can still change what's inside. In modern code I use `const` by default, `let` when I need to reassign, and never `var`."

❓ **Follow-ups**
- *Can a const object be modified?* Yes. `const user = {}; user.name = 'A'` works. Use `Object.freeze` to stop that, though it's only shallow.
- *Why avoid `var`?* It leaks out of blocks, allows silent redeclaration, and causes the loop-closure bug.

---

### 2.5 How does `this` work?

> 🗣️ "`this` depends on **how a function is called**, not where it's written. There are four rules:
> - Called as a method, `obj.fn()`: `this` is `obj`.
> - Called with `new`: `this` is the new object.
> - Called with `call`, `apply` or `bind`: `this` is whatever you pass.
> - Called plainly, `fn()`: `this` is `undefined` in strict mode.
>
> **Arrow functions** are the exception. They don't have their own `this`; they take it from the surrounding code. That's why they're great for callbacks and bad as object methods."

```javascript
const user = {
  name: 'Salman',
  regular() { return this.name; },     // 'Salman'
  arrow: () => this?.name,            // undefined, takes `this` from outside the object
};
const fn = user.regular;
fn(); // undefined (strict mode): lost the object, so `this` is gone
```

❓ **Follow-ups**
- *Arrow function as an object method?* Usually a bug, because `this` won't be the object.
- *Why do we lose `this` when passing a method as a callback?* Because it's called later as a plain function. Fix it with `.bind(obj)` or an arrow wrapper.

---

### 2.6 `call` vs `apply` vs `bind`

> 🗣️ "All three set `this`. `call` runs the function immediately with arguments listed one by one. `apply` runs it immediately with arguments as an **array**. `bind` doesn't run it; it **returns a new function** with `this` locked in. A memory trick: **A**pply = **A**rray, **B**ind = **B**ack later."

```javascript
greet.call(user, 'Hi', '!');
greet.apply(user, ['Hi', '!']);
const later = greet.bind(user, 'Hi'); later('!');
```

---

### 2.7 Prototypes and classes

> 🗣️ "Every object has a hidden link to another object, its **prototype**. When you read a property that isn't on the object, JavaScript walks up this chain until it finds it or reaches `null`.
> **Classes are just nicer syntax** over this. Methods you write in a class live once on the prototype and are shared by all instances, not copied into each one."

❓ **Follow-ups**
- *`prototype` vs `__proto__`?* `Foo.prototype` is the object that instances created with `new Foo()` will link to. `obj.__proto__` (properly `Object.getPrototypeOf(obj)`) is the link itself.
- *Own vs inherited property?* `Object.hasOwn(obj, 'x')` checks only the object itself. `'x' in obj` also checks the chain.
- *Private fields?* `#field` in classes is truly private, enforced by the language rather than by naming convention.

---

### 2.8 Promises and async/await

> 🗣️ "A **Promise** is a placeholder for a value that arrives later. It's **pending**, then either **fulfilled** or **rejected**. **async/await** is just cleaner syntax over Promises. `await` pauses only that one async function and lets everything else keep running.
> The most common mistake I see is awaiting things **one by one** when they don't depend on each other. If two calls are independent, I start both and use `Promise.all`."

```javascript
// ❌ Sequential: ~2x slower
const user = await getUser(id);
const orders = await getOrders(id);

// ✅ Parallel
const [user, orders] = await Promise.all([getUser(id), getOrders(id)]);
```

**The four combinators: know these cold**

| Method | Resolves when | Rejects when | Use it for |
|---|---|---|---|
| `Promise.all` | **all** succeed | **any one** fails (fail fast) | Independent calls you need all of |
| `Promise.allSettled` | **all** finish (either way) | never | Batch jobs where you want every result |
| `Promise.race` | **first** to settle | first to settle with an error | Timeouts |
| `Promise.any` | **first success** | **all** fail | Fastest of several mirrors |

❓ **Follow-ups**
- *Does `Promise.all` cancel the others when one fails?* No. The others keep running; you just stop waiting for them. Use `AbortController` if you need real cancellation.
- *What happens when an async function throws?* It returns a rejected Promise. If nobody catches it, you get an `unhandledRejection` (which crashes Node by default since v15).
- *How do you limit concurrency?* A small worker pool; see 21.3. Libraries: `p-limit`.
- *Timeout pattern?* `Promise.race([work, timeout(5000)])`, or better, pass an `AbortSignal.timeout(5000)` to `fetch`.

---

### 2.9 Microtasks vs macrotasks

> 🗣️ "There are two main queues. **Microtasks** are Promise callbacks and `queueMicrotask`. **Macrotasks** are things like `setTimeout`, I/O, and events.
> The rule: after each macrotask, the engine **drains the entire microtask queue** before moving on. So Promise callbacks always run before the next timer. In Node there's an even earlier queue, `process.nextTick`, which runs before Promise callbacks."

**Output puzzle (very common):**

```javascript
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
process.nextTick(() => console.log('4'));
console.log('5');
// Node (CommonJS): 1, 5, 4, 3, 2
```

How to explain it: "1 and 5 are synchronous. Then the stack is empty, so Node runs nextTick (4), then microtasks (3), then the timer phase (2)."

❓ **Follow-ups**
- *Can microtasks starve the event loop?* Yes. A Promise chain that keeps queuing new microtasks blocks timers and I/O forever.
- *Small gotcha:* In an **ES module**, the output above is `1, 5, 3, 4, 2`, because module code itself runs inside a Promise job. Mentioning this is a strong senior signal, but only if they push.

---

### 2.10 Error handling with async code

> 🗣️ "With async/await I use normal `try/catch`. The important rules: every Promise must be either awaited or have a `.catch`, otherwise errors disappear or crash the process. I catch errors at the boundary where I can actually *do* something, like return an HTTP error or retry, not at every line."

❓ **Follow-ups**
- *`return await` vs `return`?* Inside a `try` block, `return await promise` lets the `catch` handle a rejection. A plain `return promise` skips your catch.
- *Fire-and-forget?* Only with an explicit `.catch(log)`. Otherwise it's a silent failure waiting to happen.

---

### 2.11 `==` vs `===`, and type coercion

> 🗣️ "`===` compares **value and type** with no conversion. `==` converts types first, which gives weird results like `'' == 0` being true. I always use `===`. The one accepted exception is `x == null`, which checks for both `null` and `undefined` in one go."

❓ **Follow-ups**
- *`NaN === NaN`?* `false`. Use `Number.isNaN()`.
- *`typeof null`?* `"object"`. It's a historic bug.
- *`null` vs `undefined`?* `undefined` means "not set yet"; `null` means "intentionally empty".

---

### 2.12 Primitive vs reference types

> 🗣️ "Primitives (string, number, boolean, null, undefined, symbol, bigint) are copied **by value**. Objects, arrays and functions are passed **by reference to the same object**. So if you pass an object into a function and mutate it, the caller sees the change. That's why I prefer returning new objects instead of mutating inputs, especially in React."

---

### 2.13 Shallow copy vs deep copy

> 🗣️ "A **shallow** copy, like spread `{...obj}`, copies only the top level. Nested objects are still shared. A **deep** copy duplicates everything. Today I use **`structuredClone`**, which is built in and handles Dates, Maps, Sets and circular references. `JSON.parse(JSON.stringify())` is the old trick, but it loses Dates, `undefined`, functions, and breaks on cycles."

❓ **Follow-ups**
- *Why does it matter in React?* React compares by reference. If you mutate a nested object, the reference doesn't change and React may not re-render.
- *What can't `structuredClone` copy?* Functions, DOM nodes, and class prototypes (you get a plain object back).

---

### 2.14 Debounce vs throttle

> 🗣️ "**Debounce** waits until the calls **stop** for a while, then runs once. That's perfect for a search box: search only after the user stops typing for 300 ms. **Throttle** runs **at most once per interval** no matter how many calls come in. That's for scroll, resize, or tracking events."

💡 **Picture it:** Debounce is a lift door that waits until people stop walking in. Throttle is a bus that leaves every 10 minutes no matter what.

Code for both: see 21.1 and 21.2.

❓ **Follow-ups**
- *Leading vs trailing?* Trailing runs after the pause (the usual case). Leading runs immediately on the first call, then ignores calls until the pause.
- *How do you cancel a debounce?* Expose a `cancel()` that calls `clearTimeout`, for example when a React component unmounts.

---

### 2.15 `map`, `filter`, `reduce`, and immutability

> 🗣️ "`map` transforms every item, `filter` keeps some items, `reduce` boils a list down to one value. None of them mutate the original array. I use `reduce` for simple sums and grouping, but if a reduce gets complicated, a plain `for…of` loop is easier for the next person to read."

❓ **Follow-ups**
- *Which array methods mutate?* `push`, `pop`, `splice`, `sort`, `reverse`, `fill`. The newer `toSorted`, `toReversed`, `toSpliced` and `with` return copies instead.
- *Chaining cost?* Each step creates a new array. That's fine for normal sizes; for very large arrays a single loop is faster.

---

### 2.16 CommonJS vs ES Modules

> 🗣️ "**CommonJS** is Node's original system: `require` and `module.exports`, loaded synchronously at runtime. **ES Modules** are the standard: `import` and `export`. Because imports are static, tools can analyse them and **tree-shake** unused code. ESM is what I use for new code.
> Node decides which one a file is from the extension (`.mjs` / `.cjs`) or the `"type"` field in package.json."

❓ **Follow-ups**
- *Default vs named exports?* I prefer named exports. They're easier to search for, refactor and auto-import.
- *`__dirname` in ESM?* It doesn't exist. Use `import.meta.dirname` (recent Node) or derive it from `import.meta.url`.
- *Can CommonJS load ESM?* Historically only through dynamic `import()`. Recent Node versions can `require()` most ES modules synchronously.

---

### 2.17 Garbage collection and memory leaks

> 🗣️ "V8 automatically frees objects that nothing can reach anymore. So a **memory leak** in JavaScript means something is still *holding a reference* to data you don't need. The usual suspects are a cache with no size limit, a global array that keeps growing, event listeners that are never removed, timers that are never cleared, or closures holding big objects."

❓ **Follow-ups**
- *WeakMap use case?* Attaching extra data to an object without keeping it alive, for example caching metadata per request object. When the object is gone, the entry disappears automatically.
- *How do you find a leak?* See 4.14 and 17.4: heap snapshots, compare, follow the retainer path.

---

### 2.18 Generators and iterators (brief)

> 🗣️ "An **iterator** is anything with a `next()` method, which is what `for…of` uses. A **generator** (`function*`) is an easy way to write one: it pauses at each `yield`. They're useful for lazy sequences, such as processing a large dataset page by page without loading it all. **Async generators** with `for await` are great for streaming paginated API results."

```javascript
async function* allOrders(api) {
  let cursor;
  do {
    const page = await api.list({ cursor });
    yield* page.items;
    cursor = page.nextCursor;
  } while (cursor);
}
for await (const order of allOrders(api)) process(order);
```

---

### ⚡ JavaScript rapid-fire

| Question | One-line answer |
|---|---|
| `?.` vs `??` | Optional chaining stops if the value is null/undefined; nullish coalescing gives a default only for null/undefined (not `0` or `''`, unlike `\|\|`). |
| Arrow vs normal function | Arrows have no own `this`, `arguments`, or `prototype`, and can't be used with `new`. |
| `Object.freeze` | Makes an object read-only, but only shallowly. |
| Set vs Array | Set has unique values and fast `has()`; Array keeps order and allows duplicates. |
| Map vs Object | Map allows any key type, keeps insertion order, has `size`, and is better for frequent add/remove. |
| Pure function | Same input → same output, with no side effects. Easy to test. |
| Currying | Turning `f(a, b)` into `f(a)(b)`, which is useful for partial application. |
| IIFE | A function that runs immediately, `(() => {...})()`. It was used for scope before modules existed. |
| Symbol | A unique primitive, used for hidden or non-colliding object keys. |
| `Object.groupBy` | Built-in grouping (ES2024): `Object.groupBy(items, i => i.type)`. |
| Event loop in one sentence | "Run sync code, drain microtasks, take the next macrotask, repeat." |
| Truthy/falsy | Falsy: `false, 0, -0, 0n, '', null, undefined, NaN`. Everything else is truthy, including `[]` and `{}`. |

---

# 3. TypeScript

### 3.1 Why TypeScript? Does it validate data at runtime?

> 🗣️ "TypeScript adds **types** to JavaScript, so a whole class of bugs gets caught **before the code runs**: typos, wrong arguments, missing null checks. The bigger win for me is **safe refactoring**. Rename a field and the compiler shows every place that breaks.
> But the types are **erased** at build time. So TypeScript does **not** validate data coming from outside: API requests, third-party responses, environment variables. For those I use a runtime validator like **Zod**, and derive the TypeScript type from the schema so there's one source of truth."

```typescript
import { z } from 'zod';

const CreateOrder = z.object({
  sku: z.string().min(1),
  qty: z.number().int().positive(),
});
type CreateOrder = z.infer<typeof CreateOrder>; // type comes from the schema

const input = CreateOrder.parse(req.body); // throws if invalid, typed if valid
```

⚠️ **Trap:** "TypeScript makes my API safe." Types don't exist at runtime. Without validation, `req.body` can be anything.

---

### 3.2 `interface` vs `type`

> 🗣️ "Both describe the shape of an object, and for most app code either is fine. Pick one and be consistent. The differences: **`type`** can also express unions, tuples, primitives and mapped types. **`interface`** can be extended with `extends` and supports **declaration merging**, where two interfaces with the same name merge. My default is `interface` for object shapes and `type` for unions and anything computed."

❓ **Follow-ups**
- *Can an interface be a union?* No. `type Status = 'paid' | 'failed'` has to be a type.
- *What is declaration merging?* Declaring `interface Window { myVar: string }` adds to the existing `Window` type. It's used to extend library types, like adding `user` to Express's `Request`.

---

### 3.3 `any` vs `unknown` vs `never` vs `void`

> 🗣️ "**`any`** switches type checking off. It's an escape hatch, and it spreads. **`unknown`** means 'could be anything, but you must check before using it', which makes it the safe version of `any`. I use it for caught errors and parsed JSON. **`never`** means a value that can't exist: a function that always throws, or the leftover case in an exhaustive switch. **`void`** means a function returns nothing useful."

```typescript
function assertNever(x: never): never {
  throw new Error(`Unhandled case: ${JSON.stringify(x)}`);
}
```

---

### 3.4 Generics

> 🗣️ "Generics let you write one function or type that works with many types while **keeping the type information**. Without them you'd either duplicate code or fall back to `any`. Typical uses are API response wrappers, repositories, and reusable components."

```typescript
function first<T>(items: T[]): T | undefined {
  return items[0];
}
const n = first([1, 2, 3]);   // n: number | undefined

// Constraint: T must have an id
function byId<T extends { id: string }>(items: T[], id: string): T | undefined {
  return items.find(i => i.id === id);
}

interface ApiResponse<T> { data: T; error?: string }
```

❓ **Follow-ups**
- *Generic constraints?* `T extends Something` limits what T can be, so you can safely use its properties.
- *Default type parameters?* `interface Page<T = unknown>`.

---

### 3.5 Unions, intersections, and discriminated unions

> 🗣️ "A **union** (`A | B`) means one *or* the other. An **intersection** (`A & B`) means both at once. The most useful pattern is a **discriminated union**: every member shares a literal field like `status` or `type`. When you check that field, TypeScript knows exactly which shape you have. Add a `never` check and the compiler forces you to handle every case."

```typescript
type Payment =
  | { status: 'pending' }
  | { status: 'paid'; transactionId: string }
  | { status: 'failed'; reason: string };

function label(p: Payment) {
  switch (p.status) {
    case 'pending': return 'Waiting…';
    case 'paid':    return `Paid (${p.transactionId})`; // TS knows transactionId exists
    case 'failed':  return `Failed: ${p.reason}`;
    default:        return assertNever(p); // compile error if a new status is added
  }
}
```

🏢 **From my work:** I used the same idea when I replaced `is_open_now: boolean` with `OPEN | BREAK | CLOSED` (Story S3). A boolean couldn't represent the third state.

---

### 3.6 Narrowing: type guards vs type assertions

> 🗣️ "A **type assertion** (`value as User`) tells the compiler 'trust me'. It doesn't check anything at runtime. A **type guard** is a real check, like `typeof`, `instanceof`, `in`, or a custom function, and TypeScript narrows the type after it. For anything coming from outside, I use guards or schema validation, never assertions."

```typescript
function isUser(x: unknown): x is User {
  return typeof x === 'object' && x !== null && 'id' in x && 'email' in x;
}
```

❓ **Follow-ups**
- *What is `value is Type`?* A type predicate. It's the return type of a custom guard.
- *Non-null assertion `!`?* `user!.name` tells TS "not null". Use it sparingly; it's a promise you might break.

---

### 3.7 Utility types

> 🗣️ "Utility types build new types from existing ones, so you don't duplicate definitions. The ones I use most: `Partial` for update payloads, `Pick` and `Omit` for API views of a model, `Record` for dictionaries, `ReturnType` and `Parameters` for inferring from functions, and `Readonly` for config."

| Utility | What it does | Example use |
|---|---|---|
| `Partial<T>` | All fields optional | PATCH body |
| `Required<T>` | All fields required | Validated config |
| `Readonly<T>` | No reassignment | Constants |
| `Pick<T, K>` / `Omit<T, K>` | Keep / remove fields | `Omit<User, 'password'>` for responses |
| `Record<K, V>` | Object with keys K | `Record<Locale, string>` |
| `Exclude` / `Extract` | Filter a union | `Exclude<Status, 'deleted'>` |
| `NonNullable<T>` | Remove null/undefined | |
| `ReturnType<F>` / `Parameters<F>` | Infer from a function | Typing wrappers |
| `Awaited<T>` | Unwrap a Promise | `Awaited<ReturnType<typeof fetchUser>>` |

❓ **Follow-ups**
- *When is `Partial` dangerous?* When you use it for a type that really needs certain fields. You lose the guarantee and push null checks everywhere.
- *How is `Partial` built?* As a mapped type: `type Partial<T> = { [K in keyof T]?: T[K] }`.

---

### 3.8 `keyof`, `typeof`, indexed access, mapped and conditional types

> 🗣️ "`keyof T` gives a union of a type's keys. `typeof value`, used in a type position, gets the type of a variable. `T['field']` reads one property's type. **Mapped types** loop over keys to transform a type, and **conditional types** (`T extends X ? A : B`) choose a type based on a condition. Together they let you derive types instead of writing them twice."

```typescript
const config = { region: 'me-central-1', retries: 3 };
type Config = typeof config;            // { region: string; retries: number }
type ConfigKey = keyof Config;          // 'region' | 'retries'
type Retries = Config['retries'];       // number
type Unwrap<T> = T extends Promise<infer U> ? U : T;  // `infer` extracts a type
```

---

### 3.9 `as const` and `satisfies`

> 🗣️ "`as const` makes a value deeply readonly and keeps its **literal** types, so `'paid'` stays `'paid'` instead of widening to `string`. **`satisfies`** checks that a value matches a type **without** changing its inferred type. So you get validation and you keep the precise literal types. I use both for config objects and lookup tables."

```typescript
const STATUSES = ['pending', 'paid', 'failed'] as const;
type Status = typeof STATUSES[number]; // 'pending' | 'paid' | 'failed'

const routes = {
  home: '/',
  product: '/p/:slug',
} satisfies Record<string, string>;     // checked, but routes.home is still known
```

---

### 3.10 Enums: use them or avoid them?

> 🗣️ "I usually prefer a **string union** or an `as const` object over `enum`. Unions produce no runtime code, work naturally with JSON, and are simpler. Regular enums generate a runtime object, and numeric enums have surprising reverse-mapping behaviour. I'd use an enum only if the team already standardises on them."

---

### 3.11 Strict mode and important tsconfig options

> 🗣️ "I always turn on `strict`. It enables `strictNullChecks`, `noImplicitAny` and a few others, and that's where most of the value of TypeScript comes from. I also like `noUncheckedIndexedAccess`, which makes `arr[0]` possibly `undefined`, because that's actually true at runtime."

❓ **Follow-ups**
- *Migrating a JS codebase?* Turn on `allowJs`, convert file by file starting from the leaf modules, and tighten strictness gradually. Don't do a big-bang rewrite.

---

### 3.12 How do you keep types in sync between frontend and backend?

> 🗣️ "I never hand-copy types between projects, because they drift. Options, from simplest:
> 1. A **shared package** in a monorepo with Zod schemas, used for validation on the server and types on the client.
> 2. **Generate** client types from the API contract: OpenAPI codegen for REST, GraphQL Code Generator for GraphQL.
> 3. An end-to-end typed RPC like tRPC, if both sides are TypeScript and owned by the same team.
>
> For GraphQL I'd always use codegen. The schema is already the contract."

---

### 3.13 Decorators

> 🗣️ "Decorators are functions that attach behaviour or metadata to classes, methods or properties, like `@Controller()` or `@Injectable()` in NestJS, or column mappings in TypeORM. They're powerful but they **hide** behaviour, so I use them only where the framework expects them. TypeScript 5 supports the standard decorator spec. Older frameworks still use the legacy `experimentalDecorators` version."

---

### ⚡ TypeScript rapid-fire

| Question | One-line answer |
|---|---|
| Structural typing? | TS compares types by **shape**, not name. Two different interfaces with the same fields are compatible. |
| `.d.ts` files? | Type declarations only, with no code. They describe JS libraries to TS. |
| `readonly` vs `const`? | `const` is for variables; `readonly` is for properties. |
| `optional?` vs `\| undefined` | `x?: T` means the key may be missing; `x: T \| undefined` means the key must exist but can be undefined. |
| Abstract class vs interface? | An abstract class can contain shared implementation and exists at runtime; an interface is a type-only contract. |
| Function overloads? | Multiple call signatures for one implementation, used when the return type depends on the input. |
| `unknown` in `catch`? | With `useUnknownInCatchVariables` (part of strict), `catch (e)` is `unknown`. Narrow with `e instanceof Error`. |
| Can Node run TS directly now? | Recent Node versions can strip types and run `.ts` files, but they don't type-check. You still run `tsc` in CI. |

---

# 4. Node.js Core and Production Engineering

### 4.1 What is Node.js, and what is it good (and bad) at?

> 🗣️ "Node.js is a JavaScript runtime built on Chrome's V8 engine plus a library called **libuv** that handles asynchronous I/O. Your JavaScript runs on one main thread, but I/O like database calls, HTTP requests and files happens in the background without blocking.
> That makes Node excellent for **I/O-heavy** work: APIs, BFFs, real-time apps, gateways, anything that mostly waits on other systems. It's weaker for **CPU-heavy** work like image processing or big calculations, because that blocks the one thread for every user. For that I'd use worker threads, a queue with separate workers, or a different service."

---

### 4.2 Explain the Node.js event loop

> 🗣️ "The event loop is a loop that runs callbacks in **phases**. Each turn it goes:
> 1. **Timers**: callbacks from `setTimeout` and `setInterval` whose time is up.
> 2. **Pending callbacks**: some deferred system callbacks.
> 3. **Poll**: waits for and runs I/O callbacks. This is where most of your code runs.
> 4. **Check**: `setImmediate` callbacks.
> 5. **Close**: close events, like a socket closing.
>
> Between every callback, Node drains `process.nextTick` first, then Promise microtasks. The one rule that matters in production: **if any callback takes a long time, everything else waits**, including every other user's request."

💡 **Picture it:** A receptionist going round the same desks in order: timers, mail, phones, close-up. They never skip a desk, but if one visitor talks for ten minutes, the whole office queues.

❓ **Follow-ups**
- *`setImmediate` vs `setTimeout(fn, 0)`?* From the main module the order isn't guaranteed. Inside an I/O callback, `setImmediate` always runs first, because the check phase comes right after poll.
- *Where do microtasks run?* After each individual callback, not just between phases (since Node 11).

---

### 4.3 What uses the libuv thread pool?

> 🗣️ "Network sockets **don't** use the thread pool. They use the operating system's async mechanisms (epoll, kqueue, IOCP), which is why Node can hold thousands of connections. The **thread pool** is used for things the OS can't do asynchronously: most **file system** operations, **crypto** like `pbkdf2` and `scrypt`, **zlib** compression, and **`dns.lookup`**.
> The default pool size is **4**. If you're doing lots of hashing or file work in parallel, those calls queue behind each other. You can raise `UV_THREADPOOL_SIZE`, but usually the better fix is to move that work off the request path."

---

### 4.4 Why can `process.nextTick` be dangerous?

> 🗣️ "Node runs the **entire** nextTick queue before moving on. If a nextTick callback schedules another nextTick, and so on recursively, the event loop never reaches timers or I/O. It's **starved**. I almost never use it in app code. `queueMicrotask` or `setImmediate` are safer for deferring work."

---

### 4.5 How do you know if the event loop is blocked, and how do you fix it?

> 🗣️ "The symptom is that **all** endpoints get slow at the same time, even simple health checks, while CPU on that instance is high. I measure **event-loop lag** (Node has `perf_hooks.monitorEventLoopDelay()`, and most APMs show it), then take a **CPU profile** to find the hot function.
> Typical culprits: big `JSON.parse`/`stringify`, synchronous crypto or `fs.*Sync`, a catastrophic **regex**, sorting or looping over huge arrays, or heavy template rendering.
> Fixes, in order: make the work smaller (paginate, stream), make it async, move it to a **worker thread**, or move it to a **background job**."

---

### 4.6 Streams and backpressure

> 🗣️ "Streams let you process data **in chunks** instead of loading it all into memory. A 5 GB file never needs 5 GB of RAM. There are four types: **Readable**, **Writable**, **Duplex** (both, like a socket), and **Transform** (modifies data as it passes, like gzip).
> **Backpressure** is what happens when the producer is faster than the consumer. If you ignore it, data piles up in memory. When `write()` returns `false`, you should stop and wait for the `drain` event. In practice I just use **`stream.pipeline()`**: it handles backpressure, forwards errors, and cleans up all the streams if one fails."

```javascript
import { pipeline } from 'node:stream/promises';
import { createGzip } from 'node:zlib';

app.get('/export.csv.gz', async (req, res) => {
  res.setHeader('Content-Type', 'application/gzip');
  await pipeline(
    db.streamQuery('SELECT * FROM orders'), // Readable
    toCsv(),                                 // Transform
    createGzip(),                            // Transform
    res                                      // Writable
  );
});
```

💡 **Picture it:** Filling a bucket from a hose. Backpressure is turning the tap down when the bucket's nearly full, instead of flooding the floor.

❓ **Follow-ups**
- *`highWaterMark`?* The internal buffer size threshold. When the buffer passes it, the stream signals "slow down".
- *Flowing vs paused mode?* Flowing pushes data via `'data'` events; paused mode means you pull with `read()`. Piping handles this for you.
- *Why `pipeline` instead of `.pipe()`?* `.pipe()` doesn't forward errors or destroy the other streams on failure, so you can leak file handles.

🏢 **From my work:** Large exports and imports. Streaming rows straight to the response instead of building one giant array (see 17.14).

---

### 4.7 Buffer

> 🗣️ "A Buffer is a fixed-size chunk of **raw binary memory**, outside the normal JS heap. Node uses it for files, network data, crypto and encodings. I create them with `Buffer.from()` or `Buffer.alloc()`. `allocUnsafe` is faster but may contain old memory, so only use it if you overwrite every byte."

❓ **Follow-ups**
- *Buffer vs string?* A string is text in UTF-16; a Buffer is raw bytes. Converting needs an encoding (`'utf8'`, `'base64'`, `'hex'`).
- *Encoding bugs?* Splitting a UTF-8 Buffer in the middle of a multi-byte character gives garbage. Use `StringDecoder` or `setEncoding` on streams.

---

### 4.8 EventEmitter

> 🗣️ "EventEmitter is Node's built-in **publish/subscribe inside one process**. You `emit` a named event and listeners registered with `on` or `once` get called synchronously, in order. Streams, HTTP servers and sockets are all EventEmitters.
> Two things to remember: always handle the special **`'error'` event**, because an unhandled one crashes the process. And it's **in-memory only**. It's not a replacement for a real message broker across services."

❓ **Follow-ups**
- *"MaxListenersExceededWarning"?* Usually a leak: you're adding a listener on every request and never removing it.
- *EventEmitter vs Service Bus/Kafka?* EventEmitter is one process, non-durable and lost on crash. A broker is durable, works across services, and supports retries.

---

### 4.9 Worker threads vs child processes vs cluster

> 🗣️ "They solve different problems:
> - **Worker threads**: parallel JavaScript **inside the same process**. Use them for CPU-heavy work like image resizing, PDF generation or big parsing. They're cheap to communicate with and can share memory.
> - **Child processes**: a **separate process** with its own memory. Use them to run another program (ffmpeg, a Python script) or for strong isolation, so if it crashes, your server doesn't.
> - **Cluster**: runs **several copies of your server** to use all CPU cores, sharing one port.
>
> In Kubernetes I usually **don't** use cluster. I run one Node process per container and let Kubernetes scale the number of pods. It's simpler and the orchestrator handles restarts."

❓ **Follow-ups**
- *When to prefer a queue over a worker thread?* When the work can be done later, needs retries, or is big enough that it should scale separately from the API.

---

### 4.10 `spawn` vs `exec` vs `execFile` vs `fork`

> 🗣️ "`spawn` **streams** output and is best for long-running or large-output commands. `exec` runs through a **shell** and **buffers** output, which is convenient but risky with user input and limited by `maxBuffer`. `execFile` runs a binary **directly**, without a shell, and is safer. `fork` starts another **Node** script with a built-in message channel.
> Security rule: **never** put user input into `exec`. Use `execFile` or `spawn` with an arguments array so there's no shell injection."

---

### 4.11 How do you scale a Node.js API?

> 🗣️ "I think about it in layers:
> 1. **Make instances stateless**: no sessions or uploads on local disk or memory. Shared state goes to Redis, the database, or object storage.
> 2. **Scale horizontally**: multiple instances behind a load balancer, autoscaled on CPU, latency or request count.
> 3. **Take load off the database**: indexes, caching hot reads in Redis, read replicas, connection pooling.
> 4. **Move slow work out of the request**: queues and background workers for emails, exports, image processing.
> 5. **Cache at the edge**: a CDN for static assets and cacheable responses.
> 6. **Observe**: p95/p99 latency, error rate, event-loop lag, DB pool usage, queue depth.
>
> Most of the time the bottleneck turns out to be the **database**, not Node."

❓ **Follow-ups**
- *Sticky sessions?* Avoid them where possible because they break even load distribution. Needed only for things like WebSocket fallbacks; better to keep state in Redis.
- *Horizontal vs vertical?* Vertical means a bigger machine: simple, but it has a ceiling and is a single point of failure. Horizontal means more machines: needs statelessness, but scales further and survives failures.

---

### 4.12 Error handling in Node.js

> 🗣️ "I split errors into two kinds. **Operational errors** are expected failures: invalid input, not found, a timeout calling another service. I handle these and return a proper response. **Programmer errors** are bugs: reading a property of undefined, a wrong type. You can't safely 'handle' these.
> In practice: typed error classes, **one central error handler** that maps errors to HTTP responses, structured logs with a request ID, and no stack traces leaked to clients. For `uncaughtException` and `unhandledRejection`, I log, then **shut down gracefully** and let Kubernetes or PM2 restart the process, because after an unknown error the app's state can't be trusted."

---

### 4.13 Graceful shutdown

> 🗣️ "When Kubernetes wants to stop a pod, it sends **SIGTERM**, waits a grace period (30 seconds by default), then sends SIGKILL. In that window I want to:
> 1. Mark the app **not ready**, so the load balancer stops sending new traffic.
> 2. **Stop accepting** new connections.
> 3. Let **in-flight requests finish**.
> 4. Close DB pools, queue consumers and Redis.
> 5. Exit, with a hard timeout as a safety net."

```javascript
let shuttingDown = false;
app.get('/ready', (req, res) => res.sendStatus(shuttingDown ? 503 : 200));

const server = app.listen(PORT);

process.on('SIGTERM', () => {
  shuttingDown = true;
  server.close(async () => {            // waits for in-flight requests
    await Promise.allSettled([db.end(), consumer.close(), redis.quit()]);
    process.exit(0);
  });
  setTimeout(() => process.exit(1), 25_000).unref(); // never hang forever
});
```

❓ **Follow-ups**
- *What if shutdown takes longer than the grace period?* Kubernetes sends SIGKILL and in-flight work is lost. That's why queue consumers must be idempotent and messages only acknowledged after processing.
- *Senior detail:* Removing a pod from the Service endpoints happens **in parallel** with SIGTERM, so a few new requests can still arrive. A short `preStop` sleep (a few seconds) avoids dropping them.

---

### 4.14 How do you find a memory leak in production?

> 🗣️ "First I confirm it's really a leak: memory that keeps climbing **after garbage collection** and never comes back down, not just a busy period. Then I check whether it lines up with a specific deploy, endpoint or traffic pattern.
> To find the cause, I take **two or three heap snapshots** some minutes apart, on staging or one isolated instance, and compare them. Whatever object type keeps growing, I follow its **retainer path** to see what's holding it. Usual causes: an unbounded in-memory cache, listeners added per request, timers never cleared, or a global array.
> Then I fix the owner of the reference, load-test, and watch the memory graph go flat."

❓ **Follow-ups**
- *RSS vs heapUsed?* `heapUsed` is JS objects. RSS is the total process memory, including Buffers, native code and the heap. If RSS grows but the heap doesn't, look at Buffers or native modules.
- *Why does RSS stay high after load drops?* V8 doesn't always give memory back to the OS immediately. That's not necessarily a leak.
- *Quick mitigation?* Rolling restarts, or a memory limit so the pod restarts, buys time. It's not a fix.

---

### 4.15 How do you investigate 100% CPU?

> 🗣️ "First, scope it: one instance or all? Since a deploy? One endpoint or everything? Then I look at **event-loop lag** and take a **CPU profile** (`--cpu-prof`, Chrome DevTools via `--inspect`, or the APM's profiler). The flame graph shows the hot function directly.
> Common causes: heavy JSON serialization, a bad regex, synchronous crypto or compression, big loops, or **garbage collection** working overtime because of high memory pressure.
> I fix the cause first: optimize, cache, move to a worker or a queue. **Scaling out only hides it.**"

---

### 4.16 Logging and observability

> 🗣️ "Three pillars. **Logs** tell you what happened, **metrics** tell you how much and how often, and **traces** show you where the time went across services.
> For logs: **structured JSON** (pino is my default in Node), consistent levels, a **request/correlation ID** on every line, and never secrets or personal data. For metrics I watch the four golden signals: **latency (p95/p99), traffic, errors, saturation**. For Node I add event-loop lag, heap, and DB pool usage. For traces I use **OpenTelemetry**, so a request can be followed from the gateway through every service and query."

```javascript
import { AsyncLocalStorage } from 'node:async_hooks';
const als = new AsyncLocalStorage();

app.use((req, res, next) => {
  const requestId = req.get('x-request-id') ?? crypto.randomUUID();
  res.set('x-request-id', requestId);
  als.run({ requestId }, next);          // available anywhere downstream
});
// in any function: logger.info({ requestId: als.getStore()?.requestId }, 'order created')
```

❓ **Follow-ups**
- *Why p95/p99 instead of average?* An average hides the slow tail. If 1 in 100 requests takes 8 seconds, the average still looks fine, but those users (often the biggest carts) are suffering.

---

### 4.17 Configuration and environment variables

> 🗣️ "I follow the **twelve-factor** idea: the same build runs in every environment, and only **configuration** changes, via environment variables or a secret store. I **validate config at startup** with a schema, so a missing `DATABASE_URL` crashes the app immediately on deploy, not three hours later on the first request that needs it."

---

### 4.18 Node.js security basics

> 🗣️ "My checklist: validate every input with a schema; parameterized queries; authorization on every resource, server-side; `helmet` for security headers; body-size limits; rate limiting; **timeouts on every outbound call**; `npm audit` or Dependabot in CI; a lockfile and `npm ci`; secrets in a vault, not in code; run as a non-root user; and never pass user input into a shell or `eval`."

❓ **Follow-ups**
- *Prototype pollution?* Merging untrusted JSON into objects can set `__proto__` and change the behaviour of every object. Validate input, avoid unsafe deep-merge, or use `Object.create(null)` maps.
- *ReDoS?* A regex with nested quantifiers can take exponential time on crafted input, which blocks the event loop. Keep patterns simple and limit input length.
- *Supply chain?* Pin versions, review new dependencies, use `npm ci`, and consider disabling install scripts.

---

### 4.19 What's new in modern Node.js? (shows you keep up)

> 🗣️ "A few things I use in recent LTS versions: **built-in `fetch`**, so I don't need axios for simple calls. The **built-in test runner**, `node:test`. **`--watch`** mode instead of nodemon. **`--env-file`** instead of dotenv. Recent versions can also **run TypeScript files directly** by stripping types, which is great for scripts, though you still type-check in CI. And `require()` can now load most ES modules, which makes the CommonJS-to-ESM migration much less painful."

---

### ⚡ Node.js rapid-fire

| Question | One-line answer |
|---|---|
| `npm ci` vs `npm install` | `ci` installs exactly from the lockfile, fails if it's out of sync, and is faster. Use it in CI and Docker. |
| `^1.2.3` vs `~1.2.3` | Caret allows minor and patch updates; tilde allows patch only. |
| `dependencies` vs `devDependencies` vs `peerDependencies` | Runtime / build-and-test only / "the host app must provide this" (React for a component library). |
| PM2? | A process manager: restarts, cluster mode, logs. Less needed with Docker and Kubernetes. |
| How to debug Node? | `node --inspect`, then Chrome DevTools or VS Code. For production: logs, traces, profiles. |
| `require` caching? | Modules are cached after the first load, so they behave like singletons. |
| Single thread, so is Node bad for many users? | No. It handles many concurrent **I/O** requests well. It's bad at heavy **CPU** per request. |
| Node vs Deno vs Bun? | Deno and Bun are newer runtimes with built-in TS and tooling. Node has the biggest ecosystem and maturity. |
| `AbortController`? | Standard way to cancel `fetch`, timers and streams. Pass `signal` down your call chain. |

---

# 5. Express and REST API Design

### 5.1 Express request lifecycle and middleware

> 🗣️ "In Express, a request flows through a **chain of middleware functions in the order you register them**. Each one gets `req`, `res` and `next`. It can modify the request, end the response, or call `next()` to pass control on. A typical order is: security headers → body parsing → request ID and logging → authentication → validation → the route handler → a 404 handler → the **error handler** at the very end. Express recognises the error handler because it takes **four** arguments: `(err, req, res, next)`."

```javascript
app.use(helmet());
app.use(express.json({ limit: '100kb' }));
app.use(requestId);
app.use(authenticate);
app.post('/orders', validate(CreateOrderSchema), ordersController.create);
app.use(notFound);
app.use(errorHandler);   // (err, req, res, next)
```

❓ **Follow-ups**
- *What if a middleware doesn't call `next()` and doesn't respond?* The request hangs until the client times out.
- *Why does order matter?* Auth must run before handlers, body parsing before validation, and the error handler must be last.

---

### 5.2 How do you structure an Express (or any Node) API?

> 🗣️ "I separate **HTTP concerns** from **business logic**:
> - **Routes**: URL to handler mapping.
> - **Controllers**: read the request, call a service, shape the response. Thin.
> - **Services**: the business rules and use cases. No `req` or `res` in here.
> - **Repositories**: database access.
> - **Middleware**: cross-cutting things like auth, logging, rate limiting.
> - **Schemas**: input validation.
>
> The benefit is that services are **testable without HTTP**, and I could swap Express for Fastify, or call the same service from a queue consumer, without rewriting the logic. I organise folders **by feature** (`orders/`, `users/`) rather than by type once the app grows."

---

### 5.3 Centralized error handling

> 🗣️ "Controllers throw typed errors like `NotFoundError` or `ValidationError`, and **one error middleware** turns them into responses. It logs the full error internally with the request ID, returns a **consistent error shape** to the client, and hides stack traces and internal messages for 500s.
> One Express-specific detail: in **Express 4**, a rejected Promise in an async handler is **not** caught automatically, so you need a wrapper or the `express-async-errors` package. **Express 5** fixed that: rejected Promises go to the error handler automatically."

```javascript
class AppError extends Error {
  constructor(status, code, message) { super(message); this.status = status; this.code = code; }
}

app.use((err, req, res, next) => {
  const status = err instanceof AppError ? err.status : 500;
  req.log.error({ err, requestId: req.id });
  res.status(status).json({
    error: {
      code: err.code ?? 'INTERNAL_ERROR',
      message: status < 500 ? err.message : 'Something went wrong',
    },
    requestId: req.id,
  });
});
```

---

### 5.4 What makes an API "RESTful"?

> 🗣️ "REST models everything as **resources** with stable URLs, like `/orders/123`, and uses **HTTP methods** for the actions: GET to read, POST to create, PUT/PATCH to update, DELETE to remove. Each request is **stateless** and carries everything needed, like the auth token. And you respect HTTP semantics: correct status codes, caching headers, idempotency.
> In practice, a good REST API means **nouns in URLs, not verbs**: `POST /orders`, not `POST /createOrder`. For actions that don't fit CRUD, I use a sub-resource like `POST /orders/123/cancel`."

---

### 5.5 POST vs PUT vs PATCH vs DELETE

> 🗣️ "**POST** creates something or triggers an action, and it's **not idempotent**: send it twice, get two orders. **PUT** **replaces** the whole resource at a known URL and **is idempotent**: sending the same PUT twice leaves the same result. **PATCH** updates **part** of a resource. It *can* be idempotent (set `status` to `shipped`) or not (increment `qty` by 1). **DELETE** removes and is idempotent: the second delete just returns 404 or 204."

❓ **Follow-ups**
- *Can POST be made idempotent?* Yes, with an **idempotency key**. See 5.6.

---

### 5.6 Idempotency and idempotency keys

> 🗣️ "Idempotent means doing it **twice has the same effect as doing it once**. That matters because networks fail. The client sends a payment, the response times out, and the client doesn't know if it worked, so it retries. Without protection, the customer pays twice.
> The fix is an **idempotency key**. The client generates a unique ID per operation and sends it in a header. The server stores the key with the result. If the same key comes again, the server **returns the stored result** instead of processing it again."

**How it works on the server:**
1. Client sends `Idempotency-Key: 7f3a…` with `POST /payments`.
2. Server **inserts the key** into a table with a **unique constraint** (status `processing`, plus a hash of the request body).
3. Insert succeeded → process the payment → save the response against the key → return it.
4. Insert failed because the key exists:
   - status `completed` → return the saved response
   - status `processing` → return `409` (or wait briefly)
   - same key but a **different body** → return `422`; that's a client bug
5. Keys expire after a window, for example 24 hours.

🏢 **From my work:** Order creation and payment callbacks in e-commerce. Payment providers retry webhooks, so the handler must be idempotent on the provider's transaction ID.

⚠️ **Trap:** "I check if the order exists, then insert." Two retries arriving at the same moment both pass the check. The **unique constraint** is what makes it safe.

---

### 5.7 HTTP status codes you must know

| Code | Meaning | When I use it |
|---|---|---|
| 200 | OK | Successful GET/PUT/PATCH with a body |
| 201 | Created | Successful POST that created something (add a `Location` header) |
| 202 | Accepted | Work queued, not finished yet (async jobs) |
| 204 | No Content | Success with no body (DELETE) |
| 301 / 308 | Permanent redirect | URL moved forever (SEO); 308 keeps the method |
| 302 / 307 | Temporary redirect | 307 keeps the method |
| 304 | Not Modified | Client's cached copy is still valid (ETag match) |
| 400 | Bad Request | Malformed JSON, wrong types |
| 401 | Unauthorized | **Not logged in** / invalid token |
| 403 | Forbidden | Logged in, but **not allowed** |
| 404 | Not Found | Doesn't exist (or you're hiding that it exists) |
| 409 | Conflict | State conflict: duplicate, version mismatch |
| 422 | Unprocessable | Well-formed but fails business validation |
| 429 | Too Many Requests | Rate limited (add `Retry-After`) |
| 500 | Internal Error | Our bug |
| 502 | Bad Gateway | Proxy got a bad response from upstream |
| 503 | Unavailable | Overloaded or in maintenance (add `Retry-After`) |
| 504 | Gateway Timeout | Upstream took too long |

> 🗣️ **401 vs 403 in one line:** "401 means *who are you?*, and 403 means *I know who you are, and the answer is no.*"

---

### 5.8 Pagination: offset vs cursor

> 🗣️ "**Offset pagination** (`?page=5&limit=20`, `OFFSET 80`) is simple and lets users jump to page 50. But the database still reads and throws away all the skipped rows, so deep pages get **slow**. And if rows are inserted while someone is paging, they see **duplicates or miss items**.
> **Cursor pagination** (`?after=<cursor>`) remembers the last item seen and asks for 'the next 20 after this one' using an index. It's **fast at any depth** and **stable** while data changes. The trade-off is that you can't jump to an arbitrary page. I use offset for small admin tables, and cursor for feeds, infinite scroll, large lists and public APIs."

```sql
-- Cursor = the last row's (created_at, id). id breaks ties.
SELECT id, created_at, total
FROM orders
WHERE (created_at, id) < ($1, $2)
ORDER BY created_at DESC, id DESC
LIMIT 20;
-- Needs an index on (created_at, id)
```

---

### 5.9 Filtering, sorting and field selection conventions

> 🗣️ "I keep it boring and consistent: `GET /products?category=taps&minPrice=100&sort=-price&limit=20&fields=id,name,price`. A minus sign means descending. I **allow-list** which fields can be filtered and sorted, because sorting on an unindexed column is an easy way for anyone to put load on the database, and I **cap `limit`**."

---

### 5.10 API versioning

> 🗣️ "First, I try **not** to need a new version. Most changes can be **additive**: new optional fields, new endpoints. Clients ignore what they don't know. I only version for a real **breaking** change, like removing a field or changing its meaning.
> When I do version, I prefer the **URL** (`/v2/orders`) because it's obvious, easy to route and easy to see in logs. Header versioning is cleaner in theory but harder to debug. Either way: announce a **deprecation window**, track which clients still call v1, and remove it only when usage reaches zero."

🏢 **From my work:** Story S7. I added an optional argument instead of a new version, so old clients kept working unchanged.

---

### 5.11 Validation and sanitization

> 🗣️ "I validate **every** input at the edge of the system with a schema (Zod or Joi): types, required fields, ranges, formats, and **reject unknown fields**. Validation checks the **shape**. It does *not* protect against injection by itself. That's the job of **parameterized queries** for SQL and **output encoding** for HTML. Sanitizing depends on where the data ends up."

---

### 5.12 Rate limiting

> 🗣️ "Rate limiting protects you from abuse and from one client eating all the capacity. Key points:
> - Store the counters in **Redis** (or at the API gateway), not in memory, because with several instances each would count separately.
> - Choose the **key** carefully: per user or API key is better than per IP, since many users share an IP behind corporate NAT.
> - Use **different limits** for different endpoints: login is strict (brute force), search is moderate, static reads are generous.
> - Return **429** with a `Retry-After` header.
>
> Algorithms: **fixed window** is simplest but allows bursts at the boundary. **Sliding window** is smoother. **Token bucket** allows short bursts but caps the average rate, which is what most gateways use."

Design details: see 15.5.

---

### 5.13 CORS

> 🗣️ "CORS is a **browser** rule. By default, JavaScript on `shop.com` can't read responses from `api.other.com`. The server opts in with headers like `Access-Control-Allow-Origin`.
> For anything beyond a simple request (JSON content type, custom headers, PUT/DELETE), the browser first sends a **preflight** `OPTIONS` request to ask permission.
> Important points: allow only **specific origins**; you **can't** use `*` together with credentials; and CORS is **not security for your API**. It only restricts browsers. curl or a server can call your API regardless. Auth is still required."

---

### 5.14 API gateway vs load balancer vs reverse proxy

> 🗣️ "A **reverse proxy** (Nginx) sits in front of servers and forwards requests, often handling TLS, compression and caching. A **load balancer** spreads traffic across many instances and removes unhealthy ones. An **API gateway** is a smarter front door for APIs: routing to many services, authentication, rate limits, quotas, API keys, request transformation and analytics.
> The rule I follow: **business logic never goes in the gateway**. It should stay thin and policy-focused, or it becomes an untestable monolith."

❓ **Follow-ups**
- *Examples?* AWS API Gateway, Azure API Management, Kong, Red Hat 3scale. Load balancers: AWS ALB/NLB, Azure Application Gateway or Front Door.

---

### 5.15 OpenAPI and contract-first development

> 🗣️ "OpenAPI is a machine-readable description of a REST API: endpoints, request and response schemas, auth, errors. **Contract-first** means you agree the spec **before** writing the code, so frontend and backend can work in parallel against mocks. From the spec you get docs, generated clients and types, and validation. I keep the spec in the repo and check in CI that the implementation still matches it."

---

### 5.16 Long-running operations

> 🗣️ "If something takes more than a few seconds, like a report, a bulk import or a video, I don't keep the HTTP request open. I return **`202 Accepted`** with a job ID and a status URL, process it in a **background worker**, and the client either **polls** `GET /jobs/123`, gets a **webhook**, or gets a push via **SSE/WebSocket**. When the job finishes, the status includes a link to the result, often a signed URL to download a file."

---

### 5.17 Webhooks: receiving them safely

> 🗣️ "When a payment provider or partner calls our webhook:
> 1. **Verify the signature**, usually an HMAC of the raw body plus a timestamp, to prove it's really them and to block replays.
> 2. **Respond 2xx quickly**, and push the actual work onto a queue.
> 3. **Make processing idempotent** using the event ID, because providers **will** retry and send duplicates.
> 4. **Don't assume order**: a 'refunded' event can arrive before 'paid'. Check the current state, or fetch the latest from their API."

---

### 5.18 HTTP caching for APIs

> 🗣️ "Two tools. **`Cache-Control`** tells browsers and CDNs how long they can reuse a response: `public, max-age=60` for shared data, `private` for user data, `no-store` for sensitive data. `s-maxage` targets the CDN only, and `stale-while-revalidate` serves the old copy while refreshing in the background.
> **ETags** handle revalidation. The server sends a version tag, the client sends it back in `If-None-Match`, and if nothing changed the server replies `304 Not Modified` with no body. That saves bandwidth."

⚠️ **Trap:** Caching a personalised response with `public`. Now a CDN can serve one user's cart to another. (See 17.13.)

---

### 5.19 Express vs Fastify vs NestJS

> 🗣️ "**Express** is minimal and has the biggest ecosystem, but it leaves structure up to you. **Fastify** is faster, with built-in schema validation and serialization, and a good plugin system. I'd choose it for performance-sensitive APIs. **NestJS** is an opinionated framework on top of Express or Fastify, with modules, dependency injection and decorators. It's great for big teams that want consistency, but it's heavier. My choice depends on team size and how much structure they want enforced."

---

### ⚡ REST rapid-fire

| Question | One-line answer |
|---|---|
| Safe methods? | GET, HEAD, OPTIONS: they don't change state. |
| Idempotent methods? | GET, PUT, DELETE, HEAD, OPTIONS. POST is not. PATCH depends. |
| Nested resources: how deep? | One level (`/orders/123/items`). Deeper gets messy; use filters instead. |
| HATEOAS? | Responses include links to related actions. True REST, rarely done fully in practice. |
| `PUT` to create? | Yes, if the client chooses the ID (`PUT /users/abc`). |
| Bulk operations? | `POST /orders/bulk` with per-item results, or 207 Multi-Status. Make them idempotent. |
| How do you document errors? | One consistent shape (`code`, `message`, `details`, `requestId`), e.g. RFC 9457 Problem Details. |
| Timeouts on outbound calls? | Always. Without a timeout, one slow dependency ties up your whole server. |

---

# 6. GraphQL

> 🏢 This is your strongest chapter. You work with GraphQL daily on a headless commerce platform. Use Stories **S1, S2, S6 and S7** freely here.

### 6.1 What is GraphQL?

> 🗣️ "GraphQL is a query language for APIs. Instead of many endpoints that each return a fixed shape, there's **one endpoint** and a **typed schema**, and the client asks for **exactly the fields it needs**, in one request. That fixes over-fetching (getting fields you don't need) and under-fetching (making five calls to build one screen).
> The trade-off is that complexity moves to the **server**: resolver performance, N+1 queries, authorization per field, caching, and protecting yourself from very expensive queries."

💡 **Picture it:** REST is a set menu: each dish comes as it comes. GraphQL is ordering à la carte: you list exactly what you want on one plate.

---

### 6.2 The building blocks

> 🗣️ "The **schema** defines the types and what you can ask for. **Queries** read data, **mutations** change data, and **subscriptions** push updates over a long-lived connection. **Resolvers** are the functions that actually produce each field's value. The **context** is a per-request object passed to every resolver; that's where I put the logged-in user, DataLoaders and services."

```graphql
type Product {
  sku: String!
  name: String!
  price: Money!
  category: Category      # resolved by its own resolver
}

type Query {
  product(sku: String!): Product
  products(filter: ProductFilter, first: Int = 20, after: String): ProductConnection!
}

type Mutation {
  addToCart(input: AddToCartInput!): AddToCartPayload!
}
```

---

### 6.3 GraphQL vs REST: when do you pick which?

> 🗣️ "I pick **GraphQL** when there are **several clients needing different shapes** of the same data (web, mobile, partner apps) or complex UIs that would otherwise need many calls. A headless storefront is the perfect case: a product page needs product, variants, prices, stock and related items in one round trip.
> I pick **REST** for simple CRUD, public APIs where **HTTP caching** matters, **file uploads and downloads**, webhooks, and service-to-service calls where the shape is fixed. REST is also easier to monitor, because every endpoint has its own URL in the logs.
> It's not either/or. Many systems use GraphQL as the frontend layer over REST or gRPC microservices."

| | REST | GraphQL |
|---|---|---|
| Endpoints | Many | One |
| Response shape | Server decides | Client decides |
| HTTP caching | Easy (per URL) | Harder (mostly POST) |
| Over/under-fetching | Common | Solved |
| Versioning | `/v2` | Evolve the schema, deprecate fields |
| Errors | HTTP status codes | Usually 200 plus an `errors` array |
| Main risk | Too many calls | N+1, expensive queries |

---

### 6.4 The N+1 problem and DataLoader

> 🗣️ "N+1 happens because each field resolves **independently**. You query 50 products, that's 1 query, and then each product's `category` resolver runs its own query, 50 more. So 1 + N.
> **DataLoader** fixes it by **batching**. During one tick of the event loop it collects every `load(id)` call, then calls your batch function **once** with all the IDs, so it becomes a single `WHERE id IN (...)` query. It also caches per request, so the same ID is never loaded twice.
> Important: create **new loaders per request**. A global loader would cache data across users, which is both a memory leak and a security problem."

```javascript
import DataLoader from 'dataloader';

const createLoaders = (db) => ({
  categoryById: new DataLoader(async (ids) => {
    const rows = await db.query('SELECT * FROM categories WHERE id = ANY($1)', [ids]);
    const byId = new Map(rows.map(r => [r.id, r]));
    return ids.map(id => byId.get(id) ?? null); // MUST return results in the same order as ids
  }),
});

// per request
context: ({ req }) => ({ user: req.user, loaders: createLoaders(db) });

// resolver
const resolvers = {
  Product: {
    category: (product, _args, { loaders }) => loaders.categoryById.load(product.categoryId),
  },
};
```

🏢 **From my work (S2):** "On our product listing, custom resolvers were loading data per product: 1,811 queries for 24 products. I batched them and got it down to 238, with the response byte-for-byte identical."

❓ **Follow-ups**
- *What are DataLoader's two rules?* The batch function must return an array of the **same length** and in the **same order** as the keys.
- *Is DataLoader only for databases?* No. It works for any batchable source: a REST service that accepts many IDs, Redis `MGET`, and so on.
- *How do you detect N+1?* Count queries per request (logs, APM spans), or use resolver tracing.

---

### 6.5 Authorization in GraphQL

> 🗣️ "I **authenticate once**, at the start of the request, and put the user in the context. But **authorization** has to happen where the data is accessed, at the **service or field level**, because in GraphQL any field can be reached through many different paths. If I only checked at the top-level query, someone could reach the same data through a nested relationship.
> I keep the rules in the **business/service layer**, not scattered across resolvers, so REST, GraphQL and background jobs all enforce the same rules."

⚠️ **Trap:** "We hide the field in the UI." The schema is public. Anyone can query any field directly.

---

### 6.6 Protecting a GraphQL API from expensive queries

> 🗣️ "Because the client writes the query, a malicious or careless client can ask for something huge, like products → related → related → related… nested ten levels deep. My defences:
> - **Depth limit**: reject queries nested more than, say, 8 levels.
> - **Complexity or cost limit**: give each field a cost, multiply by list sizes, and reject over a budget.
> - **Pagination caps**: `first` can't exceed 100.
> - **Timeouts** per request.
> - **Persisted queries / allow-listing**: in production, the server only runs queries it already knows by hash. This is the strongest protection for first-party apps.
> - **Rate limiting by cost**, not just request count.
> - Restrict **introspection** in production if the API isn't public."

---

### 6.7 Caching in GraphQL

> 🗣️ "HTTP caching is harder because most queries are POSTs to one URL. So caching happens in layers:
> - **Client**: Apollo or urql keep a **normalized cache**, storing each object once by its ID and updating it everywhere.
> - **Per request**: DataLoader dedupes within one request.
> - **Server/data layer**: Redis cache for expensive resolvers or upstream calls.
> - **CDN**: with **persisted queries sent as GET**, responses become cacheable by URL.
> - **Full response cache**: only for public, non-personalized data, with clear invalidation.
>
> The big rule: never put user-specific data under a shared cache key."

---

### 6.8 Error handling in GraphQL

> 🗣️ "GraphQL can return **partial data**: `data` with some fields filled in and an `errors` array for the fields that failed. Usually with HTTP 200. So clients must check both.
> I split errors in two. **Unexpected errors** (bugs, timeouts) go into `errors`, get logged with a request ID, and show a generic message. **Expected business errors**, like 'out of stock' or 'invalid coupon', I prefer to model **in the schema** as typed results, so the client handles them with type safety instead of string-matching error messages."

```graphql
type AddToCartPayload {
  cart: Cart
  userErrors: [UserError!]!    # e.g. { code: OUT_OF_STOCK, field: "qty", message: "..." }
}
```

🏢 **From my work (S6):** "A library bug once **masked** the real errors. Non-ASCII characters in query comments crashed the error formatter, so every error looked like a confusing `str_repeat()` failure. Lesson: when an error makes no sense, check whether something is hiding the real one."

---

### 6.9 Pagination in GraphQL

> 🗣️ "The common standard is **cursor-based connections** from the Relay spec: you ask for `first: 20, after: <cursor>`, and get back `edges` (each with a `node` and a `cursor`) plus `pageInfo` with `hasNextPage` and `endCursor`. It's verbose but consistent, and works well with infinite scroll. Some APIs, Magento included, use simple `currentPage` and `pageSize` instead, which is fine for catalog pages where users jump between page numbers."

---

### 6.10 How do you evolve a GraphQL schema without breaking clients?

> 🗣️ "GraphQL is designed to evolve **without versions**. Adding fields, types or **optional** arguments is safe, because old clients never ask for them. To remove something, I mark it **`@deprecated(reason: ...)`**, monitor field usage until no client uses it, and only then remove it. Breaking changes are things like removing a field, making a nullable field non-null in an input, or renaming. I run **schema diff checks in CI** to catch those."

🏢 **From my work (S7):** "I added an optional `skus` argument to an existing field instead of creating a new one. Old clients were unaffected. And I deployed the backend first, because a client sending an unknown argument gets the **whole query** rejected."

---

### 6.11 Subscriptions

> 🗣️ "Subscriptions push data to the client in real time, usually over WebSockets. They're good for things like order status or live chat. In production they're harder than queries: you need a **pub/sub layer** like Redis so any server instance can notify any connected client, plus connection auth, reconnection, and limits. For simple one-way updates, **SSE** or even polling is often simpler."

---

### 6.12 Federation and schema stitching

> 🗣️ "**Federation** (Apollo Federation, for example) lets each team own a **subgraph** for its domain (products, orders, users), and a **router** composes them into one graph for clients. A type can be extended across services: the Orders subgraph can add `orders` to the `User` type owned by the Users team. It gives teams autonomy, but you need schema governance, composition checks in CI, and tracing across subgraphs. **Schema stitching** is the older, more manual approach. For one team, a single well-structured GraphQL server is usually enough."

---

### 6.13 Fragments and client tooling

> 🗣️ "**Fragments** are reusable pieces of a query. Each component declares the fields it needs as a fragment, and the page query combines them, so components stay in charge of their own data needs. With **GraphQL Code Generator** I get TypeScript types for every query automatically.
> One gotcha I've hit: if you declare a fragment and don't use it, the **entire document is rejected**, not just that part."

🏢 **From my work (S1):** "When I removed related products from the product page query, a few fragments became unused, and the whole query started failing. Removing one field can break a document in a way that isn't obvious."

---

### ⚡ GraphQL rapid-fire

| Question | One-line answer |
|---|---|
| Query vs mutation execution? | Top-level query fields can run in parallel; top-level mutation fields run **in order**. |
| `!` in a type? | Non-null. `[String!]!` means a non-null list of non-null strings. |
| Input types? | Separate types for arguments (`input AddToCartInput`); you can't reuse output types as inputs. |
| Interfaces vs unions? | An interface has shared fields (`Node { id }`); a union is "one of these types" with no shared fields. Query both with `... on Type`. |
| Introspection? | A query that returns the schema itself. Powers tooling; restrict it if your API is private. |
| Scalars? | Built-in: `Int`, `Float`, `String`, `Boolean`, `ID`. Custom: `DateTime`, `Money`, `JSON`. |
| File uploads? | Usually better as a **signed URL** to object storage than through GraphQL multipart. |
| HTTP status for GraphQL errors? | Usually 200 with `errors`. Some servers return 4xx for parse or validation errors. |
| Schema-first vs code-first? | Write SDL then resolvers, vs generate the schema from code (TypeGraphQL, Pothos, Nexus). Both fine; be consistent. |

---

# 7. Authentication and Security

### 7.1 Authentication vs authorization

> 🗣️ "**Authentication** is *who are you?*: proving identity with a password, token or SSO. **Authorization** is *what are you allowed to do?*: can this user see **this** order, edit **this** product. Authentication usually happens once per request; authorization has to happen for **every resource and action**, on the server."

💡 **Picture it:** Authentication is showing your ID at the hotel front desk. Authorization is your key card only opening **your** room.

---

### 7.2 Sessions vs JWT

> 🗣️ "With **sessions**, the server stores the session data, usually in Redis, and the browser holds only a random session ID in a cookie. It's easy to **revoke**: delete the session. But every request needs a lookup.
> With a **JWT**, the token itself carries the user info and is **signed**, so any service can verify it **without a lookup**. That's great across many services, but you **can't easily revoke** it before it expires.
> For a classic web app, server sessions are simple and secure. For multiple services and APIs, I use short-lived JWT access tokens plus a revocable refresh token."

---

### 7.3 JWT structure and how to verify it properly

> 🗣️ "A JWT has three base64 parts separated by dots: **header** (algorithm), **payload** (claims like user ID, expiry, roles), and **signature**. It's **signed, not encrypted**: anyone can decode and read the payload, so never put secrets or personal data in it.
> When verifying, I check the **signature**, I **pin the allowed algorithm** (so an attacker can't switch it to `none` or swap RS256 for HS256), and I check **`exp`, `iss`, and `aud`**."

❓ **Follow-ups**
- *HS256 vs RS256?* HS256 uses one **shared secret** to sign and verify, so every verifier could also forge tokens. RS256/ES256 use a **private key to sign** and a **public key to verify**, so services can verify without being able to create tokens. Public keys are published via a **JWKS** URL. Use asymmetric keys across services.
- *What's `aud` for?* It stops a token issued for service A from being accepted by service B.

---

### 7.4 Access tokens and refresh tokens

> 🗣️ "The **access token** is short-lived, say 5–15 minutes, and is sent with every API call. If it leaks, the damage window is small. The **refresh token** is long-lived and used **only** to get new access tokens from the auth server.
> Best practice is **refresh token rotation**: every refresh returns a **new** refresh token and invalidates the old one. If an old refresh token is ever used again, that means it was stolen, so you **revoke the whole token family** and force a login."

---

### 7.5 How do you revoke a JWT immediately?

> 🗣️ "By design you can't, which is why access tokens should be short-lived. If you need immediate revocation, like a fired employee or a compromised account, you add a bit of state back:
> - Revoke their **refresh tokens**, so no new access tokens get issued.
> - Keep a **denylist of token IDs (`jti`)** in Redis until they expire, or
> - Store a **token version** on the user. Bump it on logout or password change, and reject tokens with an older version.
>
> It's a deliberate trade-off: a small lookup per request in exchange for real revocation."

---

### 7.6 OAuth 2.0 and OpenID Connect

> 🗣️ "**OAuth 2.0** is about **delegated authorization**: letting an app access an API on a user's behalf without seeing their password. It gives you an **access token**. **OpenID Connect** sits on top and adds **authentication**: an **ID token** that says who the user is. 'Login with Google' is OIDC.
> For web and mobile apps, the recommended flow is **Authorization Code with PKCE**."

**Authorization Code + PKCE, step by step**
1. The app creates a random `code_verifier` and its hash, the `code_challenge`.
2. It redirects the user to the auth server with `client_id`, `redirect_uri`, `scope`, `state`, and `code_challenge`.
3. The user logs in and consents.
4. The auth server redirects back with a short-lived `code` (and the same `state`).
5. The app checks `state` (CSRF protection) and exchanges `code` + `code_verifier` for tokens.
6. The auth server checks that the verifier matches the challenge, then returns the access token (plus ID token and refresh token).

> 🗣️ **Why PKCE?** "If someone intercepts the authorization code, it's useless to them without the original verifier, which never left the app."

❓ **Follow-ups**
- *Implicit flow?* Deprecated. Tokens came back in the URL. Use code + PKCE.
- *Client credentials flow?* Machine-to-machine, with no user involved: a service authenticates with its own credentials.
- *SAML vs OIDC?* Both do SSO. SAML is older and XML-based, common in enterprises. OIDC is JSON/JWT-based and better for modern web and mobile.

---

### 7.7 Where should tokens be stored in the browser?

> 🗣️ "I avoid `localStorage` for anything sensitive, because **any XSS can read it**. My preference:
> - Refresh token or session in an **HttpOnly, Secure, SameSite cookie**, which JavaScript can't read at all.
> - The short-lived access token **in memory** only.
> - Even better for SPAs: a **Backend-for-Frontend (BFF)**. The tokens never reach the browser; the BFF keeps them server-side and the browser only has a session cookie.
>
> Cookies bring in CSRF, so I pair them with `SameSite` and CSRF protection."

---

### 7.8 XSS (Cross-Site Scripting)

> 🗣️ "XSS is when an attacker gets **their JavaScript to run on your site**, for example through a product review containing a `<script>` tag that gets rendered as HTML. Once it runs, it can steal tokens, act as the user, or change the page.
> Prevention: **encode output** for its context. React does this by default, so the danger points are `dangerouslySetInnerHTML` and `href="javascript:..."`. Sanitize any HTML you must render with DOMPurify. Add a **Content Security Policy**. And keep tokens in HttpOnly cookies, so even a successful XSS can't read them."

❓ **Types:** **Stored** (saved in the DB, hits every viewer), **Reflected** (in a URL parameter echoed back), **DOM-based** (client-side JS writes untrusted data into the DOM).

---

### 7.9 CSRF (Cross-Site Request Forgery)

> 🗣️ "CSRF tricks the user's **browser** into sending a request to your site from another site, and the browser **automatically attaches the cookies**. For example, a hidden form on `evil.com` that posts to `bank.com/transfer`.
> Defences: **`SameSite=Lax` or `Strict`** cookies, so cookies aren't sent on cross-site POSTs (modern browsers default to Lax). **CSRF tokens** for forms. **Checking the `Origin` header** on state-changing requests. And never changing state on a GET."

> 🗣️ **XSS vs CSRF in one line:** "XSS is the attacker running code **on** your site. CSRF is the attacker making the user's browser send a request **to** your site from somewhere else."

⚠️ **Trap:** "CORS protects us from CSRF." It doesn't. CORS controls **reading** responses; a CSRF attack doesn't need to read anything.

---

### 7.10 SQL and NoSQL injection

> 🗣️ "Injection is when user input is treated as **code**. The fix for SQL is always **parameterized queries**: the query and data are sent separately, so input can never change the query's structure. ORMs do this by default, but raw queries with string concatenation are still dangerous.
> MongoDB has its own version. If the login code does `find({ email, password: req.body.password })` and someone sends `{"$gt": ""}` as the password, it matches any value. The fix is to **validate types**, so the password must be a string, and strip `$` operators from input."

```javascript
// ❌ Injectable
db.query(`SELECT * FROM users WHERE email = '${email}'`);
// ✅ Parameterized
db.query('SELECT * FROM users WHERE email = $1', [email]);
```

---

### 7.11 SSRF (Server-Side Request Forgery)

> 🗣️ "SSRF is when your server fetches a URL the **user** controls, like 'import image from URL' or a webhook URL, and the attacker points it at **internal** things: `localhost`, internal services, or the cloud metadata endpoint `169.254.169.254`, which can leak credentials.
> Defences: **allow-list** domains where possible, resolve DNS and **block private and link-local IP ranges**, don't follow redirects blindly, use IMDSv2 on AWS, and run the fetcher with minimal network access."

---

### 7.12 Access control: RBAC, ABAC, and IDOR

> 🗣️ "**RBAC** (role-based) gives permissions through roles: admin, editor, viewer. It's simple and easy to audit. **ABAC** (attribute-based) decides using attributes: the user's department, the resource's owner, the region, time of day. It's more flexible but harder to reason about. Most real systems use **RBAC plus ownership checks**.
> The most common real-world bug is **IDOR**, also called BOLA: `GET /orders/1235` returns **someone else's** order because the code checked that you're logged in, but not that the order is **yours**. It's number one on the OWASP API Top 10. The fix is to **always scope queries by owner**: `WHERE id = $1 AND customer_id = $currentUser`."

---

### 7.13 Password storage

> 🗣️ "Passwords are **hashed, never encrypted**, because you should never be able to get them back. I use a **slow, salted** password-hashing algorithm: **Argon2id** first, or bcrypt with a proper cost factor. 'Slow' is the point: it makes brute-forcing stolen hashes very expensive. Never use plain SHA-256 or MD5; they're designed to be fast."

❓ **Follow-ups**
- *Salt vs pepper?* A salt is random and unique per password, stored with the hash, and defeats precomputed tables. A pepper is a secret shared by all passwords, stored **outside** the DB, like in a vault.
- *bcrypt limit?* It only uses the first 72 bytes of the input.

---

### 7.14 Secrets management

> 🗣️ "Secrets never go in **code, Git, Docker images or logs**. They live in a managed store (**AWS Secrets Manager**, **Azure Key Vault**, HashiCorp Vault) and are injected at runtime. Even better, use **workload identity** (an IAM role or Azure Managed Identity) so the app doesn't need a stored credential at all. Plus least privilege, rotation, and audit logs. And if a secret is ever committed to Git, **rotate it**. Deleting the commit isn't enough."

---

### 7.15 OWASP Top 10, the quick tour

> 🗣️ "The OWASP Top 10 is the industry's list of the most common web app risks. **Broken access control** has been number one for years. The 2021 list:"

| # | Risk | One-line example |
|---|---|---|
| A01 | Broken Access Control | IDOR: viewing another user's order |
| A02 | Cryptographic Failures | Plain-text passwords, no HTTPS |
| A03 | Injection | SQL injection, XSS |
| A04 | Insecure Design | No rate limit on OTP verification |
| A05 | Security Misconfiguration | Debug mode in prod, default passwords |
| A06 | Vulnerable Components | Old library with a known CVE |
| A07 | Auth Failures | No brute-force protection, weak sessions |
| A08 | Software/Data Integrity | Unsigned updates, compromised CI pipeline |
| A09 | Logging/Monitoring Failures | A breach nobody noticed for months |
| A10 | SSRF | Server fetches internal metadata URL |

*Newer editions (2025) keep access control at #1 and give software supply-chain risk more weight. The ideas are the same.*

---

### 7.16 Security headers

> 🗣️ "The ones I set by default, usually via `helmet`: **Content-Security-Policy** (which scripts can run, the strongest XSS defence), **Strict-Transport-Security** (always HTTPS), **X-Content-Type-Options: nosniff**, **frame-ancestors** / X-Frame-Options (stops clickjacking), and **Referrer-Policy**."

---

### ⚡ Security rapid-fire

| Question | One-line answer |
|---|---|
| Hashing vs encryption vs encoding? | Hashing is one-way (passwords). Encryption is two-way with a key (data at rest). Encoding is just format (base64) and gives **no** security. |
| Symmetric vs asymmetric? | Symmetric uses one shared key and is fast (AES). Asymmetric uses a public/private pair (RSA, ECDSA) for key exchange and signatures. |
| HMAC? | A hash plus a secret key. Proves a message wasn't changed and came from someone with the key (webhook signatures). |
| What does HTTPS protect? | Confidentiality and integrity in transit, plus server identity. It doesn't protect against bugs in your app. |
| MFA? | Something you know + something you have/are. TOTP apps or passkeys over SMS where possible. |
| Passkeys? | Phishing-resistant login using public-key crypto (WebAuthn). No shared secret to steal. |
| Brute-force protection? | Rate limit per account and IP, progressive delays, lockout with care (don't let attackers lock out real users), CAPTCHA after failures. |
| Least privilege? | Every user, service and token gets only the permissions it actually needs. |
| Logging and PII? | Never log passwords, tokens, or full card numbers. Mask emails and phones where possible. |

---

# 8. Web and Browser Fundamentals *(new)*

Full stack interviews often include "browser" questions. They test whether you understand what happens **between** your server and your React code.

### 8.1 What happens when you type a URL and press Enter?

> 🗣️ "At a high level:
> 1. **DNS**: the browser turns the domain into an IP address, checking its own cache, then the OS, then a DNS resolver.
> 2. **Connection**: it opens a TCP connection (or QUIC for HTTP/3) and does a **TLS handshake** for HTTPS.
> 3. **Request**: it sends the HTTP request. Often a **CDN** answers it from cache; otherwise it goes through a load balancer to an app server, which may hit a cache or database.
> 4. **Response**: the HTML comes back.
> 5. **Rendering**: the browser parses HTML into the **DOM**, CSS into the **CSSOM**, combines them, calculates **layout**, **paints** pixels, and **composites** layers. Along the way it discovers and downloads CSS, JS, images and fonts, and runs JavaScript.
> 6. **Hydration**, in a framework like Next.js: React attaches event handlers so the page becomes interactive.
>
> I can go deeper on any step. The ones that usually matter for performance are DNS and connection setup, server response time, and render-blocking resources."

💡 **Tip:** This question has no end. Give the outline, then ask "which part would you like me to go deeper on?"

---

### 8.2 HTTP/1.1 vs HTTP/2 vs HTTP/3

> 🗣️ "**HTTP/1.1** handles one request at a time per connection, so browsers opened about six connections per domain, and we used tricks like bundling and sprite sheets. **HTTP/2** **multiplexes** many requests over one connection and compresses headers. But it still runs on TCP, so one lost packet stalls everything on that connection; that's **head-of-line blocking**. **HTTP/3** runs on **QUIC over UDP**: each stream is independent, so a lost packet only delays its own stream. Connection setup is faster too, which helps a lot on mobile networks."

---

### 8.3 How does HTTPS / TLS work, simply?

> 🗣️ "When the browser connects, the server sends its **certificate**, which says 'I am shop.com', signed by a trusted Certificate Authority. The browser checks that signature. Then both sides use **asymmetric crypto** to agree on a shared secret key, and from then on everything is encrypted with fast **symmetric** encryption. TLS 1.3 does this in a single round trip. The result: nobody in between can read or modify the traffic, and you know you're talking to the real server."

---

### 8.4 Browser and CDN caching for static assets

> 🗣️ "The standard pattern: **fingerprinted asset files** (`app.3f9a1c.js`) get `Cache-Control: public, max-age=31536000, immutable`. They're cached for a year, because any change produces a new filename. The **HTML** gets `no-cache`, meaning 'always check with the server', so users pick up new deploys immediately. A **CDN** caches close to the user and absorbs most of the traffic. For deploys, you either purge the CDN or rely on new filenames."

---

### 8.5 Cookie attributes

| Attribute | What it does |
|---|---|
| `HttpOnly` | JavaScript can't read it, which protects it from XSS theft |
| `Secure` | Only sent over HTTPS |
| `SameSite=Strict` | Never sent on cross-site requests |
| `SameSite=Lax` | Sent on top-level navigation (clicking a link), not on cross-site POSTs or iframes. The modern default |
| `SameSite=None` | Always sent (needs `Secure`). For legitimate cross-site use, like embedded widgets |
| `Domain` / `Path` | Which hosts and paths receive it |
| `Max-Age` / `Expires` | Lifetime; without it, it's a session cookie |
| `__Host-` prefix | Forces Secure, Path=/ and no Domain, the strictest option |

> 🗣️ "For an auth cookie I use `HttpOnly; Secure; SameSite=Lax`, with the `__Host-` prefix if possible."

---

### 8.6 The critical rendering path, and `async` vs `defer`

> 🗣️ "The browser can't show anything until it has the HTML and **all CSS** in the head, because CSS is **render-blocking**. A normal `<script>` is **parser-blocking**: HTML parsing stops until the script downloads and runs.
> **`defer`** downloads in parallel and runs **after** the HTML is parsed, **in order**. That's what I use for app scripts. **`async`** downloads in parallel and runs **as soon as it arrives**, in any order. Good for independent scripts like analytics.
> To speed up the first paint: inline critical CSS, defer non-critical JS, `preload` the hero image and fonts, and `preconnect` to important third-party origins."

---

### 8.7 Core Web Vitals

> 🗣️ "Google's three user-experience metrics, measured at the **75th percentile** of real users:
> - **LCP (Largest Contentful Paint)**: how fast the main content shows. Good is **≤ 2.5 s**.
> - **INP (Interaction to Next Paint)**: how fast the page responds to clicks and typing. Good is **≤ 200 ms**. It replaced FID in 2024.
> - **CLS (Cumulative Layout Shift)**: how much things jump around. Good is **≤ 0.1**.
>
> They affect SEO and, in e-commerce, conversion directly."

| Metric | Usual causes | Fixes |
|---|---|---|
| **LCP** | Slow server (TTFB), big hero image, render-blocking CSS/JS, client-side-only rendering | SSR/SSG, CDN, `fetchpriority="high"` and preload on the hero image, modern formats (AVIF/WebP), don't lazy-load the LCP image |
| **INP** | Long JavaScript tasks, heavy hydration, big re-renders | Less JS, code splitting, break up long tasks, Server Components, avoid unnecessary re-renders |
| **CLS** | Images without dimensions, late-loading ads/banners, font swaps | Set `width`/`height` or `aspect-ratio`, reserve space, `font-display` plus matched fallback fonts |

🏢 **From my work (S1):** "A 4 MB HTML document hurts LCP badly, because the browser has to download and parse all of it. Cutting it to 280 KB was a direct LCP win."

---

### 8.8 Browser storage options

| Storage | Size | Lifetime | Sent to server? | Use for |
|---|---|---|---|---|
| Cookie | ~4 KB | Set by expiry | **Yes**, every request | Session/auth (HttpOnly) |
| `localStorage` | ~5–10 MB | Until cleared | No | Non-sensitive preferences |
| `sessionStorage` | ~5–10 MB | Until the tab closes | No | Per-tab temporary state |
| IndexedDB | Large | Until cleared | No | Offline data, big structured data |
| Cache API | Large | Until cleared | No | Service worker offline caching |

> 🗣️ "`localStorage` is synchronous and readable by any script on the page, so it's never the place for tokens."

---

### 8.9 WebSockets vs Server-Sent Events vs polling

> 🗣️ "**Polling** asks the server every few seconds. It's simple but wasteful. **Long polling** holds the request open until there's news. **SSE** is a one-way stream **from server to client** over normal HTTP, with auto-reconnect built in. Perfect for notifications, live order status, or streaming AI responses. **WebSockets** are a full **two-way** connection, needed for chat, multiplayer or collaborative editing.
> I choose the simplest one that fits: if the client only listens, SSE. If both sides talk constantly, WebSockets."

---

### 8.10 Event bubbling, capturing, and delegation

> 🗣️ "When you click an element, the event travels **down** from the window to the target (the **capture** phase), then **bubbles back up** through every parent. Most handlers run during bubbling.
> **Event delegation** uses that: instead of attaching a listener to 1,000 list items, you attach **one** to the parent and check `event.target`. It's less memory and it works for items added later.
> `stopPropagation()` stops the event travelling further; `preventDefault()` stops the browser's default action, like following a link or submitting a form. They're different things."

---

### 8.11 Accessibility basics

> 🗣️ "My baseline: **semantic HTML first**, so a real `<button>` instead of a clickable `<div>`, because it gives keyboard support and screen-reader roles for free. Every input has a **label**, images have **alt text**, colour contrast meets **WCAG AA**, everything works **with a keyboard** with a visible focus state, and **ARIA** is used only when native HTML can't express something. It also makes tests better: Playwright and Testing Library locate elements by role and label, so accessible apps are easier to test."

---

### 8.12 Image and font optimization

> 🗣️ "For images: serve **modern formats** (AVIF/WebP), **responsive sizes** with `srcset` and `sizes`, always set dimensions, **lazy-load** below-the-fold images but **never** the LCP image, and use a CDN or image service to resize. `next/image` does most of this for you. For fonts: **self-host** them, **subset** them, `preload` the main one, and use `font-display: swap` with a size-matched fallback to avoid layout shift."

---

### ⚡ Web rapid-fire

| Question | One-line answer |
|---|---|
| TTFB? | Time to First Byte: how long until the server starts responding. Includes DNS, connection, and server processing. |
| Reflow vs repaint? | Reflow recalculates layout (expensive); repaint redraws pixels (cheaper). Animate `transform`/`opacity` to avoid both. |
| What is a CDN? | Servers worldwide that cache your content near users. Lower latency, less load on origin. |
| Same-origin policy? | Scripts can only read responses from the same scheme + host + port. CORS relaxes it. |
| Service worker? | A background script that intercepts network requests. Enables offline support, caching and push (PWA). |
| `preload` vs `prefetch`? | `preload` means needed for **this** page, now. `prefetch` means probably needed for the **next** page, at low priority. |
| Tree shaking? | Removing unused exports at build time. Works best with ES modules. |
| SPA vs MPA? | A single-page app swaps views with JS; a multi-page app loads new HTML per page. Next.js mixes both. |

---

# 9. React

### 9.1 Virtual DOM, reconciliation, and keys

> 🗣️ "When state changes, React calls your components again to get a **new description of the UI**: a tree of plain objects, often called the virtual DOM. It **compares** that with the previous tree (that's **reconciliation**) and updates **only the real DOM nodes that changed**, because touching the real DOM is the expensive part.
> **Keys** tell React which list item is which between renders. With a stable key like a product ID, React can reorder items correctly. If you use the **array index** as the key and the list gets reordered or filtered, React can attach the wrong state to the wrong item, like a checkbox staying ticked on the wrong row."

⚠️ **Trap:** "The virtual DOM makes React fast." It's more accurate to say it makes updates **predictable** and **efficient enough**. The real performance work is avoiding unnecessary renders and expensive work.

---

### 9.2 What causes a component to re-render?

> 🗣️ "Four things: its **state** changes, its **parent re-renders**, a **context** it uses changes, or an external store it subscribes to changes.
> Notice that **props changing isn't really the trigger**. The parent re-rendering is. By default, when a parent renders, **all its children render too**, even if their props are identical.
> And a re-render isn't automatically a problem. React only touches the DOM if the output changed. It's a problem when the render itself is **expensive**, or happens hundreds of times, like on every keystroke in a big form."

---

### 9.3 State: `useState`, batching, and immutability

> 🗣️ "State updates are **asynchronous and batched**: React groups several `setState` calls into one re-render, and since React 18 it does that everywhere, including in timeouts and promises. So if the new value depends on the old one, I use the **functional form**: `setCount(c => c + 1)`.
> State must be treated as **immutable**. I create a new object or array instead of mutating, because React compares by **reference**. If I mutate and set the same object, React thinks nothing changed."

```javascript
// ❌ Mutating: React may not re-render
cart.items.push(item); setCart(cart);
// ✅ New references
setCart(prev => ({ ...prev, items: [...prev.items, item] }));
```

---

### 9.4 `useEffect`: what it's for, and when **not** to use it

> 🗣️ "`useEffect` is for **synchronizing with something outside React**: subscriptions, timers, WebSockets, browser APIs, analytics, or manually talking to a non-React widget. It runs **after** the render is committed to the screen, and the **cleanup** function runs before the next effect and on unmount.
> What I **don't** use it for:
> - **Calculating values from props or state.** Just compute them during render. An effect that sets state causes an extra render.
> - **Responding to a user event.** Put that logic in the event handler.
> - **Resetting state when a prop changes.** Give the component a `key` instead.
> - **Fetching data**, in most cases. I use a data library like TanStack Query, or Server Components in Next.js."

**Fetching race condition, and the fix:**

```javascript
useEffect(() => {
  const controller = new AbortController();
  fetch(`/api/search?q=${query}`, { signal: controller.signal })
    .then(r => r.json())
    .then(setResults)
    .catch(err => { if (err.name !== 'AbortError') setError(err); });
  return () => controller.abort(); // old request is cancelled when query changes
}, [query]);
```

> 🗣️ "Without the cleanup, if the user types 'ta' then 'tap', the 'ta' response could arrive **last** and overwrite the correct results."

❓ **Follow-ups**
- *Why does my effect run twice in development?* **Strict Mode** mounts, unmounts and re-mounts components in development to expose missing cleanups. It doesn't happen in production. If running twice breaks something, the cleanup is wrong.
- *Dependency array rules?* Include **every** reactive value the effect uses. If that causes loops, the fix is usually restructuring (moving the function inside the effect, or using an updater function), not lying to the linter.
- *`useLayoutEffect`?* It runs synchronously **before** the browser paints. Use it only for measuring the DOM to avoid a visible flicker.

---

### 9.5 `useRef`

> 🗣️ "`useRef` gives you a box whose value **persists across renders but doesn't trigger a re-render** when it changes. Two uses: holding a **DOM element** (to focus an input or measure a size), and holding **mutable values** like a timer ID, the previous value, or an AbortController."

---

### 9.6 `useMemo`, `useCallback`, `React.memo`, and the React Compiler

> 🗣️ "`useMemo` caches a **calculated value**, `useCallback` caches a **function's identity**, and `React.memo` lets a child **skip re-rendering** when its props haven't changed.
> They work as a team. `React.memo` on a child is useless if the parent passes a new function or object every render, so you `useCallback`/`useMemo` those props.
> But I only add them **after measuring** with the React Profiler. They cost memory and comparisons, and make code harder to read.
> In newer projects, the **React Compiler** does this memoization automatically at build time, so manual `useMemo` and `useCallback` are needed much less."

⚠️ **Trap:** Wrapping everything in `useMemo` "for performance". Say you **measure first**.

---

### 9.7 Controlled vs uncontrolled components

> 🗣️ "A **controlled** input has its value in React state. Every keystroke calls `setState`. That makes validation and dependent fields easy. An **uncontrolled** input keeps its value in the **DOM**, and you read it with a ref or from the form on submit. Fewer re-renders, good for big forms.
> In practice I use **React Hook Form**, which is mostly uncontrolled for performance, together with **Zod** for validation, often the same schema as the backend."

---

### 9.8 How do you choose a state management approach?

> 🗣️ "I split state into **types** first, because each type has a best tool:
> - **Server state** (products, orders, the user profile) is data that lives on the server. Use **TanStack Query** / SWR, or Server Components. They handle caching, refetching, loading states and deduping. Don't copy it into Redux.
> - **Local UI state** (is the modal open, input value) goes in **`useState`** in the component that needs it.
> - **Shared UI state** (a theme, the logged-in user, a cart drawer): **Context** if it changes rarely, or a small store like **Zustand** if it changes often.
> - **URL state** (filters, sort, page): **keep it in the URL**, so it's shareable and survives refresh.
> - Complex client-side workflows: **Redux Toolkit** if the team needs strict structure and devtools.
>
> Most apps that 'need Redux' actually just need a server-state library."

---

### 9.9 Context and performance

> 🗣️ "When a context **value changes**, every component using that context re-renders. Two common mistakes: passing a **new object every render** (`value={{ user, setUser }}`), so all consumers re-render on every parent render, and putting **fast-changing** data in one big context with slow-changing data.
> Fixes: memoize the provider value, **split contexts** by concern, keep fast-changing state local, or use a store with **selectors** like Zustand, where components subscribe only to the slice they use."

---

### 9.10 Custom hooks

> 🗣️ "A custom hook is just a function starting with `use` that **reuses stateful logic** between components: `useDebounce`, `useCart`, `useMediaQuery`. It shares **logic**, not **state**. Each component calling it gets its own state. It's how hooks replaced higher-order components and render props for most use cases."

```javascript
function useDebouncedValue(value, delay = 300) {
  const [debounced, setDebounced] = useState(value);
  useEffect(() => {
    const t = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(t);
  }, [value, delay]);
  return debounced;
}
```

---

### 9.11 Error boundaries

> 🗣️ "An error boundary catches errors **during rendering** in its child tree and shows a fallback UI instead of a blank white screen. I place them around **independent sections**, like the reviews widget or recommendations, so one broken section doesn't take down the product page, and I log the error to monitoring.
> They do **not** catch errors in **event handlers**, async code, or on the server. Those need normal try/catch."

---

### 9.12 Code splitting, `lazy`, and Suspense

> 🗣️ "Code splitting means not shipping all the JavaScript up front. `React.lazy(() => import('./HeavyChart'))` loads a component only when it's rendered, and `<Suspense fallback={...}>` shows a placeholder while it loads. I split at **routes** and around **heavy, rarely-used components**: rich editors, charts, maps, modals. Suspense is also how streaming works in Next.js: slow parts of the page stream in later while the rest shows immediately."

---

### 9.13 How do you optimize React performance?

> 🗣️ "I **measure first**: the React Profiler to see what renders and why, plus Web Vitals and Lighthouse for the user-facing view. Then, in order of how often they actually help:
> 1. **Ship less JavaScript**: Server Components, code splitting, removing heavy libraries.
> 2. **Move state down**, closer to where it's used, so fewer components re-render.
> 3. **Virtualize long lists** (TanStack Virtual), rendering only what's visible.
> 4. **Fix data fetching**: no waterfalls, cached server data.
> 5. **Optimize images and fonts.**
> 6. **Memoize**, only where the profiler shows real cost.
> 7. **Debounce** expensive input handlers; use `useTransition` or `useDeferredValue` for non-urgent updates."

🏢 **From my work (S1):** "My biggest React/Next.js performance win wasn't memoization. It was noticing that we were sending 4 MB of props to a client component. **What you send** often matters more than **how you render**."

---

### 9.14 What's new in React 19?

> 🗣️ "The main things:
> - **Actions**: you can pass an async function to a form's `action`, and React handles the pending state, errors and resets for you.
> - **`useActionState`**, **`useFormStatus`** and **`useOptimistic`** for form state, pending UI and optimistic updates, like showing an item in the cart immediately while the request runs.
> - **`use()`** to read a Promise or context during render, together with Suspense.
> - **`ref` as a normal prop**, so `forwardRef` is no longer needed.
> - **Server Components and Server Actions** are stable.
> - Separately, the **React Compiler** handles memoization automatically."

---

### 9.15 Data fetching on the client

> 🗣️ "For client-side fetching I use **TanStack Query** rather than hand-written `useEffect` fetching. It gives caching, deduping of identical requests, background refetching, retries, pagination helpers and request cancellation. It also fixes race conditions for free. In Next.js App Router, most fetching moves to **Server Components**, and the client only fetches for truly interactive things like search-as-you-type."

---

### 9.16 Testing React

> 🗣️ "I test **behaviour, not implementation**. With **React Testing Library**, I find elements the way a user would (`getByRole('button', { name: 'Add to cart' })`), interact, and assert what the user sees. I don't test internal state or which hooks ran. I mock the **network** with MSW, not my own components. Pure logic gets plain unit tests, and the most important journeys (search → add to cart → checkout) get **Playwright** end-to-end tests."

---

### ⚡ React rapid-fire

| Question | One-line answer |
|---|---|
| Props vs state? | Props come from the parent and are read-only. State is owned by the component and changes over time. |
| Lifting state up? | Moving state to the nearest common parent so siblings can share it. |
| Prop drilling? | Passing props through many layers. Fix with composition (`children`), context, or a store. |
| Fragment? | `<>…</>` groups children without an extra DOM node. |
| Portal? | Renders children into another DOM node (modals, tooltips) while keeping React context. |
| Why can't hooks be in conditions? | React tracks hooks by **call order**; conditional calls break that order. |
| HOC vs hook? | HOCs wrap components; hooks share logic more simply. Hooks are preferred today. |
| `useTransition`? | Marks an update as non-urgent so typing stays responsive while a heavy list updates. |
| `key` to reset a component? | Changing a component's `key` remounts it with fresh state. |
| Class vs function components? | Function components with hooks are standard; classes are only needed for error boundaries (or use a library). |

---

# 10. Next.js

> 🏢 You build a headless Next.js storefront on Magento GraphQL. Mention it early. It turns theory questions into "here's how I did it".

### 10.1 CSR vs SSR vs SSG vs ISR: how do you choose?

> 🗣️ "It's about **when** the HTML is produced:
> - **SSG (Static Site Generation)**: HTML built **at build time**. Fastest possible, served from a CDN. Good for marketing pages, docs, blog posts.
> - **ISR (Incremental Static Regeneration)**: static, but **regenerated** in the background after a time or **on demand**. Ideal for product and category pages: CDN speed, but fresh within minutes, and instantly if you trigger revalidation when a product changes.
> - **SSR (Server-Side Rendering)**: HTML built **on every request**. For pages that are personalized or must be real-time.
> - **CSR (Client-Side Rendering)**: the browser builds the page with JavaScript. Fine for logged-in dashboards where SEO doesn't matter.
>
> In practice, one site mixes them: static home and category pages, ISR product pages, SSR or client for cart and account."

| Page type | My choice |
|---|---|
| Home, CMS pages | SSG / ISR |
| Product & category pages | ISR + on-demand revalidation |
| Search results | SSR (depends on query) |
| Cart, checkout, account | Dynamic / client, never shared-cached |
| Admin dashboard | CSR or dynamic SSR |

---

### 10.2 App Router vs Pages Router

> 🗣️ "The **Pages Router** is the original: every file in `pages/` is a route, and data fetching uses `getServerSideProps` and `getStaticProps`. The **App Router** (`app/`) is the modern one, built on **React Server Components**: nested **layouts** that persist between pages, **streaming**, **Server Actions**, and fetching data directly inside components with `async/await`. New projects should use the App Router. Existing apps can migrate gradually, because both can coexist."

---

### 10.3 Server Components vs Client Components

> 🗣️ "In the App Router, components are **Server Components by default**. They run **only on the server**, can be `async`, can query the database or call APIs directly with secrets, and **send zero JavaScript** to the browser. **Client Components** start with `'use client'`. They're needed for anything interactive: state, effects, event handlers, browser APIs. They're still pre-rendered on the server, then **hydrated** in the browser.
> My rule: keep the page as Server Components and push `'use client'` down to the **smallest interactive pieces**: the add-to-cart button, the image gallery, the swatch picker. Not the whole page."

**The serialization boundary: a senior-level point**

> 🗣️ "Everything you pass as **props from a Server Component to a Client Component** must be serializable, and it gets **embedded in the HTML** as the RSC payload. So passing a huge object into a client component silently bloats every page."

🏢 **From my work (S1):** "That's exactly what happened to us. The product page passed the full product, with all 55 variants and related products for each, into a client component, so the HTML was almost 4 MB. Loading related products after first paint brought it down to 280 KB."

❓ **Follow-ups**
- *Can a Client Component import a Server Component?* Not directly. Everything imported into a client file becomes client code. But you can pass a Server Component **as `children`** (or another prop) to a Client Component.
- *Can Server Components use `useState`?* No. No state, no effects, no browser APIs.
- *What's the `server-only` package?* An import that makes the build fail if a server module (with secrets or DB access) is accidentally imported into client code.

---

### 10.4 Hydration and hydration mismatches

> 🗣️ "**Hydration** is React taking the server-rendered HTML and attaching event handlers and state, so the static page becomes interactive. A **mismatch** happens when the client's first render produces **different HTML** from the server's.
> Common causes: `Date.now()` or `Math.random()` in render, checking `typeof window` to render something different, **timezone or locale** formatting differences between server and browser, invalid HTML nesting like a `<div>` inside a `<p>`, and browser extensions modifying the DOM.
> Fixes: keep the first render **deterministic**. Move browser-only values into `useEffect`, render them after mount, or load that component client-only. `suppressHydrationWarning` is acceptable for a single timestamp."

🏢 **Relevant to your work:** multi-country stores. Formatting prices or dates with the server's locale vs the browser's is a classic mismatch. Pass the locale explicitly.

---

### 10.5 Data fetching in the App Router, and avoiding waterfalls

> 🗣️ "In Server Components I just `await` data directly. The main thing to watch is **waterfalls**: awaiting one request, then the next, then the next, when they don't depend on each other. I start independent requests together with `Promise.all`.
> For slow parts I use **streaming**: wrap the slow section in `<Suspense>` (or add a `loading.tsx`), so the fast parts of the page show immediately and the slow part, like recommendations or reviews, streams in when ready."

```tsx
export default async function ProductPage({ params }) {
  const { slug } = await params;
  const product = await getProduct(slug);           // needed for everything below
  return (
    <>
      <ProductHero product={product} />
      <Suspense fallback={<RelatedSkeleton />}>
        <RelatedProducts sku={product.sku} />       {/* streams in later */}
      </Suspense>
    </>
  );
}
```

---

### 10.6 Caching and revalidation in Next.js

> 🗣️ "Next.js has several cache layers, and the defaults have **changed between versions**, so I always check which version a project uses.
> The concepts stay the same:
> - **Request memoization**: the same fetch in one render runs once.
> - **Data cache**: fetched data cached across requests, with `revalidate: 60` (time-based) or **tags** for on-demand invalidation.
> - **Full route cache**: the rendered HTML/RSC output for static routes.
> - **Router cache**: on the client, recently visited routes kept in memory.
>
> Version notes: in **Next.js 15**, `fetch` stopped being cached by default, so you opt in. **Next.js 16** introduced **Cache Components** with the `'use cache'` directive plus `cacheLife` and `cacheTag`, making caching more explicit.
> For a store, I tag product data with the SKU and call `revalidateTag` from a webhook when the product changes in the backend. Pages stay static and fast, but update within seconds."

⚠️ **Trap:** Caching anything personalized (cart, prices for a logged-in B2B customer) under a shared key. That's how one user sees another user's data.

---

### 10.7 Server Actions

> 🗣️ "A Server Action is an async function marked `'use server'` that you can call from a form or client component, and it runs **on the server**. No API route needed. Good for mutations like add to cart, submit review, update profile. They work with React 19's `useActionState` and `useOptimistic`.
> Important: a Server Action is effectively a **public POST endpoint**. Anyone can call it with any arguments, so I **validate input and check authorization inside every action**, the same as an API route."

🏢 **From my work:** the storefront's swatch selection uses a server action (`fetchSelectedChild`) to load the selected child product.

---

### 10.8 Route Handlers and the Backend-for-Frontend (BFF) pattern

> 🗣️ "Route Handlers (`app/api/.../route.ts`) are API endpoints inside the Next.js app. I use them as a **BFF**: the browser talks to our Next.js server, and the server talks to Magento, the payment provider or search, **keeping API keys and tokens server-side**, combining several calls into one, and reshaping data for the UI. They're also where webhooks land, for example for cache revalidation. I keep the business logic in separate modules so it's testable outside the framework."

---

### 10.9 Middleware (now `proxy` in Next.js 16)

> 🗣️ "Middleware runs **before** a request reaches the route. It's good for quick decisions: redirects, rewrites, **locale or store detection**, A/B bucketing, and a basic 'is there a session cookie?' check. Keep it **fast and light**: no heavy DB calls. In Next.js 16 the file was renamed from `middleware.ts` to `proxy.ts`.
> And never rely on middleware **alone** for security. In 2025 there was a real vulnerability that let attackers skip Next.js middleware with a crafted header. Authorization must also happen where the **data** is accessed."

---

### 10.10 Authentication in Next.js

> 🗣️ "Session or refresh token in an **HttpOnly, Secure cookie**. Middleware for quick redirects of logged-out users. But the **real checks** live in a server-side **data access layer**: every Server Component, Server Action and Route Handler that reads or changes user data verifies the session and permissions there. I never rely on hiding a button in a client component. Libraries: Auth.js, Clerk, or a custom OIDC integration."

---

### 10.11 SEO in Next.js

> 🗣️ "The basics: content is **server-rendered** so crawlers see it. Use the **Metadata API** (`generateMetadata`) for titles, descriptions and Open Graph, **canonical URLs** to avoid duplicates (filters and sorting create lots of URLs), `sitemap.ts` and `robots.ts`, **JSON-LD structured data** (Product, Offer, BreadcrumbList for rich results), correct **status codes** (a real 404, `notFound()`, and 301 for moved products), and good Core Web Vitals.
> For multi-country stores I add **hreflang** so Google shows the UK page in the UK and the UAE page in the UAE."

---

### 10.12 Next.js performance checklist

> 🗣️ "Keep most of the tree as **Server Components**, make `'use client'` boundaries small, and watch **what props cross the boundary**. Fetch in **parallel** and **stream** slow sections. Use **`next/image`** and **`next/font`**. **Dynamic-import** heavy client widgets. Cache data intentionally with tags. Check the bundle analyzer, and measure real-user Web Vitals, not just Lighthouse on my laptop."

---

### 10.13 Deploying Next.js

> 🗣️ "**Vercel** is the zero-config option. To self-host, I use **`output: 'standalone'`** in a Docker image and run it as a Node server behind a CDN. A gotcha with self-hosting **multiple instances**: ISR and the data cache are stored **per instance** by default, so you need a **shared cache handler** (for example Redis) or instances will serve different versions. Also decide **Edge vs Node runtime** per route: Edge is fast to start and close to users but has a limited API; Node supports everything."

---

### 10.14 Headless commerce with Next.js (your territory)

> 🗣️ "In our setup, Magento is the commerce engine (catalog, pricing, inventory, cart, orders) and exposes **GraphQL**. Next.js is the storefront. The design choices that matter:
> - **Catalog pages** (product, category) are cacheable. They're static with revalidation, invalidated when the product changes.
> - **Cart, checkout and account** are private and dynamic, and never shared-cached.
> - **Price and stock** need freshness rules. Show a cached price, but always confirm at add-to-cart and checkout.
> - **Payload discipline**: GraphQL makes it easy to over-ask, and the App Router puts everything a client component receives into the HTML. That was my 4 MB lesson.
> - **Multi-store routing**: store and locale from the domain or path, passed into every GraphQL call.
> - **Deploy order**: a backend schema change must ship **before** the frontend that uses it."

---

### ⚡ Next.js rapid-fire

| Question | One-line answer |
|---|---|
| `layout.tsx` vs `template.tsx`? | A layout persists across navigation (keeps state); a template remounts on every navigation. |
| `loading.tsx` / `error.tsx` / `not-found.tsx`? | Automatic Suspense fallback / error boundary / 404 UI for that route segment. |
| `generateStaticParams`? | Lists which dynamic routes (e.g. product slugs) to pre-build at build time. |
| Dynamic segments? | `[slug]`, catch-all `[...slug]`, optional catch-all `[[...slug]]`. |
| Route groups? | `(shop)` folders organise routes without changing the URL. |
| Parallel / intercepting routes? | Show multiple pages in one layout (`@modal`), or open a route as a modal over the current page. |
| `NEXT_PUBLIC_` env vars? | Inlined into the browser bundle. **Never** put secrets there. |
| `revalidatePath` vs `revalidateTag`? | Invalidate one URL vs everything tagged with a key (better for "product X changed"). |
| Partial Prerendering? | A static shell served instantly, with dynamic holes streamed in, in the same route. |
| `next/link` prefetch? | Links in the viewport prefetch their route so navigation feels instant. |

---

# 11. Databases and Query Optimization

### 11.1 What is an index?

> 🗣️ "An index is a separate, sorted data structure, usually a **B-tree**, that lets the database find rows **without scanning the whole table**. Like the index at the back of a book: you jump to the right page instead of reading every page.
> The cost: indexes take **storage**, and every insert, update and delete must also update every index, so **writes get slower**. So I index the columns used in selective `WHERE` filters, `JOIN`s and `ORDER BY`, and I confirm with `EXPLAIN` that the index is actually used. I don't index everything 'just in case'."

---

### 11.2 Composite and covering indexes

> 🗣️ "A **composite index** covers several columns, like `(customer_id, created_at)`. **Order matters**: it works like a phone book sorted by last name, then first name. You can search by last name, or last name plus first name, but **not first name alone**. That's the **leftmost-prefix rule**.
> A good default order is: **equality** columns first, then the **sort** column, then **range** columns.
> A **covering index** contains **every column the query needs**, so the database answers from the index alone, an **index-only scan**, without touching the table at all."

```sql
-- Query: a customer's latest orders
SELECT id, created_at, total FROM orders
WHERE customer_id = 42 ORDER BY created_at DESC LIMIT 20;

-- Index that serves the filter AND the sort:
CREATE INDEX idx_orders_customer_created ON orders (customer_id, created_at DESC);
-- Covering version (PostgreSQL): no table lookup needed
CREATE INDEX idx_orders_cover ON orders (customer_id, created_at DESC) INCLUDE (total);
```

---

### 11.3 When does an index **not** help?

> 🗣️ "Common cases:
> - A **function on the column**: `WHERE LOWER(email) = ...` can't use a plain index on `email`. Use an expression index, or store it normalized.
> - **Leading wildcard**: `LIKE '%tap'` can't use a B-tree. Use full-text search or a search engine.
> - **Low selectivity**: an index on `is_active`, where half the rows are true, is rarely useful.
> - **Type mismatch**: comparing a string column to a number can force a conversion and a full scan.
> - **Wrong column order** in a composite index for that query.
> - The query returns a **large share of the table**, so a sequential scan is actually cheaper.
> - **Stale statistics**, so the planner guesses wrong."

---

### 11.4 How do you optimize a slow SQL query?

> 🗣️ "I follow the same steps every time:
> 1. **Find the real offenders.** The slow query log or `pg_stat_statements` shows which queries take the most **total** time. A 50 ms query run 10,000 times a minute matters more than one 5-second report.
> 2. **`EXPLAIN ANALYZE`** it. I look for full table scans on big tables, a big difference between **estimated and actual rows** (stale stats), sorts spilling to disk, and nested loops over large sets.
> 3. **Fix in order of cost:** add or adjust an **index**, select only the columns needed, **rewrite** the query (avoid functions on indexed columns, replace correlated subqueries with joins), **paginate**, fix **N+1** in the application code, or **cache** the result.
> 4. **Retest with realistic data volume**, because a query that's fast on 1,000 rows can be terrible on 10 million.
> 5. **Verify in production** metrics after deploy."

**What to look for in EXPLAIN**

| PostgreSQL | MySQL | Meaning |
|---|---|---|
| `Seq Scan` on a big table | `type: ALL` | Full scan, probably missing an index |
| `Index Scan` / `Index Only Scan` | `type: ref/range`, `Using index` | Good |
| Estimated rows ≠ actual rows | `rows` far from reality | Stale stats, so run `ANALYZE` |
| `Sort` with `external merge` | `Using filesort` | Sort not covered by an index |
| `Hash Join` / `Nested Loop` | `Using temporary` | Check join columns are indexed |

🏢 **From my work (S2):** "The biggest database win wasn't a slow query, it was **too many fast queries**: 1,811 per request. Always check query **count** per request, not just query duration."

---

### 11.5 Transactions and ACID

> 🗣️ "A transaction groups several operations so they **all succeed or all fail together**. Placing an order means inserting the order, reducing stock, and recording the payment. You never want half of that.
> **ACID**:
> - **Atomicity**: all or nothing.
> - **Consistency**: constraints and rules always hold.
> - **Isolation**: concurrent transactions don't see each other's half-finished work.
> - **Durability**: once committed, it survives a crash.
>
> My practical rules: keep transactions **short**, and **never make an HTTP call to another service inside a transaction**. It holds locks while you wait on the network."

---

### 11.6 Isolation levels and anomalies

> 🗣️ "Isolation levels trade **correctness** for **concurrency**. The anomalies they prevent: a **dirty read** (reading uncommitted data), a **non-repeatable read** (reading the same row twice and getting different values), and a **phantom** (re-running a query and new rows appear)."

| Level | Dirty read | Non-repeatable read | Phantom |
|---|---|---|---|
| Read Uncommitted | possible | possible | possible |
| **Read Committed** (PostgreSQL default) | prevented | possible | possible |
| **Repeatable Read** (MySQL InnoDB default) | prevented | prevented | possible in the standard (PG and InnoDB mostly prevent it) |
| Serializable | prevented | prevented | prevented |

> 🗣️ "The bug I actually see in real code is the **lost update**: two requests read stock = 5, both subtract 1, both write 4. Neither the default level nor 'check then update' prevents that. The fix is an **atomic update**, a **row lock**, or **optimistic locking**. See 11.8."

---

### 11.7 Deadlocks

> 🗣️ "A deadlock is when transaction A holds lock 1 and waits for lock 2, while B holds lock 2 and waits for lock 1. Neither can continue. The database detects it and **kills one** of them.
> Prevention: always lock rows in a **consistent order** (for example sorted by ID), keep transactions **short**, make sure updates use **indexes** (so they lock fewer rows), and **retry** the victim transaction safely, which means it must be idempotent."

---

### 11.8 Optimistic vs pessimistic locking

> 🗣️ "**Optimistic** locking assumes conflicts are rare. Each row has a `version`. You update `WHERE version = the one you read`, and if 0 rows changed, someone else got there first, so you retry or show an error. No locks are held, so it's great for things like editing a product in an admin panel.
> **Pessimistic** locking assumes conflicts are likely. You `SELECT … FOR UPDATE` inside a transaction, which locks the row until you commit. It's good for short, high-contention critical sections.
> For **stock**, there's often an even simpler option: a **single atomic conditional update**."

```sql
-- Atomic: no race, no explicit lock
UPDATE inventory SET qty = qty - 2
WHERE sku = 'TAP-001' AND qty >= 2;      -- 0 rows affected = not enough stock

-- Optimistic
UPDATE products SET price = 99, version = version + 1
WHERE id = 7 AND version = 3;            -- 0 rows = conflict, retry

-- Pessimistic
BEGIN;
SELECT * FROM accounts WHERE id = 1 FOR UPDATE;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
COMMIT;
```

❓ **Follow-up:** *What's `FOR UPDATE SKIP LOCKED`?* Each worker grabs rows nobody else has locked. It's the standard way to build a simple job queue in PostgreSQL or MySQL 8.

---

### 11.9 Normalization vs denormalization

> 🗣️ "**Normalization** means storing each fact **once**: a customer's address lives in one table, and orders reference it. That avoids inconsistencies when data changes. **Denormalization** means **duplicating** data on purpose so reads are faster, like storing `customer_name` on the order, or a pre-computed product listing table.
> Rule of thumb: normalize the **source of truth**, and denormalize **read models** for hot read paths, with a clear process to keep them in sync."

🏢 **From your Magento background:** Magento's **indexers** are exactly this. Normalized EAV source tables, plus denormalized flat and index tables (and Elasticsearch/OpenSearch) built for fast storefront reads. See 18.4.

---

### 11.10 SQL vs NoSQL: how do you choose?

> 🗣️ "I choose based on **data shape and access patterns**, not fashion.
> **Relational** (PostgreSQL, MySQL) when data is **relational** and **correctness** matters: orders, payments, inventory, users. You get joins, constraints, transactions and flexible ad-hoc queries.
> **Document** (MongoDB) when data is naturally **self-contained documents** that are read and written together, the schema varies (like product attributes that differ by category), or you need easy horizontal scaling.
> **Key-value** (Redis, DynamoDB) for simple lookups at massive scale. **Search engines** (Elasticsearch/OpenSearch) for full-text search and faceted filtering.
> Most real systems use **several**. That's called polyglot persistence."

---

### 11.11 MongoDB schema design: embed or reference?

> 🗣️ "In MongoDB you design for **how the data is read**. **Embed** when the data is **owned** by the parent, **bounded** in size, and **read together**: order line items inside the order, addresses inside the customer. **Reference** when the data is **shared**, **large**, **unbounded**, or **changes independently**: products referenced from orders, comments referenced from a post with thousands of comments.
> Watch out for **unbounded arrays**. A document has a **16 MB limit**, and huge arrays are slow to update anyway."

❓ **Follow-ups**
- *Does MongoDB support transactions?* Yes, multi-document ACID transactions (on replica sets and sharded clusters). But good schema design means most writes touch **one document**, which is atomic anyway.
- *Common patterns?* Extended reference (copy a few fields like product name into the order), Bucket (group time-series readings into hourly documents), Computed (store pre-calculated totals).

---

### 11.12 MongoDB indexes and the aggregation pipeline

> 🗣️ "For compound indexes in MongoDB I follow the **ESR rule**: **E**quality fields first, then **S**ort fields, then **R**ange fields. And I check with `explain('executionStats')`: I want `IXSCAN`, not `COLLSCAN`, and `totalDocsExamined` close to `nReturned`.
> The **aggregation pipeline** is MongoDB's version of `GROUP BY` and joins: a list of stages like `$match`, `$group`, `$sort`, `$lookup`, `$project`. Put **`$match` first** so it can use an index and shrink the data early."

```javascript
db.orders.aggregate([
  { $match: { status: 'paid', createdAt: { $gte: ISODate('2026-01-01') } } },
  { $group: { _id: '$customerId', total: { $sum: '$amount' }, orders: { $sum: 1 } } },
  { $sort: { total: -1 } },
  { $limit: 10 },
]);
```

---

### 11.13 The N+1 query problem

> 🗣️ "N+1 is one query to load a list, then **one extra query per item**. Load 100 orders, then 100 separate queries for each order's customer. Each query is fast, but together they kill performance. It's very common with **ORMs** that lazy-load relations, and with **GraphQL** resolvers.
> Fixes: a **JOIN**, **eager loading** in the ORM (`include` / `with`), a single **`WHERE id IN (...)`** batch, or **DataLoader**. Then check the result isn't a giant row explosion from joining several one-to-many relations at once."

---

### 11.14 Connection pooling

> 🗣️ "Opening a database connection is slow (TCP, TLS, authentication), so a **pool** keeps a set of open connections and reuses them. The key is **sizing**. More connections isn't faster: past a point, the database spends its time switching between them.
> The math people forget: **pool size × number of app instances** must stay below the database's `max_connections`. Ten pods with a pool of 20 is 200 connections. If autoscaling adds pods, you can run the database out of connections. That's when you add a pooler like **PgBouncer** or RDS Proxy."

---

### 11.15 Replication and read replicas

> 🗣️ "With replication, a **primary** handles writes and one or more **replicas** copy its data, usually **asynchronously**. You send reads to replicas to spread load. Replicas also help with failover.
> The catch is **replication lag**: a user updates their profile, the next page reads from a replica that's 200 ms behind, and they see the old data. Fixes: read from the **primary right after a write** (read-your-own-writes) or for sensitive flows like checkout, and use replicas for things that tolerate slight staleness, like listings and reports."

---

### 11.16 Partitioning and sharding

> 🗣️ "**Partitioning** splits one big table into pieces **inside one database**, often by date. Queries for 'this month' only touch one partition, and dropping old data is instant. **Sharding** splits data **across multiple database servers** by a **shard key** (like `customer_id`), so writes scale beyond one machine.
> Choosing the shard key is the hard part: it needs **high cardinality**, **even distribution**, and it should match your **most common queries**, because cross-shard queries are expensive. A monotonically increasing key like a timestamp sends all new writes to one hot shard.
> Sharding is a last resort. First come indexing, caching, read replicas and vertical scaling."

---

### 11.17 Zero-downtime database migrations (expand and contract)

> 🗣️ "During a rolling deploy, the **old and new code run at the same time**, so every schema change must work with both. I use **expand and contract**. Renaming a column, for example:
> 1. **Expand**: add the new column (nullable).
> 2. Deploy code that **writes to both** columns.
> 3. **Backfill** old rows in small batches.
> 4. Deploy code that **reads from the new** column.
> 5. Stop writing the old column.
> 6. **Contract**: drop the old column in a **later** release.
>
> Also: create indexes **concurrently** (`CREATE INDEX CONCURRENTLY` in Postgres, online DDL in MySQL), and use tools like gh-ost for big table changes, so you don't lock a large table during peak traffic."

⚠️ **Trap:** "I'd just rename the column in the migration." That breaks every running old instance instantly.

---

### 11.18 ORMs: pros and cons

> 🗣️ "ORMs like **Prisma**, TypeORM or Drizzle give productivity, type safety, migrations and protection from SQL injection. The downsides: they **hide the SQL**, so N+1 and inefficient queries slip in, and complex reports are awkward to express.
> My approach: the ORM for normal CRUD, **raw SQL or a query builder** for reporting and hot paths, and **query logging on in development** so I can see what's actually being run."

---

### ⚡ Database rapid-fire

| Question | One-line answer |
|---|---|
| Primary key vs unique key? | One PK per table, not null, identifies the row; there can be many unique constraints. |
| Clustered index? | The table's rows are physically ordered by it. InnoDB clusters by primary key. |
| INNER vs LEFT JOIN? | INNER returns only matches on both sides; LEFT returns all left rows, with NULLs where there's no match. |
| UNION vs UNION ALL? | UNION removes duplicates (slower); UNION ALL keeps everything. |
| DELETE vs TRUNCATE vs DROP? | Remove rows (can filter, logged) / remove all rows fast / remove the whole table. |
| View vs materialized view? | A view is a saved query, computed every time. A materialized view stores the result and is refreshed periodically. |
| UUID vs auto-increment? | Auto-increment is small, ordered and fast but guessable. Random UUIDv4 fragments indexes. **UUIDv7/ULID** are time-ordered and the best of both. |
| Soft delete? | A `deleted_at` column. Easy undo and audit, but every query and unique constraint must account for it. |
| `WHERE` vs `HAVING`? | `WHERE` filters rows before grouping; `HAVING` filters groups after aggregation. |
| CHAR vs VARCHAR? | Fixed length (padded) vs variable length. |
| What is a stored procedure? | Logic that runs inside the DB. Fast and close to the data, but harder to version, test and scale. |

---

# 12. Redis, Queues and Event-Driven Systems

### 12.1 What is Redis, and why is it so fast?

> 🗣️ "Redis is an **in-memory** data store. It keeps everything in RAM, so reads and writes take well under a millisecond. It executes commands on **a single thread**, which sounds slow but means **no locking overhead**, and every single command is **atomic**. It's not just a key-value cache either: it has real data structures, which is why it's used for so many things."

| Structure | Typical use |
|---|---|
| String | Cache entries, counters (`INCR`), simple flags |
| Hash | An object's fields (session data, a cart) |
| List | Simple queues, recent items |
| Set | Unique tags, "who's online" |
| Sorted Set | Leaderboards, sliding-window rate limits, delayed jobs |
| Stream | Durable event log with consumer groups |
| HyperLogLog | Approximate unique counts (unique visitors) using tiny memory |
| Geo | "Stores near me". Great for a store locator |

---

### 12.2 Caching patterns

> 🗣️ "The most common one is **cache-aside**: the app checks the cache, on a miss reads the database, then stores the result with a TTL. On a write, it updates the database and **deletes** the cache key so the next read reloads it.
> Other patterns: **read-through** (the cache itself loads from the DB), **write-through** (write to cache and DB together, so the cache is always fresh), and **write-behind** (write to cache, flush to the DB later. Fast, but you can lose data)."

```javascript
async function getProduct(sku) {
  const key = `product:v2:${sku}`;                 // version prefix for easy mass-invalidation
  const hit = await redis.get(key);
  if (hit) return JSON.parse(hit);

  const product = await db.getProduct(sku);
  const ttl = 300 + Math.floor(Math.random() * 60); // jitter: keys don't all expire together
  await redis.set(key, JSON.stringify(product), { EX: ttl });
  return product;
}

async function updateProduct(sku, data) {
  await db.updateProduct(sku, data);
  await redis.del(`product:v2:${sku}`);             // delete, don't overwrite
}
```

❓ **Follow-ups**
- *Why delete on write instead of updating the cache?* Two concurrent writes can update the cache in the wrong order and leave stale data. Deleting is simpler, and the next read rebuilds it from the source of truth.
- *Why always a TTL?* It's a safety net. If an invalidation is ever missed, the stale data still expires.

---

### 12.3 Cache invalidation and eviction

> 🗣️ "**Invalidation** is deciding when cached data is wrong. Options: a **TTL** (simple, slightly stale), **delete on write**, **event-driven** (a 'product updated' event clears related keys, including CDN and Next.js tags), and **versioned keys** (bump `v2` to `v3` to drop everything at once).
> **Eviction** is what Redis does when memory is full, controlled by `maxmemory-policy`. For a pure cache I use **`allkeys-lru`** or `allkeys-lfu`. The default, `noeviction`, makes writes fail when full, which is right if Redis holds data you can't lose."

---

### 12.4 Cache stampede (thundering herd)

> 🗣️ "A stampede is when a **popular key expires** and hundreds of requests miss at the same moment, and **all of them hit the database** together. That can take the database down.
> Fixes:
> - **Request coalescing / single-flight**: only one request rebuilds the key; the others wait for its result.
> - A short **lock** in Redis (`SET key NX PX`) so only one instance rebuilds.
> - **Stale-while-revalidate**: keep serving the old value while one request refreshes it in the background.
> - **TTL jitter**, so keys don't expire at the same time.
> - **Pre-warming** hot keys before a big sale."

```javascript
const inFlight = new Map();
async function singleFlight(key, loader) {
  if (!inFlight.has(key)) {
    inFlight.set(key, loader().finally(() => inFlight.delete(key)));
  }
  return inFlight.get(key);   // everyone awaits the same promise
}
```

---

### 12.5 What happens if Redis goes down?

> 🗣️ "It depends on what Redis is doing for us.
> If it's **only a cache**, the app should keep working, just slower. Redis calls need **short timeouts** and a **circuit breaker**, so we stop waiting on a dead Redis and go straight to the database. But I'd **protect the database** from the sudden extra load with rate limits, or by temporarily disabling non-critical features.
> If Redis holds **sessions, locks, rate-limit counters or queues**, it's **critical infrastructure**, and it needs high availability: replicas with automatic failover (Sentinel, or a managed service like ElastiCache or Azure Cache for Redis), plus persistence if the data can't be lost."

---

### 12.6 Distributed locks with Redis

> 🗣️ "The basic lock is `SET lock:key <unique-token> NX PX 30000`: set it only if it doesn't exist, with an expiry so a crashed holder doesn't block forever. To release, delete it **only if the token is still yours**, using a small Lua script, so you don't delete someone else's lock after yours expired.
> The honest caveat: Redis locks are fine for **efficiency**, like 'don't run this cron twice'. For **correctness-critical** things like 'never double-charge', I rely on **database constraints and transactions**, because a lock can expire while a slow process still thinks it holds it."

---

### 12.7 Redis persistence and high availability

> 🗣️ "Redis has two persistence options. **RDB** takes periodic snapshots: compact and fast to restart, but you can lose the last few minutes. **AOF** logs every write: more durable, bigger files. Many setups use both. For availability, **replicas plus Sentinel** give automatic failover, and **Redis Cluster** shards data across nodes (16,384 hash slots) when one machine isn't enough. In the cloud I'd normally use the managed service."

---

### 12.8 Redis Pub/Sub vs Streams vs a real message queue

> 🗣️ "**Pub/Sub** is fire-and-forget: a message goes only to subscribers **connected right now**. If a service is down, it misses the message forever. Fine for live notifications, or broadcasting 'clear your local cache'.
> **Redis Streams** store messages and support **consumer groups and acknowledgements**, so they're durable-ish.
> For important business events I use a **proper broker** (SQS, Service Bus, Kafka): durable storage, retries, dead-letter queues, monitoring."

---

### 12.9 Why use a queue at all? Queue vs topic

> 🗣️ "A queue **decouples** the producer from the consumer **in time**. The API accepts an order and returns immediately. Sending the email, updating the ERP and generating the invoice happen in the background. Benefits: faster responses, **absorbing traffic spikes** (the queue grows, workers catch up), **retries** when a downstream system is down, and scaling workers independently.
> A **queue** delivers each message to **one** consumer (work distribution). A **topic** delivers each message to **every subscription** (fan-out). 'Order placed' goes to a topic, and the email service, the ERP sync and analytics each get their own copy.
> The cost: **eventual consistency**, duplicates, ordering issues, and more moving parts to monitor."

💡 **Picture it:** A queue is a ticket line where each customer goes to one free counter. A topic is a newsletter where every subscriber gets their own copy.

---

### 12.10 Delivery guarantees

> 🗣️ "**At-most-once**: the message might be lost but never duplicated. **At-least-once**: never lost, but **can be delivered more than once**. This is what SQS, Service Bus and Kafka give you by default. **Exactly-once** is mostly a marketing term across systems. In practice you get **'effectively once'** by combining **at-least-once delivery with idempotent consumers**."

---

### 12.11 Idempotent consumers

> 🗣️ "Because messages can arrive twice, after a retry, a timeout or a consumer crash before acknowledging, the consumer must be safe to run twice. My usual pattern: record the **message ID** (or business ID, like order ID plus event type) in a `processed_messages` table **in the same transaction** as the business change. If it's already there, skip. And **acknowledge only after the commit**."

```javascript
async function handle(msg) {
  await db.transaction(async (tx) => {
    const res = await tx.query(
      `INSERT INTO processed_messages (message_id) VALUES ($1)
       ON CONFLICT DO NOTHING`, [msg.id]);
    if (res.rowCount === 0) return;            // duplicate, already done
    await applyBusinessChange(tx, msg.body);   // same transaction
  });
  // complete/ack the message only after commit
}
```

> 🗣️ "Sometimes the operation is **naturally** idempotent, like 'set status to SHIPPED' or an upsert, and then you don't need the table."

---

### 12.12 Retries, backoff, and dead-letter queues

> 🗣️ "I retry **transient** failures (timeouts, 503s, deadlocks) with **exponential backoff and jitter**: wait 1 s, 2 s, 4 s, plus randomness so all consumers don't retry at the same instant. I **don't** retry **permanent** failures like invalid data, because they'll fail every time.
> After a maximum number of attempts, the message goes to a **dead-letter queue** with the error details. I **alert** on the DLQ (count and age), fix the cause, and then **replay** messages with a controlled tool. That's safe because consumers are idempotent.
> A **poison message** is one that always crashes the consumer. Without a DLQ it blocks or loops forever."

❓ **Follow-ups**
- *Visibility timeout / peek-lock?* When a consumer receives a message it's **hidden**, not deleted. If the consumer doesn't complete it in time (it crashed), it reappears for another consumer. Set the timeout longer than processing time, or renew the lock for long jobs.
- *Service Bus default?* Messages dead-letter after `MaxDeliveryCount`, 10 by default. SQS uses a redrive policy with `maxReceiveCount`.

---

### 12.13 Message ordering

> 🗣️ "**Global** ordering kills scalability, because only one consumer can process at a time. Usually you only need ordering **per entity**: events for the **same order** must be in sequence, but different orders can be processed in parallel. So you partition by a key: **SQS FIFO message group ID**, **Service Bus sessions**, or the **Kafka partition key**.
> Even then, consumers should handle out-of-order and duplicate events, for example by checking a version or timestamp and ignoring older updates."

---

### 12.14 The outbox pattern

> 🗣️ "The problem is the **dual write**: you save the order to the database **and** publish an 'OrderPlaced' event. If the app crashes between the two, the database and the event stream disagree. Either the order exists with no event, or an event exists for an order that was rolled back.
> The **outbox pattern** fixes it: in the **same database transaction**, you insert the order **and** a row in an `outbox` table. A separate relay process reads the outbox and publishes to the broker, marking rows as sent. If publishing fails, it retries. It's at-least-once, so consumers are idempotent. An alternative is **CDC** (change data capture, like Debezium) reading the database log."

```
BEGIN;
  INSERT INTO orders (...) VALUES (...);
  INSERT INTO outbox (event_type, payload) VALUES ('OrderPlaced', '{...}');
COMMIT;
-- relay: SELECT ... FROM outbox WHERE sent_at IS NULL ... publish ... UPDATE outbox SET sent_at = now()
```

---

### 12.15 AWS SNS/SQS vs Azure Service Bus vs Kafka

> 🗣️ "I start from the **need**, then pick the product. They overlap a lot:"

| | AWS SNS + SQS | Azure Service Bus | Kafka |
|---|---|---|---|
| Model | SNS topic fans out to SQS queues | Queues **and** topics/subscriptions in one service | Distributed **log**; consumers track their own offset |
| Ordering | SQS FIFO per message group | **Sessions** per session ID | Per **partition** |
| Duplicates | FIFO has a 5-minute dedup window | Duplicate detection window | Idempotent producer; consumers still need care |
| Dead-letter | Redrive policy | Built-in DLQ per queue/subscription | DIY (a separate topic) |
| Replay old messages | No (deleted once consumed) | No | **Yes**, retention-based |
| Extras | Simple, serverless, huge scale | Scheduled messages, transactions, peek-lock | Very high throughput, streaming, event sourcing |
| Best for | Simple decoupling on AWS | Enterprise workflows on Azure | Event streaming, analytics, many readers of history |

🏢 **From my work:** you've used Service Bus and SNS/SQS. Prepare **Story S12** with one concrete flow: what was published, who consumed it, how duplicates and failures were handled.

---

### 12.16 Events vs commands, and event-driven trade-offs

> 🗣️ "A **command** says *do this*: `SendInvoice`. It has one handler, which can refuse. An **event** says *this happened*: `OrderPlaced`. It's past tense, a fact, and **any number** of services can react without the publisher knowing who.
> Event-driven systems are loosely coupled and resilient, but harder to **trace** and **debug**. A business flow is spread across services, so you need **correlation IDs**, distributed tracing, and good DLQ monitoring. And the UI must handle **eventual consistency**: 'Your order is being processed' instead of pretending everything finished instantly."

---

### 12.17 Background jobs in Node.js

> 🗣️ "For background jobs inside a Node system, I use **BullMQ** on Redis. It gives retries with backoff, delayed and scheduled jobs, concurrency limits, priorities, rate limiting and a dashboard. Workers run as **separate processes or pods** from the API, so heavy jobs never slow down user requests and can scale independently. For cross-service events I'd still use the cloud broker."

---

### ⚡ Redis and messaging rapid-fire

| Question | One-line answer |
|---|---|
| Redis single-threaded: a bottleneck? | Rarely. It's memory-bound and very fast; scale with replicas or Cluster. Avoid slow commands like `KEYS *` (use `SCAN`). |
| Redis vs Memcached? | Redis has data structures, persistence and replication; Memcached is a simpler, multi-threaded pure cache. |
| What's a hot key? | One key getting a huge share of traffic. Fix with local in-memory caching or by splitting the key. |
| Backpressure in a queue system? | When consumers can't keep up: scale consumers within downstream limits, or slow down / reject producers. |
| Competing consumers? | Many workers reading one queue; each message goes to one of them. That's how you scale processing. |
| Saga vs 2PC? | Two-phase commit locks across services (fragile). A saga uses local transactions plus compensations. See 15.12. |
| Event sourcing? | Store every **change** as an event; current state is derived by replaying them. Powerful audit trail, high complexity. |

---

# 13. Docker, Kubernetes and CI/CD

### 13.1 Containers vs VMs; image vs container

> 🗣️ "A **virtual machine** emulates a whole computer with its own operating system kernel. It's heavy, slow to boot, and uses GBs. A **container** is just an **isolated process** that **shares the host's kernel**. It's lightweight, starts in seconds, and is MBs.
> An **image** is the packaged, **read-only template**: your app, its dependencies, and config. A **container** is a **running instance** of that image. Like a class and an object: one image, many containers."

---

### 13.2 Dockerfile best practices for Node.js

> 🗣️ "My checklist: a **small official base image** with a pinned version. A **multi-stage build**, so build tools and dev dependencies never reach the final image. **Copy `package*.json` first** and install, *then* copy the source, so the dependency layer is cached when only code changes. **`npm ci --omit=dev`**. Run as a **non-root user**. A `.dockerignore` for `node_modules`, `.git` and `.env`. **No secrets in layers**. And start with `node server.js` directly, not `npm start`, so signals reach Node."

```dockerfile
# ---- build stage ----
FROM node:22-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build && npm prune --omit=dev

# ---- runtime stage ----
FROM node:22-alpine
ENV NODE_ENV=production
WORKDIR /app
COPY --from=build --chown=node:node /app/node_modules ./node_modules
COPY --from=build --chown=node:node /app/dist ./dist
COPY --chown=node:node package.json ./
USER node
EXPOSE 3000
CMD ["node", "dist/server.js"]
```

---

### 13.3 Layers and build caching

> 🗣️ "Each Dockerfile instruction creates a **layer**, and Docker **reuses cached layers** until something changes. Once a layer changes, every layer **after** it rebuilds. So I order instructions from **least to most frequently changing**: base image, then system packages, then dependency manifests and install, then source code. Get that wrong and every code change reinstalls all npm packages."

---

### 13.4 The PID 1 problem in Node containers

> 🗣️ "Inside a container your app usually runs as **PID 1**, and Linux treats PID 1 specially: it **doesn't get default signal handling**. So if Node has no SIGTERM handler, `docker stop` or a Kubernetes shutdown doesn't stop it gracefully. It waits for the timeout and then gets killed. Also, `npm start` as the command means npm is PID 1 and **may not forward signals** to Node.
> Fixes: run `node` directly in `CMD`, **handle SIGTERM** in the app (see 4.13), and optionally use a tiny init like `tini` (or `docker run --init`) to forward signals and reap zombie processes."

---

### 13.5 Volumes, networks, and Docker Compose

> 🗣️ "A container's filesystem is **thrown away** with the container, so anything that must survive goes in a **volume**: database files, uploads in local dev. Containers on the same **Docker network** can reach each other **by service name** through built-in DNS (`postgres:5432`). **Docker Compose** describes a multi-container setup (API, Postgres, Redis) in one YAML file, which is perfect for local development. In production, app containers should be **stateless and disposable**, and data lives in managed databases and object storage."

---

### 13.6 Kubernetes core objects

> 🗣️ "- **Pod**: the smallest unit; one or more containers sharing a network and storage. Pods are **disposable**.
> - **Deployment**: keeps N replicas of a pod running and handles **rolling updates and rollbacks**.
> - **Service**: a **stable name and IP** that load-balances to the current healthy pods, because pod IPs keep changing.
> - **Ingress** (or the newer **Gateway API**): routes external HTTP traffic (domains, paths, TLS) to Services.
> - **ConfigMap / Secret**: configuration and sensitive values injected into pods.
> - **HPA**: automatically scales replicas.
> - Also good to know: **StatefulSet** (stable identity, for databases), **DaemonSet** (one pod per node, for log agents), **Job/CronJob** (run-to-completion tasks)."

💡 **Picture it:** Deployment is the manager saying "always keep 3 cashiers on shift". Pods are the cashiers. The Service is the single "Pay here" sign that sends you to whichever cashier is free.

---

### 13.7 ConfigMap and Secret

> 🗣️ "ConfigMaps hold **non-sensitive** config; Secrets hold sensitive values. But a plain Kubernetes Secret is only **base64-encoded**, not encrypted. So for real security I enable encryption at rest, restrict access with **RBAC**, and preferably sync secrets from **AWS Secrets Manager or Azure Key Vault** using something like the External Secrets Operator or the CSI driver. And config is never baked into the image: the same image goes to every environment."

---

### 13.8 Liveness, readiness, and startup probes

> 🗣️ "**Readiness** asks *should this pod get traffic right now?* If it fails, the pod is **removed from the Service** but not restarted. Use it during startup, during shutdown, or when temporarily overloaded.
> **Liveness** asks *is this process stuck?* If it fails, Kubernetes **restarts** the container.
> **Startup** protects slow-starting apps: liveness isn't checked until startup succeeds.
> The classic mistake is a **liveness probe that checks the database**. If the DB has a blip, Kubernetes restarts **every** pod at once, which turns a small problem into a full outage. Liveness should only check that the process itself is responsive."

⚠️ **Trap:** Same endpoint for liveness and readiness, with dependency checks in both.

---

### 13.9 Requests, limits, and autoscaling (HPA)

> 🗣️ "**Requests** are what the pod is **guaranteed**, and the scheduler uses them to place pods on nodes. **Limits** are the **maximum**. Going over the memory limit gets the container **OOMKilled** (exit code 137). Going over the CPU limit **throttles** it, which for Node shows up as random latency spikes.
> The **HPA** adds or removes replicas based on metrics: CPU, memory, or custom metrics like requests per second or **queue length**. But scaling only helps if the bottleneck is in the pods. If the **database** is the bottleneck, more pods just means more pressure on it. And remember new pods take time to start, so for a known traffic spike, I **pre-scale**."

❓ **Follow-ups**
- *Node and memory limits?* Make sure V8's heap limit fits inside the container limit (`--max-old-space-size`, about 75% of the limit), or V8 may try to grow past it and get OOMKilled.

---

### 13.10 Rolling deployments and rollback

> 🗣️ "A rolling update replaces pods **gradually**: start a new one, wait until it's **ready**, then remove an old one. `maxSurge` and `maxUnavailable` control the pace. **Readiness probes** are what make it safe, because traffic only goes to pods that are actually ready.
> Rollback is `kubectl rollout undo`, or redeploying the previous image tag. But rollback only works if the **database changes are backward compatible** (expand and contract, 11.17), because old and new versions run side by side during the rollout."

---

### 13.11 Blue-green vs canary vs feature flags

> 🗣️ "**Blue-green**: run two full environments and **switch all traffic** from blue to green at once. Instant rollback (switch back), but it needs double capacity. **Canary**: send a **small percentage** of traffic, say 5%, to the new version, watch error rates and latency, then increase gradually. It limits the blast radius but needs traffic splitting and good metrics. **Feature flags** separate **deploy** from **release**: the code ships switched off and you turn it on for internal users, then 10%, then everyone. Rollback is flipping a switch.
> I like to combine canary deploys for **infrastructure risk** with feature flags for **product risk**."

---

### 13.12 What does a good CI/CD pipeline look like?

> 🗣️ "On every pull request: install with the lockfile, **lint**, **type-check**, **unit tests**, **integration tests**, a **security scan** of dependencies, and a build.
> On merge to main: build **one immutable artifact**, a Docker image tagged with the commit SHA, scan it, push it to the registry, deploy to **staging**, run **smoke and E2E tests**, then **promote the same image** to production, with an approval gate if needed. Then post-deploy checks and automatic rollback if error rates spike.
> The key principle: **build once, promote the same artifact everywhere**. Never rebuild per environment, or what you tested isn't what you ship."

---

### 13.13 Debugging a pod that's misbehaving

```bash
kubectl get pods                                  # status: CrashLoopBackOff? Pending? OOMKilled?
kubectl describe pod <pod>                        # events: probe failures, scheduling, image pull errors
kubectl logs <pod> --previous                     # logs from the crashed container
kubectl top pod <pod>                             # CPU/memory usage
kubectl get events --sort-by=.lastTimestamp       # recent cluster events
kubectl exec -it <pod> -- sh                      # look inside (if the image has a shell)
kubectl rollout history deploy/<name>             # what changed recently
```

> 🗣️ "`describe` and `logs --previous` usually tell me the story: OOMKilled, a failing probe, a missing secret, or a crash on startup."

---

### 13.14 Helm, Infrastructure as Code, and GitOps

> 🗣️ "**Helm** packages Kubernetes YAML into **templated charts**, so the same chart deploys to dev, staging and prod with different values. **Infrastructure as Code** (Terraform, Bicep, CloudFormation) defines cloud resources in version-controlled files, so environments are reproducible and reviewed like code. **GitOps** (Argo CD, Flux) takes it further: the cluster **continuously syncs itself to what's in Git**, so a deploy is just a merged pull request, and drift is corrected automatically."

---

### ⚡ Docker/K8s rapid-fire

| Question | One-line answer |
|---|---|
| `CMD` vs `ENTRYPOINT`? | ENTRYPOINT is the fixed executable; CMD gives default arguments (easy to override). |
| `COPY` vs `ADD`? | Use COPY. ADD also extracts archives and fetches URLs, which is surprising behaviour. |
| Alpine vs slim vs distroless? | Alpine is tiny but uses musl libc (some native modules break). Slim is Debian-based and compatible. Distroless has no shell, the smallest attack surface. |
| CrashLoopBackOff? | The container keeps crashing; Kubernetes waits longer between restarts. Check `logs --previous`. |
| Exit code 137? | Killed by SIGKILL, usually OOMKilled. |
| Pending pod? | Can't be scheduled: not enough CPU/memory requested on any node, or a node selector/taint mismatch. |
| ImagePullBackOff? | Wrong image name or tag, or missing registry credentials. |
| PodDisruptionBudget? | Ensures a minimum number of pods stay up during node maintenance. |
| Namespace? | A logical grouping in a cluster (per team or environment) for RBAC and quotas. |
| Sidecar? | A helper container in the same pod (log shipper, proxy, service mesh). |

---

# 14. Cloud: AWS and Azure *(new)*

You've used **both** AWS (SNS/SQS) and Azure (Service Bus). Say that. Being comfortable across clouds is a plus.

### 14.1 IaaS vs PaaS vs SaaS vs serverless

> 🗣️ "It's about **how much you manage**. **IaaS**: you rent virtual machines and manage the OS and everything above it (EC2, Azure VMs). **PaaS**: you deploy your app and the platform runs it (App Service, Elastic Beanstalk, managed Kubernetes to a degree). **Serverless / FaaS**: you deploy functions that run on demand and scale to zero, and you pay per execution (Lambda, Azure Functions). **SaaS**: you just use the software (Shopify, Salesforce).
> Moving up the list means less operational work but less control."

---

### 14.2 Serverless: when to use it, and the cold start problem

> 🗣️ "Serverless is great for **event-driven, spiky or low-traffic** workloads: processing an uploaded image, handling a webhook, a scheduled job, a queue consumer. You pay only when it runs and scaling is automatic.
> The downsides: **cold starts** (the first request after idle is slower while the runtime spins up), **execution time limits**, harder **local debugging**, and connection problems with databases. Thousands of concurrent functions can exhaust DB connections, which is why tools like RDS Proxy exist. For a steady, high-traffic API, containers are usually cheaper and more predictable."

---

### 14.3 Object storage and signed URLs

> 🗣️ "Files (product images, invoices, exports) go to **object storage**: S3 or Azure Blob Storage. It's cheap, durable and effectively unlimited. Our API server never streams large uploads itself. Instead it creates a **short-lived signed URL** (an S3 presigned URL or an Azure SAS token), and the browser **uploads directly** to storage. Downloads work the same way, or go through a **CDN**. Buckets stay **private** by default."

---

### 14.4 IAM and least privilege

> 🗣️ "Every service gets an **identity** with **only the permissions it needs**. The order service can write to the orders queue, but not delete the bucket. On AWS that's an **IAM role** attached to the workload; on Azure, a **Managed Identity**. The big benefit: **no access keys stored anywhere**. The platform provides short-lived credentials automatically. Long-lived keys in environment variables are the most common way cloud accounts get compromised."

---

### 14.5 High availability and disaster recovery

> 🗣️ "A **region** is a geographic area; an **availability zone** is a separate data centre within it. For **high availability**, I run across **multiple AZs**: load balancer, app instances and a multi-AZ database, so one data centre failing doesn't take us down. **Disaster recovery** covers losing a whole region, with backups or replicas in another region.
> Two numbers drive the design: **RPO** (how much data can we afford to lose?) and **RTO** (how long can we be down?). Lower numbers cost more, so they're a business decision, not just a technical one."

---

### 14.6 AWS ↔ Azure cheat sheet

| Need | AWS | Azure |
|---|---|---|
| Virtual machines | EC2 | Virtual Machines |
| Containers (managed K8s) | EKS | AKS |
| Simple containers | ECS / Fargate, App Runner | Container Apps |
| Serverless functions | Lambda | Functions |
| Object storage | S3 | Blob Storage |
| Relational DB | RDS / Aurora | Azure SQL, Database for PostgreSQL/MySQL |
| NoSQL | DynamoDB | Cosmos DB |
| Cache | ElastiCache | Azure Cache for Redis |
| Queue | SQS | Service Bus queues / Storage Queues |
| Pub/sub | SNS, EventBridge | Service Bus topics, Event Grid |
| Streaming | Kinesis, MSK | Event Hubs |
| API gateway | API Gateway | API Management |
| CDN / edge | CloudFront | Front Door / CDN |
| Secrets | Secrets Manager, Parameter Store | Key Vault |
| Identity for workloads | IAM roles | Managed Identity |
| Monitoring | CloudWatch, X-Ray | Azure Monitor, Application Insights |
| Infrastructure as Code | CloudFormation, CDK | ARM, Bicep |

---

### 14.7 Cost awareness

> 🗣️ "Seniors are expected to think about cost. The usual big savings: **right-sizing** instances (most are over-provisioned), **autoscaling** down at night, **reserved instances or savings plans** for steady load, **spot instances** for interruptible work, **storage lifecycle rules** (move old files to cheaper tiers), **caching and CDN** to cut compute and egress, and watching **data transfer costs** between regions and AZs, which surprise everyone. And tag resources so you know which team or feature costs what."

---

### 14.8 Observability in the cloud

> 🗣️ "I want the three pillars in one place: **logs**, **metrics** and **traces**, all linked by a **trace or correlation ID**. That's CloudWatch and X-Ray on AWS, Azure Monitor and Application Insights on Azure, or a vendor like Datadog or New Relic, ideally fed by **OpenTelemetry** so we're not locked in. Then **alerts on symptoms users feel** (error rate, p99 latency, queue age), not on every CPU spike, with dashboards per service."

---

### ⚡ Cloud rapid-fire

| Question | One-line answer |
|---|---|
| Multi-AZ vs multi-region? | Multi-AZ survives a data-centre failure (standard). Multi-region survives a regional outage (expensive, complex). |
| Vertical vs horizontal scaling in the cloud? | A bigger instance vs more instances behind a load balancer. |
| VPC / VNet? | Your private network in the cloud. Databases go in private subnets with no public IP. |
| Security groups / NSGs? | Firewall rules: which ports and sources can reach a resource. |
| What is egress cost? | Paying for data **leaving** the cloud or crossing regions/AZs. |
| Shared responsibility model? | The provider secures the cloud itself; **you** secure what you put in it (config, IAM, data, code). |

---

# 15. System Design

> System design rounds aren't about the "right answer". They're about **how you think**: do you ask questions, make reasonable assumptions, start simple, find the bottlenecks, and explain trade-offs out loud?

### 15.1 The method (for a 45-minute round)

| Step | Time | What you do | What you say |
|---|---|---|---|
| 1. Clarify | ~5 min | Functional and non-functional requirements, what's out of scope | "Before I design anything, let me ask a few questions…" |
| 2. Estimate | ~3 min | Users, requests per second, storage, read/write ratio | "Let me do some quick numbers so I know what scale we're designing for." |
| 3. API | ~5 min | Main endpoints or operations | "These are the core operations the system needs…" |
| 4. Data model | ~5 min | Main entities, choice of database | "This data is relational and needs transactions, so…" |
| 5. High-level design | ~10 min | Draw the boxes and the request flow | "Let me walk through a request end to end." |
| 6. Deep dives | ~12 min | Bottlenecks, failures, scaling, consistency | "The hardest part here is ___. Let me focus on that." |
| 7. Wrap up | ~2 min | Summary, monitoring, what you'd do next | "To summarize… With more time I'd look at…" |

**Clarifying questions to always ask**
- Who are the users, and how many? Daily active users, peak vs average?
- What are the **must-have** features for this discussion?
- Read-heavy or write-heavy?
- What latency do users expect?
- How important is **consistency** vs **availability**? (Payments: consistency. A like counter: availability.)
- Single region or global?
- Any constraints: existing tech, team size, budget, compliance?

> 🗣️ **Opening line:** "Let me start by clarifying requirements, then do some rough numbers, and I'll start with a simple design and evolve it. Stop me anytime if you want to go deeper somewhere."

---

### 15.2 Back-of-the-envelope estimation cheat sheet

| Fact | Number to remember |
|---|---|
| Seconds in a day | 86,400 ≈ **100,000** (10⁵) |
| 1 million requests/day | ≈ **12 per second** |
| 100 million requests/day | ≈ **1,200 per second** |
| Peak vs average | Usually **2–3×**, and **10×+** during sales or launches |
| 1 million rows × 1 KB | ≈ **1 GB** |
| Typical read:write ratio | 10:1 to 100:1 for most web apps |

| Latency (rough) | |
|---|---|
| Memory read | ~100 nanoseconds |
| SSD random read | ~0.1 ms |
| Redis GET in the same data centre | < 1 ms |
| Indexed DB query | ~1–10 ms |
| Round trip within one region | ~1 ms |
| Round trip across continents | ~100–150 ms |

> 🗣️ "I'll state my assumptions and round aggressively. The point is the order of magnitude, not exact numbers."

---

### 15.3 Building blocks: when to reach for what

| Component | Reach for it when… |
|---|---|
| **CDN** | Static assets, images, cacheable pages, global users |
| **Load balancer** | More than one app instance; health checks |
| **API gateway** | Many services or clients; auth, rate limits, quotas at the edge |
| **Stateless app servers** | Always. Scale horizontally |
| **Relational DB** | Transactions, relationships, correctness (orders, payments) |
| **Document / key-value DB** | Flexible schema, simple access patterns, massive scale |
| **Cache (Redis)** | Hot reads, sessions, rate limits, counters |
| **Search engine** | Full-text search, filters with counts (facets), relevance |
| **Message queue / topic** | Slow work, spikes, decoupling, retries, fan-out |
| **Object storage** | Files, images, exports, backups |
| **Read replicas** | Read-heavy load on the DB |
| **Sharding** | Writes or data size beyond one DB server (last resort) |
| **WebSockets / SSE** | Real-time updates |

---

### 15.4 Design a URL shortener

**Clarify:** Create a short link, redirect to the long URL, optional custom alias and expiry, click analytics. Extremely **read-heavy**.

**Numbers:** Say 100M new links/month ≈ 40 writes/sec. At 100:1 that's ≈ 4,000 redirects/sec on average, more at peak. 500 bytes per link × 100M × 12 months × 10 years ≈ 6 TB. Fine for a sharded store.

**API**
- `POST /links` `{ longUrl, alias?, expiresAt? }` → `{ shortUrl }`
- `GET /{code}` → redirect

**Generating the short code:** 7 characters of base62 (a–z, A–Z, 0–9) gives 62⁷ ≈ **3.5 trillion** combinations.
- **Counter + base62**: unique and short, but sequential, so guessable. Each server can take **ranges** of IDs to avoid a bottleneck.
- **Random code + unique constraint**: simple and not guessable; on a rare collision, retry.
- **Hash of the URL**: the same URL gives the same code, but you still need collision handling.

> 🗣️ "I'd go with random 7-character codes and a unique constraint. Collisions are extremely rare at this scale, and a retry is cheap."

**Architecture:** Client → CDN/LB → stateless redirect service → **Redis cache** (hot links) → database (key-value or a relational table with the code as primary key). The **redirect path is separate** from the link-management API, because they scale very differently.

**Redirect: 301 or 302?** 301 is permanent, so browsers cache it, which means less load but **you lose analytics**. 302/307 hits you every time, so you can count clicks. Pick based on whether analytics matter.

**Analytics:** never slow down the redirect. Publish a click event to a **queue or stream**, and process it asynchronously into an analytics store.

**Deep dives:** abuse (rate limit creation, scan for malware/phishing URLs), expiry (TTL plus a cleanup job), hot links (cache, CDN), custom alias conflicts (unique constraint).

---

### 15.5 Design a rate limiter

**Clarify:** Per user, per API key, or per IP? Limits per endpoint? Distributed across many instances? What happens when the limiter itself is down?

**Where:** at the **API gateway** for coarse limits, plus in the app for business-specific ones (login attempts, OTP).

**Algorithms**

| Algorithm | How it works | Pros / cons |
|---|---|---|
| Fixed window | Count per minute; reset each minute | Simplest; allows a 2× burst at the window edge |
| Sliding window log | Store each request timestamp; count those in the last 60 s | Exact; more memory |
| Sliding window counter | Weighted mix of current and previous window | Good accuracy, little memory |
| **Token bucket** | Bucket refills at a steady rate; each request takes a token | Allows short bursts, caps the average. Most common |

> 🗣️ **Token bucket in one breath:** "Every user has a bucket that holds, say, 10 tokens and refills at 1 per second. Each request takes a token; an empty bucket means 429. It allows small bursts but controls the average rate."

**Implementation:** counters in **Redis**, updated **atomically** (a Lua script or `INCR` + `EXPIRE`), so all app instances share the same count.

```
-- Fixed window, simplest form
key = "rl:{userId}:{currentMinute}"
count = INCR key
if count == 1: EXPIRE key 60
if count > limit: return 429
```

**Response:** `429 Too Many Requests` with `Retry-After` (and `RateLimit-*` headers).

**Failure mode:** if Redis is down, **fail open** for normal APIs (availability matters more) but **fail closed** for sensitive endpoints like login and OTP (security matters more).

**Deep dives:** hot keys for very large customers, multi-region (each region limits locally, so the global limit is approximate), different tiers per plan.

---

### 15.6 Design a notification system

**Clarify:** Channels (email, SMS, push, in-app)? Transactional (OTP, order confirmation) vs marketing? Volume? User preferences and opt-out?

**Flow**
1. Services call `POST /notifications` (or publish an event) with the user, template, data, and an **idempotency key**.
2. The notification service **validates**, checks **user preferences and opt-outs**, stores the request, and publishes to a **per-channel queue**.
3. **Channel workers** render the template (language, localization), apply **rate limits** and quiet hours, and call the **provider** (SES/SendGrid for email, Twilio for SMS, FCM/APNs for push).
4. Delivery status is recorded. Provider **webhooks** update delivered, bounced or failed.
5. **Retries** with backoff for transient failures, a **DLQ** for permanent ones, and **failover** to a second provider for critical channels.

**Key design points**
- **Priority queues**: an OTP must never wait behind a million-email marketing campaign.
- **Idempotency**: never send the same order confirmation twice.
- **Templates** stored and versioned separately from code.
- **Compliance**: unsubscribe links, consent records, and region rules for marketing messages.

---

### 15.7 Design an e-commerce checkout (and survive a flash sale)

> 🏢 Your home ground. Speak from experience: "On the platform I work on…"

**Clarify:** Catalog browse, cart, checkout, payment, order, inventory. Catalog is very **read-heavy**. Checkout must be **correct**: no overselling, no double charges. Expect sale-day spikes.

**High-level architecture**
```
Users → CDN (static, images, cached pages)
      → Storefront (Next.js)
      → API Gateway
          → Catalog service  → Search index (OpenSearch) + cache (Redis)
          → Cart service     → Redis / DB
          → Inventory service→ DB (source of truth)
          → Order service    → Relational DB
          → Payment service  → Payment provider (PSP)
      → Message broker (OrderPlaced, PaymentCaptured…)
          → Email, ERP sync, analytics, loyalty
```

**Checkout flow**
1. **Re-validate the cart on the server**: prices, promotions, stock. Never trust the client's total.
2. **Reserve stock** with an atomic conditional update (`qty >= n`) and a **reservation expiry** (say 15 minutes).
3. **Create the order** as `PENDING_PAYMENT`, protected by an **idempotency key**.
4. **Payment** via the provider (with 3-D Secure). The provider's **webhook** marks the order `PAID`, idempotently on the provider's transaction ID.
5. On `PAID`: confirm the reservation, and publish `OrderPlaced` via the **outbox** to email, ERP, analytics.
6. On payment failure or timeout: **release the reservation** (a delayed message or a sweeper job).

**Preventing oversell**
> 🗣️ "The core is one atomic statement: `UPDATE inventory SET qty = qty - 1 WHERE sku = ? AND qty >= 1`. If zero rows are updated, it's out of stock. No read-then-write race. For an extreme flash sale on a few SKUs, I'd hold the counter in **Redis** with an atomic decrement, and reconcile to the database asynchronously."

**Flash sale extras**
- **Pre-scale** before the sale; autoscaling reacts too slowly for a spike at 10:00:00.
- Serve the catalog from the **CDN**; nothing about browsing should touch the database.
- A **virtual waiting room** if demand far exceeds stock.
- **Rate limit per user** to stop bots.
- **Bulkhead** the checkout: its own capacity and connection pools, so browsing traffic can't starve it.
- **Turn off non-critical features** with feature flags (recommendations, reviews).
- **Load test** beforehand at 2–3× expected peak.
- **Watch business metrics live**: orders per minute, payment success rate, cart abandonment. Not only CPU.

**Deep dives they might ask:** multi-currency pricing, promotions engine, tax, the cart for guests vs logged-in users (merge on login), and the order saga with compensations (15.12).

---

### 15.8 Design product catalog and search

> 🗣️ "I'd keep the **source of truth** in a relational database or a PIM, and build a **search index** in Elasticsearch/OpenSearch for the storefront. The index holds a **denormalized document** per product per store and locale, with everything the listing page needs: name, price, stock status, attributes for filters, image, URL. That gives full-text search with **typo tolerance and synonyms**, **facets** (filters with counts, like 'Brass (24)'), and relevance tuning.
> Syncing: product changes publish events, and an indexer updates the index in **near real time**. Fast-changing fields like **stock and price** can be updated partially and more often than the rest. Category pages are cached at the CDN, and autocomplete uses a dedicated suggester."

🏢 **From your Magento background:** this is exactly how Magento works: EAV source tables, **indexers**, and Elasticsearch/OpenSearch for the storefront. And a real trap you know: products not visible individually **aren't in the search index at all**, so search-based lookups can't find them.

---

### 15.9 Design a chat system

**Core design**
- Clients hold a **WebSocket** connection to a fleet of **gateway** servers (stateful, so they need care when scaling and deploying).
- A **connection registry** in Redis maps user → gateway.
- **Send**: gateway → message service → **persist first** (partitioned by `conversation_id`) → **ack** to the sender → publish to the recipients' gateways via **pub/sub** → push to their sockets.
- **Offline** recipients get a push notification and fetch history on reconnect.
- **Ordering**: a per-conversation **sequence number** assigned by the message service.
- **History**: cursor pagination by sequence number.
- **Presence**: heartbeats with a TTL in Redis.
- **Delivery and read receipts**: separate small events.
- **Group chats**: fan-out cost grows with group size. Very large groups switch from push to "clients pull".

---

### 15.10 Design a file upload service

> 🗣️ "The API server **never handles the file bytes**. The flow:
> 1. The client asks the API for an upload. The API checks auth, size and type, creates a DB record with status `PENDING`, and returns a **short-lived presigned URL**.
> 2. The client uploads **directly to S3/Blob**, using **multipart** for big files so uploads can resume.
> 3. A storage event triggers a **queue** message, and workers do a **virus scan**, validate the real file type (by content, not extension), and generate thumbnails or previews.
> 4. Status becomes `READY` (or `REJECTED`), and the client is notified.
> 5. Files are served through a **CDN**, with **signed URLs** if private.
>
> Plus lifecycle rules to delete abandoned uploads and archive old files."

---

### 15.11 Monolith vs microservices

> 🗣️ "I'd start most products as a **modular monolith**: one deployable, but with **clear internal boundaries** (separate modules for catalog, orders and payments that only talk through defined interfaces). It's simpler to develop, test, deploy and debug, and you get real database transactions.
> I split into **microservices** when there's a real reason: **independent teams** stepping on each other, parts with very **different scaling needs**, a different **release cadence**, or **isolation** needs (payments with stricter compliance). Microservices bring real costs: network failures, distributed data and eventual consistency, tracing across services, and much more infrastructure.
> The worst outcome is a **distributed monolith**: services that must all deploy together and call each other synchronously for everything. All the cost, none of the benefit."

---

### 15.12 The saga pattern

> 🗣️ "When one business process spans several services, like *reserve stock → charge payment → create shipment*, you can't use one database transaction. A **saga** breaks it into **local transactions**, and each step has a **compensating action** that undoes it: release the stock, refund the payment.
> There are two styles. **Choreography**: each service listens for events and reacts. Decoupled, but the overall flow is hard to see and debug. **Orchestration**: a central orchestrator tells each service what to do next and handles failures. The flow is easy to follow, but the orchestrator is a critical component. For a business-critical flow like checkout, I prefer **orchestration**, often with a workflow engine (Temporal, AWS Step Functions, Durable Functions)."

```
Reserve stock ──ok──▶ Charge payment ──ok──▶ Create shipment ──ok──▶ Done
      ▲                    │ fail
      └──── Release stock ◀┘   (compensation)
```

---

### 15.13 CAP theorem and consistency

> 🗣️ "CAP says that when there's a **network partition**, a distributed system has to choose between **consistency** (everyone sees the latest data, or gets an error) and **availability** (everyone gets an answer, maybe stale). Since partitions will happen, the real choice is **C or A during a failure**.
> In practice I make that choice **per feature**, not per system. **Payments, inventory and account balances**: consistency, so it's better to fail than to oversell or double-charge. **Product reviews, view counts, recommendations**: availability, because slightly stale is fine. PACELC adds that even without failures there's a trade-off between **latency and consistency**."

---

### 15.14 Resilience patterns

> 🗣️ "These are the tools I use to stop one failing dependency taking down everything:
> - **Timeouts** on every network call. No timeout means waiting forever.
> - **Retries** with exponential backoff and jitter, only for transient errors and idempotent operations, with a **limit** (a retry budget). Unlimited retries turn a small outage into a big one.
> - **Circuit breaker**: after repeated failures, stop calling the dependency for a while and fail fast, then let a few test requests through (closed → open → half-open).
> - **Bulkhead**: separate pools and limits per dependency, so one slow service can't use up all connections.
> - **Fallbacks**: cached data, default values, or hiding a non-critical widget.
> - **Load shedding**: under extreme load, reject some requests early (503) instead of everything getting slow.
> - **Idempotency**, so retries are safe."

---

### 15.15 CQRS and event sourcing (brief)

> 🗣️ "**CQRS** separates the **write model** (commands, validation, the normalized source of truth) from **read models** that are optimized for queries, like a denormalized table or a search index updated from events. It's very useful when reads and writes look very different. Magento's indexers are a form of this.
> **Event sourcing** stores **every change as an event** instead of just the current state; the state is rebuilt by replaying events. You get a perfect audit trail and time travel, but it's complex (schema changes to events, snapshots, eventual consistency). I'd only use it where the audit history is a core business need, like ledgers."

---

### 15.16 Caching layers in a design

> 🗣️ "I think of caching as layers, from closest to the user to closest to the data: **browser cache** → **CDN** → **reverse proxy / full-page cache** (Varnish in Magento) → **application cache** (Redis) → **database buffer cache**. Each layer that answers the request saves every layer behind it. The two hard parts are always **invalidation** and **never caching personalized data in a shared layer**."

---

# 16. Testing and Playwright

### 16.1 Testing strategy: pyramid or trophy?

> 🗣️ "The classic **pyramid** says many fast **unit tests**, fewer **integration tests**, and a small number of **end-to-end tests**. The **testing trophy** idea shifts the weight toward **integration tests**, because they give the most confidence per test: they check that real pieces work together without being as slow and flaky as full browser tests.
> My practical version: unit tests for **pure logic** (pricing, validation, utilities), integration tests for **API endpoints with a real database**, and E2E tests only for the **critical user journeys**: search, add to cart, checkout, login. Plus static checks (TypeScript, ESLint), which catch a lot for free."

---

### 16.2 Unit testing Node.js services

> 🗣️ "I test the **service layer** most, because that's where business rules live. I **inject dependencies** (repository, payment client) so tests can pass in fakes, and assert on **outcomes**: the return value, errors thrown, what was saved. Not on internal method calls. I test the **edge cases and error paths**, not just the happy path: out of stock, payment declined, duplicate request. Tools: Vitest or Jest, or the built-in `node:test`."

❓ **Follow-ups**
- *Mock vs stub vs fake?* A **stub** returns canned answers. A **mock** also verifies how it was called. A **fake** is a working lightweight implementation (an in-memory repository). I prefer fakes; tests that verify lots of mock calls break on every refactor.

---

### 16.3 Integration tests with a real database

> 🗣️ "For the API and repository layer, I test against a **real database**, not a mock, because mocks don't catch wrong SQL, missing indexes or constraint violations. **Testcontainers** starts a throwaway Postgres or Redis in Docker for the test run. Each test runs in a transaction that's rolled back, or works with uniquely-named data, so tests don't affect each other. I hit the endpoints with **Supertest** and check the response and the database state."

---

### 16.4 Contract testing

> 🗣️ "When services or a frontend and backend are built by different teams, **contract tests** make sure they agree on the API without running everything together. With **Pact**, the consumer records what it expects, and the provider's CI verifies it still meets those expectations. For GraphQL, **schema checks** in CI do a similar job: they fail the build if a change would break a known client query."

---

### 16.5 What is Playwright, and why use it?

> 🗣️ "Playwright is a browser automation framework from Microsoft for **end-to-end testing**. It runs on **Chromium, Firefox and WebKit** (Safari's engine). The features that make it reliable: **auto-waiting** (it waits for elements to be visible, enabled and stable before acting), **web-first assertions** that retry until they pass or time out, **isolated browser contexts** per test (fast and clean), **network interception** for mocking, **parallel** execution, and the **Trace Viewer**, which records every step, DOM snapshot, network call and console log, so you can debug a CI failure as if you were there."

---

### 16.6 Locators and auto-waiting

> 🗣️ "I locate elements the way a **user** would: **`getByRole`** first, then `getByLabel`, `getByPlaceholder`, `getByText`, and **`getByTestId`** only when there's no good user-facing option. I avoid CSS selectors tied to styling or DOM structure; they break on every redesign. And I **never use fixed sleeps** like `waitForTimeout(3000)`. They're slow when the app is fast and flaky when it's slow. Web-first assertions wait for exactly the right moment."

```typescript
import { test, expect } from '@playwright/test';

test('guest can add a product to the cart', async ({ page }) => {
  await page.goto('/p/thermostatic-shower-mixer');
  await page.getByRole('button', { name: 'Brushed Brass' }).click();
  await page.getByRole('button', { name: 'Add to cart' }).click();

  await expect(page.getByRole('status')).toContainText('Added to cart'); // retries until true
  await expect(page.getByTestId('cart-count')).toHaveText('1');
});
```

---

### 16.7 Page Object Model

> 🗣️ "A **page object** wraps the locators and actions for one page or component behind readable methods: `productPage.selectFinish('Brass')`, `cart.checkout()`. When the UI changes, I fix it in one place instead of in 40 tests. Two rules: keep page objects **small** (per page or component, not one giant class), and keep **assertions in the tests**, so each test still reads clearly as 'what are we checking'."

```typescript
export class ProductPage {
  constructor(private page: Page) {}
  goto(slug: string) { return this.page.goto(`/p/${slug}`); }
  selectFinish(name: string) { return this.page.getByRole('button', { name }).click(); }
  addToCart() { return this.page.getByRole('button', { name: 'Add to cart' }).click(); }
}
```

---

### 16.8 Authentication and test data in Playwright

> 🗣️ "I don't log in through the UI in every test. That's slow and repetitive. A **setup project** logs in once and saves the browser state (cookies, localStorage) with **`storageState`**. Every other test starts already authenticated. For **test data**, I create it through the **API** in a fixture before the test, and clean it up after, instead of clicking through the admin UI. That makes tests faster and independent of each other."

```typescript
// auth.setup.ts
setup('authenticate', async ({ page }) => {
  await page.goto('/login');
  await page.getByLabel('Email').fill(process.env.E2E_USER!);
  await page.getByLabel('Password').fill(process.env.E2E_PASS!);
  await page.getByRole('button', { name: 'Sign in' }).click();
  await page.waitForURL('**/account');
  await page.context().storageState({ path: 'playwright/.auth/user.json' });
});
// playwright.config.ts → projects: [{ name: 'setup', testMatch: /.*\.setup\.ts/ },
//   { name: 'chromium', use: { storageState: 'playwright/.auth/user.json' }, dependencies: ['setup'] }]
```

---

### 16.9 How do you deal with flaky E2E tests?

> 🗣️ "First, I treat flakiness as a **bug**, not bad luck. The usual causes and fixes:
> - **Timing**: fixed sleeps or asserting before the app is ready. Use web-first assertions and wait for a real signal (a response, a visible element).
> - **Shared data**: tests depending on each other or on the same user or cart. Give each test its **own data**.
> - **Order dependence**: tests only pass in sequence. Make each test fully independent.
> - **Unstable selectors**: switch to role- and label-based locators.
> - **External dependencies**: payment sandboxes, third-party scripts. **Mock** them with `page.route` where they're not what's being tested.
> - **Animations and time**: disable animations, and control the clock where needed.
>
> I use the **Trace Viewer** to see what actually happened. Retries in CI are OK to **detect** flaky tests (Playwright marks them 'flaky'), but not to hide them. Flaky tests get fixed or quarantined with an owner."

---

### 16.10 API testing and network mocking with Playwright

> 🗣️ "Playwright has a built-in **`request`** context for calling APIs directly. I use it for pure API tests and to **set up data** for UI tests. And **`page.route()`** intercepts browser requests, so I can mock a slow recommendations service, or force an error to test that the UI shows a proper error state."

```typescript
test('cart API adds an item', async ({ request }) => {
  const res = await request.post('/api/cart/items', { data: { sku: 'TAP-001', qty: 1 } });
  expect(res.status()).toBe(201);
});

test('shows fallback when recommendations fail', async ({ page }) => {
  await page.route('**/api/recommendations*', route => route.fulfill({ status: 500 }));
  await page.goto('/p/basin-mixer');
  await expect(page.getByText('Recommendations unavailable')).toBeVisible();
});
```

---

### 16.11 What runs in CI, and when?

> 🗣️ "On **every pull request**: lint, type-check, unit tests, integration tests, a **small E2E smoke suite** on the most critical journeys, and a dependency security scan. It needs to be fast, ideally under 10–15 minutes, or people stop waiting for it.
> On **merge** or **nightly**: the **full E2E suite across browsers**, sharded in parallel, plus visual regression and accessibility checks.
> On failure: keep the **traces, screenshots and videos** as CI artifacts, so nobody has to reproduce locally just to see what happened."

---

### 16.12 Do you do TDD? What coverage do you aim for?

> 🗣️ "I use TDD where it fits best: **business logic with clear rules**, like pricing, discounts or validation, and **bug fixes**. I first write a test that reproduces the bug, then fix it, so it can never come back. For exploratory UI work I usually write tests right after.
> On coverage: I don't chase a number. **80% coverage of the wrong things** is worse than solid tests on the money paths. I care that **critical paths and edge cases** are covered. Coverage reports are useful to find **untested** areas, not as a target."

---

### 16.13 Load and performance testing

> 🗣️ "Before a big launch or sale, I run load tests with **k6** (or Artillery): realistic user journeys, ramping up to **2–3× the expected peak**, against a production-like environment. I watch **p95/p99 latency, error rate**, and where things **saturate first** (usually the database connections or a third-party API). The goal isn't a pass/fail number, it's knowing **what breaks first and at what load**, so we can fix it or plan capacity before customers find it."

---

### ⚡ Testing rapid-fire

| Question | One-line answer |
|---|---|
| Playwright vs Cypress? | Playwright: multi-browser including WebKit, multi-tab, parallel by default, any language. Cypress: great DX, runs inside the browser, historically Chromium-focused. |
| Playwright vs Selenium? | Playwright is faster and more modern, with auto-waiting and better tooling; Selenium has the widest language and legacy support. |
| Visual regression? | `expect(page).toHaveScreenshot()` compares against a baseline image. |
| Testing Library philosophy? | "The more your tests resemble the way your software is used, the more confidence they give you." |
| MSW? | Mock Service Worker: mocks the network at the request level, so the same mocks work in tests and the browser. |
| Snapshot tests? | Fine for small stable outputs; big snapshots get rubber-stamped and stop catching anything. |
| Shift-left? | Catch issues earlier: types, linting, tests in the PR, rather than in QA or production. |

---

# 17. Production Troubleshooting Scenarios

> These questions test **judgement under pressure**. The interviewer wants to hear a **calm, ordered process**, not a lucky guess. Every answer follows the same shape.

### 17.1 The universal incident method

> 🗣️ "Whatever the problem, I follow the same order:
> 1. **Assess impact.** Who's affected, how badly, since when? Is checkout down, or one report? That sets the urgency, and I tell people (incident channel, status page).
> 2. **Stop the bleeding.** Mitigate **before** I fully understand it: roll back the last deploy, turn off a feature flag, scale up, fail over. Restoring service comes first.
> 3. **Gather evidence.** Dashboards, logs, traces, and the most important question: **what changed?** A deploy, config, traffic, data, a dependency.
> 4. **Form a hypothesis and test it.** One change at a time.
> 5. **Fix the root cause** properly.
> 6. **Verify.** The metrics are back to normal, not just 'it looks OK'.
> 7. **Blameless post-mortem.** What happened, why our safeguards didn't catch it, and concrete action items with owners."

💡 **Picture it:** A doctor in the emergency room stabilizes the patient first, then runs tests, then treats the cause.

Each scenario below uses the same shape: **Say it → Evidence → Stop the bleeding → Root cause and fix → Prevent it.**

---

### 17.2 API latency jumps from 200 ms to 8 seconds

> 🗣️ "First I'd check the scope: all endpoints or one? All instances or one? Since when, and does it line up with a deploy or a traffic change? Then I'd open a **trace** of a slow request to see **where the time goes**: our code, the database, Redis, or an external API."

- **Evidence:** APM traces (span breakdown), p95/p99 per endpoint, deploy timeline, DB slow-query log, connection-pool wait time, event-loop lag, CPU and memory, dependency latency.
- **Stop the bleeding:** roll back if it lines up with a deploy; scale out if it's pure traffic; turn off an expensive feature; tighten timeouts so requests fail fast instead of piling up.
- **Usual root causes:** a new query missing an index; N+1 introduced by a code change; **pool exhaustion** (requests waiting for a free DB connection); a slow third-party API with no timeout; event-loop blocking; cache hit rate collapsed (eviction, or a key change in the deploy).
- **Prevent it:** latency SLO alerts on p95/p99; query review in PRs; load tests in CI for critical endpoints; timeouts and circuit breakers on all dependencies.

---

### 17.3 CPU at 100%

> 🗣️ "I'd check whether it's one instance or all, and whether it lines up with a deploy or a traffic spike. Then I'd take a **CPU profile** of an affected instance. The flame graph shows the exact hot function, so I'm not guessing."

- **Evidence:** CPU per pod, event-loop lag, requests per endpoint, CPU profile or flame graph, GC activity.
- **Stop the bleeding:** roll back, scale out temporarily, rate limit the hot endpoint.
- **Usual root causes:** an expensive loop on large data; big JSON serialization; regex backtracking; synchronous crypto or compression; **GC thrashing** from memory pressure; an infinite retry loop.
- **Fix:** optimize the code path, cache the result, paginate, move CPU work to a worker thread or background job.
- **Prevent it:** profile before release for heavy features; alert on event-loop lag; load-test.
- ⚠️ "Scaling out hides the cause. I'd only scale as a temporary measure."

---

### 17.4 Memory keeps increasing

> 🗣️ "First I'd confirm it's a **leak**: memory that keeps rising **after garbage collection** and never drops, compared with normal growth under load. Then I'd take **heap snapshots** a few minutes apart on one instance, compare them, and follow the retainer path of whatever keeps growing."

- **Evidence:** heap used vs RSS over time, restart pattern (OOMKilled), correlation with a deploy or endpoint, heap snapshot diff.
- **Stop the bleeding:** rolling restarts, or a memory limit so pods restart before they degrade. Roll back if a deploy introduced it.
- **Usual root causes:** unbounded in-memory cache or Map; event listeners added per request; timers never cleared; global arrays; closures holding large objects; large request bodies or buffers held too long.
- **Fix:** bound the cache (LRU with max size, or move it to Redis), remove listeners, clear timers.
- **Prevent it:** soak tests (run under load for hours), memory dashboards, code review for module-level caches.

---

### 17.5 The database is overloaded, or the connection pool is exhausted

> 🗣️ "I'd look at the database first: **top queries by total time**, lock waits, and active connections. Then the app side: is the **pool exhausted**, meaning requests waiting for a connection?"

- **Evidence:** `pg_stat_statements` / slow log, DB CPU and IOPS, active vs max connections, lock waits, pool wait time in the app.
- **Stop the bleeding:** kill runaway queries; turn off expensive features or reports; add a read replica for read traffic; temporarily rate limit.
- **Usual root causes:** missing index; N+1; **long transactions** holding connections (sometimes an HTTP call inside a transaction); **connection leaks** (not releasing after an error); autoscaling added pods, so **pods × pool size** exceeded `max_connections`; a heavy report running on the primary.
- **Fix:** indexes, query rewrites, batch the N+1, release connections in `finally`, a pooler (PgBouncer / RDS Proxy), move reports to a replica.
- **Prevent it:** query performance review, pool metrics and alerts, capacity planning that includes autoscaling.

---

### 17.6 An external API is slow or failing

> 🗣️ "The goal is that **their** problem doesn't become **our** outage. I'd check our dashboards for that dependency's latency and error rate, and confirm with their status page."

- **Stop the bleeding:** tighter **timeouts**; a **circuit breaker** so we fail fast; a **fallback** (cached data, a 'temporarily unavailable' message, or queue the work to retry later).
- **Fix:** retries only for idempotent calls, with backoff, jitter and a budget; a **bulkhead** (separate connection pool) so it can't consume all our resources; move it off the critical path if possible (async via a queue).
- **Prevent it:** per-dependency dashboards and alerts, SLAs with the vendor, design every integration assuming it **will** fail.

---

### 17.7 Redis outage

> 🗣️ "It depends on what Redis does for us. If it's **only a cache**, the app should degrade, not die: short timeouts, a circuit breaker, and fall back to the database. But I'd protect the database from the sudden load, maybe by disabling non-critical features. If Redis holds **sessions, rate limits or queues**, it's a real outage, and the focus is on failover."

- **Fix:** restore through managed failover; warm the cache gradually rather than all at once.
- **Prevent it:** HA (replicas plus automatic failover), and a design where cache loss is survivable. Make sure the app was actually tested with Redis down.

---

### 17.8 Duplicate orders or payments

> 🗣️ "First, contain it: identify affected customers and refund or cancel duplicates, working with the payment provider. Then find **where** the duplicate came from: a double click, a client retry after a timeout, a webhook delivered twice, or a queue message processed twice."

- **Fix:**
  - **Idempotency key** on order and payment creation (5.6).
  - **Unique constraint** on the business operation (e.g. one order per cart ID, one payment per provider transaction ID).
  - **Idempotent webhook and queue consumers** (12.11).
  - Disable the button on submit (UX only, never the real protection).
- **Prevent it:** reconciliation job comparing orders with provider transactions; an alert on duplicate patterns.

---

### 17.9 Queue backlog keeps growing

> 🗣️ "A growing backlog means messages come in **faster than we process them**. I'd compare the **incoming rate with the processing rate**, look at the **age of the oldest message** (more important than count), and check whether consumers are failing and retrying the same messages."

- **Stop the bleeding:** scale consumers, **but only as far as the downstream can handle**. If the database is the bottleneck, more consumers make it worse. Pause non-critical producers.
- **Usual root causes:** a slow downstream call in the handler; a **poison message** retried endlessly; consumers crashing; a traffic spike; prefetch or batch settings too low.
- **Fix:** optimize the handler, move poison messages to the DLQ, tune concurrency and prefetch, add backpressure.
- **Prevent it:** alerts on oldest-message age; autoscaling consumers on queue depth (KEDA in Kubernetes).

---

### 17.10 Messages are going to the dead-letter queue

> 🗣️ "I'd read a sample of the dead-lettered messages and their **failure reason**, and group them: is it bad data (permanent), a bug in a new deploy, or a downstream outage (transient)?"

- **Fix:** fix the code, config or data; then **replay** through a controlled tool, in batches, relying on **idempotent consumers**. Messages that can never succeed (invalid data) get logged and handled manually.
- **Prevent it:** alert on DLQ count **and** age; classify errors in code so permanent failures dead-letter immediately instead of retrying 10 times.

---

### 17.11 Kubernetes pods keep restarting

> 🗣️ "I'd run `kubectl describe pod` and `kubectl logs --previous` first. The **exit code and events** usually tell the story."

| Clue | Likely cause | Fix |
|---|---|---|
| `OOMKilled`, exit 137 | Memory limit too low, or a leak | Check usage trend; fix the leak, or set the right limit and V8 heap size |
| Liveness probe failed | Probe too strict, slow startup, or event loop blocked | Add a startup probe, fix the probe, don't check dependencies in liveness |
| Exit 1 right after start | App crashed on boot: missing env var or secret, can't connect to DB | Read the logs; validate config at startup with clear errors |
| `CrashLoopBackOff` after a deploy | Bug in the new version | Roll back, then debug |

- ⚠️ "I wouldn't just raise the limits or loosen the probe until I know **why**. That hides the problem."

---

### 17.12 A bad deployment reached production

> 🗣️ "**Roll back first, debug second.** I'd stop the rollout, roll back to the last good version (or flip the feature flag off), and confirm recovery on both **technical** metrics (error rate, latency) and **business** metrics (orders per minute, payment success). Then I'd keep the evidence (logs, traces, the diff) and communicate impact to stakeholders. Finally a **blameless post-mortem**: not 'who broke it' but **why our pipeline didn't catch it**: a missing test, no canary, no alert."

- **Prevent it:** canary deploys with automatic rollback on error rate, feature flags, backward-compatible DB migrations so rollback is always possible.

---

### 17.13 Users see someone else's data (caching bug)

> 🗣️ "This is a **security incident**, so I'd treat it as top priority: **purge the cache immediately**, and if needed **bypass caching** for the affected routes. Then find the cause."

- **Usual root causes:** a personalized response with `Cache-Control: public`, so the **CDN** cached it; a cache key **missing the user, customer group, currency or store**; Next.js caching a user-specific fetch; a module-level variable in Node holding per-request data and shared across requests.
- **Fix:** `private, no-store` for personalized responses; correct cache keys; per-request context instead of module-level state.
- **Prevent it:** tests that check cache headers on authenticated routes; a code review checklist item.
- **After:** assess what data was exposed, and involve security/legal if personal data was involved.

---

### 17.14 A large export crashes the service

> 🗣️ "It's almost always building the **whole result in memory**: loading all rows into an array, then building one huge string or file. The fix is to **stream**: read rows from the database with a **cursor or in pages**, transform them as a stream, and pipe straight to the response or to object storage, with backpressure handled by `stream.pipeline`. For very large exports, I'd make it a **background job**: the user requests it, a worker generates it to S3/Blob, and the user gets a **signed download link** by email or notification."

---

### 17.15 The GraphQL endpoint is slow

> 🗣️ "I'd enable **resolver-level tracing** to see which fields take the time, and **count database queries per request**, because the usual cause is **N+1** from nested resolvers. Then batch with DataLoader, add indexes for the underlying queries, cap page sizes for nested lists, and add query **complexity limits** if clients are sending huge queries."

🏢 **From my work (S2):** "Exactly this: 1,811 queries for one listing request. Batching resolvers brought it to 238 and roughly halved the response time. And I verified the response was **byte-identical** before and after."

---

### 17.16 Intermittent 502 errors behind a load balancer

> 🗣️ "Random 502s with no errors in our app logs often point at **keep-alive timeouts**. The load balancer keeps idle connections to Node open (AWS ALB's default idle timeout is 60 seconds), but Node's `server.keepAliveTimeout` defaults to **5 seconds**. So Node closes the connection just as the load balancer sends a request on it, and the load balancer returns a 502.
> The fix: set Node's `keepAliveTimeout` **higher** than the load balancer's idle timeout, and `headersTimeout` a bit higher than that."

```javascript
server.keepAliveTimeout = 65_000;  // > ALB idle timeout (60s)
server.headersTimeout = 66_000;    // > keepAliveTimeout
```

> 🗣️ "This is one of those bugs you only recognize if you've seen it. Mentioning it shows real production experience."

---

### 17.17 A cron job runs twice (or N times)

> 🗣️ "Classic cause: the scheduled job runs **inside the app**, and we scaled to 3 replicas, so it runs **3 times**. Fixes: move it to a **Kubernetes CronJob** with `concurrencyPolicy: Forbid` (or a cloud scheduler) so it runs once, outside the app pods. If it must stay in the app, take a **distributed lock** (Redis or a DB advisory lock). And make the job itself **idempotent**, because even schedulers can occasionally run twice."

---

### 17.18 Users are randomly logged out

> 🗣️ "I'd look for anything that makes **one instance** unaware of a session created on **another**:
> - **In-memory sessions** with multiple instances: works on instance A, logged out when the load balancer sends you to B. Fix: a shared session store (Redis).
> - **Different JWT secrets or keys** across instances or after a deploy.
> - **Cookie settings**: wrong `Domain`, `SameSite`, or `Secure` on HTTP.
> - **Clock skew** between servers, so token expiry checks disagree.
> - **Redis evicting sessions** under memory pressure.
> - **Refresh token rotation race**: two tabs refresh at the same time and one gets a 'reused token' error."

---

### 17.19 It works locally but fails in production

> 🗣️ "I'd list what's **different** between the two environments and check each: **environment variables and secrets**, **data volume** (fine with 100 rows, slow with 10 million), **network** (firewalls, DNS, TLS certificates, timeouts), **multiple instances** (in-memory state, race conditions), **build differences** (dev vs production mode, missing env at build time in Next.js), **OS and filesystem** differences (case-sensitive file names on Linux), and **timezone or locale**. Logs and traces from production usually narrow it down quickly."

🏢 **From my work:** "We had a local environment that was quietly running **four-day-old dependency-injection config**: compiled files were stale and the compile step exited successfully without doing anything. Lesson: when behaviour doesn't match the code, **verify the code you think is running is actually running**."

---

### 17.20 A JWT must be revoked immediately

> 🗣️ "Disable the account, **revoke all refresh tokens**, and for the access tokens still valid, either check a **denylist of token IDs** in Redis until they expire, or bump a **token version** on the user so all older tokens fail. Long-term: keep access tokens short-lived (5–15 minutes) so this window is always small."

---

### 17.21 Preparing for a high-traffic sale event

> 🗣️ "Preparation matters more than reaction:
> - **Load test** at 2–3× the expected peak, find what breaks first, fix it.
> - **Pre-scale** app instances and database capacity before the sale starts.
> - **Cache** the catalog at the CDN; **warm** caches.
> - **Isolate checkout** capacity from browsing.
> - **Rate limit** per user and protect against bots.
> - **Queue** non-critical work (emails, ERP sync).
> - **Feature flags** to switch off heavy, non-essential features.
> - **Freeze deploys** around the event.
> - **War room**: dashboards with business metrics (orders/min, payment success), on-call people, and a rollback plan."

---

### 17.22 Everything breaks after a tiny change, and the error makes no sense

> 🗣️ "When a small change breaks **everything**, I first **revert it** to restore service. Then, if the error message doesn't make sense, I stop trusting it: I **bisect** the change to find the exact line, and check my assumptions one layer down, in the framework or library code."

🏢 **From my work (S6):** "A single `{` inside a documentation string broke **every** GraphQL query, because the framework's schema reader used a regex that didn't understand strings. Another time, a library bug in error formatting **hid** the real error behind an unrelated one."

---

### 17.23 Products missing from search, or DB and search index disagree

> 🗣️ "The database is the source of truth and the search index is a **copy**, so they can drift: a failed indexing event, an indexer that's behind, or data that's filtered out on purpose. I'd check whether the product exists in the index at all, compare its indexed version with the DB, check the **indexer or queue lag** and errors, and look at the rules for what gets indexed (visibility, status, stock). Fix: reindex the affected items, fix the sync, and add a **reconciliation job** that compares counts and alerts on drift."

🏢 **From my work:** "We found products that were set to 'not visible individually' weren't in the search index at all, so any feature that looked them up through search could never find them. It was a data consistency issue that needed both a code workaround and a catalog clean-up."

---

# 18. Your PHP and Magento Background *(new)*

> Interviewers at Node/React companies **will** notice the PHP and Magento years on your CV. Don't be defensive about it. Present it as a **strength**: years of real production e-commerce at scale, which most Node developers haven't had.

### 18.1 "Your background is PHP. Why should we trust your Node.js experience?"

> 🗣️ "The fundamentals carry over directly: API design, databases and query optimization, caching, queues, security, and running real production systems under traffic. That's most of what makes a backend engineer good, and Magento is a demanding place to learn it.
> I've been working with **Node.js and TypeScript** for several years now, plus **React and Next.js**, and today I'm building a **headless Next.js storefront on GraphQL**. So I work across both sides every day.
> And I think the combination is an advantage. I understand commerce domain problems (pricing, inventory, catalog, checkout) at a depth that's hard to get otherwise, and I bring that to whatever stack I'm working in."

⚠️ **Don't say:** "PHP is outdated" or anything dismissive. It sounds negative and the interviewer may like PHP.

---

### 18.2 PHP's request model vs Node's event loop

This is a great question to **volunteer** when you talk about switching stacks. It shows deep understanding of both.

> 🗣️ "They're opposite models.
> **PHP** (with PHP-FPM) is **shared-nothing per request**: every request gets a fresh worker, runs the script from scratch, and **everything is thrown away at the end**. Blocking I/O is fine because each worker handles one request, and concurrency comes from running many workers. Memory leaks barely matter, because memory is reset after every request.
> **Node** is **one long-lived process** handling **many requests at once** on one thread. It's very efficient for I/O, but it changes the rules:
> - **State persists between requests.** A module-level variable is shared by **all users**. Put request data there by mistake and you leak one user's data to another.
> - **Memory leaks accumulate**, because the process doesn't reset.
> - **Blocking code hurts everyone**, not just one request.
> - Errors must be handled carefully, since one crash takes down many in-flight requests.
>
> Coming from PHP, being conscious of these differences made me more careful in Node, not less."

---

### 18.3 Explain Magento's architecture briefly

> 🗣️ "Magento 2 is a modular PHP framework for e-commerce. The key concepts:
> - **Modules**: everything is a module with its own config, code and database setup.
> - **Dependency injection** configured in XML (`di.xml`), with auto-generated factories and proxies.
> - **Plugins (interceptors)**: run code **before, after or around** any public method of another class without editing it. That's how you extend core behaviour safely.
> - **Observers**: react to **events** dispatched by the system, like 'order placed'.
> - **EAV**: a flexible attribute model for products, categories and customers.
> - **Indexers**: build **denormalized tables and the search index** from the EAV source data for fast storefront reads.
> - **Caching**: full-page cache (Varnish), plus config and block caches.
> - **APIs**: REST and **GraphQL**, the base for headless storefronts.
> - **Scopes**: global → website → store → store view, for multi-country, multi-language, multi-currency setups.
> - Plus message queues, cron, and service contracts (interfaces and repositories) as the stable API between modules."

---

### 18.4 The EAV model: pros and cons

> 🗣️ "**EAV (Entity-Attribute-Value)** stores attributes as **rows** instead of columns: an entity table, an attribute table, and value tables per data type. The benefit is **flexibility**: merchants can add new product attributes in the admin with no schema change, and different products can have completely different attributes. That's essential when you sell taps and towel rails in one catalog.
> The cost is **read performance and query complexity**: loading one product means joining many value tables, per attribute type, per store scope. That's why Magento relies on **indexers** to flatten the data into read-optimized tables and the search engine. It's basically CQRS: a flexible write model, plus denormalized read models.
> In a new system today, I'd consider a **JSON/JSONB column** or a document store for flexible attributes, with indexes on the attributes you filter by."

---

### 18.5 Plugins, observers, preferences: and their Node equivalents

| Magento | What it does | Closest idea in Node/TS |
|---|---|---|
| **Plugin** (before/after/around) | Wraps a method to change inputs, outputs or behaviour | Middleware, decorators, higher-order functions, a proxy/wrapper |
| **Observer** | Reacts to a dispatched event | `EventEmitter` listeners, domain events, pub/sub |
| **Preference** | Replaces one class with another for an interface | Swapping an implementation in a DI container (NestJS providers) |
| **di.xml arguments** | Inject configuration and dependencies | Constructor injection, DI containers |
| **Service contracts** | Stable interfaces between modules | Exported TypeScript interfaces / module boundaries |

> 🗣️ "It's the same set of design ideas: **open for extension without modifying the original**. Plugins are aspect-oriented programming; in Node you'd do the same with middleware or decorators."

---

### 18.6 What is headless commerce, and what are the trade-offs?

> 🗣️ "Headless means the **commerce backend** (catalog, pricing, cart, orders) is separated from the **frontend** and only talks through **APIs**, GraphQL in our case. The storefront is a separate app, like Next.js.
> **Benefits:** frontend freedom and performance (SSR/ISR, CDN, a modern React developer experience), independent deploys, and the same backend can serve web, mobile apps and kiosks.
> **Costs:** you rebuild things the monolith gave you for free (SEO routing, previews, checkout UX, the cart), **two systems** to deploy and keep in sync (API changes must be backward compatible, and deploy order matters), and you need people who understand **both** sides. That's where my background helps."

🏢 Use **S1** and **S7** here.

---

### 18.7 What did Magento teach you that applies anywhere?

> 🗣️ "Four lessons I take into every system:
> 1. **Data quality is a production concern.** Many 'bugs' were really bad data from imports. I learned to protect the code **and** fix the source (Story S5).
> 2. **Read models matter.** Indexers taught me that the shape you store data in is rarely the best shape to read it in.
> 3. **Caching is layered, and invalidation is the hard part.** Varnish, application cache, CDN.
> 4. **Scope and multi-tenancy are hard.** Multiple websites, stores, currencies and languages means every query and cache key must include the right scope. I've debugged plenty of bugs that came from that (Stories S3 and S8)."

---

### 18.8 Magento vs Shopify vs commercetools vs building your own: your opinion?

> 🗣️ "It depends on the business. **Shopify** is fastest to launch and cheapest to run for standard retail, but you're limited in deep customization. **Magento / Adobe Commerce** gives huge flexibility (B2B, complex catalogs, multi-store) at the cost of hosting and development effort. **commercetools** and other API-first platforms suit companies that want headless from day one with a cloud SaaS backend. **Building your own** in Node only makes sense if commerce is your **core differentiator**. Otherwise you spend years rebuilding tax, promotions, and checkout edge cases that platforms already solved."

---

# 19. Engineering Leadership and Practices *(new)*

> Senior roles are judged on **how you work with others** and **how you make decisions**, not only on technical depth. Expect at least a few of these.

### 19.1 How do you do code reviews?

> 🗣️ "I review in order of importance: **correctness** first (does it do what it should, including edge cases and errors?), then **design** (is it in the right place, is it maintainable?), then **security and performance** (input validation, authorization, N+1, missing indexes), then **tests**, and readability last. **Style** should be handled by Prettier and ESLint, not by humans.
> I keep comments **specific and kind**, and I separate **'must fix'** from **'suggestion'** or **'nit'** so the author knows what's blocking. I ask questions instead of giving orders: 'What happens if this list is empty?' And I try to review quickly, because a PR waiting two days slows the whole team.
> For my own PRs: **small and focused**, with a description of what, why, and how to test."

---

### 19.2 How do you handle technical debt?

> 🗣️ "I treat tech debt like financial debt: some is fine if it's **deliberate** and we know the interest we're paying. The dangerous kind is debt nobody tracks.
> In practice: I make it **visible** (tickets with the actual cost, like 'this slows every checkout change by about two days'), and I tie paying it down to **business value**, so the conversation with product is about speed and risk, not code beauty. I prefer **continuous small improvements**: the boy-scout rule, plus some capacity each sprint, over big rewrites. And I fix debt **in the area we're already changing**, where it pays back immediately."

🏢 **Example (S4):** "The URL-regeneration cron was debt: it ran constantly and fixed about 2% of the problem. Removing it and fixing the root cause was both cheaper and more correct."

---

### 19.3 How do you estimate work?

> 🗣️ "First I **break it down** into small pieces. Big tasks are where estimates go wrong. I estimate each piece, and I add explicit time for the things people forget: testing, code review, deployment, edge cases, and integration with other teams. If there's a big **unknown**, I propose a short time-boxed **spike** first, instead of guessing. I give **ranges** rather than single numbers ('3 to 5 days, depending on whether the payment API supports X'), and I **update early** if something changes, not on the deadline day."

---

### 19.4 How do you make architecture decisions?

> 🗣️ "For significant decisions I write a short **design doc or RFC**: the problem, constraints, **two or three options** with trade-offs, and a recommendation. I share it for feedback **before** building. Once decided, I record it as an **ADR** (Architecture Decision Record): what we decided, why, and what we gave up. Six months later, nobody has to guess why we chose Kafka over SQS.
> I also try to keep decisions **reversible** where possible, and I don't spend a week debating something that's easy to change later."

---

### 19.5 How do you mentor junior developers?

> 🗣️ "I try to **teach the thinking, not just give the answer**. When someone's stuck, I ask questions that lead them to it: 'What does the error say? Where would you look next?' I pair with them on tricky tasks, give **detailed, explained code reviews** (the *why*, not just 'change this'), and gradually give them **bigger ownership**, like leading a small feature end to end with me as a safety net. And I make it safe to ask questions; the worst outcome is a junior stuck for two days because they were afraid to ask."

📝 Fill in Story **S9** with a real example.

---

### 19.6 Product wants it by Friday and it's not possible. What do you do?

> 🗣️ "I don't just say no. I give **options**. First I make sure I understand what's actually essential for Friday and why: a campaign, a client promise. Then I come back with choices like: 'We can ship **A and B** by Friday, and C next week', or 'We can ship all of it by Friday if we skip the admin screen and manage it by config for now', or 'Full scope safely is Wednesday.' I'm clear about **risks**, and there are things I won't cut, like security, data integrity, and tests on payment logic. Then **they** choose with full information, and I commit to it."

---

### 19.7 How do you keep quality high while moving fast?

> 🗣️ "Speed and quality aren't opposites if the **safety net** is automated: types, linting, a solid test suite on the critical paths, CI that runs on every PR, **small PRs**, feature flags so unfinished work can ship dark, canary deploys with fast rollback, and good monitoring. Those let us move fast **because** mistakes are caught early and cheap to undo. The thing that actually slows teams down is fear of deploying."

---

### 19.8 How do you get up to speed on a new codebase?

> 🗣️ "In the first weeks I focus on **understanding before changing**. I get it running locally, read the architecture docs and ADRs, and **follow one real request end to end**, from the UI to the database and back. I pick up **small real tickets** early, because shipping something small teaches you the deploy pipeline, reviews and conventions. I ask a lot of questions and **write down what I learn**, which usually turns into onboarding docs for the next person. And I look at the dashboards and recent incidents to understand where the system actually hurts."

---

### 19.9 Incidents, on-call, and post-mortems

> 🗣️ "During an incident: **mitigate first**, communicate clearly and regularly (even 'no update yet, still investigating' is an update), one person coordinates while others investigate, and keep a timeline. After: a **blameless post-mortem**. The question isn't 'who made the mistake' but 'why did our system **allow** it, and why didn't we **catch** it sooner?' Every post-mortem ends with **concrete action items with owners and dates**: a test, an alert, a guard rail. Otherwise the same incident comes back."

---

### 19.10 Git workflow: trunk-based vs GitFlow, merge vs rebase

> 🗣️ "I prefer **trunk-based development**: short-lived feature branches (a day or two), merged to main often, with **feature flags** for unfinished work. It avoids painful long-lived branches and big-bang merges. GitFlow, with long-lived develop and release branches, suits products with scheduled releases or several supported versions.
> On merge vs rebase: I **rebase my own feature branch** to keep it current and clean, but I never rebase shared branches. For merging PRs, **squash merge** keeps main readable, one commit per change, as long as PRs are small."

---

### 19.11 Feature flags

> 🗣️ "Feature flags separate **deploying** code from **releasing** it. I use them for gradual rollouts (internal users, then 10%, then everyone), instant kill switches during incidents, A/B tests, and shipping unfinished work safely. The discipline they need: flags have an **owner and an expiry**, and old flags get **removed**, because a codebase full of dead flags becomes a maze of untested combinations."

---

### 19.12 Monorepo vs polyrepo

> 🗣️ "A **monorepo** (with Turborepo or Nx) makes **shared code and atomic changes** easy: change the API type and the frontend in one PR, with one toolchain. It needs good tooling for builds and CI so it doesn't get slow. **Polyrepos** give teams independence and clear ownership, but sharing code means publishing packages and coordinating versions. For a frontend, a BFF and shared types owned by one team, I'd lean monorepo."

---

### 19.13 How do you use AI tools in your work?

> 🗣️ "I use AI assistants daily as an **accelerator**: exploring an unfamiliar codebase, drafting tests, generating boilerplate, brainstorming debugging hypotheses, writing documentation, and reviewing my own code for things I missed. But I treat the output like code from a new team member: I **read and understand every line**, check it against the requirements and the official docs, run the tests, and I'm fully accountable for what ships. It doesn't replace understanding. If I can't explain the code, it doesn't go in."

---

### 19.14 How do you stay up to date?

> 🗣️ "A mix: release notes for the tools I use daily (Node, React, Next.js, TypeScript), a few good newsletters and engineering blogs, and **trying things hands-on** in small side projects or spikes at work, because reading about something isn't the same as using it. I'm also selective: I don't jump on every new library. I ask whether it solves a problem we actually have."

---

### 19.15 What's your definition of done?

> 🗣️ "Done means **running in production and doing its job**, not 'code merged'. That includes: tests for the important paths, reviewed, deployed, **monitoring or logs** so we'd notice if it broke, documentation updated if behaviour changed, feature flag cleaned up if there was one, and checking with product that it actually solved the problem."

---

# 20. Behavioral and HR Interview

> **STAR** = **S**ituation (1–2 sentences) → **T**ask (your responsibility) → **A**ction (most of the answer, in "**I**", with specifics) → **R**esult (numbers you can defend, plus what you learned).
> Aim for **60–90 seconds** per story. Your stories are in **Chapter 1.2**.

### 20.1 Tell me about yourself
→ See **1.1** for the full and short scripts.

---

### 20.2 Why are you looking for a change?

> 🗣️ "I've learned a lot in my current role, especially going deep on headless commerce, GraphQL and Next.js performance, and I'm not running away from anything. I'm looking for **broader scope**: owning larger systems end to end, working at a **bigger scale**, and being in a team with strong engineering practices where I can keep growing toward architecture and technical leadership. This role looks like exactly that step."

⚠️ Never criticize your current employer, manager or team, even if asked directly. Keep it about what you're moving **toward**.

---

### 20.3 Why this company?

**Template. Research before every interview:**
> 🗣️ "Three things. First, **[their product / domain]**: [one specific thing you genuinely find interesting, e.g. their scale, a technical blog post, their market in the region]. Second, the **technical challenges**: from the job description it's [Node/TypeScript/React at scale / event-driven systems / etc.], which is where I've been focusing. And third, **[culture / growth signal]**: [e.g. engineering blog, open-source work, how they described the team]."

📝 Check before every interview: their product, recent news, tech blog, the job description's top 3 requirements, the interviewer's LinkedIn.

---

### 20.4 Why should we hire you?

> 🗣️ "I bring three things together. **Depth in production e-commerce**: about ten years of real traffic, real money, and real data problems. **Modern full stack skills**: Node.js, TypeScript, GraphQL, React and Next.js, which I use every day. And **production ownership**: I don't stop at 'it works on my machine'. I measure, I find root causes, and I make sure things stay fixed. For example, I [one-line S1 or S2 result]. I think that mix lets me contribute quickly and also raise the bar around me."

---

### 20.5 What's your biggest technical challenge?
→ **S1** (the 4 MB product page) or **S2** (1,811 → 238 queries). Practise S1 out loud until it's smooth.

---

### 20.6 Tell me about a conflict with a teammate

**Structure:** a **technical** disagreement → you sought **evidence** → **decision** → you **supported** it.

> 🗣️ **Example shape** *(replace with your real story S10)*: "A colleague and I disagreed on how to handle [e.g. caching for product pages]. They wanted [approach A], I preferred [approach B] because of [concern]. Instead of arguing in the PR, I suggested we each list the trade-offs and **test both quickly**. We did a small spike and measured [metric]. The data showed [result], so we went with [decision]. It wasn't my original idea in the end, but I backed it fully, and we documented the decision so it wasn't re-debated later. What I took from it: disagreements go much better when they're about **evidence**, not opinions."

⚠️ Never a story where you were simply right and the other person was simply wrong. Never a personality clash.

---

### 20.7 Tell me about a mistake you made

**Structure:** own it → impact → how you fixed it fast → **what you changed so it can't happen again**.

> 🗣️ **Example shape** *(use S6 if it was your change, or S13)*: "I made a change to [the GraphQL schema] that looked harmless, just adding documentation. It caused [every GraphQL query to fail in that environment]. I spotted it [quickly / through monitoring], reverted, and then dug in, because the error made no sense. The framework's parser couldn't handle a curly brace inside a description string. I fixed it, wrote it down for the team, and since then I always run a **smoke query against the whole endpoint**, not just the field I changed, before I push a schema change. The lesson: **small changes can have global effects**, so verify broadly."

⚠️ Pick a **real** mistake with **recoverable** impact. "I work too hard" is not a mistake. Be honest about the scope (local, staging, or production).

---

### 20.8 Tell me about a time you showed leadership

> 🗣️ "Leadership for me is **ownership and influence**, not a title." Then tell **S9**, or S4 (you challenged the existing approach, proved it with a dry run, and changed how the team handled it), or S5 (you coordinated a fix with another team).

---

### 20.9 Strengths and weaknesses

> 🗣️ **Strength:** "Structured problem-solving across the whole stack. When something's wrong, I measure before guessing, find the real cause, and fix it in a way that stays fixed, whether it's in the database, the API, or the frontend. My product page optimization is a good example: the answer wasn't a React trick, it was noticing what we were sending."

> 🗣️ **Weakness:** "I tend to go deep on technical detail, and early in my career I'd explain things in too much depth to non-technical people. Now I **lead with the conclusion** ('we can ship Friday, with one risk') and add detail only if they want it. It's made my communication with product and management much better."

*(Pick a real weakness that isn't critical for the role, and show what you're doing about it.)*

---

### 20.10 Tell me about working under pressure / a tight deadline
→ **S11**. Key points to hit: you **prioritized** (what's essential), you **communicated risk early**, you **didn't cut** security or data integrity, and you **followed up** on shortcuts afterwards.

---

### 20.11 Tell me about a time you disagreed with your manager

> 🗣️ "I raise it **privately**, with **evidence** and a proposed alternative. Not 'this is wrong' but 'here's a risk I see and here's another option.' If they still decide differently after hearing me out, I commit to the decision and make it work, because they may have context I don't. **Disagree and commit.** The only exception would be something unethical or genuinely unsafe for users or data, and then I'd escalate properly."

*(If you have a real example, like proposing the root-cause fix instead of the cron in S4, use it.)*

---

### 20.12 Where do you see yourself in five years?

> 🗣️ "Growing toward a **staff or architect-level role**: someone who designs systems across teams, makes good technical decisions with the business, and helps other engineers grow. I still want to stay hands-on, because I think the best technical leaders still write code. I'd love to do that growth here, which is part of why this role appeals to me."

---

### 20.13 What are your salary expectations?

> 🗣️ **If you can, let them go first:** "I'd like to understand the full scope of the role first. Could you share the budgeted range for this position?"

> 🗣️ **If they insist:** "Based on my experience and the market for senior full stack roles in the UAE, I'm looking at a total monthly package in the range of **[X–Y AED]**. I'm flexible depending on the overall package: housing, bonus, leave, and growth opportunities."

📝 **Prepare:** research the market range (LinkedIn, recruiters, peers) and decide your **walk-away number** before the call. Give a **range** whose bottom is still a number you're happy with.

⚠️ If asked your **current** salary, you can say: "I'd prefer to focus on the value I'd bring to this role and the market rate for it."

---

### 20.14 What's your notice period / when can you start?

> 🗣️ "My notice period is **[X days]**. I'd like to leave my current team properly, with a clean handover, but I'm open to discussing an earlier date if [my employer agrees / it's possible]."

---

### 20.15 Do you have any questions for us?

**Always** ask 2–3. "No questions" signals low interest. Pick based on who's interviewing you:

**To an engineer / tech lead**
- "What does a typical week look like for someone in this role?"
- "What's the biggest technical challenge the team is facing right now?"
- "How do you deploy? How often, and how do you handle rollbacks?"
- "How are architecture decisions made and documented?"
- "What does the on-call rotation look like?"

**To a manager**
- "What would success look like in the first 90 days?"
- "How is the work split between backend and frontend?"
- "How do you support growth toward senior or staff roles here?"
- "What's the team's biggest priority this year?"

**To HR**
- "What's the interview process from here, and the timeline?"
- "How would you describe the team culture?"

**Closing question (bold, but effective):**
- "Is there anything in my background that makes you hesitate? I'd be glad to address it."

---

### 20.16 How do you handle feedback or criticism?

> 🗣️ "I try to treat feedback as information, not as an attack. I listen fully first, ask questions to understand the specific example, and if it's fair, I act on it and follow up later to check I've improved. For example, [real example: feedback on PR size, communication, estimates…]. If I disagree, I say so respectfully with my reasoning, but I start from the assumption that the other person sees something I don't."

---

### 20.17 How do you prioritize when everything is urgent?

> 🗣️ "I make the trade-offs **visible** instead of silently trying to do everything. I rank by **impact and urgency**: production issues affecting customers or revenue first, then commitments others depend on, then the rest. When two things truly conflict, I go to my manager or product owner with the options and the consequences ('if I do A today, B slips to Thursday'), and let them decide on the business priority."

---

### 20.18 What do you do when you're stuck or don't know something?

> 🗣️ "I time-box it. I give it a solid try first: read the error properly, check docs and the code, reproduce it in isolation, try a hypothesis. If I'm not making progress after a reasonable time, say an hour or two depending on urgency, I ask someone, and I come with what I've already tried. Staying stuck silently for two days is worse than asking early. And whatever I learn, I write down."

---

### 20.19 Closing statement (when they ask "anything else?")

> 🗣️ "Just that I'm genuinely excited about this role. The mix of [their tech/problem] and [their scale/product] lines up really well with what I've been doing, especially the headless commerce and performance work, and I'd love to bring that here. Thank you for the conversation."

---

# 21. Coding Round (with working code)

### 21.0 How to behave in a coding round

> The interviewer scores your **communication** as much as your code. A correct solution in silence scores lower than a slightly slower one explained clearly.

1. **Restate the problem** in your own words. "So I get an array of numbers and a target, and I return the two indexes that add up to it?"
2. **Ask about edge cases**: empty input? duplicates? negative numbers? very large input? Can I modify the input?
3. **Say the simple solution first**, with its complexity. "The brute force is two loops, O(n²)."
4. **Improve it** out loud. "I can trade memory for speed with a hash map and get O(n)."
5. **Code with clear names**, and talk while you type.
6. **Test it yourself**: walk through a normal case, then an edge case, by hand.
7. **State the final complexity**: time and space.

> 🗣️ **If you get stuck:** "Let me think out loud. I know I need ___. What if I ___?" Interviewers often give hints when they can hear your thinking.

---

## Part 1: JavaScript utility questions (very common for full stack)

### 21.1 Implement `debounce`

```javascript
function debounce(fn, wait) {
  let timer;
  function debounced(...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn.apply(this, args), wait);
  }
  debounced.cancel = () => clearTimeout(timer);
  return debounced;
}

// usage
const onSearch = debounce((q) => fetchResults(q), 300);
```

> 🗣️ "I keep one timer in a closure. Every call cancels the previous timer and starts a new one, so the function only runs once the calls stop for `wait` ms. I use `apply` so `this` and the arguments pass through correctly, and I added `cancel` so a React component can clean up on unmount."

---

### 21.2 Implement `throttle`

```javascript
function throttle(fn, interval) {
  let last = 0;
  return function (...args) {
    const now = Date.now();
    if (now - last >= interval) {
      last = now;
      fn.apply(this, args);
    }
  };
}
```

> 🗣️ "I remember when it last ran. If enough time has passed, it runs, otherwise the call is ignored. That's leading-edge throttle. A trailing version would also schedule one last call at the end of the interval."

---

### 21.3 Run async tasks with a concurrency limit

```javascript
async function mapWithLimit(items, limit, worker) {
  const results = new Array(items.length);
  let next = 0;

  async function runner() {
    while (next < items.length) {
      const i = next++;                 // safe: JS is single-threaded between awaits
      results[i] = await worker(items[i], i);
    }
  }

  const runners = Array.from({ length: Math.min(limit, items.length) }, runner);
  await Promise.all(runners);
  return results;                       // same order as input
}

// usage: resize 500 images, max 5 at a time
await mapWithLimit(images, 5, resizeImage);
```

> 🗣️ "I start `limit` workers. Each one keeps taking the next index until the list is empty. Results go into their original position, so the order is preserved. Failure policy: as written, the first error rejects everything, like `Promise.all`. If I wanted every result, I'd catch per item and store `{ status, value | reason }` like `allSettled`."

---

### 21.4 Implement `Promise.all`

```javascript
function promiseAll(values) {
  return new Promise((resolve, reject) => {
    const results = new Array(values.length);
    let remaining = values.length;
    if (remaining === 0) return resolve(results);

    values.forEach((value, i) => {
      Promise.resolve(value).then((v) => {
        results[i] = v;
        if (--remaining === 0) resolve(results);
      }, reject);                        // first rejection wins
    });
  });
}
```

> 🗣️ "Key details: results keep the **input order**, not completion order. Non-promise values are wrapped with `Promise.resolve`. An empty array resolves immediately. The first rejection rejects the whole thing."

---

### 21.5 Retry with exponential backoff and jitter

```javascript
const sleep = (ms) => new Promise((r) => setTimeout(r, ms));

async function retry(fn, { retries = 3, baseMs = 200, shouldRetry = () => true } = {}) {
  for (let attempt = 0; ; attempt++) {
    try {
      return await fn();
    } catch (err) {
      if (attempt >= retries || !shouldRetry(err)) throw err;
      const delay = baseMs * 2 ** attempt;        // 200, 400, 800...
      await sleep(Math.random() * delay);         // "full jitter"
    }
  }
}

// usage: only retry transient errors
await retry(() => callPaymentStatus(id), {
  shouldRetry: (e) => e.status >= 500 || e.code === 'ETIMEDOUT',
});
```

> 🗣️ "Exponential so we back off from a struggling service. Jitter so a thousand clients don't all retry at the same instant. And `shouldRetry` so we never retry permanent errors like a 400."

---

### 21.6 Implement `memoize`

```javascript
function memoize(fn, keyFn = (...args) => JSON.stringify(args)) {
  const cache = new Map();
  return function (...args) {
    const key = keyFn(...args);
    if (!cache.has(key)) cache.set(key, fn.apply(this, args));
    return cache.get(key);
  };
}
```

> 🗣️ "Caveats I'd mention: the cache grows forever, so in production I'd cap it with an LRU. JSON keys don't work for functions or circular objects. And for async functions this caches the **promise**, which is actually nice because it dedupes concurrent calls, but a rejected promise would be cached too, so I'd delete the key on failure."

---

### 21.7 Implement an LRU cache

```javascript
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.map = new Map();               // Map remembers insertion order
  }

  get(key) {
    if (!this.map.has(key)) return undefined;
    const value = this.map.get(key);
    this.map.delete(key);               // move to "most recent" end
    this.map.set(key, value);
    return value;
  }

  set(key, value) {
    if (this.map.has(key)) this.map.delete(key);
    this.map.set(key, value);
    if (this.map.size > this.capacity) {
      const oldestKey = this.map.keys().next().value; // first = least recently used
      this.map.delete(oldestKey);
    }
  }
}
```

> 🗣️ "A JavaScript `Map` keeps insertion order, so the first key is always the least recently used. On every access I delete and re-insert to move it to the end. Both operations are O(1). In other languages, you'd use a hash map plus a doubly linked list for the same effect."

---

### 21.8 Deep clone

```javascript
function deepClone(value, seen = new WeakMap()) {
  if (value === null || typeof value !== 'object') return value;  // primitives
  if (seen.has(value)) return seen.get(value);                    // circular refs
  if (value instanceof Date) return new Date(value);
  if (value instanceof Map) {
    const copy = new Map();
    seen.set(value, copy);
    value.forEach((v, k) => copy.set(k, deepClone(v, seen)));
    return copy;
  }
  const copy = Array.isArray(value) ? [] : {};
  seen.set(value, copy);
  for (const key of Object.keys(value)) copy[key] = deepClone(value[key], seen);
  return copy;
}
```

> 🗣️ "In real code I'd just use `structuredClone`. But writing it shows I know the traps: circular references (handled with a WeakMap of already-copied objects), Dates, Maps, and arrays vs objects."

---

### 21.9 Flatten a nested array

```javascript
function flatten(arr) {
  const out = [];
  for (const item of arr) {
    if (Array.isArray(item)) out.push(...flatten(item));
    else out.push(item);
  }
  return out;
}

// Iterative version (no recursion depth limit):
function flattenIterative(arr) {
  const stack = [...arr], out = [];
  while (stack.length) {
    const item = stack.pop();
    if (Array.isArray(item)) stack.push(...item);
    else out.push(item);
  }
  return out.reverse();
}
// Built-in: arr.flat(Infinity)
```

---

### 21.10 Group objects by a key

```javascript
function groupBy(items, keyFn) {
  const groups = {};
  for (const item of items) {
    const key = keyFn(item);
    (groups[key] ??= []).push(item);
  }
  return groups;
}

groupBy(orders, (o) => o.status); // { paid: [...], pending: [...] }
// Modern built-in: Object.groupBy(orders, (o) => o.status)
```

---

### 21.11 Implement an event emitter

```javascript
class Emitter {
  #listeners = new Map();

  on(event, fn) {
    if (!this.#listeners.has(event)) this.#listeners.set(event, new Set());
    this.#listeners.get(event).add(fn);
    return () => this.off(event, fn);   // handy unsubscribe function
  }

  off(event, fn) {
    this.#listeners.get(event)?.delete(fn);
  }

  once(event, fn) {
    const off = this.on(event, (...args) => { off(); fn(...args); });
    return off;
  }

  emit(event, ...args) {
    // copy first, so listeners removing themselves (once) don't break iteration
    for (const fn of [...(this.#listeners.get(event) ?? [])]) fn(...args);
  }
}
```

---

### 21.12 Implement `curry`

```javascript
function curry(fn) {
  return function curried(...args) {
    return args.length >= fn.length
      ? fn.apply(this, args)
      : (...more) => curried.apply(this, [...args, ...more]);
  };
}

const add3 = curry((a, b, c) => a + b + c);
add3(1)(2)(3); // 6
add3(1, 2)(3); // 6
```

---

### 21.13 "What does this print?" puzzles

**1. `var` vs `let` in a loop**
```javascript
for (var i = 0; i < 3; i++) setTimeout(() => console.log(i));  // 3 3 3
for (let j = 0; j < 3; j++) setTimeout(() => console.log(j));  // 0 1 2
```
> 🗣️ "`var` has one shared variable, so by the time the timers run, it's 3. `let` creates a new binding per iteration, so each callback closes over its own value."

**2. Event loop order**
```javascript
console.log('start');
setTimeout(() => console.log('timeout'), 0);
Promise.resolve().then(() => console.log('promise'));
(async () => { console.log('async start'); await null; console.log('async end'); })();
console.log('end');
// start, async start, end, promise, async end, timeout
```
> 🗣️ "Synchronous code first, including the async function up to its first `await`. Then microtasks in order: 'promise', then the rest of the async function. Then the timer, which is a macrotask."

**3. Losing `this`**
```javascript
class User {
  constructor(name) { this.name = name; }
  greet() { return `Hi ${this.name}`; }
}
const u = new User('Salman');
const greet = u.greet;
greet(); // TypeError: Cannot read properties of undefined
```
> 🗣️ "Class bodies are strict mode, and `greet` is called without an object, so `this` is `undefined`. Fix: `u.greet.bind(u)`, or an arrow function class field."

**4. The `parseInt` trap**
```javascript
['1', '2', '3'].map(parseInt); // [1, NaN, NaN]
```
> 🗣️ "`map` passes `(value, index)`, so it's `parseInt('2', 1)` and `parseInt('3', 2)`, which are invalid radixes. Use `.map(Number)` or `.map(s => parseInt(s, 10))`."

**5. Floating point**
```javascript
0.1 + 0.2 === 0.3; // false (0.30000000000000004)
```
> 🗣️ "Binary floating point can't represent 0.1 exactly. For **money**, I store **integer minor units** (cents/fils) or use a decimal library, never floats."

---

## Part 2: Algorithm patterns

### 21.14 The patterns that solve most interview problems

| Pattern | Use when you see… | Typical complexity |
|---|---|---|
| **Hash map / frequency count** | "count", "duplicates", "anagram", "first unique", "two sum" | O(n) time, O(n) space |
| **Hash set** | "seen before?", "contains duplicate" | O(n) |
| **Two pointers** | Sorted array, pairs, palindromes, "remove in place" | O(n) |
| **Sliding window** | "longest/shortest substring or subarray with…" | O(n) |
| **Stack** | Brackets, "next greater element", undo, parsing | O(n) |
| **Queue / BFS** | Shortest path in an unweighted graph, level-by-level tree | O(V + E) |
| **DFS / recursion** | Trees, graphs, all combinations, permutations | O(V + E) or exponential |
| **Binary search** | Sorted data, or "find the minimum X that works" | O(log n) |
| **Sort first** | Intervals, grouping, closest pairs | O(n log n) |
| **Heap** | "Top K", "K-th largest", merge K sorted lists | O(n log k) |

---

### 21.15 Two Sum (hash map)

```javascript
function twoSum(nums, target) {
  const seen = new Map();                 // value -> index
  for (let i = 0; i < nums.length; i++) {
    const need = target - nums[i];
    if (seen.has(need)) return [seen.get(need), i];
    seen.set(nums[i], i);
  }
  return null;
}
```
> 🗣️ "Brute force is O(n²). With a map of values I've already seen, each number checks for its complement in O(1). **O(n) time, O(n) space.**"

---

### 21.16 Valid parentheses (stack)

```javascript
function isValid(s) {
  const pairs = { ')': '(', ']': '[', '}': '{' };
  const stack = [];
  for (const ch of s) {
    if (ch === '(' || ch === '[' || ch === '{') stack.push(ch);
    else if (ch in pairs && stack.pop() !== pairs[ch]) return false;
  }
  return stack.length === 0;
}
```
> 🗣️ "Every opener goes on the stack; every closer must match the most recent opener. At the end the stack must be empty. **O(n).**"

---

### 21.17 Longest substring without repeating characters (sliding window)

```javascript
function lengthOfLongestSubstring(s) {
  const lastSeen = new Map();
  let start = 0, best = 0;
  for (let i = 0; i < s.length; i++) {
    const ch = s[i];
    if (lastSeen.has(ch) && lastSeen.get(ch) >= start) {
      start = lastSeen.get(ch) + 1;       // jump the window past the duplicate
    }
    lastSeen.set(ch, i);
    best = Math.max(best, i - start + 1);
  }
  return best;
}
```
> 🗣️ "The window `[start, i]` always has unique characters. When I see a repeat inside the window, I move `start` just past its previous position. Each character is processed once. **O(n).**"

---

### 21.18 Group anagrams

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

---

### 21.19 First non-repeating character

```javascript
function firstUnique(str) {
  const count = new Map();
  for (const ch of str) count.set(ch, (count.get(ch) ?? 0) + 1);
  for (const ch of str) if (count.get(ch) === 1) return ch;
  return null;
}
```
> 🗣️ "Two passes: count everything, then return the first character with count 1 in the original order. **O(n).**"

---

### 21.20 Binary search

```javascript
function binarySearch(arr, target) {
  let lo = 0, hi = arr.length - 1;
  while (lo <= hi) {
    const mid = Math.floor((lo + hi) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) lo = mid + 1;
    else hi = mid - 1;
  }
  return -1;
}
```
> 🗣️ "Halve the search space each step. **O(log n).** The classic bugs are off-by-one errors in `lo <= hi` and the `mid ± 1` updates, so I test with 1- and 2-element arrays."

---

### 21.21 Top K frequent elements

```javascript
function topKFrequent(nums, k) {
  const count = new Map();
  for (const n of nums) count.set(n, (count.get(n) ?? 0) + 1);
  return [...count.entries()]
    .sort((a, b) => b[1] - a[1])
    .slice(0, k)
    .map(([n]) => n);
}
```
> 🗣️ "Count, then sort by frequency: O(n log n). If they want better, a **min-heap of size k** gives O(n log k), or **bucket sort** by frequency gives O(n)."

---

### 21.22 Merge overlapping intervals

```javascript
function mergeIntervals(intervals) {
  const sorted = [...intervals].sort((a, b) => a[0] - b[0]);
  const out = [];
  for (const [start, end] of sorted) {
    const last = out[out.length - 1];
    if (last && start <= last[1]) last[1] = Math.max(last[1], end);
    else out.push([start, end]);
  }
  return out;
}
// [[1,3],[2,6],[8,10]] -> [[1,6],[8,10]]
```
> 🗣️ "Sort by start time, then either extend the last interval or start a new one. **O(n log n)** for the sort. Real-world use: merging store opening hours or booking slots."

---

### 21.23 Big-O cheat sheet

| Complexity | Name | Example |
|---|---|---|
| O(1) | Constant | Map/Set lookup, array index |
| O(log n) | Logarithmic | Binary search, B-tree index lookup |
| O(n) | Linear | One loop over the data |
| O(n log n) | Linearithmic | Good sorting |
| O(n²) | Quadratic | Nested loops over the same data |
| O(2ⁿ) | Exponential | All subsets, naive recursion |

> 🗣️ "Always state **both** time and space. Interviewers often ask, 'can you do it with less memory?'"

---

# 22. SQL Round (with working queries)

**Example schema used below** (PostgreSQL syntax; MySQL differences noted):

```
customers   (id, name, email, country, created_at)
orders      (id, customer_id, status, total, created_at)
order_items (id, order_id, product_id, qty, price)
products    (id, sku, name, category_id, price)
employees   (id, name, department_id, salary, manager_id)
```

> 🗣️ **In a SQL round, talk like this:** "First I'll get the rows I need, then group, then filter the groups, then sort." And mention which **index** would make it fast. That's the senior touch.

---

### 22.1 The order SQL actually runs in

```
FROM / JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT
```

> 🗣️ "That's why you can't use a `SELECT` alias in `WHERE` (it doesn't exist yet), but you usually can in `ORDER BY`. And why `WHERE` filters **rows** before grouping while `HAVING` filters **groups** after."

---

### 22.2 JOIN types in one picture

| Join | Returns |
|---|---|
| `INNER JOIN` | Only rows that match on **both** sides |
| `LEFT JOIN` | **All** rows from the left table, plus matches from the right (NULLs if none) |
| `RIGHT JOIN` | The mirror of LEFT (rarely used; just swap the tables) |
| `FULL OUTER JOIN` | Everything from both sides (not in MySQL; emulate with UNION) |
| `CROSS JOIN` | Every combination (careful: rows × rows) |
| Self join | A table joined to itself (employees and their managers) |

---

### 22.3 Second-highest salary

```sql
-- 1) Simple
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;

-- 2) Window function (handles ties properly, easy to change to Nth)
SELECT DISTINCT salary
FROM (
  SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
  FROM employees
) ranked
WHERE rnk = 2;

-- 3) Classic subquery
SELECT MAX(salary) FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```

> 🗣️ "I'd use `DENSE_RANK`, because if two people share the top salary, the 'second highest' should still be the next distinct value."

---

### 22.4 `ROW_NUMBER` vs `RANK` vs `DENSE_RANK`

Salaries: 100, 90, 90, 80

| salary | ROW_NUMBER | RANK | DENSE_RANK |
|---|---|---|---|
| 100 | 1 | 1 | 1 |
| 90 | 2 | 2 | 2 |
| 90 | 3 | 2 | 2 |
| 80 | 4 | **4** | **3** |

> 🗣️ "`ROW_NUMBER` is always unique, so I use it for 'pick exactly one per group' or deduplication. `RANK` leaves gaps after ties, like Olympic medals. `DENSE_RANK` has no gaps, so I use it for 'Nth highest value'."

---

### 22.5 Highest-paid employee in each department

```sql
SELECT department_id, name, salary
FROM (
  SELECT e.*,
         ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC) AS rn
  FROM employees e
) t
WHERE rn = 1;
```

> 🗣️ "`PARTITION BY` restarts the numbering for each department. If I wanted **all** people tied for the top, I'd use `RANK` instead of `ROW_NUMBER`."

---

### 22.6 Find duplicate emails, then delete duplicates keeping the oldest

```sql
-- Find
SELECT email, COUNT(*) AS cnt
FROM customers
GROUP BY email
HAVING COUNT(*) > 1;

-- Delete all but the lowest id per email
DELETE FROM customers
WHERE id IN (
  SELECT id FROM (
    SELECT id, ROW_NUMBER() OVER (PARTITION BY email ORDER BY id) AS rn
    FROM customers
  ) t
  WHERE rn > 1
);
```

> 🗣️ "The extra derived table also makes this work in MySQL, which doesn't allow selecting from the table you're deleting from directly. After cleaning up, I'd add a **unique index** on `email` so it can't happen again."

---

### 22.7 Customers who never ordered

```sql
-- LEFT JOIN + IS NULL
SELECT c.*
FROM customers c
LEFT JOIN orders o ON o.customer_id = c.id
WHERE o.id IS NULL;

-- NOT EXISTS (my preference: clear, and NULL-safe)
SELECT c.*
FROM customers c
WHERE NOT EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.id);
```

> ⚠️ **Gotcha worth mentioning:** `WHERE id NOT IN (SELECT customer_id FROM orders)` returns **no rows at all** if any `customer_id` is NULL. `NOT EXISTS` doesn't have that problem.

---

### 22.8 Total spent per customer (top 10)

```sql
SELECT c.id, c.name, COALESCE(SUM(o.total), 0) AS total_spent, COUNT(o.id) AS orders
FROM customers c
LEFT JOIN orders o ON o.customer_id = c.id AND o.status = 'paid'
GROUP BY c.id, c.name
ORDER BY total_spent DESC
LIMIT 10;
```

> 🗣️ "Notice the status filter is in the **ON** clause, not in `WHERE`. If I put it in `WHERE`, customers with no paid orders would disappear, because it turns the LEFT JOIN into an INNER JOIN. That's a very common bug."

---

### 22.9 Running total and 7-day moving average

```sql
SELECT DATE(created_at) AS day,
       SUM(total) AS revenue,
       SUM(SUM(total)) OVER (ORDER BY DATE(created_at)) AS running_total,
       AVG(SUM(total)) OVER (
         ORDER BY DATE(created_at) ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
       ) AS moving_avg_7d
FROM orders
WHERE status = 'paid'
GROUP BY DATE(created_at)
ORDER BY day;
```

> 🗣️ "The inner `SUM(total)` is the normal daily aggregate; the window function then runs **over** those daily rows. (The moving average assumes every day has orders. Otherwise I'd join to a calendar table.)"

---

### 22.10 Month-over-month revenue growth

```sql
WITH monthly AS (
  SELECT DATE_TRUNC('month', created_at) AS month,     -- MySQL: DATE_FORMAT(created_at, '%Y-%m-01')
         SUM(total) AS revenue
  FROM orders
  WHERE status = 'paid'
  GROUP BY 1
)
SELECT month,
       revenue,
       LAG(revenue) OVER (ORDER BY month) AS prev_month,
       ROUND(100.0 * (revenue - LAG(revenue) OVER (ORDER BY month))
             / NULLIF(LAG(revenue) OVER (ORDER BY month), 0), 1) AS growth_pct
FROM monthly
ORDER BY month;
```

> 🗣️ "`LAG` reads the previous row's value. `NULLIF(…, 0)` avoids dividing by zero. A CTE (`WITH`) keeps it readable."

---

### 22.11 Top 3 products per category by revenue

```sql
WITH product_revenue AS (
  SELECT p.category_id, p.id, p.name, SUM(oi.qty * oi.price) AS revenue
  FROM order_items oi
  JOIN products p ON p.id = oi.product_id
  GROUP BY p.category_id, p.id, p.name
)
SELECT category_id, name, revenue
FROM (
  SELECT pr.*,
         DENSE_RANK() OVER (PARTITION BY category_id ORDER BY revenue DESC) AS rnk
  FROM product_revenue pr
) ranked
WHERE rnk <= 3
ORDER BY category_id, revenue DESC;
```

---

### 22.12 Employees who earn more than their manager (self join)

```sql
SELECT e.name AS employee, e.salary, m.name AS manager, m.salary AS manager_salary
FROM employees e
JOIN employees m ON m.id = e.manager_id
WHERE e.salary > m.salary;
```

---

### 22.13 Orders per day by status, last 30 days (conditional aggregation)

```sql
SELECT DATE(created_at) AS day,
       COUNT(*) AS total_orders,
       SUM(CASE WHEN status = 'paid'      THEN 1 ELSE 0 END) AS paid,
       SUM(CASE WHEN status = 'cancelled' THEN 1 ELSE 0 END) AS cancelled,
       SUM(CASE WHEN status = 'paid' THEN total ELSE 0 END)  AS paid_revenue
FROM orders
WHERE created_at >= CURRENT_DATE - INTERVAL '30 days'   -- MySQL: CURRENT_DATE - INTERVAL 30 DAY
GROUP BY DATE(created_at)
ORDER BY day;
```

> 🗣️ "`CASE` inside `SUM` turns rows into columns, a pivot, and works in every database. PostgreSQL also has `COUNT(*) FILTER (WHERE …)`, which is cleaner."

---

### 22.14 Repeat customer rate

```sql
SELECT ROUND(100.0 * SUM(CASE WHEN order_count > 1 THEN 1 ELSE 0 END) / COUNT(*), 1)
       AS repeat_customer_pct
FROM (
  SELECT customer_id, COUNT(*) AS order_count
  FROM orders
  WHERE status = 'paid'
  GROUP BY customer_id
) per_customer;
```

---

### 22.15 Products never ordered

```sql
SELECT p.id, p.sku, p.name
FROM products p
WHERE NOT EXISTS (SELECT 1 FROM order_items oi WHERE oi.product_id = p.id);
```

---

### 22.16 `WHERE` vs `HAVING` in one example

```sql
SELECT customer_id, COUNT(*) AS orders
FROM orders
WHERE created_at >= '2026-01-01'   -- filters ROWS before grouping (can use an index)
GROUP BY customer_id
HAVING COUNT(*) >= 5;              -- filters GROUPS after aggregation
```

> 🗣️ "Put as much filtering as possible in `WHERE`. It reduces the rows **before** the expensive grouping, and it can use indexes."

---

### 22.17 Talking about performance after writing the query

> 🗣️ "For these queries I'd want indexes like:
> - `orders (customer_id)` for the joins,
> - `orders (status, created_at)` for 'paid orders in a date range',
> - `order_items (product_id)` and `order_items (order_id)`,
> - and `customers (email)` as a **unique** index.
>
> Then I'd check with `EXPLAIN ANALYZE` on realistic data. For heavy reporting queries like the monthly revenue, I'd run them on a **read replica** or pre-aggregate into a summary table, so they don't compete with checkout traffic."

---

# 23. Last-Minute Cheat Sheet

> Read this in the last 30 minutes. Each line is something you can **say** in one breath.

### Your answer style
- **What → How → Example → Trade-off.** Then stop.
- First answer **under 45 seconds**. Let them steer.
- Say **"I"** for your work. Use numbers you can defend.
- Scenarios: **impact → stop the bleeding → evidence → fix → verify → prevent.**
- Don't know? "I haven't used that directly, but here's how I'd reason about it…"

### Your stories (have these ready)
- **S1** Product page **3.96 MB → 0.28 MB** (14×). Too much data crossed the server-to-client component boundary. Related products now load after first paint.
- **S2** Listing **1,811 → 238 queries**, ~1.6 s → ~0.65 s. N+1 in resolvers, fixed by batching; response verified byte-identical.
- **S3** Store "open now" used the website's timezone, so **26/136** were wrong. Now uses each store's own timezone; boolean replaced by an enum.
- **S4** 128 products with no URLs; the cron could only fix ~2%. Root cause was empty URL keys colliding. Backfill tool: **128 → 3**.
- **S5** ~311 broken child products from import. Hardened the API **and** handed the exact list to the data team.
- **S6** One `{` in a doc string broke **every** GraphQL query. Bisect, check one layer down.
- **S7** Optional argument instead of a new version; backend deployed before frontend.

### JavaScript
- One thread, one call stack; **event loop** runs queued callbacks when the stack is empty.
- **Closure**: a function remembers variables from where it was created.
- `this` depends on **how** a function is called; arrows take `this` from outside.
- After each macrotask, **all microtasks** run. In Node: `nextTick` → promises → timers.
- `Promise.all` fails fast; `allSettled` waits for all; `race` first settled; `any` first success.
- `structuredClone` for deep copies. `===` always (except `== null`).

### TypeScript
- Types are **erased at runtime**, so validate input with **Zod** and infer the type.
- `unknown` over `any`. Discriminated unions + `never` for exhaustive switches.
- `satisfies` checks a type without widening; `as const` keeps literals.

### Node.js
- Great for **I/O**, bad for **CPU** on the main thread → worker threads / queue.
- Thread pool (default 4): fs, crypto, zlib, dns.lookup. Sockets don't use it.
- **Streams + `pipeline`** = constant memory + backpressure + error cleanup.
- Scale: **stateless** → replicas behind an LB → cache → queue → watch the DB.
- **Graceful shutdown**: SIGTERM → not ready → stop accepting → finish in-flight → close pools → exit.
- Leak: post-GC growth → heap snapshots → compare → retainer path.
- **Keep-alive 502s**: Node `keepAliveTimeout` (5 s) < ALB idle (60 s). Set it to 65 s.

### API and security
- **401** = who are you? **403** = I know you, and no.
- PUT replaces (idempotent); PATCH partial; POST creates (not idempotent → **idempotency key** + unique constraint).
- **Cursor pagination** for big or changing lists.
- JWT is **signed, not encrypted**. Short access token plus rotated refresh token.
- Tokens: **HttpOnly Secure SameSite cookie**, or a BFF. Never localStorage.
- **XSS** = their script on your site. **CSRF** = their site triggers your request. CORS fixes neither.
- **IDOR**: always scope by owner: `WHERE id = ? AND customer_id = ?`.
- Passwords: **Argon2id / bcrypt**, never fast hashes.

### GraphQL
- Client picks fields; complexity moves to the server.
- **N+1 → DataLoader**, a new loader per request, results in key order.
- Auth in the **service layer**, not only top-level resolvers.
- Protect: depth and complexity limits, pagination caps, **persisted queries**.
- Evolve with **optional args and `@deprecated`**, not versions.

### React and Next.js
- Re-render triggers: **state, parent render, context, external store**.
- `useEffect` = sync with **outside systems**. Not for derived values.
- Memoize **after measuring**; the React Compiler automates it.
- Server state → **TanStack Query** / Server Components, not Redux.
- **Server Components by default**; `'use client'` only on interactive leaves.
- Props crossing to client components are **serialized into the HTML** (S1!).
- SSG build time · ISR revalidate · SSR per request · CSR browser.
- Server Actions are **public endpoints**: validate and authorize inside.
- Never shared-cache personalized data.

### Web
- **LCP ≤ 2.5 s · INP ≤ 200 ms · CLS ≤ 0.1** (75th percentile).
- `defer` for app scripts; `async` for independent ones.
- Fingerprinted assets: `max-age=1y, immutable`; HTML: `no-cache`.

### Databases
- Measure: `pg_stat_statements` → `EXPLAIN ANALYZE` → index / rewrite → retest with real volume.
- Composite index: **leftmost prefix**; equality → sort → range.
- Indexes speed reads, slow writes.
- Short transactions; **no HTTP calls inside**. Consistent lock order.
- Stock: `UPDATE … SET qty = qty - n WHERE sku = ? AND qty >= n`.
- Pool size × pods < `max_connections`.
- Migrations: **expand → migrate → contract**. Never rename in one step.

### Redis and queues
- **Cache-aside**: read → miss → DB → set with TTL + jitter; on write: DB then **delete** key.
- Stampede: single-flight, lock, stale-while-revalidate, jitter.
- Queues are **at-least-once**, so consumers must be **idempotent** (processed-ID table in the same transaction).
- Retry transient errors with backoff + jitter; **DLQ** after N; alert on DLQ age.
- Order **per entity** (partition key / session / message group).
- **Outbox** fixes the dual write.

### Docker / Kubernetes / CI-CD
- Image = template; container = running process.
- Multi-stage, `npm ci --omit=dev`, non-root, `CMD ["node", …]` so signals arrive.
- **Readiness** = gets traffic? **Liveness** = restart me? Never check the DB in liveness.
- Memory over the limit → **OOMKilled (137)**; CPU over the limit → throttled.
- **Build once, promote the same image.** Canary + feature flags + fast rollback.

### System design
- Clarify → estimate → API → data → boxes → deep dive → wrap up.
- 1M req/day ≈ **12 rps**. Peak 2–3× (sales 10×).
- Start with a **modular monolith**; split for teams, scaling or isolation.
- **Saga** = local transactions + compensations. Orchestrate critical flows.
- Consistency for money and stock; availability for reviews and counters.
- Timeouts, bounded retries, circuit breaker, bulkhead, fallback, load shedding.

### Behavioral
- Never criticize your employer. Talk about what you're moving **toward**.
- Mistake story: own it → impact → fix → **what you changed** so it can't recur.
- Conflict story: **technical**, evidence-based, you committed to the decision.
- Always have **2–3 questions** for them.

### Before you join the call
- [ ] Join 5–10 minutes early. Camera, mic, internet, charger, quiet room.
- [ ] Water, CV, the job description, and these notes (closed during coding).
- [ ] One line on why **this** company.
- [ ] Your salary range and notice period decided.
- [ ] Code editor ready, with a blank file and Node available.
- [ ] Breathe. **Speak slowly.** Ask when something is unclear. Pausing to think is fine.

### If your mind goes blank
- "Let me take a second to structure my answer."
- "Can I think out loud for a moment?"
- Fall back to the formula: **What is it? How does it work? Where have I used it? What's the trade-off?**

---

# Changelog

## Version 2.0 (October 2026)
- **Rewritten in spoken style.** Every answer is now a 🗣️ "Say it" script in plain first-person English, with analogies, instead of textbook definitions.
- **Follow-up questions now have answers** (previously listed without answers).
- **New Chapter 0:** answer formula with a before/after example, golden rules, phrase bank, red flags, Top 30 priority list.
- **New Chapter 1:** "Tell me about yourself" scripts (75 s and 30 s) and a **Story Bank of 8 real STAR stories** from your recent work (product page payload, N+1 batching, timezone bug, URL root cause, bad upstream data, GraphQL parser bug, backward-compatible API, data drift), plus templates for the rest and a question → story map.
- **New chapters:** Web and Browser Fundamentals (8), Cloud AWS/Azure with a service mapping table (14), Your PHP and Magento Background (18), Engineering Leadership and Practices (19).
- **Expanded:** ~40 new technical questions (TypeScript `satisfies`, React 19, React Compiler, Next.js 15/16 caching and `proxy`, Express 5, outbox, Kafka comparison, zero-downtime migrations, PID 1, Core Web Vitals, SSRF, IDOR, OWASP, and more), plus a **rapid-fire table** at the end of each chapter.
- **Production scenarios:** rewritten into impact → mitigate → cause → prevent, with 10 new scenarios (keep-alive 502s, cron running twice, random logouts, cache leaking user data, works-locally-fails-in-prod, and more).
- **Coding round:** working code for 22 problems (debounce, throttle, concurrency limit, Promise.all, retry, LRU, deep clone, event emitter, output puzzles, classic algorithms) with what to say while coding.
- **SQL round:** 17 working queries (window functions, ranking, dedupe, running totals, month-over-month, top-N per group, conditional aggregation) with common gotchas.
- **Cheat sheet:** rebuilt as one-breath lines, including your stories and a pre-call checklist.

## Version 1.0 (August 2026)
- Created the original 17-chapter handbook.
