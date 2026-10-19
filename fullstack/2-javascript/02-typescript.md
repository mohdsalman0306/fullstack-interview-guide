# TypeScript

> **Time needed:** about 2 hours · **Questions:** 13 + quick questions
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

## Progress checklist
- [ ] 1. Why TypeScript? Does it check data at runtime?
- [ ] 2. `interface` vs `type`
- [ ] 3. `any` vs `unknown` vs `never` vs `void`
- [ ] 4. Generics
- [ ] 5. Unions, intersections, discriminated unions
- [ ] 6. Narrowing: type guards vs type assertions
- [ ] 7. Utility types
- [ ] 8. `keyof`, `typeof`, mapped and conditional types
- [ ] 9. `as const` and `satisfies`
- [ ] 10. Enums: use or avoid?
- [ ] 11. Strict mode and tsconfig
- [ ] 12. Keeping types in sync between frontend and backend
- [ ] 13. Decorators
- [ ] Quick questions
- [ ] Remember these 10 things

---

## 1. Why TypeScript? Does it validate data at runtime?

**In simple words**
TypeScript catches bugs before the code runs. But types are removed at build time, so outside data still needs a real check.

**Picture it**
TypeScript is a spell-checker for your own writing. It can't check the letters other people send you. For those you need a guard at the door (Zod).

**🗣️ Say this in the interview**
> "TypeScript adds **types** to JavaScript, so a whole group of bugs gets caught **before the code runs**: typos, wrong arguments, missing null checks. The bigger win for me is **safe refactoring**. I rename a field and the compiler shows me every place that breaks.
> But types are **erased** at build time. So TypeScript does **not** check data coming from outside: API requests, third-party responses, environment variables. For those I use a runtime validator like **Zod**, and I get the TypeScript type from the schema. That way there's one source of truth."

```typescript
import { z } from 'zod';

const CreateOrder = z.object({
  sku: z.string().min(1),
  qty: z.number().int().positive(),
});
type CreateOrder = z.infer<typeof CreateOrder>; // the type comes from the schema

const input = CreateOrder.parse(req.body); // throws if invalid; typed if valid
```

**⚠️ Avoid saying**
"TypeScript makes my API safe." Types don't exist at runtime. Without validation, `req.body` can be anything.

---

## 2. What is the difference between `interface` and `type`?

**In simple words**
Both describe object shapes. Each has a few things only it can do.

**🗣️ Say this in the interview**
> "Both describe the shape of an object, and for most app code either is fine. The main thing is to pick one and be consistent. The differences: **`type`** can also express unions, tuples, primitives and mapped types. **`interface`** can be extended with `extends`, and it supports **declaration merging**, which means two interfaces with the same name join into one. My default is `interface` for object shapes and `type` for unions and anything computed."

**They may also ask**
- *Can an interface be a union?* No. `type Status = 'paid' | 'failed'` has to be a `type`.
- *What is declaration merging?* Writing `interface Window { myVar: string }` adds a field to the existing `Window` type. It's used to extend library types, like adding `user` to Express's `Request`.

**⚠️ Avoid saying**
"`interface` is always better" (or the opposite). Say instead: both work; be consistent and know the few differences.

---

## 3. What is the difference between `any`, `unknown`, `never` and `void`?

**In simple words**
Four special types. The key point: `unknown` is the safe version of `any`.

**Picture it**
`any` is an unlocked door. `unknown` is a locked door: you can go in, but only after showing ID.

**🗣️ Say this in the interview**
> "**`any`** switches type checking off. It's an escape hatch, and it spreads to other code. **`unknown`** means 'could be anything, but you must check before using it'. It's the safe version of `any`. I use it for caught errors and parsed JSON. **`never`** means a value that can't exist: a function that always throws, or the leftover case in a switch that handles every case. **`void`** means a function returns nothing useful."

```typescript
// Helper for "handle every case" checks (used in question 5)
function assertNever(x: never): never {
  throw new Error(`Unhandled case: ${JSON.stringify(x)}`);
}
```

**⚠️ Avoid saying**
"I use `any` when the type is unclear." Say instead: I use `unknown` and narrow it.

---

## 4. What are generics?

**In simple words**
One function for many types, without losing the type. `T` is like a variable, but for a type.

**🗣️ Say this in the interview**
> "Generics let me write one function or type that works with many types while **keeping the type information**. Without them I'd either copy the code for each type or fall back to `any`. Typical uses are API response wrappers, repositories, and reusable components."

```typescript
function first<T>(items: T[]): T | undefined {
  return items[0];
}
const n = first([1, 2, 3]);   // n: number | undefined

// Constraint: T must have an `id`
function byId<T extends { id: string }>(items: T[], id: string): T | undefined {
  return items.find(i => i.id === id);
}

interface ApiResponse<T> { data: T; error?: string }
```

**They may also ask**
- *Generic constraints?* `T extends Something` limits what `T` can be, so you can safely use its properties.
- *Default type parameters?* Yes: `interface Page<T = unknown>`.

**⚠️ Avoid saying**
"Generics are like `any`." No: `any` loses the type, generics keep it.

---

## 5. Explain unions, intersections, and discriminated unions.

**In simple words**
A discriminated union is a union where every option has the same label field, like `status`. Check the label, and TypeScript knows the exact shape.

**Picture it**
Parcels with a coloured sticker. Look at the sticker, and you know how to handle the box.

**🗣️ Say this in the interview**
> "A **union**, `A | B`, means one *or* the other. An **intersection**, `A & B`, means both at once. The most useful pattern is a **discriminated union**: every member shares a literal field like `status` or `type`. When I check that field, TypeScript knows exactly which shape I have. If I add a `never` check, the compiler forces me to handle every case."

```typescript
type Payment =
  | { status: 'pending' }
  | { status: 'paid'; transactionId: string }
  | { status: 'failed'; reason: string };

function label(p: Payment) {
  switch (p.status) {
    case 'pending': return 'Waiting…';
    case 'paid':    return `Paid (${p.transactionId})`; // TS knows transactionId exists here
    case 'failed':  return `Failed: ${p.reason}`;
    default:        return assertNever(p); // compile error if someone adds a new status
  }
}
```

**From my work**
I used the same idea when I replaced `is_open_now: boolean` with `OPEN | BREAK | CLOSED`. A boolean couldn't show the third state. Use Story S3 (stores showing "open" on Dubai's clock), see `1-interview-basics/02-your-story.md`.

**⚠️ Avoid saying**
"I use optional fields for each status." Say instead: a discriminated union, so impossible combinations can't exist.

---

## 6. What is narrowing? Type guards vs type assertions?

**In simple words**
**Narrowing** means TypeScript learns a more exact type after a check. `as User` checks nothing; a guard really checks.

**🗣️ Say this in the interview**
> "A **type assertion**, `value as User`, tells the compiler 'trust me'. It doesn't check anything at runtime. A **type guard** is a real check, like `typeof`, `instanceof`, `in`, or a custom function, and TypeScript **narrows** the type after it. For anything coming from outside, I use guards or schema validation, never assertions."

```typescript
function isUser(x: unknown): x is User {
  return typeof x === 'object' && x !== null && 'id' in x && 'email' in x;
}

if (isUser(data)) {
  data.email; // TS now knows data is a User
}
```

**They may also ask**
- *What is `x is User`?* A **type predicate**. It's the return type of a custom guard.
- *Non-null assertion `!`?* `user!.name` tells TS "this is not null". Use it rarely. It's a promise you might break.

**⚠️ Avoid saying**
"I just cast it with `as`." Say instead: I check it with a guard or Zod.

---

## 7. Which utility types do you use?

**In simple words**
Built-in helpers to make new types from existing ones.

**🗣️ Say this in the interview**
> "Utility types build new types from existing ones, so I don't duplicate definitions. The ones I use most: `Partial` for update payloads, `Pick` and `Omit` for API views of a model, `Record` for dictionaries, `ReturnType` and `Parameters` to get types from functions, and `Readonly` for config."

| Utility | What it does | Example use |
|---|---|---|
| `Partial<T>` | All fields optional | PATCH body |
| `Required<T>` | All fields required | Validated config |
| `Readonly<T>` | Fields can't be reassigned | Constants |
| `Pick<T, K>` / `Omit<T, K>` | Keep / remove fields | `Omit<User, 'password'>` for responses |
| `Record<K, V>` | Object with keys K and values V | `Record<Locale, string>` |
| `Exclude` / `Extract` | Filter a union | `Exclude<Status, 'deleted'>` |
| `NonNullable<T>` | Remove null/undefined | |
| `ReturnType<F>` / `Parameters<F>` | Get types from a function | Typing wrappers |
| `Awaited<T>` | Unwrap a Promise | `Awaited<ReturnType<typeof fetchUser>>` |

**They may also ask**
- *When is `Partial` dangerous?* When the type really needs certain fields. You lose that guarantee and end up with null checks everywhere.
- *How is `Partial` built?* As a mapped type: `type Partial<T> = { [K in keyof T]?: T[K] }`.

**⚠️ Avoid saying**
Only naming `Partial`. Show you know `Pick`/`Omit`, `Record` and `ReturnType` with a real use.

---

## 8. Explain `keyof`, `typeof`, indexed access, mapped and conditional types.

**In simple words**
Tools to build types from other types. Write a type once, derive the rest.

**🗣️ Say this in the interview**
> "`keyof T` gives a union of a type's keys. `typeof value`, used in a type position, gets the type of a variable. `T['field']` reads one property's type. **Mapped types** loop over keys to change a type. **Conditional types**, `T extends X ? A : B`, choose a type based on a condition. Together they let me derive types instead of writing them twice."

```typescript
const config = { region: 'me-central-1', retries: 3 };
type Config = typeof config;            // { region: string; retries: number }
type ConfigKey = keyof Config;          // 'region' | 'retries'
type Retries = Config['retries'];       // number
type Unwrap<T> = T extends Promise<infer U> ? U : T;  // `infer` pulls out the inner type
// Unwrap<Promise<string>> is string; Unwrap<number> is number
```

**⚠️ Avoid saying**
"`typeof` is only the JavaScript runtime operator." In a type position, it reads a variable's TypeScript type.

---

## 9. What do `as const` and `satisfies` do?

**In simple words**
Two tools to keep exact types while still getting checks.

**🗣️ Say this in the interview**
> "`as const` makes a value deeply readonly and keeps its **literal** types. So `'paid'` stays `'paid'` instead of becoming `string`. **`satisfies`** checks that a value matches a type **without** changing the type TypeScript inferred. So I get the check and I keep the precise type. I use both for config objects and lookup tables."

```typescript
const STATUSES = ['pending', 'paid', 'failed'] as const;
type Status = typeof STATUSES[number]; // 'pending' | 'paid' | 'failed'

const routes = {
  home: '/',
  product: '/p/:slug',
} satisfies Record<string, string>;
// Checked against Record, but TS still knows the exact keys:
// routes.home works, routes.typo is a compile error
```

**⚠️ Avoid saying**
"`satisfies` is the same as a type annotation." An annotation replaces the inferred type; `satisfies` only checks it.

---

## 10. Enums: do you use them or avoid them?

**In simple words**
Most teams prefer string unions. They are simpler and add no code to the build.

**🗣️ Say this in the interview**
> "I usually prefer a **string union** or an `as const` object over `enum`. Unions produce no runtime code, work naturally with JSON, and are simpler. Regular enums generate a runtime object, and numeric enums have a surprising reverse-mapping behaviour. I'd use an enum only if the team already uses them everywhere."

**⚠️ Avoid saying**
"Enums are bad." Say instead: I prefer unions, but I follow the team's standard.

---

## 11. What does strict mode do? Which tsconfig options matter?

**In simple words**
`strict` turns on the important checks. Always use it.

**🗣️ Say this in the interview**
> "I always turn on `strict`. It enables `strictNullChecks`, `noImplicitAny` and a few others. That's where most of TypeScript's value comes from. I also like `noUncheckedIndexedAccess`. It makes `arr[0]` possibly `undefined`, because that's actually true at runtime."

**They may also ask**
- *How do you move a JS codebase to TS?* Turn on `allowJs`. Convert file by file, starting from the leaf modules (files that don't import others). Make the checks stricter step by step. No big-bang rewrite.

**⚠️ Avoid saying**
"We turned strict off because it had too many errors." Say instead: we turned it on gradually.

---

## 12. How do you keep types in sync between frontend and backend?

**In simple words**
Don't copy types by hand. Share them or generate them.

**🗣️ Say this in the interview**
> "I never hand-copy types between projects, because they drift. My options, from simplest:
> 1. A **shared package** in a monorepo with Zod schemas. The server uses them for validation, the client for types.
> 2. **Generate** client types from the API contract: OpenAPI codegen for REST, GraphQL Code Generator for GraphQL.
> 3. An end-to-end typed RPC like **tRPC**, if both sides are TypeScript and the same team owns them.
>
> For GraphQL I'd always use codegen. The schema is already the contract."

**⚠️ Avoid saying**
"We copy the interfaces into the frontend." That always drifts.

---

## 13. What are decorators?

**In simple words**
`@Something` functions that add behaviour to classes. NestJS uses them a lot.

**🗣️ Say this in the interview**
> "Decorators are functions that attach behaviour or metadata to classes, methods or properties. Examples are `@Controller()` or `@Injectable()` in NestJS, or column mappings in TypeORM. They're powerful, but they **hide** behaviour, so I use them only where the framework expects them. TypeScript 5 supports the standard decorator spec. Older frameworks still use the legacy `experimentalDecorators` version."

**⚠️ Avoid saying**
"I write custom decorators everywhere." Say instead: only where the framework expects them, because they hide behaviour.

---

## Quick questions
- **Structural typing?** TS compares types by **shape**, not by name. Two different interfaces with the same fields are compatible.
- **`.d.ts` files?** Type declarations only, no code. They describe JS libraries to TS.
- **`readonly` vs `const`?** `const` is for variables; `readonly` is for properties.
- **`x?: T` vs `x: T | undefined`?** `x?: T` means the key may be missing. `x: T | undefined` means the key must exist, but its value can be `undefined`.
- **Abstract class vs interface?** An abstract class can hold shared code and exists at runtime. An interface is a type-only contract.
- **Function overloads?** Several call signatures for one function. Used when the return type depends on the input.
- **`unknown` in `catch`?** With `useUnknownInCatchVariables` (part of `strict`), `catch (e)` is `unknown`. Narrow it with `e instanceof Error`.
- **Can Node run TS directly now?** Recent Node versions can strip types and run `.ts` files, but they don't type-check. You still run `tsc` in CI.

## Remember these 10 things
1. Types are erased at build time; validate outside data with Zod and use `z.infer` for the type.
2. `interface` for object shapes, `type` for unions; be consistent.
3. Use `unknown`, not `any`, then narrow it.
4. Generics keep type information; `T extends X` adds a constraint.
5. Discriminated union + `assertNever` = the compiler forces you to handle every case.
6. `as` checks nothing; type guards (`x is User`) really check.
7. `Partial`, `Pick`/`Omit`, `Record`, `ReturnType`, `Awaited` save you from writing types twice.
8. `as const` keeps literal types; `satisfies` checks without changing the inferred type.
9. Prefer string unions over enums; always turn on `strict`.
10. Share or generate types between frontend and backend; never copy them by hand.
