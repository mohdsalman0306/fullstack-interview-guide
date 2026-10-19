# How to Answer Interview Questions

> **Time needed:** about 1 hour · **Sections:** 5
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

## Progress checklist
- [ ] 1. The 4-step answer: What → How → Example → Trade-off
- [ ] 2. The 8 golden rules
- [ ] 3. Phrase bank (useful sentences)
- [ ] 4. Red flags that make a senior sound junior
- [ ] 5. Top 30 questions to prepare first

---

## 1. The 4-step answer: What → How → Example → Trade-off

**In simple words**
This is the most important page of all the study files. Use it for every technical answer.
Four short steps, then stop.

| Step | What you say | Length |
|---|---|---|
| **What** | One plain sentence. No jargon. | 1 sentence |
| **How** | How it works inside. | 2–3 sentences |
| **Example** | Where you used it, or a real case. | 1–2 sentences |
| **Trade-off** | The downside, or when *not* to use it. | 1 sentence |

Then **stop**. Let the interviewer choose the next question.
A short answer that invites a follow-up is better than a three-minute lecture.

**Picture it**
Think of a movie trailer. It shows the best parts and makes people want more. It does not show the whole film.

### Same answer, two ways

**Question: "What is a closure?"**

❌ **Textbook (sounds like reading a book):**
"A closure is the combination of a function bundled together with references to its surrounding state, the lexical environment..."

✅ **Spoken (how you should sound):**

**🗣️ Say this in the interview**
> "A closure is when a function **remembers the variables from the place it was created**. *(What)*
> So even after the outer function has finished, the inner function can still read and change those variables. *(How)*
> I use it all the time. Debounce is a closure, because it keeps the timer between calls. React hooks rely on it too. *(Example)*
> The one thing to watch is memory. If a closure holds a big object, that object can't be garbage-collected (cleaned from memory). *(Trade-off)*"

That takes about 25 seconds. And the interviewer now has three things to ask about.

**⚠️ Avoid saying**
A long definition with no example and no trade-off. Always finish with "where I used it" and "the downside".

---

## 2. The 8 golden rules

1. **Answer first, explain second.** Your first sentence should already answer the question.
2. **Keep the first answer under 45 seconds.** Then pause. Silence is fine. It is their turn.
3. **Say "I" for your own work.** "We" hides what you did.
4. **Only use numbers you can defend.** If they ask "how did you measure that?", you need an answer.
5. **Always mention a trade-off.** Seniors say: "It depends on X. In our case it was Y, so I chose Z."
6. **If you don't know, say so. Then think out loud.** Bluffing (pretending to know) loses trust fastest.
7. **In scenario questions, never guess the cause.** Measure → isolate (find the exact part) → fix → verify.
8. **In coding and design rounds, think out loud.** A silent candidate is impossible to score.

---

## 3. Phrase bank (useful sentences)

Learn a few of these by heart. Fill in the blanks with your own words.

**Starting an answer**
- "In one line: ___. The reason it matters is ___."
- "There are two parts to this, ___ and ___. Let me start with ___."
- "The short answer is ___. Want me to go into how it works?"

**Buying a few seconds**
- "Good question. Let me think about that for a second."
- "Just to make sure I answer the right thing, are you asking about ___ or ___?"

**When you don't know**
- "I haven't used that directly, but here's how I'd reason about it…"
- "I don't remember the exact API, but the idea is ___."
- "I don't know that one. If it came up at work, I'd start by checking ___."

**Sounding senior**
- "It depends on the access pattern. If it's read-heavy, ___. If it's write-heavy, ___."
- "Before changing anything, I'd want to measure ___."
- "The simplest thing that works is ___. I'd only add ___ if we actually hit ___."
- "The risk with that approach is ___, so I'd add ___ to protect against it."

**Disagreeing politely** (design rounds, code review questions)
- "That would work. One thing I'd watch out for is ___."
- "I'd lean the other way, because ___. But if ___ matters more, your approach is better."

**Wrapping up**
- "So to summarize: ___. Happy to go deeper on any part."
- "Does that answer it, or would you like me to go into ___?"

---

## 4. Red flags that make a senior sound junior

Don't do these:
- Giving a definition but never saying **when you'd use it**.
- Saying "X is always better." That sentence has no trade-off.
- Jumping straight to a fix in a scenario, without saying how you'd **confirm the cause**.
- Criticizing your current or past employer.
- Bluffing an API detail. Interviewers usually know.
- Saying "we" for everything, so they can't tell what *you* did.
- Giving a three-minute answer to a 20-second question.
- Never asking a clarifying question in system design.

---

## 5. Top 30 questions to prepare first

If you only have one evening, prepare these. Paths are inside the `fullstack/` folder.

| # | Question | Where |
|---|---|---|
| 1 | Tell me about yourself | `1-interview-basics/02-your-story.md` |
| 2 | Your biggest technical challenge / performance win | `1-interview-basics/02-your-story.md` (Stories S1, S2) |
| 3 | Explain the event loop (JS and Node) | `2-javascript/01-javascript.md`, `3-backend/01-nodejs.md` |
| 4 | Closures, and a practical use | `2-javascript/01-javascript.md` |
| 5 | Promises, async/await, `Promise.all` vs `allSettled` | `2-javascript/01-javascript.md` |
| 6 | Microtask vs macrotask, plus an output puzzle | `2-javascript/01-javascript.md` |
| 7 | How do you scale a Node.js API? | `3-backend/01-nodejs.md` |
| 8 | Streams and backpressure | `3-backend/01-nodejs.md` |
| 9 | REST vs GraphQL, and when to pick each | `3-backend/03-graphql.md` |
| 10 | The N+1 problem and DataLoader | `3-backend/03-graphql.md` |
| 11 | JWT, refresh tokens, and where to store them | `3-backend/04-security-auth.md` |
| 12 | XSS vs CSRF | `3-backend/04-security-auth.md` |
| 13 | What causes a React re-render? How do you stop extra ones? | `4-frontend/02-react.md` |
| 14 | When should you *not* use `useEffect`? | `4-frontend/02-react.md` |
| 15 | SSR vs SSG vs ISR vs CSR; Server vs Client Components | `4-frontend/03-nextjs.md` |
| 16 | How do you optimize a slow SQL query? | `3-backend/05-databases.md` |
| 17 | Indexes: composite, covering, when they don't help | `3-backend/05-databases.md` |
| 18 | Transactions and isolation levels | `3-backend/05-databases.md` |
| 19 | Caching strategy, invalidation, stampede | `3-backend/06-redis-queues.md` |
| 20 | Queues, at-least-once delivery, idempotent consumers, DLQ | `3-backend/06-redis-queues.md` |
| 21 | Idempotency keys for payments/orders | `3-backend/02-rest-api-express.md` |
| 22 | Docker best practices; liveness vs readiness | `5-devops-cloud/01-docker-kubernetes-cicd.md` |
| 23 | Zero-downtime deploys and DB migrations | `5-devops-cloud/01-docker-kubernetes-cicd.md`, `3-backend/05-databases.md` |
| 24 | Design an e-commerce checkout / flash sale | `6-system-design/01-system-design.md` |
| 25 | Design a URL shortener or rate limiter | `6-system-design/01-system-design.md` |
| 26 | "API latency jumped from 200 ms to 8 s." What do you do? | `6-system-design/02-production-troubleshooting.md` |
| 27 | "Memory keeps growing in production." | `6-system-design/02-production-troubleshooting.md` |
| 28 | Conflict with a teammate | `1-interview-basics/03-behavioral-hr.md` |
| 29 | A mistake you made | `1-interview-basics/03-behavioral-hr.md` |
| 30 | Why are you leaving? Salary? Questions for us? | `1-interview-basics/03-behavioral-hr.md` |

---

## Remember these 10 things
1. Every answer: **What → How → Example → Trade-off**. Then stop.
2. First sentence = the answer.
3. First answer under 45 seconds.
4. Say "I", not "we", for your own work.
5. Only numbers you can explain and defend.
6. Always name a downside or "when not to use it".
7. Don't know? Say so, then reason out loud. Never bluff.
8. Scenarios: measure → isolate → fix → verify. Never guess.
9. Think out loud in coding and design rounds.
10. Never criticize a past or current employer.
