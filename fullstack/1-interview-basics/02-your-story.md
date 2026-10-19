# Your Story and Story Bank

> **Time needed:** about 3 hours (plus time to fill in S9–S13) · **Items:** 15
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

> 📝 **Before the interview: you MUST fill in stories S9–S13 (section 10 to 14).**
> They are empty templates. Interviewers *will* ask about leadership, conflict, deadlines, queues and incidents. Write your real stories there.

## Progress checklist
- [ ] 1. "Tell me about yourself" (full and short version)
- [ ] 2. S1: The 4 MB product page → 280 KB
- [ ] 3. S2: 1,811 database queries → 238 (the N+1 fix)
- [ ] 4. S3: Stores showing "open" on Dubai's clock
- [ ] 5. S4: Fixing the cause, not the symptom (missing product URLs)
- [ ] 6. S5: Bad upstream data crashing the API
- [ ] 7. S6: One character that broke every GraphQL query
- [ ] 8. S7: Changing an API without breaking anyone
- [ ] 9. S8: Silent wrong-page links (data drift)
- [ ] 10. S9: Leadership / mentoring ✍️ fill in
- [ ] 11. S10: Conflict / disagreement ✍️ fill in
- [ ] 12. S11: Tight deadline / trade-off ✍️ fill in
- [ ] 13. S12: Messaging / async system ✍️ fill in
- [ ] 14. S13: Production incident ✍️ fill in
- [ ] 15. Which story for which question?

---

## 1. "Tell me about yourself"

**In simple words**
Use this order: **Present → Past → Proof → Future**. About 60–90 seconds.
Don't read your CV out loud. Think of it as a trailer that makes them want to ask more.

**🗣️ Say this in the interview (full version, about 75 seconds)**
> "I'm a Senior Full Stack Engineer with about ten years of experience, currently based in Dubai.
>
> I started my career in PHP and enterprise e-commerce, mostly Magento. That gave me a really strong foundation in backend design, databases, and how commerce systems behave under real traffic. Over the last several years my focus has moved to **Node.js and TypeScript** on the backend and **React and Next.js** on the frontend.
>
> Right now I work on a multi-brand, multi-country e-commerce platform. A lot of my work is on the headless side: GraphQL APIs on the backend, a Next.js storefront on the front. I also work on integrations, asynchronous processing with Azure Service Bus and AWS SNS/SQS, and performance. For example, recently I cut our product page payload from about **4 MB to under 300 KB**, and took a product listing from over **1,800 database queries down to about 240** by batching resolvers.
>
> What I enjoy most is owning a feature end to end, from the data model and the API to the UI and how it behaves in production. I'm looking for a role where I can do that at a bigger scale and keep growing toward architecture and technical leadership."

**🗣️ Short version (about 30 seconds)**, when they say "briefly":
> "I'm a full stack engineer with about ten years in e-commerce. I started in PHP and Magento and now work mainly with Node.js, TypeScript, GraphQL, React and Next.js. Day to day I build headless commerce APIs and storefronts and do a lot of performance work. Recently I took a product page from 4 MB down to under 300 KB. I'm looking for a bigger-scale role where I can own systems end to end."

**Tips**
- End on **why this role**. It gives them an easy next question.
- Use **one or two numbers** at most. They are hooks. Be ready to tell the full story behind them.
- Don't list every technology. Name the ones in *their* job description.
- Change the last sentence for each company: "…which is why [their product/scale/domain] interests me."

**⚠️ Avoid saying**
Your whole CV, job by job. Keep it short and end with why you want *this* role.

---

## The Story Bank (S1–S13)

Prepare **8 stories** (plus the 5 templates). Each story can answer 5 or more different questions.
When they ask "tell me about a time when…", pick the closest story.

> ⚠️ **Before you use these stories:**
> - The details come from your own recent work, with measured numbers. Re-read each one and check the numbers.
> - **Only say what you can explain in depth.**
> - Several numbers were measured on a local or test environment. Say "in our tests", not "in production", unless you confirmed production numbers.
> - Remove internal names (SKUs, module names) if you're not sure they are OK to share.

---

## 2. S1: The 4 MB product page → 280 KB

**Use it for:** biggest challenge · performance win · React/Next.js deep dive · trade-offs · "something you're proud of"

**Situation**
- Our headless storefront is a Next.js App Router frontend on Magento GraphQL.
- Some products have many finishes. One had 55 variants.
- For those products, the product page HTML was **3.96 MB**. It loaded extremely slowly in testing.
- **The goal:** make it fast, **without** breaking instant swatch switching. Clicking a colour had no network call, and the business liked that.

**What I did**
- I **measured** what was inside the page instead of guessing.
- **3.83 MB** of it was the **React Server Components payload**. That is the data React puts inside the HTML so Client Components can start in the browser.
- Why so big? The server page passed the full product, with every variant, as props into a Client Component. So all of it was turned into text (serialized) inside the page.
- **94%** of that payload was `related_products`. It was repeated for **every** variant (about 50 rows × 55 variants).
- I **kept the variants**, because they power instant swatch clicks.
- I **removed related products** from the first query. Now they load **after first paint** (after the page first shows), only for the selected variant, and are cached per SKU.
- On the backend, I added an optional `skus` argument to the GraphQL field. The frontend can ask for only the children it needs. It is backward compatible (old clients still work).
- I **deployed the backend before the frontend**. A GraphQL query with an unknown argument fails completely.

**Result**
- **3.96 MB → 0.28 MB, about 14× smaller.**
- Swatch clicks were still instant.

**Lesson**
"In the App Router, anything you pass from a Server Component to a Client Component gets serialized into the page. You have to look at the payload, not only the JS bundle."

**🗣️ Say this in the interview**
> "Our product pages were coming out at almost 4 MB of HTML for products with lots of finishes. Instead of guessing, I measured what was inside the document, and almost all of it was the React Server Components payload. We were passing the whole product with all 55 variants into a client component, and 94% of that was related products repeated for every variant. I kept the variants, because they made colour switching instant, but moved related products to load after first paint for just the selected variant. Page size went from about 4 MB to 280 KB, roughly 14 times smaller, and the swatches still switch instantly."

**They may also ask**
- *Why not just paginate the variants?* The variants also work as the swatch-to-SKU map. Removing them breaks instant switching.
- *How did you measure it?* Document size and the flight payload (the RSC data) in the browser's network tab, broken down per field.
- *What would you do differently?* Add a payload-size budget check in CI, so the page can't grow like that again without anyone noticing.

---

## 3. S2: 1,811 database queries → 238 (the N+1 fix)

**Use it for:** performance · GraphQL N+1 · database optimization · "how do you make sure you don't break things?"

**Situation**
- A GraphQL product listing (24 products) was slow.
- We had several custom resolvers: brand details, lifestyle images, downloads, swatches, variant options.
- Each one loaded its data **per product**.

**What I did**
- I counted the SQL queries for one request: **1,811 SELECTs** for 24 products.
- That is a classic **N+1** (one query for the list, then one more query for each item), multiplied across several resolvers.
- I replaced per-product loading with **batch loaders**. Each resolver first collects the IDs it needs. Then one query fetches them all for the request. This is the same idea as **DataLoader** in Node.
- I checked that the response was **byte-for-byte identical** before and after. So nothing changed for the user.

**Result**
- **1,811 → 238 queries.**
- Warm response **~1.6 s → ~0.65 s** (measured in our test environment).

**Lesson**
"With GraphQL, a resolver that looks innocent can run once per item. You have to look at query counts per request, not only slow queries."

**🗣️ Say this in the interview**
> "We had a product listing endpoint in GraphQL that felt slow. I counted the database queries for one request with 24 products and it was over 1,800. Several custom resolvers were each loading their data product by product, which is a classic N+1. I rewrote them to batch: collect all the IDs first, then load them in one query per resolver, the DataLoader pattern. To make sure I didn't break anything, I compared the full response before and after, and it was byte-for-byte identical. Queries went from 1,811 to 238, and response time from about 1.6 seconds to 0.65."

---

## 4. S3: Stores showing "open" on Dubai's clock

**Use it for:** attention to detail · debugging · API design · "a bug that was hard to spot"

**Situation**
- Our store locator API returns whether each physical store is open right now.
- There are 136 locations across many countries.
- **The problem:** it calculated "open now" using the **website's** timezone.
- So **26 of 136** locations were wrong at any given moment. Example: UK stores showed OPEN based on Dubai time, while London was still closed.

**What I did**
- I changed the calculation to use **each location's own timezone**. It comes from the store's country code, using the standard timezone database. So there is no lookup table to maintain.
- I added an override hook for countries that have several timezones.
- I replaced the boolean `is_open_now` (true/false) with an enum: `OPEN / BREAK / CLOSED`. A boolean couldn't express "on a lunch break".
- I verified it across 133 locations in 22 countries.
- I found one store with a break entered as 00:00–16:00. That was clearly a data entry error. I **flagged it to the business** instead of coding around it.

**Result**
The answer is now the same whichever site asks. And it's correct for every country.

**🗣️ Say this in the interview**
> "Our store locator showed whether each store was open right now, but it used the website's timezone for the calculation. So on the Dubai site, UK stores showed as open hours before they actually opened. About 26 of 136 stores were wrong at any moment. I changed it to compute in each store's own timezone, derived from its country, and changed the field from a true/false to an OPEN, BREAK or CLOSED enum, because a boolean couldn't express a lunch break. I also found a store with a clearly mis-entered break window, and I sent that back to the business rather than hiding it in code."

---

## 5. S4: Fixing the cause, not the symptom (missing product URLs)

**Use it for:** root-cause analysis · pushing back on the wrong fix · data quality · ownership

**Situation**
- About **128 products** had no SEO-friendly URL on some store views.
- The storefront fell back to ugly `/catalog/product/view/id/...` links. Our headless frontend couldn't resolve them.
- The existing "fix" was a **cron job that regenerated URLs**.

**What I did**
- I ran a **dry run** first (a test run that changes nothing). **125 of the 128 could not be generated at all.** So the cron could never fix them. It was only ever fixing about 2%.
- **Root cause:** those products had an **empty URL key**. So the platform built the URL from the product **name**. But a sibling product with the same name already owned that URL.
- The unique constraint failed, the transaction rolled back, and the product ended up with **zero** URLs. No visible error.
- I built CLI tools: `report`, `diagnose`, and `fix-url-keys` (which backfills unique URL keys). All have a `--dry-run` mode.
- I **removed the cron**. It was wasted work.
- I recommended the real upstream fix: the product import should always set a unique URL key.

**Result**
**128 → 3** broken products. The remaining 3 just needed a normal regeneration.

**🗣️ Say this in the interview**
> "We had about 128 products with no proper URL, and the existing fix was a cron that kept regenerating URLs. Before changing anything I did a dry run, and 125 of the 128 couldn't be generated at all, so the cron was never going to work. The real cause was that these products had no URL key, so the system built the URL from the product name, and another product with the same name already had it. The insert failed silently. I built a small CLI to diagnose and backfill unique URL keys, with a dry-run mode, removed the cron, and recommended fixing it at the import. We went from 128 broken products to 3."

---

## 6. S5: Bad upstream data crashing the API

**Use it for:** working with other teams · defensive design · "something outside your control"

**Situation**
- About **311 child products across ~144 configurable products** came from the import/PIM feed with required attribute values missing (like size).
- Those null values **crashed our GraphQL resolvers**.
- The admin screen also silently hid these products. So the business thought they weren't linked at all.

**What I did** (two tracks at the same time)
1. **Protect the API:** I added a completeness check. An incomplete child is treated as "not linked", so it's filtered out of variants and options. Healthy products pass through untouched. It runs one query per request.
2. **Fix the source:** I produced an exact list of affected SKUs and gave it to the catalog/import team.

**Result**
Product pages stopped crashing immediately. The data team had a precise list to fix.

**🗣️ Say this in the interview**
> "Our product pages started crashing for certain products. The root cause was in data we didn't own: about 300 child products came in from the import feed missing required attributes, and the nulls broke our resolvers. I did two things in parallel. First, I hardened the API so it treats an incomplete product as not linked, which means the page still works and just doesn't show that option. Second, I generated the exact list of affected products and gave it to the catalog team, because the real fix had to happen at the source. I didn't want code quietly hiding a data problem forever."

---

## 7. S6: One character that broke every GraphQL query

**Use it for:** debugging · a mistake and what you learned · "a bug where the error was misleading"

**Situation**
- I added documentation to a GraphQL schema file.
- After that, **every** query on the endpoint failed with "Unterminated string". Even small, unrelated queries.

**What I did**
- The file parsed fine on its own. So I went one layer down, into the framework's schema reader.
- It splits type definitions with a regular expression that **doesn't understand strings**. So a `{` inside a description cut the type definition short.
- I fixed it by removing the brace. I wrote down the rule for the team.

**Related lesson (another time)**
- Real errors were being **hidden** by a library bug.
- Multibyte characters (decorative `──` lines) in query comments crashed the error formatter. So you saw a confusing `str_repeat()` error instead of the real one.
- Rule: if the error makes no sense, remove non-ASCII characters from the query and run it again.

**Lesson**
"When a small change breaks everything and the error makes no sense, don't trust the error message. Bisect the change (cut it in half again and again to find the bad part) and check assumptions one layer down."

**🗣️ Say this in the interview**
> "I added some documentation to a GraphQL schema file, and suddenly every query on the endpoint failed with 'Unterminated string', even simple unrelated ones. The file itself parsed fine, so I went one layer down into the framework's schema reader. It splits type definitions with a regular expression that doesn't understand strings, so a curly brace inside a description cut the type short. I removed the brace and wrote the rule down for the team. My lesson: when a small change breaks everything and the error makes no sense, don't trust the message. Bisect the change and check one layer down."

**⚠️ Be honest about the impact**
Was it caught locally, on staging, or did it reach production? Say exactly that.

---

## 8. S7: Changing an API without breaking anyone

**Use it for:** API versioning · backward compatibility · release safety · security thinking

**Situation**
The frontend needed details for one specific child product. But the existing GraphQL field returned **all** children.

**What I did**
- I added an **optional** `skus` argument. If it's not sent, the field works exactly as before. Existing clients are not affected.
- I limited it to the parent's own children. So it **can't be abused** to fetch random hidden products.
- I planned the release order: **backend first, then frontend**. In GraphQL, a client sending an unknown argument gets the whole query rejected.

**Result**
Rolled out with no downtime and no client changes needed.

**🗣️ Say this in the interview**
> "When I need to change an API, I try to make it additive. Here, instead of a new endpoint or a version bump, I added an optional argument. Old clients don't send it and get exactly the same result. I also restricted it to the product's own children so nobody could use it to fetch hidden products. And I made sure the backend shipped before the frontend, because in GraphQL an unknown argument fails the whole query."

---

## 9. S8: Silent wrong-page links (data drift)

**Use it for:** finding bugs nobody reported · data quality · SEO/e-commerce impact

**Situation**
Category links were built from a stored `url_path` field.

**What I found**
- **943 store/category pairs** where the stored path didn't match the real live URL.
- Worse: the old path often still worked, but pointed to a **different** category. So shoppers silently landed on the wrong page.
- No 404, no log. So nobody had reported it.

**What I did**
- Switched to the URL rewrite table as the single source of truth, with a safe fallback.
- Compared per store to get an accurate count.

**🗣️ Say this in the interview**
> "The scary bugs are the ones that don't throw errors. Our category links used a stored path that had drifted from the real URLs in 943 cases, and many of those old paths still worked but went to a different category. No 404, no log, nobody reported it. I switched to the actual rewrite table as the source of truth."

---

## Stories YOU must write (S9–S13)

> 📝 **Fill these in before the interview.** The guide does not have your details for these. Use your own real experience.
> Answer the guiding questions in short sentences. Then write a 60–90 second "Say this" version and practise it out loud.
> Use "I", give at least one concrete detail, and end with a result or lesson.

---

## 10. S9: Leadership / mentoring ✍️

**Use it for:** leadership · mentoring · "a time you led something" · "a time you set a standard"

A time you guided a junior, led a feature, or set a standard for the team.

**Guiding questions**
- Who was it, and what were they struggling with?
- What did you do? (pairing, code reviews, documentation, a new standard)
- What changed afterwards? (they shipped X alone, review time dropped, fewer bugs)

**Situation:**
_____________________________________________
_____________________________________________

**What I did:**
_____________________________________________
_____________________________________________
_____________________________________________

**Result:**
_____________________________________________
_____________________________________________

**🗣️ My "Say this" version:**
_____________________________________________
_____________________________________________
_____________________________________________

---

## 11. S10: Conflict / disagreement ✍️

**Use it for:** conflict with a teammate · disagreement · "a time you changed someone's mind"

Pick a **technical** disagreement. Never a personality clash.

**Guiding questions**
- What was the disagreement about?
- How did you get evidence? (a spike, a benchmark, a prototype)
- What was decided?
- Did you support the decision, even if it wasn't your idea?

**Situation:**
_____________________________________________
_____________________________________________

**What I did:**
_____________________________________________
_____________________________________________
_____________________________________________

**Result:**
_____________________________________________
_____________________________________________

**🗣️ My "Say this" version:**
_____________________________________________
_____________________________________________
_____________________________________________

---

## 12. S11: Tight deadline / trade-off ✍️

**Use it for:** pressure · deadlines · trade-offs · "a time you had to cut scope"

Something you shipped under pressure.

**Guiding questions**
- What did you cut, and why?
- What did you refuse to cut? (tests, security)
- How did you tell others about the risk?
- What did you follow up on later?

**Situation:**
_____________________________________________
_____________________________________________

**What I did:**
_____________________________________________
_____________________________________________
_____________________________________________

**Result:**
_____________________________________________
_____________________________________________

**🗣️ My "Say this" version:**
_____________________________________________
_____________________________________________
_____________________________________________

---

## 13. S12: Messaging / async system ✍️

**Use it for:** distributed systems · queues · event-driven design · "how do you handle failures?"

Your Azure Service Bus or AWS SNS/SQS work.

**Guiding questions**
- What did you decouple (separate so they don't wait on each other), and why?
- How did you handle retries, duplicate messages, and dead letters (messages that keep failing)?
- What went wrong once, and what did you change?

**Situation:**
_____________________________________________
_____________________________________________

**What I did:**
_____________________________________________
_____________________________________________
_____________________________________________

**Result:**
_____________________________________________
_____________________________________________

**🗣️ My "Say this" version:**
_____________________________________________
_____________________________________________
_____________________________________________

---

## 14. S13: Production incident ✍️

**Use it for:** production incident · on-call · "a mistake you made" (if S6 doesn't fit)

An outage or a serious bug in production.

**Guiding questions**
- How was it detected?
- What did you do first to stop the damage?
- How did you find the cause?
- What did you add so it can't happen again? (a test, an alert, a guard rail)

**Situation:**
_____________________________________________
_____________________________________________

**What I did:**
_____________________________________________
_____________________________________________
_____________________________________________

**Result:**
_____________________________________________
_____________________________________________

**🗣️ My "Say this" version:**
_____________________________________________
_____________________________________________
_____________________________________________

---

## 15. Which story for which question?

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

## Remember these 10 things
1. "Tell me about yourself" = Present → Past → Proof → Future, 60–90 seconds, end with "why this role".
2. S1: **3.96 MB → 0.28 MB** (about 14× smaller). 94% was `related_products` repeated for 55 variants.
3. S2: **1,811 → 238 queries**, ~1.6 s → ~0.65 s, response byte-for-byte identical.
4. S3: **26 of 136** stores wrong. Fix: each store's own timezone, plus `OPEN / BREAK / CLOSED` enum.
5. S4: **128 → 3** broken products. Dry run showed 125 of 128 could never be fixed by the cron.
6. S5: **~311 children across ~144 products**. Protect the API AND fix the source.
7. S6: a `{` in a description broke every query. Don't trust a weird error, bisect and look one layer down.
8. S7: additive change (optional `skus` arg), and **backend ships before frontend**.
9. S8: **943** store/category pairs silently pointing to the wrong page.
10. Say "in our tests" unless you confirmed production numbers. Fill in S9–S13 before the interview.
