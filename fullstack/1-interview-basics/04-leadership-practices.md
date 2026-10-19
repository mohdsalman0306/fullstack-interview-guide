# Engineering Leadership and Practices

> **Time needed:** about 1.5 hours · **Questions:** 15
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

Senior roles are judged on **how you work with others** and **how you make decisions**, not only on technical depth. Expect at least a few of these.

## Progress checklist
- [ ] 1. How do you do code reviews?
- [ ] 2. How do you handle technical debt?
- [ ] 3. How do you estimate work?
- [ ] 4. How do you make architecture decisions?
- [ ] 5. How do you mentor junior developers?
- [ ] 6. Product wants it by Friday and it's not possible
- [ ] 7. How do you keep quality high while moving fast?
- [ ] 8. How do you get up to speed on a new codebase?
- [ ] 9. Incidents, on-call, and post-mortems
- [ ] 10. Git workflow: trunk-based vs GitFlow, merge vs rebase
- [ ] 11. Feature flags
- [ ] 12. Monorepo vs polyrepo
- [ ] 13. How do you use AI tools in your work?
- [ ] 14. How do you stay up to date?
- [ ] 15. What's your definition of done?

---

## 1. How do you do code reviews?

**In simple words**
Check the most important things first: does it work, is it in the right place, is it safe. Style is for tools, not people.

**🗣️ Say this in the interview**
> "I review in order of importance. **Correctness** first: does it do what it should, including edge cases and errors? Then **design**: is it in the right place, is it maintainable? Then **security and performance**: input validation, authorization, N+1 queries, missing indexes. Then **tests**, and readability last. **Style** should be handled by Prettier and ESLint, not by humans.
> I keep comments **specific and kind**, and I mark **'must fix'** separately from **'suggestion'** or **'nit'** (a tiny, optional point), so the author knows what's blocking. I ask questions instead of giving orders: 'What happens if this list is empty?' And I try to review quickly, because a PR waiting two days slows the whole team.
> For my own PRs: **small and focused**, with a description of what, why, and how to test."

**⚠️ Avoid saying**
"I mostly check formatting and naming." That's the linter's job. Start with correctness.

---

## 2. How do you handle technical debt?

**In simple words**
Tech debt is like a loan. A little is fine if you chose it on purpose. The danger is debt nobody tracks.

**🗣️ Say this in the interview**
> "I treat tech debt like financial debt: some is fine if it's **deliberate** and we know the interest we're paying. The dangerous kind is debt nobody tracks.
> In practice, I make it **visible**: tickets with the real cost, like 'this slows every checkout change by about two days'. And I tie paying it down to **business value**, so the conversation with product is about speed and risk, not beautiful code. I prefer **continuous small improvements** over big rewrites: the boy-scout rule (leave the code a bit cleaner than you found it), plus some capacity each sprint. And I fix debt **in the area we're already changing**, where it pays back immediately."

**From my work**
Use Story S4 (see `1-interview-basics/02-your-story.md`): "The URL-regeneration cron was debt. It ran constantly and fixed about 2% of the problem. Removing it and fixing the root cause was both cheaper and more correct."

**⚠️ Avoid saying**
"We should stop and rewrite it all." Big rewrites are risky. Prefer small, steady fixes.

---

## 3. How do you estimate work?

**In simple words**
Break it into small pieces. Give a range, not one number. Tell people early if things change.

**🗣️ Say this in the interview**
> "First I **break it down** into small pieces. Big tasks are where estimates go wrong. I estimate each piece, and I add time for the things people forget: testing, code review, deployment, edge cases, and working with other teams. If there's a big **unknown**, I suggest a short time-boxed **spike** first (a small, fixed-time experiment to learn the answer) instead of guessing. I give **ranges** rather than single numbers ('3 to 5 days, depending on whether the payment API supports X'), and I **update early** if something changes, not on the deadline day."

**⚠️ Avoid saying**
One exact number for a big unknown task. Give a range and name the risk.

---

## 4. How do you make architecture decisions?

**In simple words**
Write the options down, get feedback before building, and record the decision so nobody has to guess later.

**🗣️ Say this in the interview**
> "For big decisions I write a short **design doc or RFC** (a 'request for comments' document): the problem, the constraints, **two or three options** with trade-offs, and a recommendation. I share it for feedback **before** building. Once we decide, I record it as an **ADR**, an Architecture Decision Record: what we decided, why, and what we gave up. Six months later, nobody has to guess why we chose Kafka over SQS.
> I also try to keep decisions **reversible** where possible, and I don't spend a week debating something that's easy to change later."

**⚠️ Avoid saying**
"I just pick the best tool." There's no "best". Show options and trade-offs.

---

## 5. How do you mentor junior developers?

**In simple words**
Teach them how to think, not just the answer. Give them more ownership step by step.

**🗣️ Say this in the interview**
> "I try to **teach the thinking, not just give the answer**. When someone's stuck, I ask questions that lead them to it: 'What does the error say? Where would you look next?' I pair with them on tricky tasks, give **detailed, explained code reviews** (the *why*, not just 'change this'), and slowly give them **bigger ownership**, like leading a small feature end to end with me as a safety net. And I make it safe to ask questions. The worst outcome is a junior stuck for two days because they were afraid to ask."

**From my work**
📝 Fill in Story S9 with a real example (see `1-interview-basics/02-your-story.md`).

**⚠️ Avoid saying**
"I just fix it for them." That's faster today but they don't grow.

---

## 6. Product wants it by Friday and it's not possible. What do you do?

**In simple words**
Don't just say no. Give options, explain the risks, and let them choose.

**🗣️ Say this in the interview**
> "I don't just say no. I give **options**. First I make sure I understand what's really essential for Friday and why: a campaign, a client promise. Then I come back with choices like: 'We can ship **A and B** by Friday, and C next week', or 'We can ship all of it by Friday if we skip the admin screen and manage it by config for now', or 'Full scope, done safely, is Wednesday.' I'm clear about **risks**, and there are things I won't cut, like security, data integrity, and tests on payment logic. Then **they** choose with full information, and I commit to it."

**⚠️ Avoid saying**
"It's impossible." Or the opposite: "Sure, no problem," and then missing the date.

---

## 7. How do you keep quality high while moving fast?

**In simple words**
Automate the safety net. Then going fast is safe, because mistakes are caught early and are easy to undo.

**🗣️ Say this in the interview**
> "Speed and quality aren't opposites if the **safety net** is automated: types, linting, a solid test suite on the critical paths, CI that runs on every PR, **small PRs**, feature flags so unfinished work can ship hidden, canary deploys (release to a small part of traffic first) with fast rollback, and good monitoring. Those let us move fast **because** mistakes are caught early and cheap to undo. What actually slows teams down is fear of deploying."

**⚠️ Avoid saying**
"Sometimes you have to skip tests to go fast." That makes you slower later.

---

## 8. How do you get up to speed on a new codebase?

**In simple words**
Understand before you change. Follow one real request from start to end. Ship something small early.

**🗣️ Say this in the interview**
> "In the first weeks I focus on **understanding before changing**. I get it running locally, read the architecture docs and ADRs, and **follow one real request end to end**, from the UI to the database and back. I pick up **small real tickets** early, because shipping something small teaches you the deploy pipeline, reviews and conventions. I ask a lot of questions and **write down what I learn**, which usually turns into onboarding docs for the next person. And I look at the dashboards and recent incidents to understand where the system actually hurts."

**⚠️ Avoid saying**
"I'd start by refactoring the parts I don't like." Understand first.

---

## 9. Incidents, on-call, and post-mortems

**In simple words**
During an incident: stop the damage first, then find the cause. After: a blameless review with clear actions.

**🗣️ Say this in the interview**
> "During an incident: **mitigate first** (stop the damage, for example roll back), and communicate clearly and often. Even 'no update yet, still investigating' is an update. One person coordinates while others investigate, and we keep a timeline.
> After: a **blameless post-mortem**. The question isn't 'who made the mistake?' but 'why did our system **allow** it, and why didn't we **catch** it sooner?' Every post-mortem ends with **concrete action items with owners and dates**: a test, an alert, a guard rail. Otherwise the same incident comes back."

**From my work**
Use your Story S13 (production incident) if you've filled it in (see `1-interview-basics/02-your-story.md`).

**⚠️ Avoid saying**
"First I find who caused it." Blame makes people hide problems.

---

## 10. Git workflow: trunk-based vs GitFlow, merge vs rebase

**In simple words**
Trunk-based = small branches merged to main often. GitFlow = long-lived branches for planned releases.

**🗣️ Say this in the interview**
> "I prefer **trunk-based development**: short-lived feature branches, a day or two, merged to main often, with **feature flags** for unfinished work. It avoids painful long-lived branches and big-bang merges. GitFlow, with long-lived develop and release branches, suits products with scheduled releases or several supported versions.
> On merge vs rebase: I **rebase my own feature branch** to keep it up to date and clean, but I never rebase shared branches. For merging PRs, **squash merge** (turn the whole PR into one commit) keeps main readable, as long as PRs are small."

**They may also ask**
- *Why never rebase a shared branch?* Rebase rewrites history. Other people's copies no longer match, which causes confusing conflicts.

**⚠️ Avoid saying**
"Rebase is always better than merge." It depends on whether the branch is shared.

---

## 11. Feature flags

**In simple words**
A feature flag is an on/off switch in config. It lets you **deploy** code without **releasing** it to users.

**🗣️ Say this in the interview**
> "Feature flags separate **deploying** code from **releasing** it. I use them for gradual rollouts (internal users, then 10%, then everyone), instant kill switches during incidents, A/B tests, and shipping unfinished work safely. The discipline they need: each flag has an **owner and an expiry date**, and old flags get **removed**. A codebase full of dead flags becomes a maze of untested combinations."

**⚠️ Avoid saying**
Only the benefits. Mention the cost: old flags must be cleaned up.

---

## 12. Monorepo vs polyrepo

**In simple words**
Monorepo = many projects in one Git repo. Polyrepo = each project in its own repo.

**🗣️ Say this in the interview**
> "A **monorepo**, with Turborepo or Nx, makes **shared code and atomic changes** easy: I can change the API type and the frontend in one PR, with one toolchain. It needs good tooling for builds and CI so it doesn't get slow. **Polyrepos** give teams independence and clear ownership, but sharing code means publishing packages and coordinating versions. For a frontend, a BFF (a backend made just for that frontend) and shared types owned by one team, I'd lean monorepo."

**⚠️ Avoid saying**
"Monorepo is always better." It depends on team size and ownership.

---

## 13. How do you use AI tools in your work?

**In simple words**
Use AI to go faster, but you still understand and own every line that ships.

**🗣️ Say this in the interview**
> "I use AI assistants daily as an **accelerator**: exploring an unfamiliar codebase, drafting tests, generating boilerplate, brainstorming debugging ideas, writing documentation, and reviewing my own code for things I missed. But I treat the output like code from a new team member: I **read and understand every line**, check it against the requirements and the official docs, run the tests, and I'm fully accountable for what ships. It doesn't replace understanding. If I can't explain the code, it doesn't go in."

**⚠️ Avoid saying**
"I don't use AI." Or "AI writes most of my code." Show balance and ownership.

---

## 14. How do you stay up to date?

**🗣️ Say this in the interview**
> "A mix: release notes for the tools I use every day (Node, React, Next.js, TypeScript), a few good newsletters and engineering blogs, and **trying things hands-on** in small side projects or spikes at work, because reading about something isn't the same as using it. I'm also selective. I don't jump on every new library. I ask whether it solves a problem we actually have."

**⚠️ Avoid saying**
A long list of every new library you tried. Show you choose carefully.

---

## 15. What's your definition of done?

**In simple words**
Done = running in production and doing its job. Not just "code merged".

**🗣️ Say this in the interview**
> "Done means **running in production and doing its job**, not 'code merged'. That includes: tests for the important paths, reviewed, deployed, **monitoring or logs** so we'd notice if it broke, documentation updated if behaviour changed, the feature flag cleaned up if there was one, and checking with product that it actually solved the problem."

**⚠️ Avoid saying**
"When the PR is merged." That's only halfway.

---

## Remember these 10 things
1. Code review order: correctness → design → security/performance → tests → readability. Style = tools.
2. Mark comments as "must fix" vs "suggestion/nit". Ask questions, don't give orders.
3. Tech debt: make it visible, tie it to business cost, fix it in small steps.
4. Estimates: break down, add forgotten work, spike unknowns, give ranges, update early.
5. Big decisions: design doc / RFC before building, ADR after deciding.
6. Impossible deadline: give options, name risks, never cut security, data integrity or payment tests.
7. Speed + quality = automated safety net (CI, tests, flags, canary, rollback, monitoring).
8. Incidents: mitigate first, communicate often, blameless post-mortem with owners and dates.
9. Trunk-based + feature flags; rebase your own branch only; squash-merge small PRs.
10. Done = in production, monitored, and solving the real problem.
