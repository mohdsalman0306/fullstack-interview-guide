# Next.js

> **Time needed:** about 2 hours · **Questions:** 14 + 10 quick questions
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

**Tip before you start:** You built a headless Next.js storefront on Magento GraphQL. Mention it early. It turns theory questions into "here's how I did it".

## Progress checklist
- [ ] 1. CSR vs SSR vs SSG vs ISR
- [ ] 2. App Router vs Pages Router
- [ ] 3. Server Components vs Client Components
- [ ] 4. Hydration and hydration mismatches
- [ ] 5. Data fetching and avoiding waterfalls
- [ ] 6. Caching and revalidation
- [ ] 7. Server Actions
- [ ] 8. Route Handlers and the BFF pattern
- [ ] 9. Middleware (now `proxy`)
- [ ] 10. Authentication in Next.js
- [ ] 11. SEO in Next.js
- [ ] 12. Performance checklist
- [ ] 13. Deploying Next.js
- [ ] 14. Headless commerce with Next.js
- [ ] Quick questions
- [ ] Remember these 10 things

---

## 1. CSR vs SSR vs SSG vs ISR: how do you choose?

**In simple words**
All four are about **when** the HTML is made. At build time, on every request, or in the browser. One website usually mixes them, page by page.

**🗣️ Say this in the interview**
> "It's about **when** the HTML is produced:
> - **SSG (Static Site Generation)**: HTML is built **at build time**. Fastest possible, served from a CDN. Good for marketing pages, docs, blog posts.
> - **ISR (Incremental Static Regeneration)**: static, but **rebuilt** in the background after some time, or **on demand**. Perfect for product and category pages. You get CDN speed, but the page is fresh within minutes, or instantly if I trigger revalidation when a product changes.
> - **SSR (Server-Side Rendering)**: HTML is built **on every request**. For pages that are personalized or must be real-time.
> - **CSR (Client-Side Rendering)**: the browser builds the page with JavaScript. Fine for logged-in dashboards where SEO doesn't matter.
>
> In practice one site mixes them: static home and category pages, ISR product pages, and SSR or client rendering for cart and account."

| Page type | My choice |
|---|---|
| Home, CMS pages | SSG / ISR |
| Product & category pages | ISR + on-demand revalidation |
| Search results | SSR (depends on the query) |
| Cart, checkout, account | Dynamic / client, never shared-cached |
| Admin dashboard | CSR or dynamic SSR |

**⚠️ Avoid saying**
"I use SSR for everything to be safe." Say: I choose per page; most catalog pages can be static with revalidation.

---

## 2. App Router vs Pages Router: what's the difference?

**In simple words**
The Pages Router is the old way. The App Router is the new way, built on React Server Components. New projects should use the App Router.

**🗣️ Say this in the interview**
> "The **Pages Router** is the original. Every file in `pages/` is a route, and data fetching uses `getServerSideProps` and `getStaticProps`.
> The **App Router**, in the `app/` folder, is the modern one. It's built on **React Server Components**. It gives nested **layouts** that stay in place between pages, **streaming**, **Server Actions**, and fetching data directly inside components with `async/await`.
> New projects should use the App Router. Old apps can move step by step, because both routers can live in the same project."

**⚠️ Avoid saying**
"You must rewrite the whole app to move to the App Router." Both can run side by side.

---

## 3. Server Components vs Client Components: when do you use each?

**In simple words**
In the App Router, components run on the server by default and send no JavaScript to the browser. You add `'use client'` only for parts that need clicks, state or browser APIs. Keep those parts small.

**🗣️ Say this in the interview**
> "In the App Router, components are **Server Components by default**. They run **only on the server**. They can be `async`, can query the database or call APIs with secret keys, and **send zero JavaScript** to the browser.
> **Client Components** start with `'use client'`. I need them for anything interactive: state, effects, event handlers, browser APIs. They are still rendered to HTML on the server first, and then **hydrated** in the browser, which means React makes them interactive.
> My rule: keep the page as Server Components, and push `'use client'` down to the **smallest interactive pieces**: the add-to-cart button, the image gallery, the swatch picker. Not the whole page."

**The senior-level point: the serialization boundary**
> "Everything I pass as **props from a Server Component to a Client Component** must be **serializable**, meaning it can be turned into plain data like JSON. And it gets **embedded in the HTML** as the RSC payload. So passing a huge object into a client component quietly makes every page heavier."

**They may also ask**
- *Can a Client Component import a Server Component?* Not directly. Everything imported into a client file becomes client code. But you can pass a Server Component **as `children`** (or another prop) to a Client Component.
- *Can Server Components use `useState`?* No. No state, no effects, no browser APIs.
- *What's the `server-only` package?* You import it in a server file. If that file (with secrets or DB access) is ever imported into client code by mistake, the build fails.

**From my work**
"That's exactly what happened to us. The product page passed the full product, with all 55 variants and the related products for each, into a client component. The HTML was almost 4 MB. Loading related products after first paint brought it down to 280 KB." Use Story S1 — see `1-interview-basics/02-your-story.md`.

**⚠️ Avoid saying**
"I put `'use client'` at the top of the page so everything works." Say: I keep client boundaries small and watch what props cross them.

---

## 4. What is hydration, and what causes hydration mismatches?

**In simple words**
The server sends finished HTML. Then React in the browser "wakes it up" by adding click handlers and state. That's **hydration**. A **mismatch** happens when the browser's first render doesn't match the server's HTML.

**🗣️ Say this in the interview**
> "**Hydration** is React taking the server-rendered HTML and attaching event handlers and state, so the static page becomes interactive. A **mismatch** happens when the client's first render produces **different HTML** from the server's.
> Common causes: `Date.now()` or `Math.random()` in render. Checking `typeof window` to render something different. **Timezone or locale** differences between server and browser when formatting. Invalid HTML nesting, like a `<div>` inside a `<p>`. And browser extensions changing the DOM.
> Fixes: make the first render **give the same result on server and client**. Move browser-only values into `useEffect` and show them after mount, or load that component on the client only. `suppressHydrationWarning` is OK for a single timestamp."

**From my work**
Multi-country stores. Formatting prices or dates with the server's locale vs the browser's locale is a classic mismatch. I pass the locale explicitly.

**⚠️ Avoid saying**
"I just add `suppressHydrationWarning` to fix it." Say: I find the cause and make the first render the same; the flag is only for small cases like a timestamp.

---

## 5. How do you fetch data in the App Router, and avoid waterfalls?

**In simple words**
In Server Components you just `await` the data. The danger is a **waterfall**: request 2 waits for request 1 for no reason. Start independent requests together. Stream slow parts in later.

**Picture it**
Cooking dinner. You don't wait for the rice to finish before you start the curry. You start both at once.

**🗣️ Say this in the interview**
> "In Server Components I just `await` data directly. The main thing to watch is **waterfalls**: awaiting one request, then the next, then the next, when they don't depend on each other. I start independent requests together with `Promise.all`.
> For slow parts I use **streaming**. I wrap the slow section in `<Suspense>`, or add a `loading.tsx`. Then the fast parts of the page show immediately, and the slow part, like recommendations or reviews, streams in when it's ready."

**Waterfall vs parallel:**

```tsx
// ❌ Waterfall: the second request waits for the first for no reason
const product = await getProduct(slug);
const reviews = await getReviews(slug);

// ✅ Parallel: both start at the same time
const [product, reviews] = await Promise.all([
  getProduct(slug),
  getReviews(slug),
]);
```

**Streaming a slow section:**

```tsx
export default async function ProductPage({ params }) {
  const { slug } = await params;            // params is a Promise in Next.js 15+
  const product = await getProduct(slug);   // needed for everything below
  return (
    <>
      <ProductHero product={product} />
      <Suspense fallback={<RelatedSkeleton />}>
        <RelatedProducts sku={product.sku} />  {/* streams in later */}
      </Suspense>
    </>
  );
}
```

**⚠️ Avoid saying**
"I fetch everything one after another at the top of the page." Say: independent requests run in parallel, slow parts stream.

---

## 6. How do caching and revalidation work in Next.js?

**In simple words**
Next.js has several caches. The default behavior changed between versions, so always check the project's version. For a store: keep pages cached, and clear the cache for a product when it changes.

**🗣️ Say this in the interview**
> "Next.js has several cache layers, and the defaults have **changed between versions**, so I always check which version a project uses.
> The ideas stay the same:
> - **Request memoization**: the same fetch inside one render runs only once.
> - **Data cache**: fetched data cached across requests, with `revalidate: 60` for time-based refresh, or **tags** for on-demand clearing.
> - **Full route cache**: the rendered HTML and RSC output for static routes.
> - **Router cache**: in the browser, recently visited routes kept in memory.
>
> Version notes: in **Next.js 15**, `fetch` stopped being cached by default, so you opt in. **Next.js 16** added **Cache Components** with the `'use cache'` directive, plus `cacheLife` and `cacheTag`. That makes caching more explicit.
> For a store, I tag product data with the SKU. When the product changes in the backend, a webhook calls `revalidateTag`. Pages stay static and fast, but update within seconds."

**⚠️ Avoid saying**
Anything that caches personal data (cart, prices for a logged-in B2B customer) under a shared key. That's how one user sees another user's data.

---

## 7. What are Server Actions?

**In simple words**
A Server Action is a function that runs on the server, but you can call it from a form or a button. No need to write an API route. But anyone can call it, so you must check everything inside it.

**🗣️ Say this in the interview**
> "A Server Action is an async function marked `'use server'`. I can call it from a form or a client component, and it runs **on the server**. No API route needed. It's good for changes like add to cart, submit a review, update profile. It works with React 19's `useActionState` and `useOptimistic`.
> Important: a Server Action is really a **public POST endpoint**. Anyone can call it with any arguments. So I **validate the input and check authorization inside every action**, the same as an API route."

**From my work**
The storefront's swatch selection uses a server action (`fetchSelectedChild`) to load the selected child product.

**⚠️ Avoid saying**
"Server Actions are safe because they're on the server." Say: they're public endpoints, so I validate and check permissions in each one.

---

## 8. What are Route Handlers, and what is the BFF pattern?

**In simple words**
Route Handlers are API endpoints inside your Next.js app. A **BFF (Backend-for-Frontend)** is a small server that sits between the browser and your real backends. It keeps secrets safe and gives the UI exactly the data it needs.

**🗣️ Say this in the interview**
> "Route Handlers, in `app/api/.../route.ts`, are API endpoints inside the Next.js app. I use them as a **BFF**. The browser talks to our Next.js server, and the server talks to Magento, the payment provider or search. That **keeps API keys and tokens on the server**. It also combines several calls into one, and reshapes data for the UI. Webhooks also land there, for example for cache revalidation.
> I keep the business logic in separate modules, so I can test it outside the framework."

**⚠️ Avoid saying**
"The browser calls Magento and the payment API directly with the keys." Secrets must stay on the server.

---

## 9. What is Middleware (now `proxy` in Next.js 16)?

**In simple words**
Middleware runs before a request reaches the page. It's for quick decisions like redirects or picking the right country store. It should not be your only security check.

**Picture it**
A receptionist at the door. They point you to the right floor. But each office still checks your badge.

**🗣️ Say this in the interview**
> "Middleware runs **before** a request reaches the route. It's good for quick decisions: redirects, rewrites, **locale or store detection**, A/B test buckets, and a basic 'is there a session cookie?' check. I keep it **fast and light**, with no heavy database calls. In Next.js 16, the file was renamed from `middleware.ts` to `proxy.ts`.
> And I never rely on middleware **alone** for security. In 2025 there was a real vulnerability that let attackers skip Next.js middleware with a crafted header. Authorization must also happen where the **data** is accessed."

**⚠️ Avoid saying**
"My auth is in middleware, so the app is protected." Say: middleware is a first filter; real checks are at the data layer.

---

## 10. How do you handle authentication in Next.js?

**In simple words**
Store the session in a safe cookie. Use middleware for quick redirects. But do the real permission checks on the server, next to the data.

**🗣️ Say this in the interview**
> "The session or refresh token goes in an **HttpOnly, Secure cookie**. HttpOnly means JavaScript in the page can't read it. Secure means it's sent only over HTTPS. Middleware does quick redirects of logged-out users.
> But the **real checks** live in a server-side **data access layer**. Every Server Component, Server Action and Route Handler that reads or changes user data checks the session and permissions there. I never rely on hiding a button in a client component.
> Libraries: Auth.js, Clerk, or a custom OIDC integration."

**⚠️ Avoid saying**
"I hide the admin button if the user isn't an admin." Hiding UI is not security; the server must check.

More on cookies, JWT and sessions: see `3-backend/04-security-auth.md`.

---

## 11. How do you do SEO in Next.js?

**In simple words**
Make sure Google can see the content (server-rendered HTML). Then add the right titles, URLs, structured data and status codes.

**🗣️ Say this in the interview**
> "The basics: content is **server-rendered** so crawlers see it. I use the **Metadata API** (`generateMetadata`) for titles, descriptions and Open Graph. **Canonical URLs** avoid duplicates, because filters and sorting create lots of URLs for the same page. I add `sitemap.ts` and `robots.ts`. **JSON-LD structured data**, like Product, Offer and BreadcrumbList, gives rich results in Google. I return correct **status codes**: a real 404 with `notFound()`, and a 301 redirect for moved products. And good Core Web Vitals.
> For multi-country stores I add **hreflang**, so Google shows the UK page in the UK and the UAE page in the UAE."

**⚠️ Avoid saying**
"Next.js does SEO automatically." It helps with server rendering, but metadata, canonicals and status codes are your job.

---

## 12. What's on your Next.js performance checklist?

**In simple words**
Send less JavaScript, send less data, fetch in parallel, and measure with real users.

**🗣️ Say this in the interview**
> "I keep most of the tree as **Server Components**, keep `'use client'` boundaries small, and watch **what props cross the boundary**. I fetch in **parallel** and **stream** slow sections. I use **`next/image`** and **`next/font`**. I **dynamic-import** heavy client widgets. I cache data on purpose, with tags. I check the bundle analyzer. And I measure **real-user** Web Vitals, not just Lighthouse on my laptop."

**⚠️ Avoid saying**
"Lighthouse gives 95, so performance is fine." Say: I also check real-user Web Vitals.

---

## 13. How do you deploy Next.js?

**In simple words**
Vercel is the easy option. You can also run it yourself in Docker. If you run many copies, they must share one cache.

**🗣️ Say this in the interview**
> "**Vercel** is the zero-config option. To self-host, I use **`output: 'standalone'`** in a Docker image and run it as a Node server behind a CDN.
> A gotcha when self-hosting **multiple instances**: ISR and the data cache are stored **per instance** by default. So I need a **shared cache handler**, for example Redis. Otherwise different instances serve different versions of a page.
> I also choose **Edge vs Node runtime** per route. Edge starts fast and runs close to users, but has a limited API. Node supports everything."

**⚠️ Avoid saying**
"Just run three containers and it works." Without a shared cache, they serve different versions.

More on Docker and CI/CD: see `5-devops-cloud/01-docker-kubernetes-cicd.md`.

---

## 14. How would you build headless commerce with Next.js? (your territory)

**In simple words**
**Headless** means the backend (Magento) and the frontend (Next.js) are separate. They talk through an API (GraphQL). The key is to decide which pages can be cached and which must be private and fresh.

**🗣️ Say this in the interview**
> "In our setup, Magento is the commerce engine: catalog, pricing, inventory, cart, orders. It exposes **GraphQL**. Next.js is the storefront. The design choices that matter:
> - **Catalog pages**, product and category, are cacheable. They're static with revalidation, and cleared when the product changes.
> - **Cart, checkout and account** are private and dynamic, and never shared-cached.
> - **Price and stock** need freshness rules. I show a cached price, but always confirm it at add-to-cart and checkout.
> - **Payload discipline**: GraphQL makes it easy to ask for too much, and the App Router puts everything a client component receives into the HTML. That was my 4 MB lesson.
> - **Multi-store routing**: the store and locale come from the domain or path, and go into every GraphQL call.
> - **Deploy order**: a backend schema change must ship **before** the frontend that uses it."

**From my work**
This is your real project. Use Story S1 (the 4 MB → 280 KB fix) — see `1-interview-basics/02-your-story.md`. For GraphQL details, see `3-backend/03-graphql.md`.

**⚠️ Avoid saying**
"We cache everything at the CDN." Say: catalog is cached, cart and account are never shared-cached, and price is confirmed at checkout.

---

## Quick questions
- **`layout.tsx` vs `template.tsx`?** A layout stays in place across navigation (keeps its state). A template mounts fresh on every navigation.
- **`loading.tsx` / `error.tsx` / `not-found.tsx`?** Automatic loading UI (Suspense fallback) / error boundary / 404 page for that route segment.
- **`generateStaticParams`?** Lists which dynamic routes (for example product slugs) to pre-build at build time.
- **Dynamic segments?** `[slug]`, catch-all `[...slug]`, optional catch-all `[[...slug]]`.
- **Route groups?** Folders like `(shop)` organise routes without changing the URL.
- **Parallel / intercepting routes?** Show several pages in one layout (`@modal`), or open a route as a modal on top of the current page.
- **`NEXT_PUBLIC_` env vars?** They are put into the browser bundle. **Never** put secrets there.
- **`revalidatePath` vs `revalidateTag`?** Clear one URL vs clear everything tagged with a key (better for "product X changed").
- **Partial Prerendering?** A static shell is served instantly, and dynamic parts stream into the holes, in the same route.
- **`next/link` prefetch?** Links visible on screen load their route in advance, so navigation feels instant.

## Remember these 10 things
1. SSG, ISR, SSR, CSR = when the HTML is made. Mix them per page.
2. Product and category pages: ISR plus on-demand revalidation with tags.
3. Server Components by default. Push `'use client'` down to small interactive parts.
4. Props from server to client go into the HTML. Big props = big pages (my 4 MB → 280 KB story).
5. Hydration mismatch = server HTML and first client render differ. Watch dates, random values, locale.
6. Run independent fetches with `Promise.all`. Stream slow parts with `<Suspense>`.
7. Caching defaults changed in Next.js 15 and 16. Always check the version.
8. Never cache personal data (cart, B2B prices) under a shared key.
9. Server Actions are public POST endpoints. Validate and check auth in every one.
10. Middleware/proxy is a first filter only. Real auth checks happen where the data is read.
