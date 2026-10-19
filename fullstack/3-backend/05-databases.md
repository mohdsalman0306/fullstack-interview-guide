# Databases and Query Optimization

> **Time needed:** about 2 hours · **Questions:** 18 + 11 quick questions
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

## Progress checklist
- [ ] 1. What is an index?
- [ ] 2. Composite and covering indexes
- [ ] 3. When does an index not help?
- [ ] 4. How do you fix a slow SQL query?
- [ ] 5. Transactions and ACID
- [ ] 6. Isolation levels
- [ ] 7. Deadlocks
- [ ] 8. Optimistic vs pessimistic locking
- [ ] 9. Normalization vs denormalization
- [ ] 10. SQL vs NoSQL
- [ ] 11. MongoDB: embed or reference?
- [ ] 12. MongoDB indexes and aggregation
- [ ] 13. The N+1 query problem
- [ ] 14. Connection pooling
- [ ] 15. Read replicas
- [ ] 16. Partitioning and sharding
- [ ] 17. Zero-downtime migrations
- [ ] 18. ORMs: pros and cons
- [ ] Quick questions

---

## 1. What is an index?

**In simple words**
A sorted copy of some columns that helps the database find rows fast. Reads get faster, writes get slower.

**Picture it**
The index at the back of a book. You jump to the right page instead of reading every page.

**🗣️ Say this in the interview**
> "An index is a separate, sorted structure, usually a **B-tree** (a sorted tree that is quick to search). It lets the database find rows **without scanning the whole table**.
> The cost: it takes **storage**, and every insert, update and delete must update every index, so **writes get slower**.
> So I index columns used in selective `WHERE` filters, `JOIN`s and `ORDER BY`, and I check with `EXPLAIN` that it's really used. I don't index everything 'just in case'."

**⚠️ Avoid saying**
"More indexes are always better." Each one slows down every write.

---

## 2. What are composite and covering indexes?

**In simple words**
A **composite index** is one index on several columns, and the order matters. A **covering index** holds every column the query needs, so the table is never opened.

**Picture it**
A phone book sorted by last name, then first name. Easy to find "Khan". Impossible to find all "Salman"s quickly.

**🗣️ Say this in the interview**
> "A **composite index** covers several columns, like `(customer_id, created_at)`. **Order matters**, like a phone book: you can search by last name, or last name plus first name, but **not first name alone**. That's the **leftmost-prefix rule**.
> Good default order: **equality** columns, then the **sort** column, then **range** columns.
> A **covering index** has **every column the query needs**, so the database answers from the index alone. That's an **index-only scan**."

```sql
-- Query: a customer's latest orders
SELECT id, created_at, total FROM orders
WHERE customer_id = 42 ORDER BY created_at DESC LIMIT 20;

-- Helps the filter AND the sort:
CREATE INDEX idx_orders_customer_created ON orders (customer_id, created_at DESC);
-- Covering version (PostgreSQL): "total" is in the index too, so no table lookup
CREATE INDEX idx_orders_cover ON orders (customer_id, created_at DESC) INCLUDE (total);
```

**⚠️ Avoid saying**
"An index on (a, b) also helps queries on b alone." It doesn't.

---

## 3. When does an index NOT help?

**In simple words**
The index exists, but the database can't use it, or a full scan is cheaper.

**🗣️ Say this in the interview**
> "Common cases:
> - A **function on the column**: `WHERE LOWER(email) = ...` can't use a plain index on `email`. Use an expression index, or store it lowercased.
> - A **leading wildcard**: `LIKE '%tap'` can't use a B-tree. Use full-text search or a search engine.
> - **Low selectivity**: an index on `is_active`, where half the rows are true, rarely helps.
> - **Type mismatch**: comparing a text column to a number can force a full scan.
> - **Wrong column order** in a composite index.
> - The query returns a **big part of the table**, so a full scan is cheaper.
> - **Old statistics**, so the planner (the part that picks how to run the query) guesses wrong."

**⚠️ Avoid saying**
"If there is an index, it is always used." The planner decides.

---

## 4. How do you optimize a slow SQL query?

**In simple words**
Measure first, find the queries that cost the most total time, fix the cheapest thing, then measure again.

**🗣️ Say this in the interview**
> "I follow the same steps:
> 1. **Find the real offenders** with the slow query log or `pg_stat_statements`, by **total** time. A 50 ms query run 10,000 times a minute matters more than one 5-second report.
> 2. **`EXPLAIN ANALYZE`** it. I look for full scans on big tables, **estimated vs actual rows** far apart, sorts spilling to disk, and nested loops over large sets.
> 3. **Fix cheapest first:** an **index**, only the needed columns, a **rewrite** (no functions on indexed columns, joins instead of correlated subqueries), **pagination**, fixing **N+1**, or a **cache**.
> 4. **Retest with realistic data**: fast on 1,000 rows can be terrible on 10 million.
> 5. **Check production metrics** after deploy."

**What to look for in EXPLAIN**

| PostgreSQL | MySQL | Meaning |
|---|---|---|
| `Seq Scan` on a big table | `type: ALL` | Full scan, probably missing an index |
| `Index Scan` / `Index Only Scan` | `type: ref/range`, `Using index` | Good |
| Estimated rows ≠ actual rows | `rows` far from reality | Old stats, run `ANALYZE` |
| `Sort` with `external merge` | `Using filesort` | Sort not covered by an index |
| `Hash Join` / `Nested Loop` | `Using temporary` | Check join columns are indexed |

**From my work**
"The biggest win wasn't a slow query, it was **too many fast queries**: 1,811 per request. Check query **count** per request, not just duration." Use Story S2 — see `1-interview-basics/02-your-story.md`.

**⚠️ Avoid saying**
"I'd just add an index." Measure with `EXPLAIN ANALYZE` first.

---

## 5. What is a transaction? What is ACID?

**In simple words**
A transaction makes several changes succeed or fail together. ACID is its four promises.

**🗣️ Say this in the interview**
> "A transaction groups operations so they **all succeed or all fail together**. Placing an order means inserting the order, reducing stock and recording the payment. You never want half of that.
> **ACID**: **Atomicity**, all or nothing. **Consistency**, constraints always hold. **Isolation**, transactions running at the same time don't see each other's half-done work. **Durability**, once committed, it survives a crash.
> My rules: keep transactions **short**, and **never make an HTTP call inside a transaction**, because it holds locks while waiting on the network."

**⚠️ Avoid saying**
"Consistency means all replicas match." In ACID it means the rules always hold.

---

## 6. What are isolation levels?

**In simple words**
They decide how much parallel transactions see of each other. Stricter is safer but slower.

**🗣️ Say this in the interview**
> "Isolation levels trade **correctness** for **concurrency**. They prevent a **dirty read** (reading uncommitted data), a **non-repeatable read** (same row read twice, different values), and a **phantom** (same query again, new rows appear).
> The bug I actually see in real code is the **lost update**: two requests read stock = 5, both subtract 1, both write 4. The default level and 'check then update' don't prevent it. The fix is an **atomic update**, a **row lock**, or **optimistic locking**."

| Level | Dirty read | Non-repeatable read | Phantom |
|---|---|---|---|
| Read Uncommitted | possible | possible | possible |
| **Read Committed** (PostgreSQL default) | prevented | possible | possible |
| **Repeatable Read** (MySQL InnoDB default) | prevented | prevented | possible in the standard (PG and InnoDB mostly prevent it) |
| Serializable | prevented | prevented | prevented |

**⚠️ Avoid saying**
"A transaction stops all race conditions." The lost update still happens (fix in question 8).

---

## 7. What is a deadlock? How do you prevent it?

**In simple words**
Two transactions each hold a lock the other needs. The database kills one of them.

**🗣️ Say this in the interview**
> "Transaction A holds lock 1 and waits for lock 2. B holds lock 2 and waits for lock 1. Neither can continue, so the database **kills one**.
> Prevention: lock rows in the **same order** (for example sorted by ID), keep transactions **short**, make updates use **indexes** so they lock fewer rows, and **retry** the killed transaction. The retry must be **idempotent**, meaning safe to run twice."

**⚠️ Avoid saying**
"Deadlocks mean something is broken." They are normal under load: reduce them and retry.

---

## 8. Optimistic vs pessimistic locking?

**In simple words**
Optimistic checks for a conflict when saving. Pessimistic locks the row first. For stock, one smart `UPDATE` is often enough.

**🗣️ Say this in the interview**
> "**Optimistic** locking assumes conflicts are rare. Each row has a `version`. I update `WHERE version = the one I read`. If 0 rows changed, someone else saved first, so I retry or show an error. No locks are held, so it's great for editing a product in an admin panel.
> **Pessimistic** locking assumes conflicts are likely. `SELECT … FOR UPDATE` in a transaction locks the row until commit. Good for short, busy critical sections.
> For **stock**, a single **atomic conditional update** is often simplest."

```sql
-- Atomic: no race, no explicit lock
UPDATE inventory SET qty = qty - 2
WHERE sku = 'TAP-001' AND qty >= 2;      -- 0 rows changed = not enough stock

-- Optimistic
UPDATE products SET price = 99, version = version + 1
WHERE id = 7 AND version = 3;            -- 0 rows changed = conflict, retry

-- Pessimistic
BEGIN;
SELECT * FROM accounts WHERE id = 1 FOR UPDATE;   -- lock the row
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
COMMIT;                                            -- lock released
```

**They may also ask**
- *What is `FOR UPDATE SKIP LOCKED`?* Each worker takes rows nobody else has locked. The standard way to build a simple job queue in PostgreSQL or MySQL 8.

**⚠️ Avoid saying**
"I read the stock, check it in code, then write it." That is the lost-update race.

---

## 9. Normalization vs denormalization?

**In simple words**
Normalize: store each fact once. Denormalize: copy data on purpose for faster reads.

**🗣️ Say this in the interview**
> "**Normalization** means storing each fact **once**. The customer's address lives in one table and orders point to it, so data can't get out of sync.
> **Denormalization** means **copying** data on purpose for faster reads, like `customer_name` on the order, or a pre-built product listing table.
> My rule: normalize the **source of truth**, denormalize **read models** for hot paths, with a clear process to keep them in sync."

**From my work**
Magento's **indexers** do exactly this: normalized EAV source tables, plus flat and index tables (and Elasticsearch/OpenSearch) for fast storefront reads. See `1-interview-basics/05-php-magento-background.md`.

**⚠️ Avoid saying**
"Denormalization is bad design." It's a normal choice when you keep the copies in sync.

---

## 10. SQL vs NoSQL: how do you choose?

**In simple words**
Pick by the shape of the data and how you read it, not by fashion.

**🗣️ Say this in the interview**
> "I choose by **data shape and access patterns**.
> **Relational** (PostgreSQL, MySQL) when data is connected and **correctness** matters: orders, payments, inventory, users. I get joins, constraints, transactions and flexible queries.
> **Document** (MongoDB) when data is **self-contained documents** read and written together, the schema varies (like product attributes per category), or I need easy horizontal scaling.
> **Key-value** (Redis, DynamoDB) for simple lookups at huge scale. **Search engines** (Elasticsearch/OpenSearch) for full-text search and filters.
> Most real systems use **several**. That's **polyglot persistence**."

**⚠️ Avoid saying**
"NoSQL scales better, so I'd use it." It depends on the data and queries.

---

## 11. MongoDB schema design: embed or reference?

**In simple words**
Put small, owned data inside the parent. Point to big or shared data by ID.

**🗣️ Say this in the interview**
> "In MongoDB I design for **how the data is read**. I **embed** when data is **owned** by the parent, **limited** in size, and **read together**: line items inside the order, addresses inside the customer.
> I **reference** when it's **shared**, **large**, **grows without limit**, or **changes on its own**: products from orders, or comments on a post with thousands of them.
> I avoid **arrays that keep growing**: a document has a **16 MB limit**, and huge arrays are slow to update."

**They may also ask**
- *Does MongoDB support transactions?* Yes, multi-document ACID transactions on replica sets and sharded clusters. But with good design most writes touch **one document**, which is atomic anyway.
- *Common patterns?* Extended reference (copy a few fields, like product name, into the order). Bucket (group time-series readings into hourly documents). Computed (store pre-calculated totals).

**⚠️ Avoid saying**
"I just embed everything." Growing arrays hit the 16 MB limit.

---

## 12. MongoDB indexes and the aggregation pipeline?

**In simple words**
ESR is the ordering rule for compound indexes. The aggregation pipeline is MongoDB's `GROUP BY`, done in steps.

**🗣️ Say this in the interview**
> "For compound indexes I use the **ESR rule**: **E**quality fields, then **S**ort, then **R**ange. I check with `explain('executionStats')`: I want `IXSCAN` (index scan), not `COLLSCAN` (full scan), and `totalDocsExamined` close to `nReturned`.
> The **aggregation pipeline** is MongoDB's `GROUP BY` and joins: stages like `$match`, `$group`, `$sort`, `$lookup`, `$project`. I put **`$match` first** so it uses an index and shrinks the data early."

```javascript
// Top 10 customers by paid amount this year
db.orders.aggregate([
  { $match: { status: 'paid', createdAt: { $gte: ISODate('2026-01-01') } } }, // filter first
  { $group: { _id: '$customerId', total: { $sum: '$amount' }, orders: { $sum: 1 } } },
  { $sort: { total: -1 } },
  { $limit: 10 },
]);
```

**⚠️ Avoid saying**
"I filter at the end." Filter first, so less data flows through the stages.

---

## 13. What is the N+1 query problem?

**In simple words**
1 query for a list, then 1 more per item. 100 items = 101 queries.

**Picture it**
Going to the shop 100 times for 100 eggs, instead of buying one box.

**🗣️ Say this in the interview**
> "N+1 is one query for a list, then **one extra query per item**: 100 orders, then 100 queries for each order's customer. Each is fast, but together they kill performance. It's common with **ORMs** that lazy-load relations, and with **GraphQL** resolvers.
> Fixes: a **JOIN**, **eager loading** (`include` / `with`), one **`WHERE id IN (...)`** batch, or **DataLoader**, which collects IDs and loads them in one batch. Then I check I didn't cause a huge row explosion by joining several one-to-many relations at once."

**From my work**
Story S2 (1,811 → 238 queries per request) — see `1-interview-basics/02-your-story.md`.

**⚠️ Avoid saying**
"Each query is only 2 ms, so it's fine." The count is the problem.

---

## 14. What is connection pooling?

**In simple words**
Keep some database connections open and reuse them. The hard part is the size.

**🗣️ Say this in the interview**
> "Opening a connection is slow (TCP, TLS, login), so a **pool** keeps open connections and reuses them. More connections is not faster: after a point, the database spends its time switching between them.
> The math people forget: **pool size × app instances** must stay below `max_connections`. Ten pods with a pool of 20 is 200 connections. If autoscaling adds pods, the database can run out. That's when I add a pooler like **PgBouncer** or RDS Proxy."

**⚠️ Avoid saying**
"I'll set the pool to 500 to be safe." It multiplies by every instance.

---

## 15. What is replication? How do read replicas work?

**In simple words**
The **primary** takes writes. **Replicas** copy it and serve reads, a little behind.

**🗣️ Say this in the interview**
> "A **primary** handles writes and **replicas** copy its data, usually **asynchronously** (with a small delay). Reads go to replicas to spread load, and replicas help with failover.
> The catch is **replication lag**: a user updates their profile, the next page reads from a replica 200 ms behind, and they see old data.
> Fix: read from the **primary right after a write** (read-your-own-writes) and for sensitive flows like checkout. Use replicas for things that can be slightly old, like listings and reports."

**⚠️ Avoid saying**
"Replicas always match the primary." Usually they lag a little.

---

## 16. Partitioning vs sharding?

**In simple words**
Partitioning cuts a table into pieces inside one database. Sharding spreads data over several servers.

**🗣️ Say this in the interview**
> "**Partitioning** splits a big table **inside one database**, often by date. 'This month' queries touch one partition, and dropping old data is instant.
> **Sharding** splits data **across servers** by a **shard key** like `customer_id`, so writes scale beyond one machine.
> The shard key is the hard part: it needs **many different values**, an **even spread**, and it should match the **most common queries**, because cross-shard queries are expensive. An always-growing key like a timestamp sends all new writes to one hot shard.
> Sharding is a last resort, after indexing, caching, read replicas and a bigger server."

**⚠️ Avoid saying**
"I'd shard by created_at." All new writes would hit one shard.

---

## 17. How do you change the schema with zero downtime?

**In simple words**
Old and new code run together during a deploy, so change the schema in small safe steps: **expand and contract**.

**🗣️ Say this in the interview**
> "In a rolling deploy, **old and new code run at the same time**, so each change must work with both. To rename a column:
> 1. **Expand**: add the new nullable column.
> 2. Deploy code that **writes to both**.
> 3. **Backfill** old rows in small batches.
> 4. Deploy code that **reads the new** column.
> 5. Stop writing the old one.
> 6. **Contract**: drop the old column in a **later** release.
>
> I also build indexes **concurrently** (`CREATE INDEX CONCURRENTLY` in Postgres, online DDL in MySQL) and use tools like gh-ost for big tables, so I don't lock a large table at peak traffic."

**⚠️ Avoid saying**
"I'd just rename the column in the migration." It breaks every old instance instantly.

---

## 18. ORMs: pros and cons?

**In simple words**
An ORM lets you use code objects instead of SQL. It saves time but hides slow queries.

**🗣️ Say this in the interview**
> "ORMs like **Prisma**, TypeORM or Drizzle give productivity, type safety, migrations and protection from SQL injection. But they **hide the SQL**, so N+1 and slow queries slip in, and complex reports are awkward.
> My approach: ORM for normal CRUD, **raw SQL or a query builder** for reports and hot paths, and **query logging on in development** so I see what really runs."

**⚠️ Avoid saying**
"With an ORM I don't need SQL." You need it to fix what the ORM generates.

---

## Quick questions
- **Primary key vs unique key?** One primary key per table, never null, identifies the row. You can have many unique constraints.
- **Clustered index?** Rows are physically stored in that order. InnoDB clusters by primary key.
- **INNER vs LEFT JOIN?** INNER: only matches on both sides. LEFT: all left rows, NULLs where no match.
- **UNION vs UNION ALL?** UNION removes duplicates (slower). UNION ALL keeps everything.
- **DELETE vs TRUNCATE vs DROP?** Remove rows (can filter, logged) / remove all rows fast / remove the whole table.
- **View vs materialized view?** A view is a saved query, run every time. A materialized view stores the result and is refreshed from time to time.
- **UUID vs auto-increment?** Auto-increment is small, ordered, fast, but guessable. Random UUIDv4 scatters the index. **UUIDv7/ULID** are time-ordered: best of both.
- **Soft delete?** A `deleted_at` column. Easy undo and audit, but every query and unique constraint must handle it.
- **WHERE vs HAVING?** `WHERE` filters rows before grouping. `HAVING` filters groups after.
- **CHAR vs VARCHAR?** Fixed length (padded) vs variable length.
- **Stored procedure?** Logic inside the database. Fast and close to the data, but harder to version, test and scale.

## Remember these 10 things
1. Index = book index: faster reads, slower writes. Check with `EXPLAIN`.
2. Composite index: leftmost-prefix rule. Equality, then sort, then range.
3. Functions on columns, leading `%`, and type mismatches block an index.
4. Slow query: total time → `EXPLAIN ANALYZE` → cheapest fix → retest with real volume → check production.
5. Count queries per request, not only duration (1,811 per request).
6. Keep transactions short. No HTTP calls inside them.
7. Lost update: fix with `UPDATE ... WHERE qty >= n`, a row lock, or a version column.
8. Deadlocks: same lock order, short transactions, safe retry.
9. Pool size × instances < `max_connections`. Read your own writes from the primary.
10. Schema changes: expand and contract. Never rename a column in one step.
