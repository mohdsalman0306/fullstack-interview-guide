# Express and REST API Design

> **Time needed:** about 2.5 hours · **Questions:** 19 + quick questions
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

## Progress checklist
- [ ] 1. Express request lifecycle and middleware
- [ ] 2. Structuring an Express API
- [ ] 3. Central error handling
- [ ] 4. What makes an API "RESTful"?
- [ ] 5. POST vs PUT vs PATCH vs DELETE
- [ ] 6. Idempotency and idempotency keys
- [ ] 7. HTTP status codes
- [ ] 8. Pagination: offset vs cursor
- [ ] 9. Filtering, sorting and field selection
- [ ] 10. API versioning
- [ ] 11. Validation and sanitization
- [ ] 12. Rate limiting
- [ ] 13. CORS
- [ ] 14. API gateway vs load balancer vs reverse proxy
- [ ] 15. OpenAPI and contract-first
- [ ] 16. Long-running operations
- [ ] 17. Receiving webhooks safely
- [ ] 18. HTTP caching for APIs
- [ ] 19. Express vs Fastify vs NestJS
- [ ] Quick questions

---

## 1. Explain the Express request lifecycle and middleware

**In simple words**
In Express, a request passes through a line of **middleware** functions, in the order you added them. Each one can change the request, send the response, or call `next()` to pass it on. The error handler sits at the very end.

**Picture it**
An airport. Check-in, then security, then passport control, then the gate. Skip a step, or do them in the wrong order, and things go wrong.

**🗣️ Say this in the interview**
> "In Express, a request flows through a chain of middleware functions, in the order I register them. Each one gets `req`, `res` and `next`. It can change the request, end the response, or call `next()` to pass control on.
> A typical order is: security headers, body parsing, request ID and logging, authentication, validation, the route handler, a 404 handler, and the error handler at the very end. Express knows it's the error handler because it takes **four** arguments: `err, req, res, next`."

```javascript
app.use(helmet());                         // security headers
app.use(express.json({ limit: '100kb' })); // parse JSON body, with a size limit
app.use(requestId);
app.use(authenticate);
app.post('/orders', validate(CreateOrderSchema), ordersController.create);
app.use(notFound);                         // no route matched -> 404
app.use(errorHandler);                     // (err, req, res, next) - always last
```

**They may also ask**
- *What if a middleware doesn't call `next()` and doesn't respond?* The request hangs until the client times out.
- *Why does order matter?* Auth must run before handlers, body parsing before validation, and the error handler must be last.

**⚠️ Avoid saying**
"Middleware order doesn't really matter." It matters a lot. Say why: auth before handlers, error handler last.

---

## 2. How do you structure an Express (or any Node) API?

**In simple words**
Keep the HTTP code separate from the business rules. Then you can test the rules without HTTP, and reuse them from other places, like a queue consumer.

**🗣️ Say this in the interview**
> "I separate HTTP concerns from business logic.
> **Routes** map URLs to handlers.
> **Controllers** read the request, call a service, and shape the response. They stay thin.
> **Services** hold the business rules. No `req` or `res` in there.
> **Repositories** do the database access.
> **Middleware** handles shared things like auth, logging and rate limiting.
> **Schemas** do input validation.
> The benefit: services are testable without HTTP. I could swap Express for Fastify, or call the same service from a queue consumer, without rewriting the logic. Once the app grows, I organise folders **by feature**, like `orders/` and `users/`, not by type."

**⚠️ Avoid saying**
"I put the database queries in the route handler." Keep controllers thin and logic in services.

---

## 3. How do you do centralized error handling?

**In simple words**
Your code throws clear error types, like `NotFoundError`. One error middleware catches them all and turns them into a clean response. Users never see stack traces.

**🗣️ Say this in the interview**
> "Controllers throw typed errors, like `NotFoundError` or `ValidationError`, and **one error middleware** turns them into responses. It logs the full error internally with the request ID, returns the same error shape to the client every time, and hides stack traces and internal messages for 500s.
> One Express detail: in **Express 4**, a rejected Promise in an async handler is **not** caught automatically. You need a wrapper or the `express-async-errors` package. **Express 5** fixed that: rejected Promises go to the error handler automatically."

```javascript
class AppError extends Error {
  constructor(status, code, message) {
    super(message);
    this.status = status; // HTTP status, e.g. 404
    this.code = code;     // stable code for clients, e.g. 'ORDER_NOT_FOUND'
  }
}

// The error handler: 4 arguments, registered last
app.use((err, req, res, next) => {
  const status = err instanceof AppError ? err.status : 500;
  req.log.error({ err, requestId: req.id }); // full details stay in our logs
  res.status(status).json({
    error: {
      code: err.code ?? 'INTERNAL_ERROR',
      // show our message for 4xx, hide internals for 5xx
      message: status < 500 ? err.message : 'Something went wrong',
    },
    requestId: req.id,
  });
});
```

**⚠️ Avoid saying**
"Express catches async errors for me." Only in Express 5. In Express 4 you need a wrapper.

---

## 4. What makes an API "RESTful"?

**In simple words**
REST treats everything as a **resource** (a thing) with its own URL, like `/orders/123`. You use HTTP methods for the action. Each request carries everything the server needs, like the auth token.

**🗣️ Say this in the interview**
> "REST models everything as **resources** with stable URLs, like `/orders/123`, and uses **HTTP methods** for the actions: GET to read, POST to create, PUT or PATCH to update, DELETE to remove. Each request is **stateless**, meaning it carries everything needed, like the auth token. And it follows the HTTP rules: correct status codes, caching headers, idempotency.
> In practice, a good REST API uses **nouns in URLs, not verbs**: `POST /orders`, not `POST /createOrder`. For actions that don't fit create/read/update/delete, I use a sub-resource, like `POST /orders/123/cancel`."

**⚠️ Avoid saying**
"REST means JSON over HTTP." It's about resources, methods, statelessness and using HTTP properly.

---

## 5. POST vs PUT vs PATCH vs DELETE

**In simple words**
The big difference is **idempotency**: does sending the same request twice give the same result as sending it once? PUT and DELETE: yes. POST: no. PATCH: it depends.

**🗣️ Say this in the interview**
> "**POST** creates something or triggers an action, and it's **not idempotent**: send it twice, get two orders. **PUT** **replaces** the whole resource at a known URL, and it **is idempotent**: the same PUT twice leaves the same result. **PATCH** updates **part** of a resource. It *can* be idempotent, like 'set status to shipped', or not, like 'add 1 to qty'. **DELETE** removes, and it's idempotent: the second delete just returns 404 or 204."

| Method | Does what | Idempotent? |
|---|---|---|
| POST | Create / action | No |
| PUT | Replace whole thing | Yes |
| PATCH | Change part | Depends |
| DELETE | Remove | Yes |

**They may also ask**
- *Can POST be made idempotent?* Yes, with an **idempotency key** (see question 6).

**⚠️ Avoid saying**
"PUT and PATCH are the same." PUT replaces the whole resource, PATCH changes part of it.

---

## 6. What is idempotency, and how do idempotency keys work?

**In simple words**
**Idempotent** means doing it twice has the same effect as doing it once. Networks fail, so clients retry. Without protection, a retried payment charges the customer twice. An **idempotency key** is a unique ID the client sends, so the server can spot a repeat.

**Picture it**
A coat check ticket. If you show the same ticket twice, you get the same coat back. You don't get a second coat.

**🗣️ Say this in the interview**
> "Idempotent means doing it twice has the same effect as doing it once. That matters because networks fail. The client sends a payment, the response times out, the client doesn't know if it worked, so it retries. Without protection, the customer pays twice.
> The fix is an **idempotency key**. The client creates a unique ID for each operation and sends it in a header. The server stores the key with the result. If the same key comes again, the server returns the stored result instead of processing it again. And the safety comes from a **unique constraint** in the database, not from a 'check, then insert' in code."

**How it works on the server**
1. Client sends `Idempotency-Key: 7f3a…` with `POST /payments`.
2. Server **inserts the key** into a table that has a **unique constraint**, with status `processing` and a hash of the request body.
3. Insert worked → process the payment → save the response with the key → return it.
4. Insert failed because the key already exists:
   - status `completed` → return the saved response
   - status `processing` → return `409` (or wait a little)
   - same key but a **different body** → return `422`. That's a client bug.
5. Keys expire after some time, for example 24 hours.

```javascript
import { createHash } from 'node:crypto';

app.post('/payments', async (req, res) => {
  const key = req.get('Idempotency-Key');
  if (!key) return res.status(400).json({ error: { code: 'IDEMPOTENCY_KEY_REQUIRED' } });
  const bodyHash = createHash('sha256').update(JSON.stringify(req.body)).digest('hex');

  // Try to claim the key. The UNIQUE constraint makes this safe even if two retries arrive together.
  const claimed = await db.query(
    `INSERT INTO idempotency_keys (key, request_hash, status)
     VALUES ($1, $2, 'processing')
     ON CONFLICT (key) DO NOTHING`,
    [key, bodyHash]
  );

  if (claimed.rowCount === 0) {
    // We have seen this key before
    const { rows: [saved] } = await db.query(
      'SELECT request_hash, status, response_code, response_body FROM idempotency_keys WHERE key = $1',
      [key]
    );
    if (saved.request_hash !== bodyHash) return res.status(422).json({ error: { code: 'KEY_REUSED_WITH_DIFFERENT_BODY' } });
    if (saved.status === 'processing') return res.status(409).json({ error: { code: 'REQUEST_IN_PROGRESS' } });
    return res.status(saved.response_code).json(saved.response_body); // replay the saved result
  }

  // First time: do the real work, then save the result against the key
  // (in real code, also handle failure here, e.g. delete the key so the client can retry)
  const payment = await payments.charge(req.body);
  await db.query(
    `UPDATE idempotency_keys
     SET status = 'completed', response_code = 201, response_body = $2
     WHERE key = $1`,
    [key, payment]
  );
  res.status(201).json(payment);
});
```

**From my work**
Order creation and payment callbacks in e-commerce. Payment providers retry webhooks, so the handler must be idempotent on the provider's transaction ID.

**⚠️ Avoid saying**
"I check if the order exists, then insert." Two retries arriving at the same moment both pass the check. Say the **unique constraint** is what makes it safe.

---

## 7. Which HTTP status codes must you know?

**In simple words**
Status codes tell the client what happened. 2xx = it worked. 3xx = go somewhere else. 4xx = the client did something wrong. 5xx = the server failed.

| Code | Meaning | When I use it |
|---|---|---|
| 200 | OK | Successful GET/PUT/PATCH with a body |
| 201 | Created | POST that created something (add a `Location` header) |
| 202 | Accepted | Work is queued, not finished yet (async jobs) |
| 204 | No Content | Success, no body (DELETE) |
| 301 / 308 | Permanent redirect | URL moved forever (SEO). 308 keeps the method |
| 302 / 307 | Temporary redirect | 307 keeps the method |
| 304 | Not Modified | Client's cached copy is still fine (ETag match) |
| 400 | Bad Request | Broken JSON, wrong types |
| 401 | Unauthorized | **Not logged in** / bad token |
| 403 | Forbidden | Logged in, but **not allowed** |
| 404 | Not Found | Doesn't exist (or you hide that it exists) |
| 409 | Conflict | State conflict: duplicate, version mismatch |
| 422 | Unprocessable | Valid format, but fails business rules |
| 429 | Too Many Requests | Rate limited (add `Retry-After`) |
| 500 | Internal Error | Our bug |
| 502 | Bad Gateway | Proxy got a bad answer from the server behind it |
| 503 | Unavailable | Overloaded or in maintenance (add `Retry-After`) |
| 504 | Gateway Timeout | Server behind the proxy took too long |

**🗣️ Say this in the interview**
> "For 401 versus 403, in one line: 401 means *who are you?*, and 403 means *I know who you are, and the answer is no.* And 400 versus 422: 400 is broken input, like bad JSON. 422 is well-formed input that fails a business rule."

**⚠️ Avoid saying**
"I return 200 with `success: false` for errors." Use the right status code, so clients, proxies and monitoring understand it.

---

## 8. Pagination: offset vs cursor

**In simple words**
**Offset pagination** says "skip 80 rows, give me 20". Simple, but slow on deep pages, and it can show duplicates when data changes. **Cursor pagination** says "give me 20 after this last item". It's fast at any depth and stable, but you can't jump to page 50.

**Picture it**
Offset is counting pages from the start of a book every time. Cursor is using a bookmark.

**🗣️ Say this in the interview**
> "**Offset pagination**, like `?page=5&limit=20`, is simple and lets users jump to page 50. But the database still reads and throws away all the skipped rows, so deep pages get slow. And if rows are added while someone is paging, they see duplicates or miss items.
> **Cursor pagination**, like `?after=<cursor>`, remembers the last item seen and asks for 'the next 20 after this one' using an index. It's fast at any depth and stable while data changes. The trade-off is you can't jump to any page. I use offset for small admin tables, and cursor for feeds, infinite scroll, large lists and public APIs."

```sql
-- The cursor is the last row's (created_at, id). id breaks ties when times are equal.
SELECT id, created_at, total
FROM orders
WHERE (created_at, id) < ($1, $2)
ORDER BY created_at DESC, id DESC
LIMIT 20;
-- Needs an index on (created_at, id)
```

**⚠️ Avoid saying**
"Offset is fine for everything." It gets slow on deep pages and is unstable when data changes.

---

## 9. What are your conventions for filtering, sorting and field selection?

**In simple words**
Keep the query parameters simple and the same everywhere. Only allow filters and sorts you have chosen, and put a maximum on `limit`, so nobody can overload the database.

**🗣️ Say this in the interview**
> "I keep it boring and consistent: `GET /products?category=taps&minPrice=100&sort=-price&limit=20&fields=id,name,price`. A minus sign means descending. I **allow-list** which fields can be filtered and sorted, because sorting on a column with no index is an easy way for anyone to load the database. And I **cap `limit`**."

**⚠️ Avoid saying**
"Clients can sort by any field." Allow-list the fields and cap the limit.

---

## 10. How do you version an API?

**In simple words**
Try not to need a new version. Most changes can be **additive** (only adding things), and old clients just ignore new fields. Make a new version only for a real **breaking change**.

**🗣️ Say this in the interview**
> "First, I try **not** to need a new version. Most changes can be additive: new optional fields, new endpoints. Clients ignore what they don't know. I only version for a real breaking change, like removing a field or changing what it means.
> When I do version, I prefer the **URL**, like `/v2/orders`, because it's obvious, easy to route, and easy to see in logs. Header versioning is cleaner in theory but harder to debug. Either way: announce a **deprecation window**, track which clients still call v1, and remove it only when usage reaches zero."

**From my work**
Use Story S7 (changing an API without breaking anyone): I added an optional argument instead of a new version, so old clients kept working unchanged. See `1-interview-basics/02-your-story.md`.

**⚠️ Avoid saying**
"Every change gets a new version." Additive changes don't need one.

---

## 11. Validation and sanitization

**In simple words**
**Validation** checks the shape of the input: right types, required fields, sensible values. It does NOT stop injection on its own. To stop SQL injection you use parameterized queries. To stop HTML injection you encode the output.

**🗣️ Say this in the interview**
> "I validate every input at the edge of the system with a schema, using Zod or Joi: types, required fields, ranges, formats, and I **reject unknown fields**. Validation checks the shape. It does *not* protect against injection by itself. That's the job of **parameterized queries** for SQL and **output encoding** for HTML. How you sanitize depends on where the data ends up."

**⚠️ Avoid saying**
"We validate input, so we're safe from injection." You still need parameterized queries and output encoding.

---

## 12. How do you do rate limiting?

**In simple words**
**Rate limiting** caps how many requests a client can make in some time. It stops abuse, and stops one client using all the capacity. Keep the counters in a shared place like Redis, so all servers count together.

**🗣️ Say this in the interview**
> "Rate limiting protects us from abuse, and from one client eating all the capacity. Key points:
> I store the counters in **Redis**, or at the API gateway, not in memory. With several instances, each one would count separately.
> I choose the **key** carefully. Per user or API key is better than per IP, because many users share one IP behind a company network.
> **Different limits** for different endpoints: login is strict, against password guessing. Search is moderate. Static reads are generous.
> I return **429** with a `Retry-After` header.
> For the algorithm: **fixed window** is simplest, but allows bursts at the edge of the window. **Sliding window** is smoother. **Token bucket** allows short bursts but caps the average rate. That's what most gateways use."

**They may also ask**
- *How would you design it in detail?* See the System Design file (`6-system-design/01-system-design.md`), rate limiter section.

**⚠️ Avoid saying**
"I count requests in a JavaScript object in memory." It breaks as soon as you run more than one instance.

---

## 13. What is CORS?

**In simple words**
**CORS** is a browser rule. By default, JavaScript on one site can't read responses from another site. The server can allow it with special headers. CORS only limits browsers. It does not protect your API from curl or other servers.

**🗣️ Say this in the interview**
> "CORS is a **browser** rule. By default, JavaScript on `shop.com` can't read responses from `api.other.com`. The server allows it with headers like `Access-Control-Allow-Origin`.
> For anything beyond a simple request, like a JSON content type, custom headers, or PUT and DELETE, the browser first sends a **preflight** `OPTIONS` request to ask permission.
> Important points: allow only **specific origins**. You **can't** use `*` together with credentials. And CORS is **not security for your API**. It only limits browsers. curl or a server can call your API anyway. Auth is still required."

**⚠️ Avoid saying**
"CORS protects our API." It only limits browsers. Auth protects your API.

---

## 14. API gateway vs load balancer vs reverse proxy

**In simple words**
All three sit in front of your servers. A **reverse proxy** forwards requests. A **load balancer** spreads traffic across many servers. An **API gateway** is a smarter front door with auth, rate limits and routing to many services.

**🗣️ Say this in the interview**
> "A **reverse proxy**, like Nginx, sits in front of servers and forwards requests. It often handles TLS, compression and caching. A **load balancer** spreads traffic across many instances and removes unhealthy ones. An **API gateway** is a smarter front door for APIs: routing to many services, authentication, rate limits, quotas, API keys, changing requests, and analytics.
> The rule I follow: **business logic never goes in the gateway**. It should stay thin and focused on policies, or it becomes a big thing nobody can test."

**They may also ask**
- *Examples?* Gateways: AWS API Gateway, Azure API Management, Kong, Red Hat 3scale. Load balancers: AWS ALB/NLB, Azure Application Gateway or Front Door.

**⚠️ Avoid saying**
"We put the pricing rules in the gateway." Keep business logic in services.

---

## 15. What is OpenAPI and contract-first development?

**In simple words**
**OpenAPI** is a file that describes your REST API in a way machines can read: endpoints, inputs, outputs, errors. **Contract-first** means you agree on this file before writing code, so frontend and backend can work at the same time.

**🗣️ Say this in the interview**
> "OpenAPI is a machine-readable description of a REST API: endpoints, request and response schemas, auth, and errors. **Contract-first** means we agree the spec **before** writing the code, so frontend and backend can work in parallel against mocks. From the spec we get docs, generated clients and types, and validation. I keep the spec in the repo, and check in CI that the code still matches it."

**⚠️ Avoid saying**
"We write the docs after the API is done." Say the spec comes first and CI keeps it in sync.

---

## 16. How do you handle long-running operations?

**In simple words**
Don't keep an HTTP request open for minutes. Say "got it" right away with `202 Accepted` and a job ID. Do the work in the background. The client checks back, or gets told when it's done.

**Picture it**
A dry cleaner. You get a ticket now, and pick up the clothes later.

**🗣️ Say this in the interview**
> "If something takes more than a few seconds, like a report, a bulk import or a video, I don't keep the HTTP request open. I return **`202 Accepted`** with a job ID and a status URL, and process it in a **background worker**. The client either **polls** `GET /jobs/123`, gets a **webhook**, or gets a push through **SSE or WebSocket**. When the job finishes, the status includes a link to the result, often a signed URL to download the file."

**⚠️ Avoid saying**
"I'd just raise the request timeout." Use 202 plus a background job.

---

## 17. How do you receive webhooks safely?

**In simple words**
A **webhook** is another system calling your URL when something happens, like "payment succeeded". Check it's really them, answer fast, and expect duplicates and wrong order.

**🗣️ Say this in the interview**
> "When a payment provider or partner calls our webhook:
> one, **verify the signature**. Usually it's an HMAC, a kind of signed hash, of the raw body plus a timestamp. That proves it's really them and blocks replays.
> Two, **respond 2xx quickly**, and push the real work onto a queue.
> Three, **make processing idempotent** using the event ID, because providers **will** retry and send duplicates.
> Four, **don't assume order**. A 'refunded' event can arrive before 'paid'. Check the current state, or fetch the latest from their API."

**From my work**
Payment callbacks in e-commerce: providers retry webhooks, so the handler is idempotent on the provider's transaction ID (same story as question 6).

**⚠️ Avoid saying**
"I process the webhook fully before replying." Reply fast and queue the work, or the provider times out and retries.

---

## 18. How does HTTP caching work for APIs?

**In simple words**
Two tools. **`Cache-Control`** says how long a browser or CDN can reuse a response. **ETags** are version tags: the client asks "has it changed?" and the server can answer "no" without sending the data again.

**🗣️ Say this in the interview**
> "Two tools. **`Cache-Control`** tells browsers and CDNs how long they can reuse a response: `public, max-age=60` for shared data, `private` for user data, `no-store` for sensitive data. `s-maxage` is for the CDN only, and `stale-while-revalidate` serves the old copy while refreshing in the background.
> **ETags** handle checking for changes. The server sends a version tag, the client sends it back in `If-None-Match`, and if nothing changed, the server replies `304 Not Modified` with no body. That saves bandwidth."

**⚠️ Avoid saying**
"I set `public` caching on the cart endpoint." Then a CDN can serve one user's cart to another. Use `private` or `no-store` for personal data. (More in the Production Troubleshooting file, `6-system-design/02-production-troubleshooting.md`.)

---

## 19. Express vs Fastify vs NestJS

**In simple words**
Express is small and flexible. Fastify is faster and has validation built in. NestJS gives you a full structure, good for big teams. Pick based on the team and how much structure you want.

**🗣️ Say this in the interview**
> "**Express** is minimal and has the biggest ecosystem, but it leaves the structure up to me. **Fastify** is faster, with built-in schema validation and serialization, and a good plugin system. I'd choose it for performance-sensitive APIs. **NestJS** is a framework with strong opinions, built on top of Express or Fastify, with modules, dependency injection and decorators. It's great for big teams that want consistency, but it's heavier. My choice depends on team size and how much structure the team wants enforced."

**⚠️ Avoid saying**
"X is always the best." Say it depends on team size and needs.

---

## Quick questions
- **Safe methods?** GET, HEAD, OPTIONS. They don't change state.
- **Idempotent methods?** GET, PUT, DELETE, HEAD, OPTIONS. POST is not. PATCH depends.
- **Nested resources: how deep?** One level (`/orders/123/items`). Deeper gets messy, so use filters instead.
- **What is HATEOAS?** Responses include links to related actions. It's "true" REST, but rarely done fully in practice.
- **Can PUT create?** Yes, if the client chooses the ID (`PUT /users/abc`).
- **Bulk operations?** `POST /orders/bulk` with a result for each item, or 207 Multi-Status. Make them idempotent.
- **How do you document errors?** One consistent shape (`code`, `message`, `details`, `requestId`), for example RFC 9457 Problem Details.
- **Timeouts on outbound calls?** Always. Without a timeout, one slow dependency ties up your whole server.

## Remember these 10 things
1. Middleware runs in order. Error handler has 4 arguments and goes last.
2. Thin controllers, business logic in services, DB in repositories. Folders by feature.
3. Express 4 doesn't catch async errors. Express 5 does.
4. Nouns in URLs (`POST /orders`), actions as sub-resources (`POST /orders/123/cancel`).
5. PUT and DELETE are idempotent. POST is not. PATCH depends.
6. Idempotency key + **unique constraint** = no double payments. Not "check, then insert".
7. 401 = who are you? 403 = I know you, and no.
8. Cursor pagination for big lists: fast and stable. Offset only for small tables.
9. Try additive changes before a new version. If needed, `/v2/` in the URL.
10. CORS is a browser rule, not API security. Webhooks: verify signature, reply fast, idempotent, no order assumed.
