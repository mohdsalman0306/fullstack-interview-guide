# Redis, Queues and Event-Driven Systems

> **Time needed:** about 2 hours · **Questions:** 17 + 7 quick questions
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

## Progress checklist
- [ ] 1. What is Redis and why is it fast?
- [ ] 2. Caching patterns (cache-aside)
- [ ] 3. Cache invalidation and eviction
- [ ] 4. Cache stampede
- [ ] 5. What if Redis goes down?
- [ ] 6. Distributed locks with Redis
- [ ] 7. Redis persistence and high availability
- [ ] 8. Pub/Sub vs Streams vs a real queue
- [ ] 9. Why use a queue? Queue vs topic
- [ ] 10. Delivery guarantees
- [ ] 11. Idempotent consumers
- [ ] 12. Retries, backoff and dead-letter queues
- [ ] 13. Message ordering
- [ ] 14. The outbox pattern
- [ ] 15. SNS/SQS vs Service Bus vs Kafka
- [ ] 16. Events vs commands
- [ ] 17. Background jobs in Node.js

---

## 1. What is Redis, and why is it so fast?

**In simple words**
Redis keeps all data in memory (RAM), so it answers in well under a millisecond. It runs one command at a time, so there is no locking.

**🗣️ Say this in the interview**
> "Redis is an **in-memory** data store, so reads and writes take well under a millisecond. It runs commands on **a single thread**. That means **no locking**, and every command is **atomic**: it runs fully, and nothing can cut in halfway. It's more than a key-value cache. It has real data structures, so we use it for caching, sessions, rate limits, leaderboards and queues."

| Structure | Typical use |
|---|---|
| String | Cache entries, counters (`INCR`), flags |
| Hash | An object's fields (session, cart) |
| List | Simple queues, recent items |
| Set | Unique tags, "who's online" |
| Sorted Set | Leaderboards, sliding-window rate limits, delayed jobs |
| Stream | Durable event log with consumer groups |
| HyperLogLog | Approximate unique counts (unique visitors), tiny memory |
| Geo | "Stores near me" (store locator) |

**⚠️ Avoid saying**
"Redis is just a key-value cache." Mention the data structures.

---

## 2. What caching patterns do you know?

**In simple words**
Most common is **cache-aside**: check the cache, and on a miss read the database and save the result with a **TTL** (an expiry time). On a write, update the database and delete the cache key.

**🗣️ Say this in the interview**
> "I mostly use **cache-aside**. The app checks the cache. On a miss it reads the database and stores the result with a TTL. On a write it updates the database and **deletes** the key, so the next read loads fresh data.
> Others: **read-through**, where the cache loads from the database itself. **Write-through**, where you write to cache and database together, so the cache is always fresh. And **write-behind**, where you write to the cache and save to the database later. Fast, but you can lose data."

```javascript
async function getProduct(sku) {
  const key = `product:v2:${sku}`;                  // "v2": change it to clear all keys at once
  const hit = await redis.get(key);
  if (hit) return JSON.parse(hit);

  const product = await db.getProduct(sku);         // cache miss: read the database
  const ttl = 300 + Math.floor(Math.random() * 60); // random extra so keys don't all expire together
  await redis.set(key, JSON.stringify(product), { EX: ttl });
  return product;
}

async function updateProduct(sku, data) {
  await db.updateProduct(sku, data);
  await redis.del(`product:v2:${sku}`);             // delete the key, don't overwrite it
}
```

**They may also ask**
- *Why delete instead of updating the cache?* Two writes at once can update the cache in the wrong order and leave old data. Deleting is simpler; the next read rebuilds it.
- *Why always a TTL?* A safety net. If you miss an invalidation, old data still expires.

**⚠️ Avoid saying**
"I update the cache on every write." Say you delete the key.

---

## 3. How do you handle cache invalidation and eviction?

**In simple words**
**Invalidation** is removing data that is now wrong. **Eviction** is Redis removing keys because memory is full.

**🗣️ Say this in the interview**
> "For **invalidation** I can use a **TTL** (simple, a bit stale), **delete on write**, **event-driven** clearing (a 'product updated' event clears related keys, including CDN and Next.js cache tags), or **versioned keys**: change `v2` to `v3` to drop everything at once.
> **Eviction** is set by `maxmemory-policy`. For a pure cache I use **`allkeys-lru`** (remove least recently used keys) or `allkeys-lfu` (least often used). The default, `noeviction`, makes writes fail when memory is full. That's right if Redis holds data you can't lose."

**⚠️ Avoid saying**
Mixing the two up. Invalidation = data is wrong. Eviction = memory is full.

---

## 4. What is a cache stampede, and how do you prevent it?

**In simple words**
A popular key expires, hundreds of requests miss at the same moment, and they all hit the database together. Also called "thundering herd".

**Picture it**
Shop doors open on Black Friday and everyone runs in at once. You need someone at the door.

**🗣️ Say this in the interview**
> "A stampede is when a **popular key expires** and **all the requests hit the database** at once. That can take it down. My fixes:
> - **Single-flight** (request coalescing): only one request rebuilds the key; the others wait for its result.
> - A short Redis **lock** (`SET key NX PX`) so only one server rebuilds.
> - **Stale-while-revalidate**: serve the old value while one request refreshes it in the background.
> - **TTL jitter**, so keys don't expire together.
> - **Pre-warming** hot keys before a big sale."

```javascript
const inFlight = new Map();
async function singleFlight(key, loader) {
  if (!inFlight.has(key)) {
    // the first caller starts the load; remove it from the map when done
    inFlight.set(key, loader().finally(() => inFlight.delete(key)));
  }
  return inFlight.get(key);   // everyone waits for the same promise
}
```

**⚠️ Avoid saying**
"Just make the TTL longer." That only delays the problem.

---

## 5. What happens if Redis goes down?

**🗣️ Say this in the interview**
> "It depends on what Redis does for us.
> If it's **only a cache**, the app keeps working, just slower. Redis calls need **short timeouts** and a **circuit breaker** (after many failures, stop calling Redis for a while and go straight to the database). But I also **protect the database** from the extra load, with rate limits or by turning off non-critical features.
> If Redis holds **sessions, locks, rate-limit counters or queues**, it's **critical**. Then it needs replicas with automatic failover (Sentinel, or managed ElastiCache / Azure Cache for Redis), plus persistence if the data can't be lost."

**⚠️ Avoid saying**
"It just falls back to the database." Say how you protect the database too.

---

## 6. How do distributed locks with Redis work?

**In simple words**
A lock makes sure only one server does a job at a time. You create a key only if it doesn't exist, with an expiry.

**Picture it**
One bathroom key at a petrol station. It "comes back" by itself after 30 seconds if someone forgets it.

**🗣️ Say this in the interview**
> "The basic lock is `SET lock:key <unique-token> NX PX 30000`. `NX` means only if it doesn't exist. The 30-second expiry means a crashed server can't block everyone forever. To release, I delete it **only if the token is still mine**, with a small Lua script, so I never delete someone else's lock after mine expired.
> Honestly, Redis locks are for **efficiency**, like 'don't run this cron twice'. For **correctness**, like 'never double-charge', I use **database constraints and transactions**. A lock can expire while a slow process still thinks it holds it."

**⚠️ Avoid saying**
"A Redis lock prevents double payments." The database protects correctness.

---

## 7. How does Redis handle persistence and high availability?

**🗣️ Say this in the interview**
> "Two ways to save to disk. **RDB** takes snapshots every so often: compact and fast to restart, but you can lose the last few minutes. **AOF** logs every write: safer, bigger files. Many setups use both.
> For availability, **replicas plus Sentinel** give automatic failover. **Redis Cluster** splits data across nodes (16,384 hash slots) when one machine isn't enough. In the cloud I'd use the managed service."

**⚠️ Avoid saying**
"Redis loses everything on restart." Not with RDB or AOF turned on.

---

## 8. Redis Pub/Sub vs Redis Streams vs a real message queue?

**Picture it**
Pub/Sub is live radio: if you weren't listening, you missed it. A queue is voicemail: the message waits for you.

**🗣️ Say this in the interview**
> "**Pub/Sub** is fire-and-forget. A message only reaches subscribers **connected right now**. If a service is down, it misses it forever. Fine for live notifications, or telling all servers 'clear your local cache'.
> **Redis Streams** store messages and support **consumer groups and acknowledgements**, so they're more durable.
> For important business events I use a **proper broker** (SQS, Service Bus, Kafka): durable storage, retries, dead-letter queues, monitoring."

**⚠️ Avoid saying**
"I'd send order events with Pub/Sub." A service that's down loses them.

---

## 9. Why use a queue at all? Queue vs topic?

**Picture it**
A queue is a ticket line: each customer goes to one free counter. A topic is a newsletter: every subscriber gets a copy.

**🗣️ Say this in the interview**
> "A queue **decouples** the sender and receiver **in time**. The API accepts the order and returns right away. The email, ERP update and invoice happen in the background. Benefits: faster responses, **absorbing traffic spikes** (the queue grows, workers catch up), **retries** when another system is down, and scaling workers separately.
> A **queue** gives each message to **one** consumer, to share the work. A **topic** gives each message to **every subscription** (fan-out). 'Order placed' goes to a topic, and email, ERP sync and analytics each get a copy.
> The cost: **eventual consistency** (data becomes correct a bit later), duplicates, ordering issues, more to monitor."

**⚠️ Avoid saying**
Only the benefits. Always name the costs.

---

## 10. What delivery guarantees do message systems give?

**🗣️ Say this in the interview**
> "**At-most-once**: might be lost, never duplicated. **At-least-once**: never lost, but **can arrive more than once**. That's the default for SQS, Service Bus and Kafka. **Exactly-once** across systems is mostly marketing. In practice you get **'effectively once'** with **at-least-once delivery plus idempotent consumers**. Idempotent means running it twice gives the same result as once."

**⚠️ Avoid saying**
"We have exactly-once, so no duplicates." Say at-least-once plus idempotency.

---

## 11. How do you make a consumer idempotent?

**Picture it**
A cashier stamps a coupon "USED". The same coupon handed in again gets refused.

**🗣️ Say this in the interview**
> "Messages can arrive twice: after a retry, a timeout, or a crash before acknowledging. So the consumer must be safe to run twice. I save the **message ID**, or a business ID like order ID plus event type, in a `processed_messages` table, **in the same transaction** as the business change. If it's already there, I skip. And I **acknowledge only after the commit**.
> If the action is **naturally** idempotent, like 'set status to SHIPPED' or an upsert, I don't need the table."

```javascript
async function handle(msg) {
  await db.transaction(async (tx) => {
    const res = await tx.query(
      `INSERT INTO processed_messages (message_id) VALUES ($1)
       ON CONFLICT DO NOTHING`, [msg.id]);
    if (res.rowCount === 0) return;            // already processed: skip
    await applyBusinessChange(tx, msg.body);   // same transaction
  });
  // acknowledge (complete) the message only after the commit
}
```

**⚠️ Avoid saying**
"I check if the ID exists, then insert later." Two copies can pass the check together. Use the unique insert in one transaction.

---

## 12. How do you handle retries, backoff and dead-letter queues?

**Picture it**
A courier tries three times, then the parcel goes to the "failed deliveries" shelf and someone calls the customer.

**🗣️ Say this in the interview**
> "I retry **temporary** failures (timeouts, 503s, deadlocks) with **exponential backoff and jitter**: wait 1 s, 2 s, 4 s, plus randomness so consumers don't all retry at once. I **don't** retry **permanent** failures like invalid data.
> After the max attempts, the message goes to a **dead-letter queue (DLQ)** with the error details. I **alert** on the DLQ count and age, fix the cause, then **replay** with a controlled tool. That's safe because consumers are idempotent.
> A **poison message** always crashes the consumer. Without a DLQ it blocks or loops forever."

**They may also ask**
- *Visibility timeout / peek-lock?* A received message is **hidden**, not deleted. If the consumer doesn't finish in time (it crashed), it comes back for another consumer. Set it longer than the processing time, or renew the lock for long jobs.
- *When does Service Bus dead-letter?* After `MaxDeliveryCount`, 10 by default. SQS uses a redrive policy with `maxReceiveCount`.

**⚠️ Avoid saying**
"I retry until it works." Permanent failures never will.

---

## 13. How do you keep messages in order?

**🗣️ Say this in the interview**
> "**Global** ordering kills scale, because only one consumer can work at a time. Usually I only need order **per entity**: events for the **same order** in sequence, different orders in parallel. So I group by a key: **SQS FIFO message group ID**, **Service Bus sessions**, or the **Kafka partition key**.
> Even then, consumers handle late and duplicate events, for example by checking a version or timestamp and ignoring older updates."

**⚠️ Avoid saying**
"The queue keeps order, so I don't worry." Still check versions.

---

## 14. What is the outbox pattern?

**Picture it**
You drop the letter in the office outbox tray at the same moment you file the paperwork. The mail person posts it later, even if you go home.

**🗣️ Say this in the interview**
> "The problem is the **dual write**: I save the order **and** publish 'OrderPlaced'. If the app crashes in between, they disagree. An order with no event, or an event for a rolled-back order.
> The **outbox pattern**: in the **same database transaction**, insert the order **and** a row in an `outbox` table. A separate relay reads the outbox, publishes to the broker, and marks rows as sent, retrying on failure. It's at-least-once, so consumers are idempotent. The alternative is **CDC** (change data capture, like Debezium) reading the database log."

```sql
BEGIN;
  INSERT INTO orders (...) VALUES (...);
  INSERT INTO outbox (event_type, payload) VALUES ('OrderPlaced', '{...}');
COMMIT;
-- relay: read rows WHERE sent_at IS NULL, publish, then UPDATE outbox SET sent_at = now()
```

**⚠️ Avoid saying**
"I publish right after the commit." A crash in between loses the event.

---

## 15. AWS SNS/SQS vs Azure Service Bus vs Kafka?

**🗣️ Say this in the interview**
> "I start from the **need**, then pick the product. SNS plus SQS is simple and serverless on AWS. Service Bus has queues **and** topics in one service, with sessions and a built-in DLQ, good for enterprise workflows on Azure. Kafka is a **log** that keeps messages for a retention period, so consumers can **replay** history. It's for very high throughput and streaming."

| | AWS SNS + SQS | Azure Service Bus | Kafka |
|---|---|---|---|
| Model | SNS topic fans out to SQS queues | Queues **and** topics in one service | Distributed **log**; consumers track their own offset |
| Ordering | SQS FIFO per message group | **Sessions** per session ID | Per **partition** |
| Duplicates | FIFO 5-minute dedup window | Duplicate detection window | Idempotent producer; consumers still need care |
| Dead-letter | Redrive policy | Built-in DLQ | Do it yourself (separate topic) |
| Replay old messages | No | No | **Yes**, by retention |
| Best for | Simple decoupling on AWS | Enterprise workflows on Azure | Streaming, analytics, many readers of history |

**From my work**
Use **Story S12** (your Service Bus / SNS-SQS flow: what was published, who consumed it, how duplicates and failures were handled) — see `1-interview-basics/02-your-story.md`.

**⚠️ Avoid saying**
"Kafka is always best." It depends on the need.

---

## 16. Events vs commands? Trade-offs of event-driven systems?

**Picture it**
A command: "Ahmed, send the invoice." An event: an office announcement, "A new order came in." Whoever cares reacts.

**🗣️ Say this in the interview**
> "A **command** says *do this*, like `SendInvoice`. One handler, and it can refuse. An **event** says *this happened*, like `OrderPlaced`. It's a past-tense fact, and **any number** of services can react without the publisher knowing who.
> Event-driven systems are loosely coupled and resilient, but harder to **trace** and **debug**. So I use **correlation IDs** (one ID passed through the whole flow), distributed tracing and DLQ monitoring. The UI handles **eventual consistency**: 'Your order is being processed', not pretending it finished instantly."

**⚠️ Avoid saying**
"Event-driven is always better." Name the debugging cost.

---

## 17. How do you run background jobs in Node.js?

**🗣️ Say this in the interview**
> "Inside a Node system I use **BullMQ** on Redis. It gives retries with backoff, delayed and scheduled jobs, concurrency limits, priorities, rate limiting and a dashboard. Workers run as **separate processes or pods** from the API, so heavy jobs never slow user requests and scale on their own. For events between services, I still use the cloud broker."

**⚠️ Avoid saying**
"I use `setTimeout` in the API." Lost on restart, no retries.

---

## Quick questions
- **Redis is single-threaded: a bottleneck?** Rarely. It's in memory and very fast. Scale with replicas or Cluster. Avoid slow commands like `KEYS *`; use `SCAN`.
- **Redis vs Memcached?** Redis has data structures, persistence and replication. Memcached is a simpler, multi-threaded pure cache.
- **What's a hot key?** One key getting a huge share of traffic. Fix with a local in-memory cache, or split the key.
- **Backpressure in a queue system?** Consumers can't keep up. Add consumers (within downstream limits), or slow down or reject producers.
- **Competing consumers?** Many workers on one queue; each message goes to one of them. That's how you scale.
- **Saga vs 2PC?** Two-phase commit locks across services (fragile). A saga uses local transactions plus "undo" steps (compensations). See the System Design file.
- **Event sourcing?** Store every **change** as an event; current state comes from replaying them. Great audit trail, high complexity.

## Remember these 10 things
1. Redis is fast: in memory, single-threaded, every command atomic.
2. Cache-aside: on miss, read DB and save with a TTL. On write, update DB and **delete** the key.
3. Always set a TTL, with jitter.
4. Stampede fixes: single-flight, lock, stale-while-revalidate, jitter, pre-warming.
5. Redis locks = efficiency. Database constraints = correctness.
6. Pub/Sub loses messages if nobody listens. Use a real broker for business events.
7. Queue = one consumer per message. Topic = every subscriber gets a copy.
8. At-least-once delivery + idempotent consumers = "effectively once".
9. Retry temporary errors with backoff and jitter; then DLQ, alert, fix, replay.
10. Outbox: save the event in the same transaction; a relay publishes it.
