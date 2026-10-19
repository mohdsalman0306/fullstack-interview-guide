# Node.js Core and Production

> **Time needed:** about 3 hours · **Questions:** 19 + quick questions
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

## Progress checklist
- [ ] 1. What is Node.js good and bad at?
- [ ] 2. The event loop
- [ ] 3. The libuv thread pool
- [ ] 4. Why `process.nextTick` can be dangerous
- [ ] 5. A blocked event loop: find it and fix it
- [ ] 6. Streams and backpressure
- [ ] 7. Buffer
- [ ] 8. EventEmitter
- [ ] 9. Worker threads vs child processes vs cluster
- [ ] 10. `spawn` vs `exec` vs `execFile` vs `fork`
- [ ] 11. Scaling a Node.js API
- [ ] 12. Error handling
- [ ] 13. Graceful shutdown
- [ ] 14. Finding a memory leak
- [ ] 15. Investigating 100% CPU
- [ ] 16. Logging and observability
- [ ] 17. Config and environment variables
- [ ] 18. Security basics
- [ ] 19. What's new in modern Node.js
- [ ] Quick questions

---

## 1. What is Node.js, and what is it good (and bad) at?

**In simple words**
Node.js runs JavaScript outside the browser. It uses Chrome's V8 engine to run your code, and a library called **libuv** to do I/O (network, files) in the background. Your code runs on one main thread. So Node is great when the work is mostly "waiting", and bad when the work is heavy math.

**Picture it**
A waiter who takes many orders and passes them to the kitchen. He doesn't stand and wait for each dish. But if he has to cook a dish himself, every table waits.

**🗣️ Say this in the interview**
> "Node.js is a JavaScript runtime. It's built on Chrome's V8 engine plus a library called libuv, which handles async I/O. My JavaScript runs on one main thread, but things like database calls, HTTP requests and file reads happen in the background without blocking.
> So Node is excellent for I/O-heavy work: APIs, BFFs, real-time apps, gateways, anything that mostly waits on other systems. It's weaker for CPU-heavy work, like image processing or big calculations, because that blocks the one thread for every user. For that I'd use worker threads, a queue with separate workers, or a different service."

**⚠️ Avoid saying**
"Node is single-threaded, so it can't handle many users." Say instead: one thread runs your JS, but I/O runs in the background, so it handles many concurrent requests well.

---

## 2. Explain the Node.js event loop

**In simple words**
The **event loop** is the loop that decides which callback runs next. It goes round in fixed steps called **phases**. Only one callback runs at a time. So if one callback is slow, everything else waits.

**Picture it**
A receptionist going round the same desks in order: timers, mail, phones, close-up. She never skips a desk. But if one visitor talks for ten minutes, the whole office queues.

**🗣️ Say this in the interview**
> "The event loop runs callbacks in phases. Each turn goes:
> one, **timers**: `setTimeout` and `setInterval` callbacks whose time is up.
> Two, **pending callbacks**: some delayed system callbacks.
> Three, **poll**: waits for I/O and runs I/O callbacks. Most of your code runs here.
> Four, **check**: `setImmediate` callbacks.
> Five, **close**: close events, like a socket closing.
> Between every callback, Node first empties the `process.nextTick` queue, then the Promise microtasks. The one rule that matters in production: if any callback takes a long time, everything else waits, including every other user's request."

**They may also ask**
- *`setImmediate` vs `setTimeout(fn, 0)`?* From the main module, the order is not guaranteed. Inside an I/O callback, `setImmediate` always runs first, because the check phase comes right after poll.
- *Where do microtasks run?* After each single callback, not only between phases (since Node 11).

**⚠️ Avoid saying**
"The event loop runs my code in parallel." It runs one callback at a time. The parallel part is the I/O in the background.

---

## 3. What uses the libuv thread pool?

**In simple words**
Network work does NOT use the thread pool. The operating system already does network I/O without blocking. The **thread pool** (a small group of background threads) is for jobs the OS can't do async. By default it has only **4** threads.

**🗣️ Say this in the interview**
> "Network sockets don't use the thread pool. They use the operating system's own async tools, like epoll, kqueue or IOCP. That's why Node can hold thousands of connections.
> The thread pool is for things the OS can't do async: most **file system** work, **crypto** like `pbkdf2` and `scrypt`, **zlib** compression, and `dns.lookup`.
> The default pool size is 4. So if I do a lot of hashing or file work at the same time, those calls queue behind each other. I can raise `UV_THREADPOOL_SIZE`, but usually the better fix is to move that work off the request path."

**⚠️ Avoid saying**
"Every async call goes to the thread pool." Network calls don't, only files, some crypto, zlib and `dns.lookup`.

---

## 4. Why can `process.nextTick` be dangerous?

**In simple words**
Node runs the whole nextTick queue before it moves on. If a nextTick callback keeps adding more nextTicks, Node never gets to timers or I/O. This is called **starving** the event loop.

**🗣️ Say this in the interview**
> "Node empties the entire nextTick queue before moving on. If a nextTick callback schedules another nextTick, again and again, the event loop never reaches timers or I/O. It's starved. So I almost never use it in app code. `queueMicrotask` or `setImmediate` are safer ways to delay work."

**⚠️ Avoid saying**
"nextTick runs on the next loop turn." It runs before the loop continues, which is exactly why it can block.

---

## 5. How do you know if the event loop is blocked, and how do you fix it?

**In simple words**
When the loop is blocked, every request on that server gets slow at once, even the health check. You measure **event-loop lag** (how late callbacks run) and take a **CPU profile** to find the slow function.

**🗣️ Say this in the interview**
> "The sign is that all endpoints get slow at the same time, even simple health checks, while CPU on that instance is high. I measure **event-loop lag**. Node has `perf_hooks.monitorEventLoopDelay()`, and most APM tools show it. Then I take a CPU profile to find the hot function.
> Usual causes: a big `JSON.parse` or `JSON.stringify`, sync crypto or `fs.*Sync`, a bad regex, sorting or looping over huge arrays, or heavy template rendering.
> Fixes, in order: make the work smaller, by paginating or streaming. Make it async. Move it to a worker thread. Or move it to a background job."

**⚠️ Avoid saying**
"I'd just add more servers." That hides the problem. Find the slow function first.

---

## 6. Explain streams and backpressure

**In simple words**
A **stream** handles data in small pieces (chunks) instead of loading it all into memory. A 5 GB file never needs 5 GB of RAM. **Backpressure** is when the sender is faster than the receiver. If you ignore it, data piles up in memory.

**Picture it**
Filling a bucket from a hose. Backpressure is turning the tap down when the bucket is nearly full, instead of flooding the floor.

**🗣️ Say this in the interview**
> "Streams let me process data in chunks instead of loading it all into memory. There are four types: **Readable**, **Writable**, **Duplex**, which is both, like a socket, and **Transform**, which changes the data as it passes, like gzip.
> **Backpressure** happens when the producer is faster than the consumer. If you ignore it, data piles up in memory. When `write()` returns `false`, you should stop and wait for the `drain` event. In practice I just use `stream.pipeline()`. It handles backpressure, passes errors along, and cleans up all the streams if one fails."

```javascript
import { pipeline } from 'node:stream/promises';
import { createGzip } from 'node:zlib';

// Export all orders as a gzipped CSV, without loading them all into memory
app.get('/export.csv.gz', async (req, res) => {
  res.setHeader('Content-Type', 'application/gzip');
  await pipeline(
    db.streamQuery('SELECT * FROM orders'), // Readable: rows come out one by one
    toCsv(),                                 // Transform: row -> CSV line
    createGzip(),                            // Transform: compress
    res                                      // Writable: send to the client
  );
});
```

**They may also ask**
- *What is `highWaterMark`?* The buffer size limit. When the buffer goes past it, the stream says "slow down".
- *Flowing vs paused mode?* Flowing pushes data to you with `'data'` events. Paused means you pull with `read()`. Piping handles this for you.
- *Why `pipeline` and not `.pipe()`?* `.pipe()` doesn't pass errors along or close the other streams on failure, so you can leak file handles.

**From my work**
Large exports and imports: streaming rows straight to the response instead of building one giant array. See the Production Troubleshooting file (`6-system-design/02-production-troubleshooting.md`).

**⚠️ Avoid saying**
"I use `.pipe()` everywhere." Say `pipeline()`, because it handles errors and cleanup.

---

## 7. What is a Buffer?

**In simple words**
A **Buffer** is a fixed-size block of raw bytes. It lives outside the normal JS memory (the heap). Node uses it for files, network data, crypto and encodings.

**🗣️ Say this in the interview**
> "A Buffer is a fixed-size chunk of raw binary memory, outside the normal JS heap. Node uses it for files, network data, crypto and encodings. I create one with `Buffer.from()` or `Buffer.alloc()`. `allocUnsafe` is faster, but it may contain old memory, so I only use it if I overwrite every byte."

**They may also ask**
- *Buffer vs string?* A string is text (UTF-16 inside JS). A Buffer is raw bytes. To convert between them you need an encoding like `'utf8'`, `'base64'` or `'hex'`.
- *Encoding bugs?* If you cut a UTF-8 Buffer in the middle of a multi-byte character, you get garbage. Use `StringDecoder`, or `setEncoding` on streams.

**⚠️ Avoid saying**
"`allocUnsafe` is fine, it's faster." Only if you overwrite every byte, or you may leak old data.

---

## 8. What is EventEmitter?

**In simple words**
**EventEmitter** is Node's built-in publish/subscribe, but only inside one process. You `emit` an event and all listeners for it run, one after another. Streams, HTTP servers and sockets are all EventEmitters.

**🗣️ Say this in the interview**
> "EventEmitter is Node's built-in publish/subscribe inside one process. I `emit` a named event, and the listeners registered with `on` or `once` get called synchronously, in order. Streams, HTTP servers and sockets are all EventEmitters.
> Two things to remember. Always handle the special `'error'` event, because an unhandled one crashes the process. And it's in-memory only. It's not a replacement for a real message broker between services."

**They may also ask**
- *What does "MaxListenersExceededWarning" mean?* Usually a leak. You add a listener on every request and never remove it.
- *EventEmitter vs Service Bus or Kafka?* EventEmitter is one process, and events are lost on crash. A broker keeps messages safely, works across services, and supports retries.

**⚠️ Avoid saying**
"EventEmitter listeners run async." They run synchronously, in the order they were added.

---

## 9. Worker threads vs child processes vs cluster

**In simple words**
All three give you more than one thread or process, but for different jobs. Worker threads are for heavy JS work. Child processes are for running another program. Cluster runs several copies of your server.

**🗣️ Say this in the interview**
> "They solve different problems.
> **Worker threads** run JavaScript in parallel inside the same process. I use them for CPU-heavy work like image resizing, PDF generation or big parsing. Talking to them is cheap, and they can share memory.
> **Child processes** are separate processes with their own memory. I use them to run another program, like ffmpeg or a Python script, or for strong isolation, so if it crashes, my server doesn't.
> **Cluster** runs several copies of my server to use all CPU cores, sharing one port.
> In Kubernetes I usually don't use cluster. I run one Node process per container and let Kubernetes scale the number of pods. It's simpler, and Kubernetes handles restarts."

| | Runs where | Use it for |
|---|---|---|
| Worker thread | Same process | Heavy JS (resize, PDF, parsing) |
| Child process | New process | Another program, or isolation |
| Cluster | Many copies of server | Use all CPU cores (less needed in K8s) |

**They may also ask**
- *When is a queue better than a worker thread?* When the work can happen later, needs retries, or is big enough to scale separately from the API.

**⚠️ Avoid saying**
"I use cluster mode in every Kubernetes pod." Usually one process per pod, and let Kubernetes scale pods.

---

## 10. `spawn` vs `exec` vs `execFile` vs `fork`

**In simple words**
These are four ways to start another process from Node. They differ in whether they use a shell and whether they stream or collect the output. The big point is security: never put user input into a shell.

**🗣️ Say this in the interview**
> "`spawn` **streams** the output, so it's best for long-running commands or big output. `exec` runs through a **shell** and **buffers** the output. It's handy, but risky with user input, and limited by `maxBuffer`. `execFile` runs a program **directly**, without a shell, so it's safer. `fork` starts another **Node** script with a built-in message channel.
> My security rule: never put user input into `exec`. I use `execFile` or `spawn` with an arguments array, so there's no shell injection."

| | Shell? | Output | Best for |
|---|---|---|---|
| `spawn` | No (by default) | Streamed | Long or big output |
| `exec` | Yes | Buffered | Quick trusted commands |
| `execFile` | No | Buffered | Running a binary safely |
| `fork` | No | Message channel | Another Node script |

**⚠️ Avoid saying**
"I'd build the command string with the user's filename." That's shell injection. Pass arguments as an array.

---

## 11. How do you scale a Node.js API?

**In simple words**
Make each server "forget" everything between requests (stateless), then run many of them. Then take load off the database and move slow work into the background. Usually the database is the real bottleneck, not Node.

**🗣️ Say this in the interview**
> "I think about it in layers.
> One, make instances **stateless**: no sessions or uploads on local disk or memory. Shared state goes to Redis, the database, or object storage.
> Two, **scale horizontally**: many instances behind a load balancer, autoscaled on CPU, latency or request count.
> Three, **take load off the database**: indexes, caching hot reads in Redis, read replicas, connection pooling.
> Four, **move slow work out of the request**: queues and background workers for emails, exports, image processing.
> Five, **cache at the edge**: a CDN for static files and cacheable responses.
> Six, **observe**: p95 and p99 latency, error rate, event-loop lag, DB pool usage, queue depth.
> Most of the time the bottleneck turns out to be the database, not Node."

**They may also ask**
- *Sticky sessions?* Avoid them when you can, because they break even load spreading. You need them only for things like WebSocket fallbacks. Better to keep state in Redis.
- *Horizontal vs vertical?* Vertical means a bigger machine: simple, but it has a limit and is a single point of failure. Horizontal means more machines: you must be stateless, but it scales further and survives failures.

**⚠️ Avoid saying**
"Just add more pods." If the database is the bottleneck, more pods make it worse.

---

## 12. How do you handle errors in Node.js?

**In simple words**
There are two kinds of errors. **Operational errors** are expected problems, like bad input or a timeout. You handle them. **Programmer errors** are bugs. You can't safely "handle" a bug, so you log it and restart cleanly.

**🗣️ Say this in the interview**
> "I split errors into two kinds. **Operational errors** are expected failures: invalid input, not found, a timeout calling another service. I handle these and return a proper response. **Programmer errors** are bugs, like reading a property of undefined. You can't safely handle those.
> In practice I use typed error classes, one central error handler that turns errors into HTTP responses, structured logs with a request ID, and I never send stack traces to clients. For `uncaughtException` and `unhandledRejection`, I log, then shut down gracefully, and let Kubernetes or PM2 restart the process. After an unknown error, the app's state can't be trusted."

**⚠️ Avoid saying**
"I catch `uncaughtException` and keep running." Log it, shut down gracefully, and restart.

---

## 13. How do you do a graceful shutdown?

**In simple words**
**Graceful shutdown** means stopping the server without dropping requests that are still running. Kubernetes sends **SIGTERM** (a "please stop" signal), waits 30 seconds by default, then sends **SIGKILL** (force stop). You use that window to finish work and close connections.

**Picture it**
A shop at closing time. Lock the front door, let the people inside finish paying, then turn off the lights.

**🗣️ Say this in the interview**
> "When Kubernetes wants to stop a pod, it sends SIGTERM, waits a grace period, 30 seconds by default, then sends SIGKILL. In that window I want to:
> one, mark the app **not ready**, so the load balancer stops sending new traffic.
> Two, **stop accepting** new connections.
> Three, let **in-flight requests finish**.
> Four, close DB pools, queue consumers and Redis.
> Five, exit, with a hard timeout as a safety net, so it never hangs."

```javascript
let shuttingDown = false;
// Readiness check: return 503 while shutting down, so no new traffic comes
app.get('/ready', (req, res) => res.sendStatus(shuttingDown ? 503 : 200));

const server = app.listen(PORT);

process.on('SIGTERM', () => {
  shuttingDown = true;
  server.close(async () => {            // stop new connections, wait for running requests
    await Promise.allSettled([db.end(), consumer.close(), redis.quit()]);
    process.exit(0);
  });
  setTimeout(() => process.exit(1), 25_000).unref(); // safety net: never hang forever
});
```

**They may also ask**
- *What if shutdown takes longer than the grace period?* Kubernetes sends SIGKILL and running work is lost. That's why queue consumers must be idempotent (safe to run twice), and messages are only acknowledged after processing.
- *Senior detail:* Removing the pod from the Service happens **at the same time** as SIGTERM, so a few new requests can still arrive. A short `preStop` sleep (a few seconds) stops you dropping them.

**⚠️ Avoid saying**
"I just call `process.exit()` on SIGTERM." That drops every running request.

---

## 14. How do you find a memory leak in production?

**In simple words**
A **memory leak** is memory that keeps growing and never comes back down, because something still holds a reference to old objects. You compare **heap snapshots** (photos of memory) taken a few minutes apart and see what keeps growing.

**🗣️ Say this in the interview**
> "First I confirm it's really a leak: memory that keeps climbing **after garbage collection** and never comes down, not just a busy period. Then I check if it lines up with a deploy, an endpoint, or a traffic pattern.
> To find the cause, I take two or three **heap snapshots** some minutes apart, on staging or one isolated instance, and compare them. Whatever object type keeps growing, I follow its **retainer path**, meaning what is holding on to it. Usual causes: an in-memory cache with no limit, listeners added per request, timers never cleared, or a global array.
> Then I fix whatever holds the reference, load-test, and check the memory graph goes flat."

**They may also ask**
- *RSS vs heapUsed?* `heapUsed` is JS objects. RSS is the total process memory, including Buffers, native code and the heap. If RSS grows but the heap doesn't, look at Buffers or native modules.
- *Why does RSS stay high after traffic drops?* V8 doesn't always give memory back to the OS straight away. That's not always a leak.
- *Quick fix?* Rolling restarts, or a memory limit so the pod restarts. That buys time. It's not a real fix.

**⚠️ Avoid saying**
"Memory is high, so it's a leak." Check it keeps growing after GC first.

---

## 15. How do you investigate 100% CPU?

**In simple words**
First find out how wide the problem is. Then take a **CPU profile**, which shows a **flame graph** (a picture of which functions use the most time). Fix the cause, don't just add servers.

**🗣️ Say this in the interview**
> "First I scope it: one instance or all? Since a deploy? One endpoint or everything? Then I look at **event-loop lag** and take a **CPU profile**, with `--cpu-prof`, Chrome DevTools through `--inspect`, or the APM's profiler. The flame graph shows the hot function directly.
> Common causes: heavy JSON serialization, a bad regex, sync crypto or compression, big loops, or **garbage collection** working too hard because memory is under pressure.
> I fix the cause first: optimize, cache, or move the work to a worker or a queue. Scaling out only hides it."

**⚠️ Avoid saying**
"I'd scale up the instances." Say you'd profile first, then fix the cause.

---

## 16. How do you do logging and observability?

**In simple words**
**Observability** means you can see what your system is doing from the outside. It has three parts: **logs** (what happened), **metrics** (how much and how often), and **traces** (where the time went across services).

**🗣️ Say this in the interview**
> "Three pillars. **Logs** tell me what happened. **Metrics** tell me how much and how often. **Traces** show where the time went across services.
> For logs: structured JSON, pino is my default in Node, consistent levels, a **request ID** (also called correlation ID) on every line, and never secrets or personal data.
> For metrics I watch the four golden signals: **latency (p95/p99), traffic, errors, and saturation**, meaning how full the system is. For Node I add event-loop lag, heap, and DB pool usage.
> For traces I use **OpenTelemetry**, so I can follow one request from the gateway through every service and query."

```javascript
import { AsyncLocalStorage } from 'node:async_hooks';
const als = new AsyncLocalStorage();

// Give every request an ID, and make it available everywhere in that request
app.use((req, res, next) => {
  const requestId = req.get('x-request-id') ?? crypto.randomUUID();
  res.set('x-request-id', requestId);
  als.run({ requestId }, next);
});
// In any function later: logger.info({ requestId: als.getStore()?.requestId }, 'order created')
```

**They may also ask**
- *Why p95/p99 and not the average?* The average hides the slow tail. If 1 in 100 requests takes 8 seconds, the average still looks fine, but those users (often the biggest carts) are suffering.

**⚠️ Avoid saying**
"I use `console.log`." Say structured JSON logs with a request ID.

---

## 17. How do you handle configuration and environment variables?

**In simple words**
Build the app once, and run the same build everywhere. Only the config changes between environments. Check the config when the app starts, so a missing value fails fast.

**🗣️ Say this in the interview**
> "I follow the **twelve-factor** idea: the same build runs in every environment, and only the configuration changes, through environment variables or a secret store. I **validate config at startup** with a schema. So a missing `DATABASE_URL` crashes the app right away on deploy, not three hours later on the first request that needs it."

**⚠️ Avoid saying**
"We build a separate image for each environment." One build, different config.

---

## 18. What are your Node.js security basics?

**In simple words**
Security is a checklist, not one tool. Check all input, never trust the client, put limits on everything, and keep dependencies and secrets safe.

**🗣️ Say this in the interview**
> "My checklist: validate every input with a schema. Parameterized queries. Authorization on every resource, on the server. `helmet` for security headers. Body-size limits. Rate limiting. **Timeouts on every outbound call**. `npm audit` or Dependabot in CI. A lockfile and `npm ci`. Secrets in a vault, not in code. Run as a non-root user. And never pass user input into a shell or `eval`."

**They may also ask**
- *Prototype pollution?* Merging untrusted JSON into objects can set `__proto__` and change how every object behaves. Validate input, avoid unsafe deep-merge, or use `Object.create(null)` maps.
- *ReDoS?* A regex with nested repeats (like `(a+)+`) can take a very long time on crafted input, which blocks the event loop. Keep patterns simple and limit input length.
- *Supply chain?* Pin versions, review new dependencies, use `npm ci`, and think about disabling install scripts.

**⚠️ Avoid saying**
"Validation protects us from SQL injection." Validation checks shape. Parameterized queries stop injection.

---

## 19. What's new in modern Node.js?

**In simple words**
This question checks if you keep up. Name a few built-in features that replace old packages.

**🗣️ Say this in the interview**
> "A few things I use in recent LTS versions. **Built-in `fetch`**, so I don't need axios for simple calls. The **built-in test runner**, `node:test`. **`--watch`** mode instead of nodemon. **`--env-file`** instead of dotenv. Recent versions can also **run TypeScript files directly** by stripping the types, which is great for scripts, though I still type-check in CI. And `require()` can now load most ES modules, which makes moving from CommonJS to ESM much less painful."

**⚠️ Avoid saying**
"Node runs TypeScript now, so we don't need `tsc`." It strips types, it doesn't check them. Still type-check in CI.

---

## Quick questions
- **`npm ci` vs `npm install`?** `ci` installs exactly from the lockfile, fails if it's out of sync, and is faster. Use it in CI and Docker.
- **`^1.2.3` vs `~1.2.3`?** Caret allows minor and patch updates. Tilde allows patch only.
- **`dependencies` vs `devDependencies` vs `peerDependencies`?** Needed at runtime / only for build and test / "the host app must provide this" (like React for a component library).
- **What is PM2?** A process manager: restarts, cluster mode, logs. Less needed with Docker and Kubernetes.
- **How do you debug Node?** `node --inspect`, then Chrome DevTools or VS Code. In production: logs, traces, profiles.
- **`require` caching?** A module is cached after the first load, so it acts like a singleton (one shared copy).
- **Single thread, so is Node bad for many users?** No. It handles many concurrent I/O requests well. It's bad at heavy CPU work per request.
- **Node vs Deno vs Bun?** Deno and Bun are newer runtimes with built-in TypeScript and tools. Node has the biggest ecosystem and is the most mature.
- **What is `AbortController`?** The standard way to cancel `fetch`, timers and streams. Pass its `signal` down your call chain.

## Remember these 10 things
1. Node = V8 + libuv. One JS thread, I/O in the background. Great for I/O, bad for heavy CPU.
2. Event loop phases: timers → pending → poll → check → close. nextTick, then Promises, run after each callback.
3. One slow callback makes every user wait.
4. Thread pool (default 4): files, crypto, zlib, `dns.lookup`. Not network.
5. Blocked loop: measure event-loop lag, take a CPU profile, then shrink / async / worker / queue.
6. Use `stream.pipeline()`: it handles backpressure, errors and cleanup.
7. Worker threads for CPU, child processes for other programs, one process per pod in Kubernetes.
8. Never put user input into `exec`. Use `execFile` or `spawn` with an args array.
9. Graceful shutdown: not ready → stop new connections → finish requests → close pools → exit with a timeout.
10. Memory leak: compare heap snapshots, follow the retainer path. Watch p95/p99, not the average.
