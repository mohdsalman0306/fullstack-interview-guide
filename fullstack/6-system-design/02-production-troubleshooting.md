# Production Troubleshooting Scenarios

> **Time needed:** about 2 hours · **Questions:** 23
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

These questions test **calm thinking under pressure**. The interviewer wants a **clear, ordered process**, not a lucky guess.

## Progress checklist
- [ ] 1. The general incident method
- [ ] 2. API latency jumps from 200 ms to 8 seconds
- [ ] 3. CPU at 100%
- [ ] 4. Memory keeps increasing
- [ ] 5. Database overloaded / connection pool exhausted
- [ ] 6. An external API is slow or failing
- [ ] 7. Redis is down
- [ ] 8. Duplicate orders or payments
- [ ] 9. Queue backlog keeps growing
- [ ] 10. Messages going to the dead-letter queue
- [ ] 11. Kubernetes pods keep restarting
- [ ] 12. A bad deployment reached production
- [ ] 13. Users see someone else's data
- [ ] 14. A large export crashes the service
- [ ] 15. The GraphQL endpoint is slow
- [ ] 16. Random 502 errors behind a load balancer
- [ ] 17. A cron job runs twice
- [ ] 18. Users are randomly logged out
- [ ] 19. Works locally, fails in production
- [ ] 20. A JWT must be revoked now
- [ ] 21. Preparing for a big sale
- [ ] 22. A tiny change breaks everything, and the error makes no sense
- [ ] 23. Products missing from search

---

## 1. Something is broken in production. What do you do?

**In simple words**
Use the same order every time. First protect users, then find the cause, then stop it from happening again.

**Picture it**
A doctor in the emergency room. First stabilize the patient, then run tests, then treat the cause.

**What you'd do, in order**
1. **Impact.** Who is affected, how badly, since when? Checkout down, or one report? Tell people (incident channel, status page).
2. **Stop the bleeding.** Mitigate **before** you fully understand it: roll back, turn off a feature flag, scale up, fail over.
3. **Evidence.** Dashboards, logs, traces. Key question: **what changed?** Deploy, config, traffic, data, a dependency.
4. **Test one idea at a time.**
5. **Fix the root cause** properly.
6. **Verify.** Metrics are back to normal, not just "it looks OK".
7. **Blameless post-mortem.** What happened, why our safety checks missed it, action items with owners.

**🗣️ Say this in the interview**
> "I always follow the same order. First, impact: who's affected and since when, and I tell people. Then I stop the bleeding before I fully understand it: roll back, flip a feature flag, scale up. Restoring service comes first. Then I gather evidence and ask: what changed? I test one idea at a time, fix the real cause, and check the metrics are truly normal. Finally, a blameless post-mortem with action items and owners."

**⚠️ Avoid saying**
Don't jump into debugging code while customers are down. Mitigate first.

---

## 2. API latency jumps from 200 ms to 8 seconds. What do you do?

**What you'd check first, in order**
1. Scope: all endpoints or one? All servers or one?
2. Since when? Does it match a deploy or a traffic change?
3. Open a **trace** of a slow request (a timeline of where the time went): our code, DB, Redis, or an external API?
4. p95/p99 per endpoint (speed of the slowest 5% / 1% of requests), DB slow-query log, **connection-pool wait time**, event-loop lag, CPU, memory.

**Common causes:** a new query without an **index**; a new **N+1** (one query per item instead of one for all); **pool exhaustion** (requests waiting for a free DB connection); a slow third-party API with **no timeout**; event-loop blocking; cache hit rate collapsed (eviction, or a cache key changed in the deploy).

**The fix**
- Now: roll back if it matches a deploy; scale out if it's only traffic; turn off an expensive feature; shorter timeouts so requests fail fast instead of piling up.
- Prevent: alerts on p95/p99, query review in PRs, load tests for critical endpoints, timeouts and circuit breakers on all dependencies.

**🗣️ Say this in the interview**
> "First I check the scope: all endpoints or one, all servers or one, and does it line up with a deploy or traffic change? Then I open a trace of a slow request to see where the time goes: our code, the database, Redis, or an external API. If it matches a deploy, I roll back first. Usual causes are a missing index, a new N+1, a full connection pool, or a slow third-party call with no timeout."

**⚠️ Avoid saying**
Don't guess "it's probably the database" before you look at a trace.

---

## 3. CPU is at 100%. What do you do?

**What you'd check first, in order**
1. One server or all?
2. Does it match a deploy or a traffic spike? Which endpoint is hot?
3. Take a **CPU profile** of one affected server. The **flame graph** (a picture of where CPU time goes) shows the exact hot function.
4. Check event-loop lag and garbage collection (GC) activity.

**Common causes:** a heavy loop over big data; huge JSON serialization; **regex backtracking** (a bad regex that tries millions of paths); sync crypto or compression; **GC thrashing** (memory so full the garbage collector runs all the time); an endless retry loop.

**The fix**
- Now: roll back, scale out for a while, rate limit the hot endpoint.
- Then: optimize the code, cache the result, paginate, or move CPU work to a worker thread or background job.
- Prevent: profile heavy features before release, alert on event-loop lag, load test.

**🗣️ Say this in the interview**
> "I check if it's one server or all, and if it matches a deploy or a traffic spike. Then I take a CPU profile. The flame graph shows the exact hot function, so I'm not guessing. Often it's a big loop, huge JSON, a bad regex, or sync crypto on the main thread. I might scale out, but only for a short time, because scaling hides the cause."

**⚠️ Avoid saying**
Don't say "I'd just add more servers". That hides the cause.

---

## 4. Memory keeps increasing. What do you do?

**What you'd check first, in order**
1. Is it a **leak**? A leak keeps rising **after garbage collection** and never drops. Growth under load that goes back down is normal.
2. Heap used vs RSS (total process memory) over time. Are pods **OOMKilled** (killed for using too much memory)?
3. Does it match a deploy or an endpoint?
4. Take **heap snapshots** a few minutes apart, compare them, and follow what is holding on to the growing objects (the "retainer path").

**Common causes:** an in-memory cache or `Map` with no size limit; event listeners added per request; timers never cleared; global arrays; closures holding big objects; big buffers kept too long.

**The fix**
- Now: rolling restarts, or a memory limit so pods restart before they get slow. Roll back if a deploy caused it.
- Then: limit the cache (LRU with a max size, or move it to Redis), remove listeners, clear timers.
- Prevent: soak tests (hours under load), memory dashboards, review module-level caches.

**🗣️ Say this in the interview**
> "First I confirm it's a leak: memory that keeps rising even after garbage collection. Then I take heap snapshots a few minutes apart, compare them, and follow what's holding the growing objects. Usual causes are an unbounded in-memory cache, listeners added per request, or timers never cleared. Rolling restarts keep us alive short term. The real fix is to bound the cache or move it to Redis."

**⚠️ Avoid saying**
Don't call normal growth under load a "leak". Check after GC.

---

## 5. The database is overloaded, or the connection pool is exhausted. What do you do?

**What you'd check first, in order**
1. On the DB: **top queries by total time** (`pg_stat_statements` / slow log), CPU and IOPS.
2. Active vs max connections, and **lock waits**.
3. On the app: **pool wait time**. Are requests waiting for a connection?

**Common causes:** missing index; N+1; **long transactions** holding connections (sometimes an HTTP call inside a transaction); **connection leaks** (not released after an error); autoscaling added pods, so **pods × pool size** is more than `max_connections`; a heavy report on the primary DB.

**The fix**
- Now: kill runaway queries; turn off expensive features or reports; send reads to a replica; rate limit for a while.
- Then: indexes, query rewrites, batch the N+1, release connections in `finally`, a pooler (PgBouncer / RDS Proxy), reports on a replica.
- Prevent: query review, pool metrics and alerts, capacity planning that includes autoscaling.

**🗣️ Say this in the interview**
> "I look at the database first: top queries by total time, lock waits, active connections. Then the app: are requests waiting for a free connection? A common surprise is autoscaling: more pods times the pool size goes over the database's max connections. Other causes are a missing index, N+1, long transactions, or connections not released after an error. Fixes: indexes, release in finally, a pooler like PgBouncer, reports on a replica."

**⚠️ Avoid saying**
Don't just raise the pool size. It pushes more load onto a struggling database.

---

## 6. An external API is slow or failing. What do you do?

**What you'd check first, in order**
1. Our dashboards: that dependency's latency and error rate.
2. Their status page, to confirm.
3. Is it on our critical path (like checkout)?

**Common causes:** the vendor is down or slow, and we have no timeout, so our requests pile up waiting.

**The fix**
- Now: shorter **timeouts**; a **circuit breaker** so we fail fast; a **fallback** (cached data, a "temporarily unavailable" message, or queue the work for later).
- Then: retry only idempotent calls, with backoff, jitter and a limit; a **bulkhead** (its own connection pool) so it can't use all our resources; move it off the critical path (async via a queue).
- Prevent: per-dependency alerts, SLAs with the vendor, design every integration as if it **will** fail.

**🗣️ Say this in the interview**
> "The goal is that their problem doesn't become our outage. I check our dashboards for that dependency and their status page. Then I tighten timeouts, add a circuit breaker so we fail fast, and a fallback like cached data or queueing the work. I give it its own connection pool, and only retry idempotent calls, with backoff and a limit."

**⚠️ Avoid saying**
Don't say "we wait for the vendor". Show how you protect your own system.

---

## 7. Redis is down. What happens, and what do you do?

**What you'd check first, in order**
1. What does Redis do for us? Only cache, or also sessions, rate limits, queues?
2. Is the app degrading or dying?
3. Is the database getting hit by the sudden extra load?

**Common causes:** a Redis node failure, memory pressure, and no automatic failover.

**The fix**
- **Only a cache:** the app should get slower, not die. Short timeouts, a circuit breaker, fall back to the DB. Protect the DB, maybe by turning off non-critical features.
- **Sessions, rate limits or queues:** it's a real outage. Focus on failover.
- Restore through managed failover. **Warm the cache slowly**, not all at once.
- Prevent: replicas plus automatic failover, a design where losing the cache is survivable, and really test the app with Redis down.

**🗣️ Say this in the interview**
> "It depends on what Redis does for us. If it's only a cache, the app should degrade, not die: short timeouts, a circuit breaker, and fall back to the database. But I'd protect the database from the sudden load. If Redis holds sessions, rate limits or queues, it's a real outage and I focus on failover. Afterwards, I make sure we actually test with Redis down."

**⚠️ Avoid saying**
Don't say "it's just a cache, no problem". Losing it can overload the database.

---

## 8. Customers got duplicate orders or payments. What do you do?

**What you'd check first, in order**
1. **Contain it**: find affected customers. Refund or cancel duplicates with the payment provider.
2. Find **where** the duplicate came from: a double click, a client retry after a timeout, a webhook sent twice, or a queue message processed twice.

**Common causes:** no idempotency key; no unique constraint; consumers that create a second result when they run twice.

**The fix**
- **Idempotency key** on order and payment creation (see `3-backend/02-rest-api-express.md`).
- **Unique constraint** on the business action, e.g. one order per cart ID, one payment per provider transaction ID.
- **Idempotent webhook and queue consumers** (see `3-backend/06-redis-queues.md`).
- Disable the button on submit. This is only UX, never the real protection.
- Prevent: a reconciliation job comparing orders with provider transactions; an alert on duplicate patterns.

**🗣️ Say this in the interview**
> "First I contain it: find the affected customers and refund or cancel duplicates with the payment provider. Then I find the source: a double click, a client retry, a webhook sent twice, or a message processed twice. The real fix is on the server: an idempotency key on creation, a unique constraint like one payment per provider transaction ID, and idempotent consumers. Disabling the button helps UX but is never the real protection."

**⚠️ Avoid saying**
Don't give "disable the button" as the main fix.

---

## 9. The queue backlog keeps growing. What do you do?

**What you'd check first, in order**
1. **Incoming rate** vs **processing rate**.
2. **Age of the oldest message**. This matters more than the count.
3. Are consumers failing and retrying the same messages?
4. Is a downstream (DB, API) slow?

**Common causes:** a slow downstream call in the handler; a **poison message** (a bad message that always fails and is retried forever); consumers crashing; a traffic spike; prefetch or batch settings too low.

**The fix**
- Now: add consumers, **but only as much as the downstream can handle**. If the DB is the bottleneck, more consumers make it worse. Pause non-critical producers.
- Then: optimize the handler, move poison messages to the DLQ, tune concurrency and prefetch, add **backpressure** (slow producers down when consumers can't keep up).
- Prevent: alert on oldest-message age; autoscale consumers on queue depth (KEDA in Kubernetes).

**🗣️ Say this in the interview**
> "A growing backlog means messages come in faster than we process them. I compare the incoming and processing rates, and look at the age of the oldest message, which matters more than the count. I check for a poison message retried forever. I can add consumers, but only as far as the downstream can handle. If the database is the bottleneck, more consumers make it worse."

**⚠️ Avoid saying**
Don't say "just add more consumers".

---

## 10. Messages are going to the dead-letter queue. What do you do?

**In simple words**
The **dead-letter queue (DLQ)** is where messages go after failing too many times. Like the "returned mail" box at the post office.

**What you'd check first, in order**
1. Read a sample of the messages and their **failure reason**.
2. Group them: bad data (permanent), a bug in a new deploy, or a downstream outage (temporary)?

**The fix**
- Fix the code, config or data.
- Then **replay** them with a controlled tool, in batches. Safe because consumers are **idempotent**.
- Messages that can never succeed (invalid data) are logged and handled by hand.
- Prevent: alert on DLQ count **and** age; sort errors in code so permanent failures go to the DLQ at once, instead of retrying 10 times.

**🗣️ Say this in the interview**
> "I read a sample of the dead-lettered messages and their failure reasons, and group them: bad data, a bug in a new deploy, or a downstream outage. I fix the cause, then replay in batches with a controlled tool. That's safe because the consumers are idempotent. Messages that can never succeed get logged and handled manually."

**⚠️ Avoid saying**
Don't replay everything before fixing the cause. It will just fail again.

---

## 11. Kubernetes pods keep restarting. What do you do?

**What you'd check first, in order**
1. `kubectl describe pod`: the **exit code and events**.
2. `kubectl logs --previous`: logs of the crashed container, not the new one.
3. Match the clue:

| Clue | Likely cause | Fix |
|---|---|---|
| `OOMKilled`, exit 137 | Memory limit too low, or a leak | Check the trend; fix the leak, or set the right limit and V8 heap size |
| Liveness probe failed | Probe too strict, slow startup, or event loop blocked | Add a startup probe, fix the probe, don't check dependencies in liveness |
| Exit 1 right after start | Crash on boot: missing env var or secret, can't reach DB | Read logs; validate config at startup with clear errors |
| `CrashLoopBackOff` after a deploy | Bug in the new version | Roll back, then debug |

**🗣️ Say this in the interview**
> "I run kubectl describe pod and kubectl logs with --previous first. The exit code and events usually tell the story. OOMKilled, exit 137, means memory. A failed liveness probe can mean the probe is too strict or the event loop is blocked. Exit 1 right after start is usually a missing env var or secret. CrashLoopBackOff after a deploy means roll back, then debug. I don't just raise limits or loosen the probe until I know why."

**⚠️ Avoid saying**
Don't say "I'd increase the memory limit" before you know why.

---

## 12. A bad deployment reached production. What do you do?

**What you'd do, in order**
1. **Stop the rollout.**
2. **Roll back** to the last good version, or turn the feature flag off.
3. Confirm recovery on **technical** metrics (errors, latency) **and business** metrics (orders per minute, payment success).
4. Keep the evidence (logs, traces, the diff). Tell stakeholders the impact.
5. Blameless post-mortem: not "who broke it" but **why the pipeline didn't catch it** (missing test, no canary, no alert).

**Prevent it:** canary deploys (release to a few users first) with automatic rollback on error rate, feature flags, backward-compatible DB migrations so rollback is always possible.

**🗣️ Say this in the interview**
> "Roll back first, debug second. I stop the rollout, roll back or turn the flag off, and confirm recovery on technical metrics and business metrics like orders per minute. I keep the evidence and tell stakeholders. Then a blameless post-mortem: not who broke it, but why our pipeline didn't catch it, like a missing test, no canary, or no alert."

**⚠️ Avoid saying**
Don't try to "fix forward" while customers are affected.

---

## 13. Users see someone else's data. What do you do?

**In simple words**
This is a **security incident**. Usually a caching bug: a page made for one user was saved and shown to others.

**What you'd check first, in order**
1. **Purge the cache now.** If needed, **bypass caching** for the affected routes.
2. Cache headers on the affected responses.
3. Cache keys: do they include user, customer group, currency, store?
4. Shared state in the code.

**Common causes:** a personal response with `Cache-Control: public`, so the **CDN** cached it; a cache key **missing user, customer group, currency or store**; Next.js caching a user-specific fetch; a module-level variable in Node holding per-request data, shared across requests.

**The fix**
- `private, no-store` for personal responses; correct cache keys; per-request context instead of module-level state.
- Prevent: tests that check cache headers on logged-in routes; a code review checklist item.
- After: check what data was exposed, and bring in security/legal if personal data was involved.

**🗣️ Say this in the interview**
> "This is a security incident, so it's top priority. I purge the cache immediately and bypass caching for those routes if needed. Usually the cause is a personal response marked public so the CDN cached it, a cache key missing the user or store, or a module-level variable in Node shared across requests. The fix is private, no-store, correct cache keys, and per-request state. Then I check what was exposed and involve security or legal if personal data leaked."

**⚠️ Avoid saying**
Don't treat it as a normal ticket for next sprint.

---

## 14. A large export crashes the service. What do you do?

**What you'd check first, in order**
1. Memory at the time of the crash. Was it out of memory?
2. Does the code load all rows into an array, or build one huge string or file?

**Common cause:** building the **whole result in memory**.

**The fix**
- **Stream** it: read rows with a **cursor or in pages**, transform as a stream, and pipe straight to the response or to object storage.
- Use `stream.pipeline` so **backpressure** is handled (reading slows down when writing can't keep up).
- Very large exports become a **background job**: a worker writes the file to S3/Blob, and the user gets a **signed download link** by email or notification.

**🗣️ Say this in the interview**
> "It's almost always building the whole result in memory: all rows in an array, then one huge string. The fix is to stream: read rows with a cursor or in pages, transform them as a stream, and pipe straight to the response or storage, with stream.pipeline handling backpressure. For very big exports, a background job writes the file to S3, and the user gets a signed download link."

**⚠️ Avoid saying**
Don't say "give it more memory". The next, bigger export will crash it again.

---

## 15. The GraphQL endpoint is slow. What do you do?

**What you'd check first, in order**
1. Turn on **resolver-level tracing**: which fields take the time?
2. **Count database queries per request.**
3. Check nested list sizes and very large client queries.

**Common causes:** **N+1** from nested resolvers (each item loads its own data); missing indexes; no limit on nested lists; huge client queries.

**The fix:** batch with **DataLoader** (collect all IDs, load them in one query); add indexes; cap page sizes for nested lists; add query **complexity limits**.

**🗣️ Say this in the interview**
> "I turn on resolver-level tracing to see which fields take the time, and count database queries per request, because the usual cause is N+1 from nested resolvers. Then I batch with DataLoader, add indexes, cap page sizes for nested lists, and add complexity limits if clients send huge queries."

**From my work**
Use Story S2 (the N+1 fix) — see `1-interview-basics/02-your-story.md`. "Exactly this: 1,811 queries for one listing request. Batching resolvers brought it to 238 and roughly halved the response time. And I checked the response was **byte-identical** before and after."

**⚠️ Avoid saying**
Don't only check the slow-query log. With N+1 each query is fast; the problem is the **count**.

---

## 16. You see random 502 errors behind a load balancer. What do you do?

**What you'd check first, in order**
1. Are there errors in our app logs at the same time? Often there are **none**.
2. Compare the load balancer's **idle timeout** with Node's **`keepAliveTimeout`**.

**Common cause: keep-alive timeout mismatch.** The load balancer keeps idle connections to Node open (AWS ALB default idle timeout: 60 seconds). Node's `server.keepAliveTimeout` default is **5 seconds**. So Node closes the connection just as the load balancer sends a request on it, and the load balancer returns a 502.

**The fix:** set `keepAliveTimeout` **higher** than the load balancer's idle timeout, and `headersTimeout` a bit higher again.

```javascript
server.keepAliveTimeout = 65_000;  // more than the ALB idle timeout (60s)
server.headersTimeout = 66_000;    // more than keepAliveTimeout
```

**🗣️ Say this in the interview**
> "Random 502s with no errors in our app logs often point to keep-alive timeouts. AWS ALB keeps idle connections open for 60 seconds by default, but Node closes them after 5. So Node closes a connection just as the load balancer sends a request on it, and the user gets a 502. The fix is to set Node's keepAliveTimeout above the load balancer's timeout, and headersTimeout a bit higher. You only recognize this bug if you've seen it, so it shows real production experience."

**⚠️ Avoid saying**
Don't blame "network issues" and move on.

---

## 17. A cron job runs twice (or N times). Why?

**What you'd check first, in order**
1. Does the scheduler run **inside the app**?
2. How many replicas (copies) of the app are running?

**Common cause:** the job runs inside the app, and we scaled to 3 replicas, so it runs **3 times**. Even real schedulers can sometimes run a job twice.

**The fix**
- Move it to a **Kubernetes CronJob** with `concurrencyPolicy: Forbid` (or a cloud scheduler), so it runs once, outside the app pods.
- If it must stay in the app, take a **distributed lock** (Redis or a DB advisory lock).
- Make the job **idempotent** (running it twice gives the same result).

**🗣️ Say this in the interview**
> "The classic cause is that the job runs inside the app, and we scaled to 3 replicas, so it runs 3 times. I'd move it to a Kubernetes CronJob with concurrencyPolicy Forbid, or a cloud scheduler. If it must stay in the app, I take a distributed lock in Redis or the database. And I make the job idempotent, because even schedulers can occasionally run twice."

**⚠️ Avoid saying**
Don't rely on "only one pod will run it". Make it safe to run twice.

---

## 18. Users are randomly logged out. Why?

**What you'd check first, in order**
Look for anything that makes **one server** unaware of a session made on **another**:
1. Where are sessions stored? In each server's memory?
2. Same JWT secret/keys on all servers? Did they change in a deploy?
3. Cookie settings: `Domain`, `SameSite`, `Secure`.
4. Are server clocks in sync?
5. Is Redis evicting (removing) sessions because memory is full?
6. Two tabs refreshing the token at once?

**Common causes**
- **In-memory sessions** with several servers: logged in on A, the load balancer sends you to B, which doesn't know you.
- **Different JWT keys** across servers or after a deploy.
- **Cookies**: wrong `Domain`, `SameSite`, or `Secure` on plain HTTP.
- **Clock skew**: clocks differ, so expiry checks disagree.
- **Redis evicting sessions** under memory pressure.
- **Refresh token rotation race**: two tabs refresh together, one gets a "reused token" error.

**The fix:** a shared session store (Redis) with enough memory, the same keys everywhere, correct cookies, synced clocks, refresh logic that handles two tabs.

**🗣️ Say this in the interview**
> "I look for anything that makes one server unaware of a session created on another. The classic is in-memory sessions with several servers: fine on server A, logged out on B. The fix is a shared store like Redis. Other causes: different JWT keys after a deploy, wrong cookie settings, clocks out of sync, Redis evicting sessions, or two tabs refreshing the token at the same time."

**⚠️ Avoid saying**
Don't just make tokens last longer. That hides the problem and weakens security.

---

## 19. It works locally but fails in production. What do you do?

**What you'd check first, in order**
List what's **different**, and check each:
1. **Env variables and secrets.**
2. **Data size**: fine with 100 rows, slow with 10 million.
3. **Network**: firewalls, DNS, TLS certificates, timeouts.
4. **Multiple servers**: in-memory state, race conditions.
5. **Build**: dev vs production mode, env missing at build time in Next.js.
6. **OS/filesystem**: file names are case-sensitive on Linux.
7. **Timezone or locale.**

Production logs and traces usually narrow it down fast.

**🗣️ Say this in the interview**
> "I list what's different between the two environments and check each: env variables and secrets, data volume, network things like DNS and TLS, multiple servers sharing state, build mode, case-sensitive file names on Linux, and timezone. Production logs and traces usually narrow it down quickly. And I check that the code I think is running is really the code that's running."

**From my work**
"Our local environment was quietly running **four-day-old dependency-injection config**. The compiled files were stale, and the compile step finished 'successfully' without doing anything. Lesson: when behaviour doesn't match the code, **check the code you think is running is actually running**."

**⚠️ Avoid saying**
Don't say "it works on my machine". Show a method for finding the difference.

---

## 20. A JWT must be revoked right now. How?

**In simple words**
A JWT stays valid until it expires. The server doesn't normally check a list. To kill one early, you need an extra check.

**What you'd do, in order**
1. Disable the account.
2. **Revoke all refresh tokens.**
3. For access tokens still valid: check a **denylist of token IDs** in Redis until they expire, **or** bump a **token version** on the user so all older tokens fail.
4. Long term: short-lived access tokens (5–15 minutes), so this window is always small.

**🗣️ Say this in the interview**
> "I disable the account and revoke all refresh tokens. For access tokens still valid, I either check a denylist of token IDs in Redis until they expire, or bump a token version on the user so all older tokens fail. Long term, I keep access tokens short-lived, 5 to 15 minutes, so this window is always small."

**⚠️ Avoid saying**
Don't say "just delete the token". It lives in the client, not on the server.

---

## 21. How do you prepare for a high-traffic sale event?

**What you'd do, in order**
1. **Load test** at 2–3× the expected peak. Find what breaks first. Fix it.
2. **Pre-scale** app servers and database capacity before the start.
3. **Cache** the catalog at the CDN; **warm** the caches.
4. **Isolate checkout** capacity from browsing.
5. **Rate limit** per user; block bots.
6. **Queue** non-critical work (emails, ERP sync).
7. **Feature flags** ready to switch off heavy, non-essential features.
8. **Freeze deploys** around the event.
9. **War room**: business dashboards (orders/min, payment success), people on call, a rollback plan.

**🗣️ Say this in the interview**
> "Preparation matters more than reaction. I load test at 2 to 3 times the expected peak and fix what breaks first. I pre-scale, cache the catalog at the CDN and warm it, and isolate checkout from browsing. I rate limit per user, queue non-critical work like emails, and have feature flags ready. We freeze deploys around the event, and run a war room with business dashboards, on-call people and a rollback plan."

**Tip:** this is your home ground. You can start with "On the platform I work on…". See also the checkout design in `6-system-design/01-system-design.md`.

**⚠️ Avoid saying**
Don't say "autoscaling will handle it". It's too slow for a spike at the exact sale start.

---

## 22. A tiny change breaks everything, and the error makes no sense. What do you do?

**What you'd do, in order**
1. **Revert the change** to restore service.
2. **Stop trusting the error message.**
3. **Bisect** the change (undo half, test, repeat) to find the exact line.
4. Check your assumptions **one layer down**, in the framework or library code.

**🗣️ Say this in the interview**
> "When a small change breaks everything, I first revert it to restore service. Then, if the error doesn't make sense, I stop trusting it. I bisect the change to find the exact line, and check my assumptions one layer down, in the framework or library code."

**From my work**
Use Story S6 (one character that broke every GraphQL query) — see `1-interview-basics/02-your-story.md`. "A single `{` inside a documentation string broke **every** GraphQL query, because the framework's schema reader used a regex that didn't understand strings. Another time, a library bug in error formatting **hid** the real error behind an unrelated one."

**⚠️ Avoid saying**
Don't keep changing random things. Bisect step by step.

---

## 23. Products are missing from search, or the DB and search index disagree. What do you do?

**In simple words**
The database is the source of truth. The search index is a **copy**, and copies can drift apart.

**What you'd check first, in order**
1. Is the product in the index at all?
2. Compare the indexed version with the DB.
3. **Indexer or queue lag**, and errors.
4. The rules for what gets indexed: visibility, status, stock.

**Common causes:** a failed indexing event; the indexer is behind; data filtered out on purpose.

**The fix:** reindex the affected items, fix the sync, and add a **reconciliation job** that compares counts and alerts on drift.

**🗣️ Say this in the interview**
> "The database is the source of truth and the search index is a copy, so they can drift. I check if the product is in the index at all, compare it with the database, check indexer lag and errors, and check the rules for what gets indexed, like visibility and status. Then I reindex the affected items, fix the sync, and add a reconciliation job that alerts on drift."

**From my work**
"Products set to 'not visible individually' weren't in the search index at all, so any feature that looked them up through search could never find them. It needed both a code workaround and a catalog clean-up."

**⚠️ Avoid saying**
Don't just "reindex everything". Find why it drifted, or it will happen again.

---

## Remember these 10 things
1. Same order every time: impact → stop the bleeding → evidence → fix → verify → blameless post-mortem.
2. Always ask: **what changed?** Deploy, config, traffic, data, dependency.
3. Roll back first, debug second.
4. Use traces and profiles (flame graphs, heap snapshots). Don't guess.
5. Scaling out is short-term. It hides the cause.
6. Pods × pool size can go over the database's max connections.
7. Every external call needs a timeout, a circuit breaker and a fallback.
8. Duplicates: idempotency keys + unique constraints + idempotent consumers. The button is only UX.
9. Seeing someone else's data = security incident. Purge the cache; use `private, no-store`.
10. Random 502s behind an ALB: set Node's `keepAliveTimeout` above 60 s.
