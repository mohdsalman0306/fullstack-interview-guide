# System Design

> **Time needed:** about 3 hours · **Questions:** 16
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

## Progress checklist
- [ ] 1. How do you run a 45-minute design round?
- [ ] 2. Rough numbers (estimation cheat sheet)
- [ ] 3. Building blocks: when to use what
- [ ] 4. Design a URL shortener
- [ ] 5. Design a rate limiter
- [ ] 6. Design a notification system
- [ ] 7. Design an e-commerce checkout (and survive a flash sale)
- [ ] 8. Design product catalog and search
- [ ] 9. Design a chat system
- [ ] 10. Design a file upload service
- [ ] 11. Monolith or microservices?
- [ ] 12. The saga pattern
- [ ] 13. CAP theorem and consistency
- [ ] 14. Resilience patterns
- [ ] 15. CQRS and event sourcing
- [ ] 16. Caching layers in a design

---

## 1. How do you approach a system design round?

**In simple words**
There is no single "right answer". The interviewer watches **how you think**. Do you ask questions? Do you start simple? Do you find the weak spots and explain trade-offs out loud?

**The 7 steps (45 minutes)**

| Step | Time | What you do | What you say |
|---|---|---|---|
| 1. Clarify | ~5 min | What it must do, what "good" means (speed, uptime), what is out of scope | "Before I design anything, let me ask a few questions…" |
| 2. Estimate | ~3 min | Users, requests per second, storage, reads vs writes | "Let me do some quick numbers so I know the scale." |
| 3. API | ~5 min | Main endpoints or operations | "These are the core operations…" |
| 4. Data model | ~5 min | Main tables/entities, which database | "This data is relational and needs transactions, so…" |
| 5. High-level design | ~10 min | Draw the boxes and the request flow | "Let me walk through one request end to end." |
| 6. Deep dives | ~12 min | Bottlenecks, failures, scaling, consistency | "The hardest part here is ___. Let me focus on that." |
| 7. Wrap up | ~2 min | Summary, monitoring, next steps | "To summarize… With more time I'd look at…" |

**Questions to always ask first**
- Who are the users, and how many? Daily active users? Peak vs normal?
- Which features are **must-have** for today?
- More reads or more writes?
- How fast must it feel (latency)?
- What matters more: **consistency** (always correct data) or **availability** (always answers)? Payments need consistency. A like counter needs availability.
- One region or global?
- Any limits: existing tech, team size, budget, legal rules?

**🗣️ Say this in the interview**
> "Let me start by clarifying the requirements. Then I'll do some rough numbers, so I know the scale. I'll start with a simple design and improve it step by step. Please stop me anytime if you want to go deeper somewhere."

**⚠️ Avoid saying**
Don't start drawing boxes in the first minute. Ask questions first.

---

## 2. Can you do a quick estimate? (Back-of-the-envelope numbers)

**In simple words**
You don't need exact numbers. You need the right **size** (thousands per second, or millions?). Round hard and say your assumptions out loud.

**Numbers to remember**

| Fact | Number |
|---|---|
| Seconds in a day | 86,400 ≈ **100,000** |
| 1 million requests/day | ≈ **12 per second** |
| 100 million requests/day | ≈ **1,200 per second** |
| Peak vs average | Usually **2–3×**, and **10×+** during sales or launches |
| 1 million rows × 1 KB | ≈ **1 GB** |
| Normal read:write ratio | 10:1 to 100:1 for most web apps |

**How fast things are (rough)**

| Action | Time |
|---|---|
| Read from memory | ~100 nanoseconds |
| SSD random read | ~0.1 ms |
| Redis GET in the same data centre | < 1 ms |
| Database query with an index | ~1–10 ms |
| Network round trip in one region | ~1 ms |
| Network round trip across continents | ~100–150 ms |

**🗣️ Say this in the interview**
> "I'll state my assumptions and round a lot. The point is the order of magnitude, not exact numbers. So, 100 million requests a day is about 1,200 per second on average. At a sale peak it could be 10 times more, so I'll design for around 12,000 per second."

**⚠️ Avoid saying**
Don't do slow, exact math on the whiteboard. Round to easy numbers and move on.

---

## 3. Which building blocks do you use, and when?

**In simple words**
Most designs use the same parts. Know when each one is needed.

| Part | Use it when… |
|---|---|
| **CDN** (copies of files on servers near users) | Static files, images, cacheable pages, users around the world |
| **Load balancer** (spreads traffic across servers) | You have more than one app server; it also checks server health |
| **API gateway** (one front door for all APIs) | Many services or clients; auth, rate limits, quotas in one place |
| **Stateless app servers** (keep no user data in memory) | Always. Then you can just add more servers |
| **Relational DB** (Postgres, MySQL) | Transactions, relationships, correctness (orders, payments) |
| **Document / key-value DB** | Flexible data shape, simple lookups, huge scale |
| **Cache (Redis)** | Hot reads, sessions, rate limits, counters |
| **Search engine** (Elasticsearch/OpenSearch) | Full-text search, filters with counts (facets), ranking |
| **Message queue / topic** | Slow work, traffic spikes, retries, one event to many listeners |
| **Object storage** (S3, Blob) | Files, images, exports, backups |
| **Read replicas** (read-only copies of the DB) | Too many reads for one database |
| **Sharding** (splitting data across many DBs) | Too many writes or too much data for one server. Last option |
| **WebSockets / SSE** | Real-time updates to the browser |

**🗣️ Say this in the interview**
> "I keep app servers stateless so I can scale them by adding more. I put a CDN in front for static and cacheable content, Redis for hot reads, and a queue for slow work. Sharding is my last option, because it adds a lot of complexity. Read replicas and caching usually come first."

**⚠️ Avoid saying**
Don't add every box "just in case". Say *why* each part is there.

---

## 4. Design a URL shortener

**1) What it must do**
- Create a short link from a long URL. Optional custom alias and expiry date.
- Redirect the short link to the long URL.
- Count clicks (analytics).
- It is **very read-heavy**: many more clicks than new links.

**2) Rough numbers**
- 100M new links per month ≈ **40 writes per second**.
- Reads at 100:1 ≈ **4,000 redirects per second** on average, more at peak.
- Storage: 500 bytes × 100M × 12 months × 10 years ≈ **6 TB**. Fine for a sharded store.

**API**
- `POST /links` `{ longUrl, alias?, expiresAt? }` → `{ shortUrl }`
- `GET /{code}` → redirect

**3) The simple design**
```
Client → CDN / Load balancer → Redirect service → Redis cache (hot links)
                                                → Database (code is the primary key)
Link management API (create, edit) → same database
Redirect service → click event → Queue → Analytics store
```
The **redirect path is separate** from the link-management API. They scale very differently.

**4) The hard part: making the short code**
7 characters of base62 (a–z, A–Z, 0–9) gives 62⁷ ≈ **3.5 trillion** codes. Three ways:
- **Counter + base62**: unique and short, but in order, so people can guess codes. Each server can take a **range** of IDs, so there's no single bottleneck.
- **Random code + unique constraint**: simple and hard to guess. On a rare clash, just try again.
- **Hash of the URL**: the same URL gives the same code. You still need to handle clashes.

**301 or 302 redirect?**
- **301** = permanent. Browsers cache it. Less load, but **you lose click counts**.
- **302/307** = temporary. Every click reaches you, so you can count it.
- Pick based on whether analytics matter.

**Other hard parts**
- **Analytics must not slow the redirect.** Send a click event to a queue and process it later.
- **Abuse**: rate limit link creation, scan for malware and phishing URLs.
- **Expiry**: a TTL (time to live) plus a cleanup job.
- **Hot links**: cache and CDN.
- **Custom alias conflicts**: the unique constraint handles it.

**5) 🗣️ Say this in the interview**
> "This system is very read-heavy, around 40 writes but 4,000 reads per second. So I keep the redirect path separate and fast: a stateless service, Redis for hot links, and the database behind it. For codes, I'd use random 7-character base62 codes with a unique constraint. Clashes are extremely rare at this scale, and a retry is cheap. I'd use a 302 redirect so we can count clicks, and send each click to a queue so analytics never slows the redirect."

**They may also ask**
- *Why not just use a counter?* It works and is unique, but codes are guessable. People could walk through all your links.

**⚠️ Avoid saying**
Don't pick 301 without saying you lose analytics. Show you know the trade-off.

---

## 5. Design a rate limiter

**1) What it must do**
- Limit how many requests a user, API key or IP can make (for example, 100 per minute).
- Maybe different limits per endpoint or per plan.
- Work across many app servers.
- Ask: what happens if the limiter itself is down?

**2) Rough numbers**
Every request checks the limiter, so it must be very fast (well under 1 ms). That points to Redis.

**3) The simple design**
```
Client → API gateway (rough limits, per IP / API key)
       → App (business limits: login attempts, OTP)
              ↘ Redis (shared counters for all app servers)
```
- Counters live in **Redis**, updated **atomically** (in one step, so two servers can't both read the old value). Use a Lua script or `INCR` + `EXPIRE`.
- Too many requests → `429 Too Many Requests` with a `Retry-After` header (and `RateLimit-*` headers).

```
-- Fixed window, simplest form
key = "rl:{userId}:{currentMinute}"
count = INCR key                    -- add 1 and get the new value
if count == 1: EXPIRE key 60        -- first request: key disappears after 60 s
if count > limit: return 429
```

**4) The hard part: choosing the algorithm**

| Algorithm | How it works | Good / bad |
|---|---|---|
| Fixed window | Count per minute, reset each minute | Simplest; allows 2× burst at the edge of two windows |
| Sliding window log | Save every request time, count the last 60 s | Exact; uses more memory |
| Sliding window counter | Mix of current and previous window counts | Quite accurate, little memory |
| **Token bucket** | Bucket refills at a steady rate; each request takes a token | Allows short bursts, controls the average. Most common |

**Picture it**
Token bucket is like a coffee card with 10 stamps. You get one new stamp every second. Each request uses one stamp. No stamps left? Wait.

**When Redis is down**
- **Fail open** (let requests through) for normal APIs. Being available matters more.
- **Fail closed** (block requests) for login and OTP. Security matters more.

**Other hard parts:** very big customers creating hot keys; multi-region (each region limits on its own, so the global limit is only approximate); different limits per plan.

**5) 🗣️ Say this in the interview**
> "I'd put rough limits at the API gateway, and business limits like login attempts in the app. I'd use a token bucket. Every user has a bucket that holds, say, 10 tokens and refills at 1 per second. Each request takes a token. An empty bucket means a 429 with a Retry-After header. It allows small bursts but controls the average. The counters live in Redis and are updated atomically, so all app servers share the same count. If Redis is down, I fail open for normal APIs but fail closed for login and OTP."

**⚠️ Avoid saying**
Don't keep counters in each server's memory. With 5 servers, the user gets 5× the limit.

---

## 6. Design a notification system

**1) What it must do**
- Send email, SMS, push and in-app messages.
- Two kinds: **transactional** (OTP, order confirmation) and **marketing**.
- Respect user preferences and opt-outs.
- Ask about volume.

**2) Rough numbers**
Ask the interviewer. The key point: a marketing campaign can be millions of messages at once, while an OTP is one message that must arrive in seconds.

**3) The simple design**
```
Services → POST /notifications (or an event) → Notification service
             → checks preferences, saves request
             → per-channel queues: [email] [sms] [push] [in-app]
                 → channel workers → providers (SES/SendGrid, Twilio, FCM/APNs)
                 ← provider webhooks (delivered / bounced / failed)
```
Step by step:
1. A service sends the user, template, data and an **idempotency key** (a unique ID so the same message is never sent twice).
2. The notification service **checks** the request and the user's **preferences and opt-outs**, saves it, and puts it on a **queue per channel**.
3. **Channel workers** fill in the template (language, localization), apply **rate limits** and quiet hours, and call the **provider**.
4. Delivery status is saved. Provider **webhooks** update it to delivered, bounced or failed.
5. **Retries** with backoff for short failures, a **DLQ** (dead-letter queue, a "failed" box) for permanent ones, and a **second provider** as backup for critical channels.

**4) The hard parts**
- **Priority queues**: an OTP must never wait behind a million-email marketing campaign.
- **Idempotency**: never send the same order confirmation twice.
- **Templates** saved and versioned outside the code.
- **Legal rules**: unsubscribe links, consent records, region rules for marketing.

**5) 🗣️ Say this in the interview**
> "Services send a request with an idempotency key, so retries never create duplicate messages. The notification service checks user preferences and puts the message on a queue per channel. Workers render the template and call the provider, like SendGrid or Twilio. Webhooks tell us if it was delivered. I'd use separate priority queues, so an OTP never waits behind a big marketing campaign. Failures retry with backoff, permanent failures go to a dead-letter queue, and critical channels have a backup provider."

**⚠️ Avoid saying**
Don't put OTPs and marketing emails on the same queue. Always mention priority.

---

## 7. Design an e-commerce checkout (and survive a flash sale)

This is your home ground. Speak from experience: "On the platform I work on…"

**1) What it must do**
- Browse the catalog, cart, checkout, payment, order, inventory.
- The catalog is **very read-heavy**.
- Checkout must be **correct**: no overselling, no double charges.
- Expect big spikes on sale days.

**2) Rough numbers**
Ask for normal traffic and expected sale peak. Plan for **10×+** normal at the start of a sale. Browsing is most of the traffic; checkout is a small share but the most important.

**3) The simple design**
```
Users → CDN (static files, images, cached pages)
      → Storefront (Next.js)
      → API Gateway
          → Catalog service   → Search index (OpenSearch) + cache (Redis)
          → Cart service      → Redis / DB
          → Inventory service → DB (source of truth)
          → Order service     → Relational DB
          → Payment service   → Payment provider (PSP)
      → Message broker (OrderPlaced, PaymentCaptured…)
          → Email, ERP sync, analytics, loyalty
```

**Checkout flow**
1. **Check the cart again on the server**: prices, promotions, stock. Never trust the total sent by the browser.
2. **Reserve stock** with one atomic conditional update (`qty >= n`) and a **reservation expiry** (say 15 minutes).
3. **Create the order** as `PENDING_PAYMENT`, protected by an **idempotency key**.
4. **Pay** through the provider (with 3-D Secure). The provider's **webhook** marks the order `PAID`. Make it idempotent on the provider's transaction ID, because webhooks can arrive twice.
5. On `PAID`: confirm the reservation, and publish `OrderPlaced` through the **outbox** (save the event in the same DB transaction as the order, then a worker publishes it) to email, ERP and analytics.
6. If payment fails or times out: **release the reservation** (a delayed message or a cleanup job).

**4) The hard part: no overselling**
Use one atomic statement:
```sql
UPDATE inventory SET qty = qty - 1 WHERE sku = ? AND qty >= 1;
-- 0 rows updated = out of stock. No "read, then write" race.
```
For an extreme flash sale on a few SKUs: keep the counter in **Redis** with an atomic decrement, and sync it back to the database later.

**Flash sale extras**
- **Pre-scale** before the sale. Autoscaling is too slow for a spike at exactly 10:00:00.
- Serve the catalog from the **CDN**. Browsing should not touch the database.
- A **virtual waiting room** if demand is much bigger than stock.
- **Rate limit per user** to stop bots.
- **Bulkhead** the checkout (give it its own capacity and connection pools), so browsing can't starve it.
- **Turn off extras** with feature flags (recommendations, reviews).
- **Load test** before, at 2–3× the expected peak.
- **Watch business numbers live**: orders per minute, payment success rate, cart abandonment. Not only CPU.

**5) 🗣️ Say this in the interview**
> "The catalog is read-heavy, so it comes from the CDN, a search index and Redis. Checkout must be correct, so it uses a relational database. On checkout I re-check the cart on the server, reserve stock, and create a pending order with an idempotency key. The payment webhook marks it paid, and I publish OrderPlaced through an outbox. To prevent overselling, I use one atomic statement: update the stock where quantity is at least one. If zero rows change, it's out of stock. No race. For a flash sale, I pre-scale, serve browsing from the CDN, rate limit per user, isolate checkout capacity, and watch orders per minute live."

**They may also ask**
- *Guest cart vs logged-in cart?* Keep a guest cart by cookie, and merge it into the user's cart when they log in.
- *Order across several services?* Use a saga with compensations (see question 12).
- Other deep dives: multi-currency pricing, promotions engine, tax.

**⚠️ Avoid saying**
Don't say "read the stock, check it, then update it". Two users can both pass the check. Use one atomic update.

---

## 8. Design product catalog and search

**1) What it must do**
- Full-text search with **typo tolerance** and **synonyms**.
- **Facets**: filters with counts, like "Brass (24)".
- Good ranking (relevance), fast category pages, autocomplete.

**2) Rough numbers**
Very read-heavy. Many stores × locales × products. Price and stock change often; names and descriptions change rarely.

**3) The simple design**
```
Source of truth (relational DB or PIM)
   → product change events → Indexer → Search index (Elasticsearch/OpenSearch)
Storefront → search API → Search index
Category pages → CDN cache
Autocomplete → dedicated suggester
```
The index holds a **denormalized document** (one flat document with everything ready, no joins) per product, per store and locale: name, price, stock status, filter attributes, image, URL.

**4) The hard part: keeping the index in sync**
- Product changes publish events. An indexer updates the index in **near real time**.
- Fast-changing fields like **stock and price** get small partial updates, more often than the rest.

**5) 🗣️ Say this in the interview**
> "I'd keep the source of truth in a relational database or a PIM, and build a search index in OpenSearch for the storefront. Each product gets one flat document per store and locale, with everything the listing page needs. That gives full-text search with typo tolerance and synonyms, facets with counts, and relevance tuning. Product changes publish events, and an indexer updates the index in near real time. Stock and price get partial updates more often. Category pages are cached at the CDN."

**From my work**
This is exactly how Magento works: EAV source tables, **indexers**, and Elasticsearch/OpenSearch for the storefront. A real trap you know: products set to "not visible individually" **aren't in the search index at all**, so search-based lookups can't find them. (See `1-interview-basics/05-php-magento-background.md`.)

**⚠️ Avoid saying**
Don't run storefront search with SQL `LIKE '%...%'` on the main database. It's slow and has no typo tolerance or facets.

---

## 9. Design a chat system

**1) What it must do**
- Send and receive messages in real time, one-to-one and groups.
- Message history, online status (presence), delivery and read receipts.
- Offline users get the message later.

**2) Rough numbers**
Ask for users online at the same time and messages per second. Each online user holds one open connection, so connection count is the main number.

**3) The simple design**
```
Client ⇄ WebSocket ⇄ Gateway servers ⇄ Message service → DB (by conversation_id)
                         ↑                    ↓
          Redis: user → gateway        Pub/sub → recipients' gateways → their sockets
```
- Clients keep a **WebSocket** (an open two-way connection) to **gateway** servers. Gateways are stateful (they hold connections), so scaling and deploys need care.
- A **connection registry** in Redis maps each user to their gateway.
- **Sending**: gateway → message service → **save first** (split by `conversation_id`) → **ack** to the sender → publish to the recipients' gateways with **pub/sub** → push to their sockets.

**4) The hard parts**
- **Offline users**: send a push notification; they load history when they reconnect.
- **Ordering**: the message service gives each message a **sequence number** per conversation.
- **History**: cursor pagination by sequence number.
- **Presence**: heartbeats with a TTL in Redis. No heartbeat = offline.
- **Receipts**: delivered and read are separate small events.
- **Big groups**: sending to every member costs more as the group grows. Very large groups switch from "push to everyone" to "clients pull".

**5) 🗣️ Say this in the interview**
> "Clients hold a WebSocket to a fleet of gateway servers, and Redis tracks which gateway each user is on. When you send a message, the message service saves it first, then acks you, then publishes it through pub/sub to the recipients' gateways. Each conversation has a sequence number, so order is clear and history uses cursor pagination. Offline users get a push notification and load history on reconnect. Presence uses heartbeats with a TTL in Redis."

**⚠️ Avoid saying**
Don't ack the sender before the message is saved. If the server crashes, the message is lost.

---

## 10. Design a file upload service

**1) What it must do**
- Users upload files (maybe very big).
- Check for viruses and real file type.
- Make thumbnails or previews.
- Serve files fast, and keep private files private.

**2) Rough numbers**
Ask for file sizes and uploads per day. Big files are the reason the API server should not touch the bytes.

**3) The simple design**
```
Client → API: "I want to upload"  → DB record (PENDING) → presigned URL back
Client → uploads directly to S3/Blob (multipart for big files)
S3 event → Queue → Workers: virus scan, check real type, thumbnails → READY / REJECTED
Files served via CDN (signed URLs if private)
```
A **presigned URL** is a short-lived link that allows one upload straight to storage, without your server in the middle.

**4) The hard part**
- The API server **never handles file bytes**. It would run out of memory and bandwidth.
- **Multipart upload** lets big uploads resume after a network drop.
- Check the real file type by its **content**, not the file extension.
- **Lifecycle rules** delete abandoned uploads and archive old files.

**5) 🗣️ Say this in the interview**
> "The API server never handles the file bytes. The client asks the API for an upload. The API checks auth, size and type, creates a record as PENDING, and returns a short-lived presigned URL. The client uploads straight to S3, using multipart for big files so it can resume. A storage event puts a message on a queue, and workers run a virus scan, check the real file type by content, and make thumbnails. Then the status becomes READY or REJECTED and the client is told. Files are served through a CDN, with signed URLs if private. Lifecycle rules clean up abandoned uploads."

**⚠️ Avoid saying**
Don't stream the upload through your Node API "to check it". Check after upload, in a worker.

---

## 11. Monolith or microservices?

**In simple words**
A **monolith** is one app you deploy as one piece. **Microservices** are many small apps that talk over the network. Most products should start as a **modular monolith**: one app, but with clean internal parts.

**🗣️ Say this in the interview**
> "I'd start most products as a modular monolith. That's one deployable, but with clear internal boundaries, like separate modules for catalog, orders and payments that only talk through defined interfaces. It's simpler to build, test, deploy and debug, and you get real database transactions.
> I split into microservices when there's a real reason: independent teams stepping on each other, parts with very different scaling needs, a different release speed, or isolation needs, like payments with stricter compliance. Microservices have real costs: network failures, data spread across services, eventual consistency, tracing across services, and much more infrastructure.
> The worst result is a distributed monolith: services that must all deploy together and call each other for everything. All the cost, none of the benefit."

**They may also ask**
- *What is eventual consistency?* Data in different places becomes the same after a short delay, not instantly.

**⚠️ Avoid saying**
Don't say "microservices are always better" or "modern". Give a reason to split.

---

## 12. What is the saga pattern?

**In simple words**
Sometimes one business process spans several services. You can't use one database transaction across them. A **saga** breaks the process into small local steps. Each step has an **undo step** (a "compensating action").

**Picture it**
Booking a trip: flight, then hotel, then car. If the car fails, you cancel the hotel and the flight. Each booking has its own "cancel".

```
Reserve stock ──ok──▶ Charge payment ──ok──▶ Create shipment ──ok──▶ Done
      ▲                    │ fail
      └──── Release stock ◀┘   (compensation)
```

**Two styles**
- **Choreography**: each service listens for events and reacts. Loosely connected, but the full flow is hard to see and debug.
- **Orchestration**: one central "conductor" tells each service what to do next and handles failures. Easy to follow, but the orchestrator is critical.

**🗣️ Say this in the interview**
> "When one process spans several services, like reserve stock, charge payment, create shipment, I can't use one database transaction. A saga breaks it into local transactions, and each step has a compensating action that undoes it: release the stock, refund the payment. With choreography, services react to each other's events. It's decoupled, but the flow is hard to see. With orchestration, a central orchestrator drives each step and handles failures. For a business-critical flow like checkout, I prefer orchestration, often with a workflow engine like Temporal, AWS Step Functions or Durable Functions."

**⚠️ Avoid saying**
Don't say a saga "rolls back" like a database. It runs new undo actions, and the world may see the middle state for a moment.

---

## 13. Explain the CAP theorem

**In simple words**
When the network between servers breaks (a **partition**), a distributed system must choose. **Consistency**: everyone sees the latest data, or gets an error. **Availability**: everyone gets an answer, maybe old. Network breaks will happen, so the real choice is C or A **during a failure**.

**Picture it**
Two bank branches lose their phone line. Either they stop giving out money until the line is back (consistency), or they keep serving and fix the books later (availability).

**🗣️ Say this in the interview**
> "CAP says that during a network partition, a distributed system must choose between consistency, where everyone sees the latest data or gets an error, and availability, where everyone gets an answer, maybe stale. Since partitions will happen, the real choice is C or A during a failure. I make that choice per feature, not per system. Payments, inventory and account balances need consistency. It's better to fail than to oversell or double-charge. Reviews, view counts and recommendations need availability, because slightly old data is fine."

**They may also ask**
- *What is PACELC?* It adds that even with no failure, there is a trade-off between **latency** (speed) and **consistency**.

**⚠️ Avoid saying**
Don't say "pick two of three". Partitions are not optional, so you only choose during a failure.

---

## 14. Which resilience patterns do you use?

**In simple words**
These are tools to stop one broken dependency from taking down your whole system.

**Picture it**
A house has a fuse box. When one room has a problem, its fuse trips, and the rest of the house keeps its lights on.

**🗣️ Say this in the interview**
> "I use a few tools so one failing dependency can't take everything down:
> - **Timeouts** on every network call. No timeout means waiting forever.
> - **Retries** with exponential backoff and jitter (waiting longer each time, with a bit of randomness), only for temporary errors and idempotent operations, and with a limit. Unlimited retries turn a small outage into a big one.
> - **Circuit breaker**: after repeated failures, stop calling the dependency for a while and fail fast. Then let a few test requests through. Closed, open, half-open.
> - **Bulkhead**: separate pools and limits per dependency, so one slow service can't use all connections.
> - **Fallbacks**: cached data, default values, or hiding a non-critical widget.
> - **Load shedding**: under extreme load, reject some requests early with a 503, instead of everything getting slow.
> - **Idempotency**, so retries are safe."

**They may also ask**
- *Why jitter?* So thousands of clients don't all retry at the exact same moment and hit the service together.

**⚠️ Avoid saying**
Don't say "we just retry". Retries without limits and backoff make outages worse.

---

## 15. What are CQRS and event sourcing?

**In simple words**
**CQRS** means you use one model for writing data and a different model for reading it. **Event sourcing** means you save every change as an event, and build the current state by replaying the events.

**Picture it**
Event sourcing is like a bank statement. The balance is not stored alone. It comes from adding up every transaction.

**🗣️ Say this in the interview**
> "CQRS separates the write model, with commands, validation and the normalized source of truth, from read models built for queries, like a denormalized table or a search index updated from events. It's very useful when reads and writes look very different. Magento's indexers are a form of this.
> Event sourcing stores every change as an event instead of just the current state. You rebuild the state by replaying events. You get a perfect audit trail and you can see the state at any past time. But it's complex: changing event formats, snapshots, eventual consistency. I'd only use it where audit history is a core business need, like ledgers."

**⚠️ Avoid saying**
Don't suggest event sourcing for a normal CRUD app. It's too heavy unless you need full history.

---

## 16. Where do you cache in a design?

**In simple words**
Caching happens in layers. Each layer that answers a request saves all the layers behind it from doing work.

**🗣️ Say this in the interview**
> "I think of caching as layers, from closest to the user to closest to the data: browser cache, then CDN, then reverse proxy or full-page cache, like Varnish in Magento, then the application cache in Redis, then the database's own buffer cache. Each layer that answers the request saves every layer behind it. The two hard parts are always invalidation, meaning removing old data at the right time, and never caching personalized data in a shared layer."

**They may also ask**
- *What happens if you cache personal data at the CDN?* Other users can see it. It's a security incident (see `6-system-design/02-production-troubleshooting.md`).

**⚠️ Avoid saying**
Don't say "just add a cache" without saying how you invalidate it.

---

## Remember these 10 things
1. Clarify first, then estimate, then simple design, then deep dive. Say each step out loud.
2. 1 million requests/day ≈ 12 per second. Peak is 2–3×, sales 10×+.
3. Keep app servers stateless; scale by adding more.
4. Read-heavy? CDN + cache + read replicas. Sharding is the last option.
5. Slow work goes to a queue. Use idempotency keys so retries are safe.
6. No overselling: one atomic `UPDATE ... WHERE qty >= 1`. Zero rows = out of stock.
7. Rate limiter: token bucket in Redis. Fail open normally, fail closed for login/OTP.
8. Files go straight to S3 with a presigned URL. The API never touches the bytes.
9. Start with a modular monolith. Split only for a real reason. Avoid a distributed monolith.
10. Every network call needs a timeout. Add limited retries with backoff, circuit breakers and fallbacks.
