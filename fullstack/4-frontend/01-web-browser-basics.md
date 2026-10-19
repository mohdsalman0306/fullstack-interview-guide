# Web and Browser Basics

> **Time needed:** about 1.5 hours · **Questions:** 12 + 8 quick questions
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

Full stack interviews often include "browser" questions. They check if you understand what happens **between** your server and your React code.

## Progress checklist
- [ ] 1. What happens when you type a URL
- [ ] 2. HTTP/1.1 vs HTTP/2 vs HTTP/3
- [ ] 3. How HTTPS / TLS works
- [ ] 4. Caching static files (browser and CDN)
- [ ] 5. Cookie attributes
- [ ] 6. Critical rendering path, `async` vs `defer`
- [ ] 7. Core Web Vitals
- [ ] 8. Browser storage options
- [ ] 9. WebSockets vs SSE vs polling
- [ ] 10. Event bubbling, capturing and delegation
- [ ] 11. Accessibility basics
- [ ] 12. Image and font optimization
- [ ] Quick questions

---

## 1. What happens when you type a URL and press Enter?

**In simple words**
The browser finds the server's address, connects, asks for the page, gets HTML back, and turns it into pixels. This question has no end, so give the outline and offer to go deeper.

**🗣️ Say this in the interview**
> At a high level:
> 1. **DNS**: the browser turns the domain into an IP address. It checks its own cache, then the OS, then a DNS resolver.
> 2. **Connection**: it opens a TCP connection, or QUIC for HTTP/3, and does a **TLS handshake** for HTTPS.
> 3. **Request**: it sends the HTTP request. Often a **CDN** answers from its cache. If not, it goes through a load balancer to an app server, which may use a cache or a database.
> 4. **Response**: the HTML comes back.
> 5. **Rendering**: the browser turns HTML into the **DOM** and CSS into the **CSSOM**, combines them, works out the **layout**, **paints** the pixels, and **composites** the layers together. Along the way it finds and downloads CSS, JS, images and fonts, and runs JavaScript.
> 6. **Hydration**, in a framework like Next.js: React attaches event handlers so the page becomes interactive.
>
> I can go deeper on any step. For performance, the ones that usually matter are DNS and connection setup, server response time, and render-blocking files.

**They may also ask**
- *Which part would you go deeper on?* Ask them back: "Which part would you like me to go deeper on?" Let them choose.

**⚠️ Avoid saying**
Don't spend five minutes on DNS alone. Give the full outline first, then go deeper where they ask.

---

## 2. What is the difference between HTTP/1.1, HTTP/2 and HTTP/3?

**In simple words**
HTTP/1.1 sends one request at a time per connection. HTTP/2 sends many at once over one connection. HTTP/3 does the same over QUIC, so one lost packet doesn't slow down everything.

**Picture it**
HTTP/2 is many cars on one single-lane road: if one car stops, all stop. HTTP/3 gives each car its own lane.

**🗣️ Say this in the interview**
> **HTTP/1.1** handles one request at a time per connection. So browsers opened about six connections per domain, and we used tricks like bundling and sprite sheets.
> **HTTP/2** **multiplexes**, which means it sends many requests over one connection at the same time, and it compresses headers. But it still runs on TCP. So one lost packet stalls everything on that connection. That's called **head-of-line blocking**.
> **HTTP/3** runs on **QUIC over UDP**. Each stream is independent, so a lost packet only delays its own stream. Setting up the connection is faster too, which helps a lot on mobile networks.

**⚠️ Avoid saying**
"HTTP/2 fixed head-of-line blocking completely." It fixed it at the HTTP level, but TCP still has it. HTTP/3 fixes that.

---

## 3. How does HTTPS / TLS work, in simple terms?

**In simple words**
The server proves who it is with a certificate. Then both sides agree on a secret key, and everything after that is encrypted.

**🗣️ Say this in the interview**
> When the browser connects, the server sends its **certificate**. It says "I am shop.com" and is signed by a trusted Certificate Authority. The browser checks that signature.
> Then both sides use **asymmetric crypto** (a public/private key pair) to agree on a shared secret key. From then on, everything is encrypted with fast **symmetric** encryption, using that one shared key.
> TLS 1.3 does this in a single round trip.
> The result: nobody in between can read or change the traffic, and I know I'm talking to the real server.

**⚠️ Avoid saying**
"All the data is encrypted with the server's public key." The public key part is only for agreeing on the shared key.

---

## 4. How do you cache static files in the browser and CDN?

**In simple words**
Files with a hash in the name can be cached for a year, because a change means a new name. The HTML must always be checked again, so users get new deploys right away.

**🗣️ Say this in the interview**
> The standard pattern: **fingerprinted files**, like `app.3f9a1c.js`, get `Cache-Control: public, max-age=31536000, immutable`. They're cached for a year, because any change makes a new filename.
> The **HTML** gets `no-cache`, which means "always check with the server first". So users pick up new deploys immediately.
> A **CDN** (servers around the world that cache content near users) takes most of the traffic.
> On deploy, I either purge the CDN or rely on the new filenames.

**⚠️ Avoid saying**
"`no-cache` means don't cache." It means cache, but always check with the server before using it. `no-store` means don't cache.

---

## 5. What do the cookie attributes do?

**In simple words**
Cookie attributes control who can read the cookie, when it is sent, and how long it lives.

| Attribute | What it does |
|---|---|
| `HttpOnly` | JavaScript can't read it, so XSS can't steal it |
| `Secure` | Only sent over HTTPS |
| `SameSite=Strict` | Never sent on cross-site requests |
| `SameSite=Lax` | Sent when you click a link to the site, but not on cross-site POSTs or iframes. The modern default |
| `SameSite=None` | Always sent (needs `Secure`). For real cross-site use, like embedded widgets |
| `Domain` / `Path` | Which hosts and paths get the cookie |
| `Max-Age` / `Expires` | How long it lives. Without it, it's a session cookie (gone when the browser closes) |
| `__Host-` prefix | Forces Secure, Path=/ and no Domain. The strictest option |

**🗣️ Say this in the interview**
> For an auth cookie I use `HttpOnly; Secure; SameSite=Lax`, with the `__Host-` prefix if possible. HttpOnly stops JavaScript from reading it, Secure keeps it on HTTPS, and Lax stops it being sent on cross-site POSTs.

**⚠️ Avoid saying**
"`SameSite=None` is fine for login cookies." Use it only when you truly need cross-site cookies.

---

## 6. What is the critical rendering path? What is `async` vs `defer`?

**In simple words**
The browser can't show the page until it has the HTML and all the CSS in the head. Normal scripts stop the HTML parsing. `defer` and `async` let scripts download without blocking.

**Picture it**
`defer` is "read the whole recipe, then cook the steps in order". `async` is "cook each step the moment it arrives, any order".

**🗣️ Say this in the interview**
> The browser can't show anything until it has the HTML and **all the CSS** in the head, because CSS is **render-blocking**. A normal `<script>` is **parser-blocking**: HTML parsing stops until the script downloads and runs.
> **`defer`** downloads in parallel and runs **after** the HTML is parsed, **in order**. That's what I use for app scripts.
> **`async`** downloads in parallel and runs **as soon as it arrives**, in any order. Good for independent scripts like analytics.
> To speed up the first paint: inline the critical CSS, defer non-critical JS, `preload` the hero image and fonts, and `preconnect` to important third-party domains.

**⚠️ Avoid saying**
"`async` and `defer` are the same." Order and timing are different.

---

## 7. What are Core Web Vitals, and how do you improve them?

**In simple words**
They are Google's three numbers for user experience: how fast the main content shows, how fast the page reacts, and how much it jumps around. They affect SEO and sales.

**🗣️ Say this in the interview**
> They're Google's three user-experience metrics, measured at the **75th percentile** of real users:
> - **LCP (Largest Contentful Paint)**: how fast the main content shows. Good is **≤ 2.5 s**.
> - **INP (Interaction to Next Paint)**: how fast the page reacts to clicks and typing. Good is **≤ 200 ms**. It replaced FID in 2024.
> - **CLS (Cumulative Layout Shift)**: how much things jump around. Good is **≤ 0.1**.
>
> They affect SEO, and in e-commerce they affect conversion directly.

| Metric | Usual causes | Fixes |
|---|---|---|
| **LCP** | Slow server (TTFB), big hero image, render-blocking CSS/JS, rendering only in the browser | SSR/SSG, CDN, `fetchpriority="high"` and preload on the hero image, modern formats (AVIF/WebP), don't lazy-load the LCP image |
| **INP** | Long JavaScript tasks, heavy hydration, big re-renders | Less JS, code splitting, break up long tasks, Server Components, avoid extra re-renders |
| **CLS** | Images without sizes, late ads or banners, font swaps | Set `width`/`height` or `aspect-ratio`, reserve space, `font-display` with matched fallback fonts |

**From my work**
Use Story S1 (the 4 MB → 280 KB HTML cut): a 4 MB HTML document hurts LCP badly, because the browser must download and parse all of it. Cutting it to 280 KB was a direct LCP win. See `1-interview-basics/02-your-story.md`.

**⚠️ Avoid saying**
"FID is one of the Core Web Vitals." INP replaced FID in 2024.

---

## 8. What are the browser storage options?

**In simple words**
Cookies are small and go to the server with every request. The others stay in the browser. None of the JavaScript-readable ones are safe for tokens.

| Storage | Size | How long it lasts | Sent to server? | Use for |
|---|---|---|---|---|
| Cookie | ~4 KB | Until it expires | **Yes**, every request | Session/auth (HttpOnly) |
| `localStorage` | ~5–10 MB | Until cleared | No | Non-sensitive settings |
| `sessionStorage` | ~5–10 MB | Until the tab closes | No | Temporary state for one tab |
| IndexedDB | Large | Until cleared | No | Offline data, big structured data |
| Cache API | Large | Until cleared | No | Service worker offline caching |

**🗣️ Say this in the interview**
> `localStorage` is synchronous, which means it blocks while it reads and writes. And any script on the page can read it. So it's never the place for tokens. For auth I use HttpOnly cookies.

**⚠️ Avoid saying**
"I store the JWT in `localStorage`." Say HttpOnly cookie or memory (see `3-backend/04-security-auth.md`).

---

## 9. WebSockets vs Server-Sent Events vs polling: which do you pick?

**In simple words**
Polling keeps asking "anything new?". SSE lets the server push to the client, one way. WebSockets let both sides talk anytime.

**Picture it**
Polling is a kid asking "are we there yet?" every minute. SSE is a radio. WebSockets are a phone call.

**🗣️ Say this in the interview**
> **Polling** asks the server every few seconds. It's simple but wasteful.
> **Long polling** keeps the request open until there's news.
> **SSE** (Server-Sent Events) is a one-way stream **from server to client** over normal HTTP, with auto-reconnect built in. It's perfect for notifications, live order status, or streaming AI responses.
> **WebSockets** are a full **two-way** connection. I need them for chat, multiplayer games or editing a document together.
> I choose the simplest one that fits. If the client only listens, SSE. If both sides talk all the time, WebSockets.

**⚠️ Avoid saying**
"Always use WebSockets for real time." SSE is simpler when only the server sends.

---

## 10. What are event bubbling, capturing and delegation?

**In simple words**
A click event first travels down to the element, then bubbles back up through all its parents. Delegation means one listener on the parent handles clicks for all its children.

**🗣️ Say this in the interview**
> When I click an element, the event travels **down** from the window to the target. That's the **capture** phase. Then it **bubbles back up** through every parent. Most handlers run during bubbling.
> **Event delegation** uses this. Instead of adding a listener to 1,000 list items, I add **one** to the parent and check `event.target`. It uses less memory, and it works for items added later.
> `stopPropagation()` stops the event from travelling further. `preventDefault()` stops the browser's default action, like following a link or submitting a form. They're different things.

**⚠️ Avoid saying**
"`preventDefault()` stops the event from bubbling." It doesn't. That's `stopPropagation()`.

---

## 11. What are your accessibility basics?

**In simple words**
Use real HTML elements first. Give everything labels, text alternatives, good contrast, and keyboard support. Accessible apps are also easier to test.

**🗣️ Say this in the interview**
> My baseline is **semantic HTML first**. A real `<button>` instead of a clickable `<div>`, because it gives keyboard support and screen-reader roles for free.
> Every input has a **label**. Images have **alt text**. Colour contrast meets **WCAG AA** (the common accessibility standard). Everything works **with a keyboard**, with a visible focus state. And I use **ARIA** only when normal HTML can't express something.
> It also makes tests better. Playwright and Testing Library find elements by role and label, so accessible apps are easier to test.

**⚠️ Avoid saying**
"We add ARIA to everything." Native HTML first; ARIA only when needed.

---

## 12. How do you optimize images and fonts?

**In simple words**
Send small, modern image files at the right size, and lazy-load what is below the fold. Host fonts yourself and stop them from shifting the layout.

**🗣️ Say this in the interview**
> For images: serve **modern formats** like AVIF or WebP. Serve **responsive sizes** with `srcset` and `sizes`. Always set width and height. **Lazy-load** images below the fold, but **never** the LCP image. Use a CDN or image service to resize. In Next.js, `next/image` does most of this for me.
> For fonts: **self-host** them, **subset** them (keep only the characters I need), `preload` the main one, and use `font-display: swap` with a fallback font of matching size, so the layout doesn't shift.

**⚠️ Avoid saying**
"We lazy-load all images." Never lazy-load the main hero (LCP) image.

---

## Quick questions
- **What is TTFB?** Time to First Byte: how long until the server starts responding. It includes DNS, connection and server work.
- **Reflow vs repaint?** Reflow recalculates layout (expensive). Repaint redraws pixels (cheaper). Animate `transform` and `opacity` to avoid both.
- **What is a CDN?** Servers around the world that cache your content near users. Lower latency, less load on your own server.
- **What is the same-origin policy?** Scripts can only read responses from the same scheme + host + port. CORS relaxes this rule.
- **What is a service worker?** A background script that sits between the page and the network. It enables offline support, caching and push notifications (PWA).
- **`preload` vs `prefetch`?** `preload` means "needed for **this** page, now". `prefetch` means "probably needed for the **next** page", at low priority.
- **What is tree shaking?** Removing unused exports at build time. Works best with ES modules.
- **SPA vs MPA?** A single-page app swaps views with JavaScript. A multi-page app loads new HTML for each page. Next.js mixes both.

## Remember these 10 things
1. URL → DNS → connect + TLS → request (CDN/server) → HTML → DOM + CSSOM → layout → paint → hydrate.
2. HTTP/2 = many requests on one connection. HTTP/3 = QUIC over UDP, no TCP head-of-line blocking.
3. TLS: certificate proves identity, asymmetric crypto agrees on a key, symmetric crypto encrypts.
4. Hashed file names: cache for a year (`immutable`). HTML: `no-cache`.
5. Auth cookie: `HttpOnly; Secure; SameSite=Lax`, plus `__Host-` if possible.
6. CSS blocks rendering. `defer` = after parsing, in order. `async` = as soon as ready, any order.
7. Core Web Vitals: LCP ≤ 2.5 s, INP ≤ 200 ms, CLS ≤ 0.1, at the 75th percentile.
8. Your story: 4 MB → 280 KB HTML was a direct LCP win.
9. SSE for server-to-client only; WebSockets for two-way. Never tokens in `localStorage`.
10. Semantic HTML first; never lazy-load the LCP image.
