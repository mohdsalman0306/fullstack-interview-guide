# Your PHP and Magento Background

> **Time needed:** about 1 hour · **Questions:** 8
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

Interviewers at Node/React companies **will** notice the PHP and Magento years on your CV. Don't be defensive. Present it as a **strength**: years of real production e-commerce at scale. Most Node developers don't have that.

## Progress checklist
- [ ] 1. "Your background is PHP. Why should we trust your Node.js experience?"
- [ ] 2. PHP's request model vs Node's event loop
- [ ] 3. Explain Magento's architecture briefly
- [ ] 4. The EAV model: pros and cons
- [ ] 5. Plugins, observers, preferences, and their Node equivalents
- [ ] 6. What is headless commerce, and what are the trade-offs?
- [ ] 7. What did Magento teach you that applies anywhere?
- [ ] 8. Magento vs Shopify vs commercetools vs building your own

---

## 1. "Your background is PHP. Why should we trust your Node.js experience?"

**In simple words**
The basics of good backend work are the same in any language. And you use Node, TypeScript and React every day now.

**🗣️ Say this in the interview**
> "The fundamentals carry over directly: API design, databases and query optimization, caching, queues, security, and running real production systems under traffic. That's most of what makes a backend engineer good, and Magento is a demanding place to learn it.
> I've been working with **Node.js and TypeScript** for several years now, plus **React and Next.js**, and today I'm building a **headless Next.js storefront on GraphQL**. So I work across both sides every day.
> And I think the combination is an advantage. I understand commerce problems (pricing, inventory, catalog, checkout) at a depth that's hard to get otherwise, and I bring that to whatever stack I'm working in."

**⚠️ Avoid saying**
"PHP is outdated" or anything negative about it. It sounds bad, and the interviewer may like PHP.

---

## 2. PHP's request model vs Node's event loop

**In simple words**
PHP starts fresh for every request and throws everything away at the end. Node is one long-running process that serves many requests at the same time.
This is a great point to **bring up yourself** when you talk about switching stacks. It shows you understand both deeply.

**Picture it**
- **PHP** is like a hotel room: cleaned and reset after every guest.
- **Node** is like a shared kitchen: everyone uses the same space at once. If someone leaves a mess, it stays for the next person.

**🗣️ Say this in the interview**
> "They're opposite models.
> **PHP**, with PHP-FPM, is **shared-nothing per request**. Every request gets a fresh worker, runs the script from scratch, and **everything is thrown away at the end**. Blocking I/O is fine, because each worker handles one request, and you get concurrency by running many workers. Memory leaks barely matter, because memory is reset after every request.
> **Node** is **one long-lived process** handling **many requests at once** on one thread. It's very efficient for I/O, but it changes the rules:
> - **State stays between requests.** A module-level variable is shared by **all users**. If you put request data there by mistake, you leak one user's data to another.
> - **Memory leaks add up**, because the process doesn't reset.
> - **Blocking code hurts everyone**, not just one request.
> - Errors must be handled carefully, because one crash takes down many requests in flight.
>
> Coming from PHP, being aware of these differences made me more careful in Node, not less."

| | PHP (PHP-FPM) | Node.js |
|---|---|---|
| Process | Fresh worker per request | One long-lived process |
| State between requests | Thrown away | Kept (shared by all users) |
| Memory leaks | Reset each request | Add up over time |
| Blocking code | Affects one request | Affects everyone |
| Concurrency | Many workers | One thread + event loop |

**⚠️ Avoid saying**
"Node is just faster than PHP." The real difference is the model, not raw speed.

---

## 3. Explain Magento's architecture briefly

**In simple words**
Magento 2 is a modular PHP framework for e-commerce. Everything is a module, and you extend it without editing core code.

**🗣️ Say this in the interview**
> "Magento 2 is a modular PHP framework for e-commerce. The key ideas:
> - **Modules**: everything is a module, with its own config, code and database setup.
> - **Dependency injection** set up in XML (`di.xml`), with auto-generated factories and proxies.
> - **Plugins (interceptors)**: run code **before, after or around** any public method of another class, without editing it. That's how you safely change core behaviour.
> - **Observers**: react to **events** the system sends, like 'order placed'.
> - **EAV**: a flexible attribute model for products, categories and customers.
> - **Indexers**: build **flattened tables and the search index** from the EAV data, so the storefront can read fast.
> - **Caching**: full-page cache (Varnish), plus config and block caches.
> - **APIs**: REST and **GraphQL**, the base for headless storefronts.
> - **Scopes**: global → website → store → store view, for multi-country, multi-language, multi-currency setups.
> - Plus message queues, cron, and service contracts (interfaces and repositories) as the stable API between modules."

**⚠️ Avoid saying**
Every detail you know. Give the list, then let them pick one to go deeper.

---

## 4. The EAV model: pros and cons

**In simple words**
**EAV (Entity-Attribute-Value)** stores each attribute as a **row**, not a column. Very flexible, but slow and complex to read.

**🗣️ Say this in the interview**
> "**EAV** stores attributes as **rows** instead of columns: an entity table, an attribute table, and value tables per data type. The benefit is **flexibility**: merchants can add new product attributes in the admin with no schema change, and different products can have completely different attributes. That's essential when you sell taps and towel rails in one catalog.
> The cost is **read performance and query complexity**. Loading one product means joining many value tables, per attribute type, per store scope. That's why Magento relies on **indexers** to flatten the data into read-optimized tables and the search engine. It's basically CQRS: a flexible model for writing, plus flattened models for reading.
> In a new system today, I'd consider a **JSON/JSONB column** or a document store for flexible attributes, with indexes on the attributes you filter by."

**They may also ask**
- *What is CQRS?* Using a different data model for writing and for reading. Each is shaped for its own job.

**⚠️ Avoid saying**
"EAV is bad." Say what it's good for (flexibility) and what it costs (slow reads).

---

## 5. Plugins, observers, preferences, and their Node equivalents

**In simple words**
Magento's extension tools are the same design ideas you find in Node, just with different names.

| Magento | What it does | Closest idea in Node/TS |
|---|---|---|
| **Plugin** (before/after/around) | Wraps a method to change inputs, outputs or behaviour | Middleware, decorators, higher-order functions, a proxy/wrapper |
| **Observer** | Reacts to an event | `EventEmitter` listeners, domain events, pub/sub |
| **Preference** | Replaces one class with another for an interface | Swapping an implementation in a DI container (NestJS providers) |
| **di.xml arguments** | Inject config and dependencies | Constructor injection, DI containers |
| **Service contracts** | Stable interfaces between modules | Exported TypeScript interfaces / module boundaries |

**🗣️ Say this in the interview**
> "It's the same set of design ideas: **open for extension without changing the original**. Plugins are aspect-oriented programming (adding behaviour around existing code from the outside). In Node you'd do the same with middleware or decorators."

**⚠️ Avoid saying**
"Magento is totally different from Node." Show you see the shared ideas.

---

## 6. What is headless commerce, and what are the trade-offs?

**In simple words**
Headless = the shop backend and the website are separate apps. They only talk through APIs.

**Picture it**
A restaurant kitchen with many counters. The same kitchen (backend) can serve the dining room (web), the delivery app (mobile) and a kiosk.

**🗣️ Say this in the interview**
> "Headless means the **commerce backend** (catalog, pricing, cart, orders) is separated from the **frontend**, and they only talk through **APIs**, GraphQL in our case. The storefront is a separate app, like Next.js.
> **Benefits:** frontend freedom and performance (SSR/ISR, CDN, a modern React developer experience), independent deploys, and the same backend can serve web, mobile apps and kiosks.
> **Costs:** you rebuild things the monolith gave you for free, like SEO routing, previews, checkout UX and the cart. You have **two systems** to deploy and keep in sync: API changes must be backward compatible, and deploy order matters. And you need people who understand **both** sides. That's where my background helps."

**From my work**
Use Story S1 (4 MB → 280 KB product page) and Story S7 (changing an API without breaking anyone). See `1-interview-basics/02-your-story.md`.

**⚠️ Avoid saying**
Only the benefits. Headless has real costs, and seniors name them.

---

## 7. What did Magento teach you that applies anywhere?

**In simple words**
Four lessons that work in any stack: data quality, read models, caching, and scope.

**🗣️ Say this in the interview**
> "Four lessons I take into every system:
> 1. **Data quality is a production concern.** Many 'bugs' were really bad data from imports. I learned to protect the code **and** fix the source.
> 2. **Read models matter.** Indexers taught me that the shape you store data in is rarely the best shape to read it in.
> 3. **Caching is layered, and invalidation is the hard part.** Varnish, application cache, CDN.
> 4. **Scope and multi-tenancy are hard.** Multiple websites, stores, currencies and languages means every query and cache key must include the right scope. I've debugged plenty of bugs that came from that."

(**Cache invalidation** = knowing when to throw away old cached data. **Multi-tenancy** = one system serving many separate stores or customers.)

**From my work**
- Lesson 1: Story S5 (bad upstream data crashing the API).
- Lesson 4: Stories S3 (store timezones) and S8 (wrong-page links).
- All in `1-interview-basics/02-your-story.md`.

**⚠️ Avoid saying**
Magento-only details. Show lessons that apply to *their* stack.

---

## 8. Magento vs Shopify vs commercetools vs building your own: your opinion?

**In simple words**
It depends on the business. Each option fits a different need.

**🗣️ Say this in the interview**
> "It depends on the business. **Shopify** is fastest to launch and cheapest to run for standard retail, but you're limited in deep customization. **Magento / Adobe Commerce** gives huge flexibility (B2B, complex catalogs, multi-store) at the cost of hosting and development effort. **commercetools** and other API-first platforms suit companies that want headless from day one with a cloud SaaS backend. **Building your own** in Node only makes sense if commerce is your **core differentiator**. Otherwise you spend years rebuilding tax, promotions, and checkout edge cases that platforms already solved."

**⚠️ Avoid saying**
"Magento is the best" just because you know it. Say "it depends" and explain on what.

---

## Remember these 10 things
1. PHP/Magento years are a **strength**: real production e-commerce at scale. Never be defensive.
2. Never call PHP "outdated".
3. PHP = fresh worker per request, everything thrown away. Node = one long process, many requests.
4. In Node: module-level state is shared by all users, leaks add up, blocking hurts everyone.
5. Magento: modules, DI (`di.xml`), plugins (before/after/around), observers, EAV, indexers, scopes.
6. EAV = attributes as rows. Flexible writes, slow reads, so indexers flatten it (like CQRS).
7. Today I'd use JSON/JSONB columns with indexes for flexible attributes.
8. Plugin ≈ middleware/decorator. Observer ≈ EventEmitter. Preference ≈ DI swap.
9. Headless: frontend freedom and multi-channel, but two systems, backward-compatible APIs, deploy order matters.
10. Platform choice depends on the business. Build your own only if commerce is the core differentiator.
