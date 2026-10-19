# Behavioral and HR Interview

> **Time needed:** about 2 hours · **Questions:** 19
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

**The STAR method** (use it for every "tell me about a time…" question)
- **S**ituation: 1–2 sentences of background.
- **T**ask: what *you* were responsible for.
- **A**ction: most of the answer. Say "**I**", give specifics.
- **R**esult: numbers you can defend, plus what you learned.

Aim for **60–90 seconds** per story. Your stories are in `1-interview-basics/02-your-story.md`.

## Progress checklist
- [ ] 1. Tell me about yourself
- [ ] 2. Why are you looking for a change?
- [ ] 3. Why this company?
- [ ] 4. Why should we hire you?
- [ ] 5. What's your biggest technical challenge?
- [ ] 6. Tell me about a conflict with a teammate
- [ ] 7. Tell me about a mistake you made
- [ ] 8. Tell me about a time you showed leadership
- [ ] 9. Strengths and weaknesses
- [ ] 10. Working under pressure / a tight deadline
- [ ] 11. A time you disagreed with your manager
- [ ] 12. Where do you see yourself in five years?
- [ ] 13. What are your salary expectations?
- [ ] 14. Notice period / when can you start?
- [ ] 15. Do you have any questions for us?
- [ ] 16. How do you handle feedback or criticism?
- [ ] 17. How do you prioritize when everything is urgent?
- [ ] 18. What do you do when you're stuck?
- [ ] 19. Closing statement ("anything else?")

---

## 1. Tell me about yourself

**In simple words**
This is almost always the first question. The full script (75 seconds) and the short script (30 seconds) are in `1-interview-basics/02-your-story.md`, item 1.

**⚠️ Avoid saying**
Your whole CV in order. Keep it short and end with why you want this role.

---

## 2. Why are you looking for a change?

**In simple words**
They want to know you are moving *toward* something, not running away.

**🗣️ Say this in the interview**
> "I've learned a lot in my current role, especially going deep on headless commerce, GraphQL and Next.js performance, and I'm not running away from anything. I'm looking for **broader scope**: owning larger systems end to end, working at a **bigger scale**, and being in a team with strong engineering practices where I can keep growing toward architecture and technical leadership. This role looks like exactly that step."

**⚠️ Avoid saying**
Anything bad about your current employer, manager or team, even if they ask directly. Talk about what you're moving **toward**.

---

## 3. Why this company?

**In simple words**
This is a template. You must research the company before every interview and fill in the blanks.

**🗣️ Say this in the interview**
> "Three things. First, **[their product / domain]**: [one specific thing you really find interesting, e.g. their scale, a technical blog post, their market in the region]. Second, the **technical challenges**: from the job description it's [Node/TypeScript/React at scale / event-driven systems / etc.], which is where I've been focusing. And third, **[culture / growth signal]**: [e.g. engineering blog, open-source work, how they described the team]."

📝 **Check before every interview:** their product, recent news, tech blog, the top 3 requirements in the job description, and the interviewer's LinkedIn.

**⚠️ Avoid saying**
"You're a big, stable company." That fits every company. Say one specific thing about *them*.

---

## 4. Why should we hire you?

**In simple words**
Give three clear strengths and one proof.

**🗣️ Say this in the interview**
> "I bring three things together. **Depth in production e-commerce**: about ten years of real traffic, real money, and real data problems. **Modern full stack skills**: Node.js, TypeScript, GraphQL, React and Next.js, which I use every day. And **production ownership**: I don't stop at 'it works on my machine'. I measure, I find root causes, and I make sure things stay fixed. For example, I [one-line S1 or S2 result]. I think that mix lets me contribute quickly and also raise the bar around me."

Example line to fill in: "…I took a product page from about 4 MB to 280 KB" (S1) or "…cut a listing from 1,811 database queries to 238" (S2).

**⚠️ Avoid saying**
"I'm hard-working and a fast learner." Everyone says that. Give a real result.

---

## 5. What's your biggest technical challenge?

**In simple words**
Use **S1** (the 4 MB product page) or **S2** (1,811 → 238 queries). Both are in `1-interview-basics/02-your-story.md`.
Practise S1 out loud until it's smooth.

**⚠️ Avoid saying**
A story where you can't explain the details or the numbers. They *will* ask follow-ups.

---

## 6. Tell me about a conflict with a teammate

**In simple words**
Use this shape: a **technical** disagreement → you looked for **evidence** → a **decision** → you **supported** it.
Replace the example below with your real story **S10** (`1-interview-basics/02-your-story.md`).

**🗣️ Say this in the interview** (example shape)
> "A colleague and I disagreed on how to handle [e.g. caching for product pages]. They wanted [approach A], I preferred [approach B] because of [concern]. Instead of arguing in the PR, I suggested we each list the trade-offs and **test both quickly**. We did a small spike and measured [metric]. The data showed [result], so we went with [decision]. It wasn't my original idea in the end, but I backed it fully, and we documented the decision so it wasn't re-debated later. What I took from it: disagreements go much better when they're about **evidence**, not opinions."

**⚠️ Avoid saying**
A story where you were simply right and they were simply wrong. Never a personality clash.

---

## 7. Tell me about a mistake you made

**In simple words**
Use this shape: own it → the impact → how you fixed it fast → **what you changed so it can't happen again**.
Use **S6** if it was your change, or **S13** (`1-interview-basics/02-your-story.md`).

**🗣️ Say this in the interview** (example shape, based on S6)
> "I made a change to [the GraphQL schema] that looked harmless, just adding documentation. It caused [every GraphQL query to fail in that environment]. I spotted it [quickly / through monitoring], reverted, and then dug in, because the error made no sense. The framework's parser couldn't handle a curly brace inside a description string. I fixed it, wrote it down for the team, and since then I always run a **smoke query against the whole endpoint**, not just the field I changed, before I push a schema change. The lesson: **small changes can have global effects**, so verify broadly."

(A **smoke query** is a quick, simple test query to check the whole thing still works.)

**⚠️ Avoid saying**
"I work too hard." That is not a mistake. Pick a **real** mistake with **recoverable** impact. Be honest about where it happened: local, staging, or production.

---

## 8. Tell me about a time you showed leadership

**In simple words**
Leadership is not a job title. It's taking ownership and influencing others.

**🗣️ Say this in the interview**
> "Leadership for me is **ownership and influence**, not a title."

Then tell one story (all in `1-interview-basics/02-your-story.md`):
- **S9** (your mentoring/leadership story), or
- **S4**: you challenged the existing approach, proved it with a dry run, and changed how the team handled it, or
- **S5**: you coordinated a fix with another team.

**⚠️ Avoid saying**
"I haven't been a team lead, so I don't have one." Ownership counts as leadership.

---

## 9. Strengths and weaknesses

**In simple words**
Strength: one real strength with proof. Weakness: a real one that isn't critical for the role, plus what you're doing about it.

**🗣️ Say this in the interview: strength**
> "Structured problem-solving across the whole stack. When something's wrong, I measure before guessing, find the real cause, and fix it in a way that stays fixed, whether it's in the database, the API, or the frontend. My product page optimization is a good example: the answer wasn't a React trick, it was noticing what we were sending."

**🗣️ Say this in the interview: weakness**
> "I tend to go deep on technical detail, and early in my career I'd explain things in too much depth to non-technical people. Now I **lead with the conclusion** ('we can ship Friday, with one risk') and add detail only if they want it. It's made my communication with product and management much better."

**⚠️ Avoid saying**
A fake weakness ("I'm a perfectionist") or one that is critical for the job.

---

## 10. Working under pressure / a tight deadline

**In simple words**
Use your story **S11** (`1-interview-basics/02-your-story.md`). Make sure you hit these 4 points:
- You **prioritized** (what is really essential).
- You **told people about the risk early**.
- You **did not cut** security or data integrity.
- You **followed up** on the shortcuts afterwards.

**⚠️ Avoid saying**
"I just worked all weekend." That shows effort, not judgment.

---

## 11. A time you disagreed with your manager

**In simple words**
Raise it privately, with evidence. If they still decide differently, commit to it.

**🗣️ Say this in the interview**
> "I raise it **privately**, with **evidence** and a proposed alternative. Not 'this is wrong' but 'here's a risk I see and here's another option.' If they still decide differently after hearing me out, I commit to the decision and make it work, because they may have context I don't. **Disagree and commit.** The only exception would be something unethical or genuinely unsafe for users or data, and then I'd escalate properly."

**From my work**
If you have a real example, use it. For example: proposing the root-cause fix instead of the cron in Story S4 (`1-interview-basics/02-your-story.md`).

**⚠️ Avoid saying**
"I just do what my manager says." Or the opposite: "I went ahead with my way anyway."

---

## 12. Where do you see yourself in five years?

**🗣️ Say this in the interview**
> "Growing toward a **staff or architect-level role**: someone who designs systems across teams, makes good technical decisions with the business, and helps other engineers grow. I still want to stay hands-on, because I think the best technical leaders still write code. I'd love to do that growth here, which is part of why this role appeals to me."

**⚠️ Avoid saying**
"Running my own company" or "in your job". It sounds like you'll leave soon.

---

## 13. What are your salary expectations?

**In simple words**
Try to let them say a number first. If they insist, give a range.

**🗣️ Say this in the interview: first try**
> "I'd like to understand the full scope of the role first. Could you share the budgeted range for this position?"

**🗣️ Say this in the interview: if they insist**
> "Based on my experience and the market for senior full stack roles in the UAE, I'm looking at a total monthly package in the range of **[X–Y AED]**. I'm flexible depending on the overall package: housing, bonus, leave, and growth opportunities."

📝 **Prepare before the call:**
- Research the market range (LinkedIn, recruiters, friends in the industry).
- Decide your **walk-away number** (the lowest you would accept).
- Give a **range** where even the bottom number makes you happy.

**They may also ask**
- *What is your current salary?* You can say: "I'd prefer to focus on the value I'd bring to this role and the market rate for it."

**⚠️ Avoid saying**
A single number with no range, or a range whose bottom you would not really accept.

---

## 14. What's your notice period / when can you start?

**🗣️ Say this in the interview**
> "My notice period is **[X days]**. I'd like to leave my current team properly, with a clean handover, but I'm open to discussing an earlier date if [my employer agrees / it's possible]."

**⚠️ Avoid saying**
"I can leave tomorrow." It makes them wonder if you'd do the same to them.

---

## 15. Do you have any questions for us?

**In simple words**
**Always** ask 2–3 questions. "No questions" sounds like low interest. Choose based on who is interviewing you.

**To an engineer / tech lead**
- "What does a typical week look like for someone in this role?"
- "What's the biggest technical challenge the team is facing right now?"
- "How do you deploy? How often, and how do you handle rollbacks?"
- "How are architecture decisions made and documented?"
- "What does the on-call rotation look like?"

**To a manager**
- "What would success look like in the first 90 days?"
- "How is the work split between backend and frontend?"
- "How do you support growth toward senior or staff roles here?"
- "What's the team's biggest priority this year?"

**To HR**
- "What's the interview process from here, and the timeline?"
- "How would you describe the team culture?"

**Closing question (bold, but it works)**
- "Is there anything in my background that makes you hesitate? I'd be glad to address it."

**⚠️ Avoid saying**
"No, I think you covered everything." Always have questions ready.

---

## 16. How do you handle feedback or criticism?

**🗣️ Say this in the interview**
> "I try to treat feedback as information, not as an attack. I listen fully first, ask questions to understand the specific example, and if it's fair, I act on it and follow up later to check I've improved. For example, [real example: feedback on PR size, communication, estimates…]. If I disagree, I say so respectfully with my reasoning, but I start from the assumption that the other person sees something I don't."

📝 Fill in one real example of feedback you got and what you changed.

**⚠️ Avoid saying**
"I don't really get negative feedback." Nobody believes that.

---

## 17. How do you prioritize when everything is urgent?

**🗣️ Say this in the interview**
> "I make the trade-offs **visible** instead of silently trying to do everything. I rank by **impact and urgency**: production issues affecting customers or revenue first, then commitments others depend on, then the rest. When two things truly conflict, I go to my manager or product owner with the options and the consequences ('if I do A today, B slips to Thursday'), and let them decide on the business priority."

**⚠️ Avoid saying**
"I just work harder and do it all." Show that you make choices and communicate them.

---

## 18. What do you do when you're stuck or don't know something?

**🗣️ Say this in the interview**
> "I time-box it. I give it a solid try first: read the error properly, check docs and the code, reproduce it in isolation, try a hypothesis. If I'm not making progress after a reasonable time, say an hour or two depending on urgency, I ask someone, and I come with what I've already tried. Staying stuck silently for two days is worse than asking early. And whatever I learn, I write down."

(**Time-box** = decide in advance how much time you'll spend before you ask for help.)

**⚠️ Avoid saying**
"I ask a senior right away." Or "I never ask for help." Show a balance.

---

## 19. Closing statement (when they ask "anything else?")

**🗣️ Say this in the interview**
> "Just that I'm genuinely excited about this role. The mix of [their tech/problem] and [their scale/product] lines up really well with what I've been doing, especially the headless commerce and performance work, and I'd love to bring that here. Thank you for the conversation."

**⚠️ Avoid saying**
"No, nothing." Use the chance to finish strong.

---

## Remember these 10 things
1. STAR: Situation → Task → **Action (most of it, in "I")** → Result. 60–90 seconds.
2. Leaving your job: talk about what you move **toward**. Never criticize your employer.
3. "Why this company": research every time. Say one specific thing about them.
4. Biggest challenge: S1 (4 MB → 280 KB) or S2 (1,811 → 238 queries).
5. Conflict: technical, evidence-based, and you supported the final decision.
6. Mistake: real, recoverable, honest about scope, and what you changed after.
7. Disagree with manager: privately, with evidence, then **disagree and commit**.
8. Salary: let them go first. If not, give a range in AED whose bottom you're happy with.
9. Always ask 2–3 questions at the end.
10. Fill in the [blanks] in questions 3, 4, 6, 13, 14 and 16 before the interview.
