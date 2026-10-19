# GraphQL

> **Time needed:** about 1.5 hours · **Questions:** 13 + 9 quick questions
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

This is your strongest topic. You use GraphQL every day on a headless commerce platform. Use Stories S1, S2, S6 and S7 freely here (see `1-interview-basics/02-your-story.md`).

## Progress checklist
- [ ] 1. What is GraphQL?
- [ ] 2. The building blocks
- [ ] 3. GraphQL vs REST
- [ ] 4. N+1 and DataLoader
- [ ] 5. Authorization
- [ ] 6. Stopping expensive queries
- [ ] 7. Caching
- [ ] 8. Error handling
- [ ] 9. Pagination
- [ ] 10. Changing the schema safely
- [ ] 11. Subscriptions
- [ ] 12. Federation and schema stitching
- [ ] 13. Fragments and client tools
- [ ] Quick questions

---

## 1. What is GraphQL?

**In simple words**
GraphQL is a way for clients to ask an API for data. There is one endpoint and a typed schema. The client lists exactly the fields it wants and gets them in one request.

**Picture it**
REST is a set menu: each dish comes the way the kitchen makes it. GraphQL is ordering à la carte: you list exactly what you want on one plate.

**🗣️ Say this in the interview**
> GraphQL is a query language for APIs. Instead of many endpoints with fixed responses, there is **one endpoint** and a **typed schema**. The client asks for **exactly the fields it needs**, in one request.
> That fixes **over-fetching**, which means getting fields you don't need, and **under-fetching**, which means making five calls to build one screen.
> The trade-off is that the hard work moves to the **server**: resolver speed, N+1 queries, checking permissions per field, caching, and protecting against very expensive queries.

**⚠️ Avoid saying**
"GraphQL is always better than REST." Say it solves some problems and moves other problems to the server.

---

## 2. What are the main building blocks of GraphQL?

**In simple words**
The schema says what you can ask for. Queries read, mutations write, subscriptions push live updates. Resolvers are the functions that get the data.

**🗣️ Say this in the interview**
> The **schema** defines the types and what clients can ask for. **Queries** read data. **Mutations** change data. **Subscriptions** push updates over a long-lived connection.
> **Resolvers** are the functions that actually produce the value of each field.
> The **context** is an object created for each request and passed to every resolver. That's where I put the logged-in user, the DataLoaders and the services.

```graphql
type Product {
  sku: String!
  name: String!
  price: Money!
  category: Category      # has its own resolver
}

type Query {
  product(sku: String!): Product
  products(filter: ProductFilter, first: Int = 20, after: String): ProductConnection!
}

type Mutation {
  addToCart(input: AddToCartInput!): AddToCartPayload!
}
```

**⚠️ Avoid saying**
"Resolvers are just for top-level queries." Every field can have a resolver, and nested fields like `category` usually do.

---

## 3. GraphQL vs REST: when do you pick which?

**In simple words**
GraphQL is great when many clients need different shapes of the same data. REST is great when the shape is fixed and you want simple HTTP caching.

**🗣️ Say this in the interview**
> I pick **GraphQL** when **several clients need different shapes** of the same data, like web, mobile and partner apps. Or when a screen would need many REST calls. A headless storefront is the perfect case: a product page needs product, variants, prices, stock and related items in one round trip.
> I pick **REST** for simple CRUD, public APIs where **HTTP caching** matters, **file uploads and downloads**, webhooks, and service-to-service calls with a fixed shape. REST is also easier to monitor, because each endpoint has its own URL in the logs.
> It's not either/or. Many systems put GraphQL in front of REST or gRPC microservices.

| | REST | GraphQL |
|---|---|---|
| Endpoints | Many | One |
| Who decides the response shape | Server | Client |
| HTTP caching | Easy (per URL) | Harder (mostly POST) |
| Over/under-fetching | Common | Solved |
| Versioning | `/v2` | Change the schema, deprecate fields |
| Errors | HTTP status codes | Usually 200 plus an `errors` array |
| Main risk | Too many calls | N+1, expensive queries |

**⚠️ Avoid saying**
"REST is old, GraphQL replaced it." Say each fits different jobs, and they often work together.

---

## 4. What is the N+1 problem, and how does DataLoader fix it?

**In simple words**
Each field resolves on its own. So 50 products can cause 1 query for the list and 50 more for their categories. DataLoader collects those 50 calls and runs one query instead.

**Picture it**
Instead of driving to the shop 50 times for 50 items, you write one shopping list and go once.

**🗣️ Say this in the interview**
> N+1 happens because each field resolves **independently**. I query 50 products, that's 1 query. Then each product's `category` resolver runs its own query, so 50 more. That's 1 + N.
> **DataLoader** fixes it with **batching**. During one tick of the event loop, it collects every `load(id)` call. Then it calls my batch function **once** with all the IDs, so it becomes one `WHERE id IN (...)` query. It also **caches per request**, so the same ID is never loaded twice.
> One important rule: I create **new loaders for every request**. A global loader would share cached data between users. That's a memory leak and a security problem.

```javascript
import DataLoader from 'dataloader';

const createLoaders = (db) => ({
  categoryById: new DataLoader(async (ids) => {
    // One query for all the IDs collected in this tick
    const rows = await db.query('SELECT * FROM categories WHERE id = ANY($1)', [ids]);
    const byId = new Map(rows.map(r => [r.id, r]));
    // MUST return one result per id, in the same order as ids
    return ids.map(id => byId.get(id) ?? null);
  }),
});

// In the server setup: build NEW loaders for every request
const context = async ({ req }) => ({ user: req.user, loaders: createLoaders(db) });

// Resolver: just call load(), DataLoader does the batching
const resolvers = {
  Product: {
    category: (product, _args, { loaders }) => loaders.categoryById.load(product.categoryId),
  },
};
```

**They may also ask**
- *What are DataLoader's two rules?* The batch function must return an array of the **same length** as the keys, and in the **same order**.
- *Is DataLoader only for databases?* No. It works for anything that can take many IDs at once: a REST service, Redis `MGET`, and so on.
- *How do you find N+1?* Count the queries per request (logs, APM spans), or use resolver tracing.

**From my work**
Use Story S2 (the N+1 fix): custom resolvers loaded data per product, 1,811 queries for 24 products. You batched them down to 238, and the response was byte-for-byte identical. See `1-interview-basics/02-your-story.md`.

**⚠️ Avoid saying**
"I make one global DataLoader for the whole app." Say loaders are created per request.

---

## 5. How do you handle authorization in GraphQL?

**In simple words**
Check who the user is once per request. But check what they are allowed to see where the data is loaded, because one field can be reached through many paths.

**🗣️ Say this in the interview**
> I **authenticate once**, at the start of the request, and put the user in the context.
> But **authorization**, meaning what the user is allowed to do, must happen where the data is accessed: at the **service or field level**. In GraphQL, a field can be reached through many different paths. If I only check at the top-level query, someone can reach the same data through a nested relationship.
> I keep the rules in the **business or service layer**, not spread across resolvers. That way REST, GraphQL and background jobs all use the same rules.

**⚠️ Avoid saying**
"We hide that field in the UI." The schema is public. Anyone can query any field directly.

---

## 6. How do you protect a GraphQL API from very expensive queries?

**In simple words**
The client writes the query, so a bad or careless client can ask for something huge. You need limits on depth, cost, page size and time.

**🗣️ Say this in the interview**
> Because the client writes the query, someone can ask for products, then related, then related, then related, ten levels deep. My defences are:
> - **Depth limit**: reject queries nested more than, say, 8 levels.
> - **Cost limit**: give each field a cost, multiply by list sizes, and reject queries over a budget.
> - **Pagination caps**: `first` can't be more than 100.
> - **Timeouts** per request.
> - **Persisted queries**, also called allow-listing: in production the server only runs queries it already knows by their hash. This is the strongest protection for our own apps.
> - **Rate limiting by cost**, not just by number of requests.
> - Turning off **introspection** in production if the API isn't public.

**⚠️ Avoid saying**
"Rate limiting by request count is enough." One GraphQL request can be very cheap or very expensive, so limit by cost.

---

## 7. How does caching work in GraphQL?

**In simple words**
Normal HTTP caching is hard, because most queries are POST requests to one URL. So you cache in several layers instead.

**🗣️ Say this in the interview**
> HTTP caching is harder because most queries are POSTs to one URL. So I cache in layers:
> - **Client**: Apollo or urql keep a **normalized cache**. Each object is stored once by its ID and updated everywhere it appears.
> - **Per request**: DataLoader removes duplicate loads inside one request.
> - **Server**: a Redis cache for expensive resolvers or slow upstream calls.
> - **CDN**: if I send **persisted queries as GET**, responses can be cached by URL.
> - **Full response cache**: only for public data that is the same for everyone, with clear invalidation.
>
> The big rule: never put user-specific data under a shared cache key.

**⚠️ Avoid saying**
"GraphQL can't be cached." It can, just not as simply as REST.

---

## 8. How do you handle errors in GraphQL?

**In simple words**
GraphQL can return some data and some errors in the same response, usually with HTTP 200. Expected business errors are best modelled as normal types in the schema.

**🗣️ Say this in the interview**
> GraphQL can return **partial data**: `data` with some fields filled in, plus an `errors` array for the fields that failed. Usually with HTTP 200. So clients must check both.
> I split errors into two groups. **Unexpected errors**, like bugs or timeouts, go into `errors`. I log them with a request ID and show a generic message.
> **Expected business errors**, like "out of stock" or "invalid coupon", I put **in the schema** as typed results. Then the client handles them safely with types, instead of matching error message strings.

```graphql
type AddToCartPayload {
  cart: Cart
  userErrors: [UserError!]!    # e.g. { code: OUT_OF_STOCK, field: "qty", message: "..." }
}
```

**From my work**
Use Story S6 (the hidden error): non-ASCII characters in query comments crashed a library's error formatter, so every error looked like a confusing `str_repeat()` failure. Lesson: when an error makes no sense, check if something is hiding the real one. See `1-interview-basics/02-your-story.md`.

**⚠️ Avoid saying**
"If the status is 200, the request worked." Always check the `errors` array too.

---

## 9. How do you do pagination in GraphQL?

**In simple words**
The common way is cursor-based "connections". A cursor is a bookmark that says "continue from here".

**🗣️ Say this in the interview**
> The common standard is **cursor-based connections** from the Relay spec. I ask for `first: 20, after: <cursor>`. I get back `edges`, each with a `node` and a `cursor`, plus `pageInfo` with `hasNextPage` and `endCursor`.
> It's a bit long to write, but it's consistent and works well with infinite scroll.
> Some APIs, Magento included, use simple `currentPage` and `pageSize` instead. That's fine for catalog pages where users jump between page numbers.

**⚠️ Avoid saying**
"Cursor pagination is always the right choice." Page numbers are fine when users need to jump to page 5.

---

## 10. How do you change a GraphQL schema without breaking clients?

**In simple words**
You add new things freely. To remove something, you mark it deprecated first, wait until nobody uses it, then remove it.

**🗣️ Say this in the interview**
> GraphQL is designed to change **without versions**. Adding fields, types or **optional** arguments is safe, because old clients never ask for them.
> To remove something, I mark it **`@deprecated(reason: ...)`**. I watch field usage until no client uses it, and only then remove it.
> Breaking changes are things like removing a field, renaming, or making an optional input required. I run **schema diff checks in CI** to catch those.

**They may also ask**
- *In what order do you deploy?* Backend first. If a client sends an argument the server doesn't know yet, the **whole query** is rejected.

**From my work**
Use Story S7 (the optional `skus` argument): you added an optional argument to an existing field instead of making a new field. Old clients were not affected, and you deployed the backend first. See `1-interview-basics/02-your-story.md`.

**⚠️ Avoid saying**
"We just make a `/v2` GraphQL endpoint." Say you evolve the schema and deprecate fields.

---

## 11. What are subscriptions, and when would you use them?

**In simple words**
Subscriptions push data to the client in real time, usually over WebSockets. They are harder to run in production than queries.

**🗣️ Say this in the interview**
> Subscriptions push data to the client in real time, usually over WebSockets. They're good for things like order status or live chat.
> In production they're harder than queries. I need a **pub/sub layer** like Redis, so any server instance can notify any connected client. I also need auth on the connection, reconnection handling, and limits.
> For simple one-way updates, **SSE** (server-sent events) or even polling is often simpler.

**⚠️ Avoid saying**
"Use subscriptions for every live feature." Pick the simplest tool that works.

---

## 12. What are federation and schema stitching?

**In simple words**
Federation lets each team own its own part of the graph. A router joins all parts into one big graph for the clients.

**🗣️ Say this in the interview**
> **Federation**, for example Apollo Federation, lets each team own a **subgraph** for its area: products, orders, users. A **router** combines them into one graph for clients.
> A type can be extended across services. For example, the Orders subgraph can add an `orders` field to the `User` type owned by the Users team.
> It gives teams freedom, but you need schema rules, composition checks in CI, and tracing across subgraphs.
> **Schema stitching** is the older, more manual way. For one team, a single well-organized GraphQL server is usually enough.

**⚠️ Avoid saying**
"Every project should use federation." It adds overhead; it pays off with many teams.

---

## 13. What are fragments, and what client tools do you use?

**In simple words**
A fragment is a reusable piece of a query. Each component says which fields it needs, and the page query puts them together.

**🗣️ Say this in the interview**
> **Fragments** are reusable pieces of a query. Each component declares the fields it needs as a fragment, and the page query combines them. So each component owns its own data needs.
> With **GraphQL Code Generator**, I get TypeScript types for every query automatically.
> One gotcha I've hit: if you declare a fragment and don't use it, the **whole document is rejected**, not just that part.

**From my work**
Use Story S1 (the product page query): when you removed related products from the query, some fragments became unused, and the whole query started failing. See `1-interview-basics/02-your-story.md`.

**⚠️ Avoid saying**
"An unused fragment is just ignored." It makes the whole document fail validation.

---

## Quick questions
- **How do queries and mutations run differently?** Top-level query fields can run in parallel. Top-level mutation fields run **in order**, one after another.
- **What does `!` mean in a type?** Non-null. `[String!]!` means a list that is never null, holding strings that are never null.
- **What are input types?** Special types for arguments, like `input AddToCartInput`. You can't use output types as inputs.
- **Interfaces vs unions?** An interface has shared fields (like `Node { id }`). A union is "one of these types" with no shared fields. You query both with `... on Type`.
- **What is introspection?** A query that returns the schema itself. Tools use it. Turn it off if your API is private.
- **What scalars are there?** Built-in: `Int`, `Float`, `String`, `Boolean`, `ID`. Custom ones: `DateTime`, `Money`, `JSON`.
- **How do you handle file uploads?** Usually better with a **signed URL** to object storage (like S3) than through GraphQL multipart.
- **What HTTP status do GraphQL errors use?** Usually 200 with an `errors` array. Some servers return 4xx when the query can't be parsed or validated.
- **Schema-first vs code-first?** Schema-first: write the SDL, then resolvers. Code-first: build the schema from code (TypeGraphQL, Pothos, Nexus). Both are fine; just be consistent.

## Remember these 10 things
1. GraphQL: one endpoint, typed schema, client asks for exactly the fields it needs.
2. The cost moves to the server: N+1, per-field auth, caching, expensive queries.
3. Pick GraphQL for many clients with different needs; REST for simple CRUD, HTTP caching, files, webhooks.
4. N+1 = 1 query for the list + 1 query per item. DataLoader batches them into one.
5. DataLoader: same length, same order as the keys, and new loaders per request.
6. Your story: 1,811 → 238 queries for 24 products, same response.
7. Authenticate once; authorize in the service layer, because fields have many paths.
8. Protect with depth limits, cost limits, page caps, timeouts and persisted queries.
9. Expected errors go in the schema (`userErrors`); unexpected ones go in `errors`.
10. Evolve, don't version: add optional things, deprecate, then remove. Deploy backend first.
