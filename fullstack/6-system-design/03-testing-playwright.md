# Testing and Playwright

> **Time needed:** about 1.5 hours · **Questions:** 13 + 7 quick questions
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

## Progress checklist
- [ ] 1. Testing pyramid or trophy?
- [ ] 2. Unit testing Node.js services
- [ ] 3. Integration tests with a real database
- [ ] 4. Contract testing
- [ ] 5. What is Playwright, and why use it?
- [ ] 6. Locators and auto-waiting
- [ ] 7. Page Object Model
- [ ] 8. Login and test data in Playwright
- [ ] 9. Flaky E2E tests
- [ ] 10. API testing and network mocking with Playwright
- [ ] 11. What runs in CI, and when?
- [ ] 12. TDD and code coverage
- [ ] 13. Load and performance testing
- [ ] Quick questions

---

## 1. What is your testing strategy? Pyramid or trophy?

**In simple words**
There are three main kinds of tests. **Unit tests** check one small piece. **Integration tests** check that real pieces work together. **End-to-end (E2E) tests** click through the real app in a browser. The question is how many of each.

**Picture it**
Checking a car. Unit test = test each part on the bench. Integration test = test the engine with the gearbox. E2E test = drive the full car around the block.

- **Pyramid**: many fast unit tests, fewer integration tests, very few E2E tests.
- **Trophy**: puts the most weight on **integration tests**, because they give the most confidence per test. They test real pieces together, but they are not as slow and flaky as browser tests.

**🗣️ Say this in the interview**
> "The classic pyramid says many fast unit tests, fewer integration tests, and a small number of end-to-end tests. The testing trophy moves the weight toward integration tests, because they give the most confidence per test. My practical version: unit tests for pure logic like pricing, validation and utilities. Integration tests for API endpoints with a real database. And E2E tests only for the critical user journeys: search, add to cart, checkout, login. Plus static checks like TypeScript and ESLint, which catch a lot for free."

**⚠️ Avoid saying**
Don't say "we test everything end to end". E2E tests are slow and flaky; keep them for critical journeys.

---

## 2. How do you unit test a Node.js service?

**In simple words**
Test the **service layer** most. That's where the business rules live. Pass in fake versions of the things it depends on, and check the **result**, not how it works inside.

**🗣️ Say this in the interview**
> "I test the service layer most, because that's where the business rules live. I inject dependencies, like the repository or the payment client, so tests can pass in fakes. I check outcomes: the return value, errors thrown, what was saved. Not internal method calls. And I test edge cases and error paths, not just the happy path: out of stock, payment declined, duplicate request. Tools: Vitest or Jest, or the built-in node:test."

**They may also ask**
- *Mock vs stub vs fake?* A **stub** returns fixed answers. A **mock** also checks how it was called. A **fake** is a small working version, like an in-memory repository. I prefer fakes, because tests full of mock-call checks break on every refactor.
- *What is dependency injection?* You pass dependencies into a class or function instead of creating them inside. Then tests can pass in fakes.

**⚠️ Avoid saying**
Don't say "I check that method X was called 3 times". Check the outcome instead.

---

## 3. How do you do integration tests with a real database?

**In simple words**
For the API and the database layer, test against a **real database**, not a mock. Mocks can't catch wrong SQL, missing indexes or broken constraints.

**🗣️ Say this in the interview**
> "For the API and repository layer, I test against a real database, not a mock, because mocks don't catch wrong SQL, missing indexes or constraint violations. Testcontainers starts a throwaway Postgres or Redis in Docker for the test run. Each test runs in a transaction that's rolled back at the end, or uses uniquely-named data, so tests don't affect each other. I call the endpoints with Supertest and check both the response and what's in the database."

**⚠️ Avoid saying**
Don't say "I mock the database in every test". You miss real SQL bugs.

---

## 4. What is contract testing?

**In simple words**
When two teams build two sides of an API (frontend and backend, or two services), a **contract test** checks they still agree, without running both together.

**Picture it**
A plug and a socket. Each factory checks its part matches the agreed shape. Then they fit, even if they never meet before the shop.

**🗣️ Say this in the interview**
> "When a frontend and backend, or two services, are built by different teams, contract tests make sure they agree on the API without running everything together. With Pact, the consumer records what it expects, and the provider's CI checks it still meets those expectations. For GraphQL, schema checks in CI do a similar job. They fail the build if a change would break a known client query."

**⚠️ Avoid saying**
Don't say "we just run full E2E tests for that". It's slow and needs every service running.

---

## 5. What is Playwright, and why use it?

**In simple words**
**Playwright** is a tool from Microsoft that drives real browsers for **end-to-end tests**. It is popular because its tests are reliable and easy to debug.

**🗣️ Say this in the interview**
> "Playwright is a browser automation framework from Microsoft for end-to-end testing. It runs on Chromium, Firefox and WebKit, which is Safari's engine. What makes it reliable: auto-waiting, so it waits for elements to be visible, enabled and stable before acting. Web-first assertions that retry until they pass or time out. An isolated browser context for each test, which is fast and clean. Network interception for mocking, parallel runs, and the Trace Viewer. The trace records every step, DOM snapshot, network call and console log, so I can debug a CI failure as if I was there."

**They may also ask**
- *What is a browser context?* Like a fresh private window: its own cookies and storage, so tests don't share state.

**⚠️ Avoid saying**
Don't only say "it's like Selenium". Name auto-waiting and the Trace Viewer.

---

## 6. How do you find elements, and how does auto-waiting work?

**In simple words**
Find elements the way a **user** sees them: by role and label, not by CSS classes. And never use fixed sleeps. Playwright waits for the right moment by itself.

**🗣️ Say this in the interview**
> "I find elements the way a user would: getByRole first, then getByLabel, getByPlaceholder, getByText, and getByTestId only when there's no good user-facing option. I avoid CSS selectors tied to styling or DOM structure, because they break on every redesign. And I never use fixed sleeps like waitForTimeout(3000). They're slow when the app is fast and flaky when it's slow. Web-first assertions wait for exactly the right moment."

```typescript
import { test, expect } from '@playwright/test';

test('guest can add a product to the cart', async ({ page }) => {
  await page.goto('/p/thermostatic-shower-mixer');
  await page.getByRole('button', { name: 'Brushed Brass' }).click();
  await page.getByRole('button', { name: 'Add to cart' }).click();

  // These assertions retry until true (or time out)
  await expect(page.getByRole('status')).toContainText('Added to cart');
  await expect(page.getByTestId('cart-count')).toHaveText('1');
});
```

**⚠️ Avoid saying**
Don't use `waitForTimeout` to "fix" a flaky test.

---

## 7. What is the Page Object Model?

**In simple words**
A **page object** is a small class that holds the locators and actions for one page. Tests call readable methods like `productPage.selectFinish('Brass')`. When the UI changes, you fix one place instead of 40 tests.

**Picture it**
A TV remote. You press "volume up". You don't need to know the wires inside.

**🗣️ Say this in the interview**
> "A page object wraps the locators and actions for one page or component behind readable methods, like productPage.selectFinish or cart.checkout. When the UI changes, I fix it in one place instead of in 40 tests. Two rules: keep page objects small, one per page or component, not one giant class. And keep assertions in the tests, so each test still clearly shows what we're checking."

```typescript
export class ProductPage {
  constructor(private page: Page) {}
  goto(slug: string) { return this.page.goto(`/p/${slug}`); }
  selectFinish(name: string) { return this.page.getByRole('button', { name }).click(); }
  addToCart() { return this.page.getByRole('button', { name: 'Add to cart' }).click(); }
}
```

**⚠️ Avoid saying**
Don't put `expect` calls inside page objects. Keep assertions in the test.

---

## 8. How do you handle login and test data in Playwright?

**In simple words**
Log in **once**, save the browser state, and reuse it in every test. Create test data through the **API**, not by clicking through admin screens.

**🗣️ Say this in the interview**
> "I don't log in through the UI in every test. That's slow and repetitive. A setup project logs in once and saves the browser state, meaning cookies and localStorage, with storageState. Every other test starts already logged in. For test data, I create it through the API in a fixture before the test, and clean it up after, instead of clicking through the admin UI. That makes tests faster and independent of each other."

```typescript
// auth.setup.ts — runs once before the other tests
setup('authenticate', async ({ page }) => {
  await page.goto('/login');
  await page.getByLabel('Email').fill(process.env.E2E_USER!);
  await page.getByLabel('Password').fill(process.env.E2E_PASS!);
  await page.getByRole('button', { name: 'Sign in' }).click();
  await page.waitForURL('**/account');
  // Save cookies + localStorage to a file
  await page.context().storageState({ path: 'playwright/.auth/user.json' });
});
// playwright.config.ts → projects: [{ name: 'setup', testMatch: /.*\.setup\.ts/ },
//   { name: 'chromium', use: { storageState: 'playwright/.auth/user.json' }, dependencies: ['setup'] }]
```

**They may also ask**
- *What is a fixture?* Setup code Playwright runs for a test, like creating a user or a product, and cleaning up after.

**⚠️ Avoid saying**
Don't say every test logs in through the login form. It's slow and adds flakiness.

---

## 9. How do you deal with flaky E2E tests?

**In simple words**
A **flaky test** sometimes passes and sometimes fails, with no code change. Treat it as a **bug**, not bad luck.

**Common causes and fixes**
- **Timing**: fixed sleeps, or checking before the app is ready. → Use web-first assertions and wait for a real signal (a response, a visible element).
- **Shared data**: tests use the same user or cart. → Give each test **its own data**.
- **Order dependence**: tests only pass in a certain order. → Make each test fully independent.
- **Unstable selectors** → Use role- and label-based locators.
- **External services** (payment sandboxes, third-party scripts) → **Mock** them with `page.route` when they're not what you're testing.
- **Animations and time** → Turn off animations, and control the clock where needed.

**🗣️ Say this in the interview**
> "First, I treat flakiness as a bug, not bad luck. The usual causes are timing, shared data between tests, tests that depend on order, unstable selectors, and external services. The fixes: web-first assertions instead of sleeps, own data per test, independent tests, role-based locators, and mocking third parties with page.route. I use the Trace Viewer to see what really happened. Retries in CI are OK to detect flaky tests, because Playwright marks them as flaky, but not to hide them. Flaky tests get fixed, or quarantined with an owner."

**⚠️ Avoid saying**
Don't say "we just turn on retries". Retries find flaky tests; they don't fix them.

---

## 10. How do you do API testing and network mocking with Playwright?

**In simple words**
Playwright can call APIs directly with its **`request`** helper. It can also catch browser requests with **`page.route()`** and return fake responses.

**🗣️ Say this in the interview**
> "Playwright has a built-in request context for calling APIs directly. I use it for pure API tests, and to set up data for UI tests. And page.route intercepts browser requests, so I can mock a slow recommendations service, or force an error to check that the UI shows a proper error state."

```typescript
test('cart API adds an item', async ({ request }) => {
  const res = await request.post('/api/cart/items', { data: { sku: 'TAP-001', qty: 1 } });
  expect(res.status()).toBe(201);
});

test('shows fallback when recommendations fail', async ({ page }) => {
  // Make the recommendations API return a 500 error
  await page.route('**/api/recommendations*', route => route.fulfill({ status: 500 }));
  await page.goto('/p/basin-mixer');
  await expect(page.getByText('Recommendations unavailable')).toBeVisible();
});
```

**⚠️ Avoid saying**
Don't mock the thing you are actually testing. Mock only outside services.

---

## 11. What runs in CI, and when?

**In simple words**
Fast checks on every pull request. Slower, full checks after merge or every night.

**🗣️ Say this in the interview**
> "On every pull request: lint, type-check, unit tests, integration tests, a small E2E smoke suite on the most critical journeys, and a dependency security scan. It must be fast, ideally under 10 to 15 minutes, or people stop waiting for it.
> On merge or nightly: the full E2E suite across browsers, sharded (split across machines) to run in parallel, plus visual regression and accessibility checks.
> On failure: we keep the traces, screenshots and videos as CI artifacts, so nobody has to reproduce it locally just to see what happened."

**⚠️ Avoid saying**
Don't put the full cross-browser E2E suite on every PR. It makes CI too slow.

---

## 12. Do you do TDD? What coverage do you aim for?

**In simple words**
**TDD** (test-driven development) means you write the test first, then the code. It works best for clear business rules and for bug fixes. **Coverage** is the percentage of code your tests run.

**🗣️ Say this in the interview**
> "I use TDD where it fits best: business logic with clear rules, like pricing, discounts or validation, and bug fixes. For a bug, I first write a test that reproduces it, then fix it, so it can never come back. For exploratory UI work I usually write tests right after.
> On coverage, I don't chase a number. 80% coverage of the wrong things is worse than solid tests on the money paths. I care that critical paths and edge cases are covered. Coverage reports are useful to find untested areas, not as a target."

**⚠️ Avoid saying**
Don't say "we need 100% coverage". High coverage doesn't mean good tests.

---

## 13. How do you do load and performance testing?

**In simple words**
Before a big launch or sale, simulate many users at once. The goal is to learn **what breaks first, and at what load**.

**🗣️ Say this in the interview**
> "Before a big launch or sale, I run load tests with k6 or Artillery. I use realistic user journeys, ramp up to 2 to 3 times the expected peak, and run against a production-like environment. I watch p95 and p99 latency, the error rate, and what saturates first. Usually it's the database connections or a third-party API. The goal isn't a pass or fail number. It's knowing what breaks first and at what load, so we fix it or plan capacity before customers find it."

**They may also ask**
- *What are p95 and p99?* The response time that 95% (or 99%) of requests are faster than. They show the slow experience that averages hide.

**⚠️ Avoid saying**
Don't only look at average response time. Look at p95/p99 and errors.

---

## Quick questions
- **Playwright vs Cypress?** Playwright: many browsers including WebKit, multiple tabs, parallel by default, many languages. Cypress: great developer experience, runs inside the browser, historically focused on Chromium.
- **Playwright vs Selenium?** Playwright is faster and more modern, with auto-waiting and better tools. Selenium supports the most languages and older setups.
- **Visual regression?** `expect(page).toHaveScreenshot()` compares the page to a saved baseline image.
- **Testing Library idea?** Test the app the way users use it. The closer the test is to real use, the more you can trust it.
- **MSW?** Mock Service Worker: fakes the network at the request level, so the same mocks work in tests and in the browser.
- **Snapshot tests?** Fine for small, stable outputs. Big snapshots get approved without reading, so they stop catching anything.
- **Shift-left?** Catch problems earlier: types, linting and tests in the PR, not in QA or production.

## Remember these 10 things
1. Unit tests for pure logic, integration tests with a real DB, E2E only for critical journeys.
2. Check outcomes, not internal calls. Prefer fakes over heavy mocks.
3. Testcontainers gives a real throwaway Postgres/Redis for tests.
4. Contract tests (Pact, GraphQL schema checks) keep two teams' APIs in agreement.
5. Playwright: auto-waiting, web-first assertions, isolated contexts, Trace Viewer.
6. Locators: `getByRole` first, `getByTestId` last. Never fixed sleeps.
7. Page objects: small, one per page, no assertions inside.
8. Log in once with `storageState`; create test data through the API.
9. Flaky test = bug. Retries detect it; they don't fix it.
10. Coverage is a map of untested areas, not a target. Load test to 2–3× peak and watch p95/p99.
