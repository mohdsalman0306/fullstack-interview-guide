# Last-Minute Cheat Sheet

> Read this in the last 30–60 minutes before the call. Each line is something you can **say** in one breath.
> Full stories: `1-interview-basics/02-your-story.md`.

## How to answer
- **What → How → Example → Trade-off.** Then stop.
- First answer **under 45 seconds**. Let them ask for more.
- Say **"I"** for your own work. Only use numbers you can explain.
- Problem scenarios: **impact → stop the damage → find evidence → fix → check it worked → prevent it next time.**
- Don't know? "I haven't used that directly, but here's how I'd think about it…"

## Your stories (have these ready)
- **S1** Product page **3.96 MB → 0.28 MB** (14×). Too much data was sent from server components to client components. Related products now load after the first paint.
- **S2** Listing page **1,811 → 238 queries**, ~1.6 s → ~0.65 s. N+1 in resolvers, fixed by batching. Checked the response was byte-identical.
- **S3** Store "open now" used the website's timezone, so **26/136** stores were wrong. Now each store uses its own timezone, and the true/false flag became an enum (a fixed list of states).
- **S4** 128 products had no URLs. The cron could only fix ~2%. Root cause: empty URL keys clashing. Built a backfill tool: **128 → 3**.
- **S5** ~311 broken child products from an import. Made the API safer **and** gave the exact list to the data team.
- **S6** One `{` in a doc string broke **every** GraphQL query. Lesson: cut the problem in half (bisect), and check one layer down.
- **S7** Added an optional argument instead of a new API version. Deployed the backend before the frontend.

## JavaScript
- One thread, one call stack. The **event loop** runs waiting callbacks when the stack is empty.
- **Closure**: a function remembers the variables from where it was created.
- `this` depends on **how** a function is called. Arrow functions take `this` from outside.
- After each macrotask (like a timer), **all microtasks** (promises) run. In Node: `nextTick` → promises → timers.
- `Promise.all` fails on the first error. `allSettled` waits for all. `race` = first to finish. `any` = first success.
- `structuredClone` for deep copies. Always `===` (except `== null` to check null or undefined).
- Money: never floats. Store integer cents/fils.

## TypeScript
- Types **disappear at runtime**. So check input with **Zod** and get the type from the schema.
- `unknown` instead of `any`. Discriminated unions + `never` make sure a switch handles every case.
- `satisfies` checks a type without making it wider. `as const` keeps exact values.

## Node.js
- Great for **I/O** (waiting on network, disk, DB). Bad for heavy **CPU** work on the main thread → use worker threads or a queue.
- Thread pool (default 4 threads): fs, crypto, zlib, dns.lookup. Network sockets don't use it.
- **Streams + `pipeline`** = memory stays flat, handles backpressure (slows the fast side down), cleans up on errors.
- Scale: make it **stateless** → many copies behind a load balancer → add cache → add queue → watch the DB.
- **Graceful shutdown**: SIGTERM → mark not ready → stop taking new requests → finish current ones → close DB pools → exit.
- Memory leak: memory still grows after garbage collection → take heap snapshots → compare → find what is still holding the objects.
- **Random 502s with keep-alive**: Node's `keepAliveTimeout` (5 s) is shorter than the ALB idle timeout (60 s). Set Node to 65 s.

## API and security
- **401** = "Who are you?" **403** = "I know you, and the answer is no."
- PUT replaces (safe to repeat). PATCH changes part. POST creates (not safe to repeat → use an **idempotency key** + a unique constraint).
- **Cursor pagination** ("give me items after this one") for big or changing lists.
- A JWT is **signed, not encrypted**. Anyone can read it. Use a short-lived access token + a refresh token that rotates.
- Store tokens in an **HttpOnly Secure SameSite cookie**, or use a BFF (a small backend for your frontend). Never localStorage.
- **XSS** = their script runs on your site. **CSRF** = their site makes your browser send a request to you. CORS fixes neither.
- **IDOR** (seeing someone else's data by changing an ID): always check the owner: `WHERE id = ? AND customer_id = ?`.
- Passwords: **Argon2id or bcrypt**. Never fast hashes like MD5 or SHA.

## GraphQL
- The client picks the fields. The hard work moves to the server.
- **N+1 → DataLoader.** A new loader per request. Return results in the same order as the keys.
- Check permissions in the **service layer**, not only in top-level resolvers.
- Protect the server: depth and complexity limits, max page size, **persisted queries** (only allow known queries).
- Change the schema with **optional arguments and `@deprecated`**, not new versions (S7).

## React and Next.js
- A component re-renders when: **its state changes, its parent renders, a context it uses changes, or an external store changes.**
- `useEffect` is for syncing with **things outside React**. Not for calculating values from props or state.
- Memoize (`useMemo`, `memo`) only **after measuring**. The React Compiler can do it for you.
- Server data → **TanStack Query** or Server Components, not Redux.
- **Server Components by default.** `'use client'` only on small interactive parts.
- Props passed to client components are **put into the HTML** (S1!).
- SSG = built at build time · ISR = rebuilt every X seconds · SSR = built per request · CSR = built in the browser.
- Server Actions are **public endpoints**. Validate input and check permissions inside them.
- Never put personal user data in a shared cache.

## Web
- Core Web Vitals: **LCP ≤ 2.5 s · INP ≤ 200 ms · CLS ≤ 0.1** (for 75% of visits).
- `defer` for your app scripts. `async` for scripts that don't depend on anything.
- Files with a hash in the name: `max-age=1y, immutable`. HTML: `no-cache`.

## Databases
- Slow query? Find it (`pg_stat_statements`) → `EXPLAIN ANALYZE` → add an index or rewrite → test again with real data size.
- Composite index (on several columns) works from the **leftmost column**. Order: equality columns → sort column → range column.
- Indexes make reads faster and writes slower.
- Keep transactions short. **No HTTP calls inside.** Always lock rows in the same order.
- Safe stock update: `UPDATE … SET qty = qty - n WHERE sku = ? AND qty >= n`.
- Pool size × number of pods must stay under `max_connections`.
- Migrations: **expand → migrate → contract** (add new, move data, remove old). Never rename in one step.

## Redis and queues
- **Cache-aside**: read cache → miss → read DB → save with a TTL + some randomness (jitter). On write: update DB, then **delete** the cache key.
- Cache stampede (everyone misses at once): only one request rebuilds (single-flight/lock), serve stale while refreshing, add jitter.
- Queues deliver **at least once**, so a message can come twice. Consumers must be **idempotent**: save processed message IDs in the same transaction.
- Retry temporary errors with backoff + jitter. After N failures, send to a **DLQ** (dead-letter queue). Alert when DLQ messages get old.
- Keep order **per entity** (partition key / session / message group).
- **Outbox** fixes the "dual write" problem: save the event in the same DB transaction, then publish it.

## Docker / Kubernetes / CI-CD
- Image = the template. Container = the running process.
- Multi-stage build, `npm ci --omit=dev`, run as non-root, `CMD ["node", …]` so Node gets shutdown signals.
- **Readiness** = "Should I get traffic?" **Liveness** = "Should I be restarted?" Never check the DB in liveness.
- Over the memory limit → **OOMKilled (exit code 137)**. Over the CPU limit → slowed down (throttled).
- **Build once, promote the same image** to each environment. Canary + feature flags + fast rollback.

## System design
- Steps: clarify → estimate → API → data → boxes → deep dive → wrap up.
- 1M requests/day ≈ **12 per second**. Peak is 2–3× (sales: 10×).
- Start with a **modular monolith**. Split into services only for teams, scaling, or isolation.
- **Saga** = a chain of local transactions, each with an "undo" step. Use a central orchestrator for critical flows.
- Choose consistency for money and stock. Choose availability for reviews and counters.
- Stay up when things fail: timeouts, limited retries, circuit breaker, bulkhead (separate pools so one failure doesn't sink all), fallback, load shedding (drop extra traffic).

## Coding round
- Restate → edge cases → brute force first → improve → code and talk → test by hand → say time **and** space.
- Clue words: "count/duplicate" = hash map · brackets = stack · "longest substring" = sliding window · sorted = binary search · intervals = sort first · "top K" = heap.

## Behavioral
- Never criticize your employer. Talk about what you are moving **toward**.
- Mistake story: own it → impact → fix → **what you changed** so it can't happen again.
- Conflict story: make it **technical**, based on evidence, and say you committed to the final decision.
- Always have **2–3 questions** for them.

## Before you join the call
- [ ] Join 5–10 minutes early. Camera, mic, internet, charger, quiet room.
- [ ] Water, CV, the job description, and these notes (closed during coding).
- [ ] One line on why **this** company.
- [ ] Your salary range and notice period decided.
- [ ] Code editor ready, with a blank file and Node working.
- [ ] Breathe. **Speak slowly.** Ask when something is unclear. Pausing to think is fine.

## If your mind goes blank
- "Let me take a second to structure my answer."
- "Can I think out loud for a moment?"
- Fall back to the formula: **What is it? How does it work? Where have I used it? What's the trade-off?**
