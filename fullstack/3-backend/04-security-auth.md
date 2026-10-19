# Authentication and Security

> **Time needed:** about 2 hours · **Questions:** 16 + 9 quick questions
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

## Progress checklist
- [ ] 1. Authentication vs authorization
- [ ] 2. Sessions vs JWT
- [ ] 3. JWT structure and checking it
- [ ] 4. Access and refresh tokens
- [ ] 5. Revoking a JWT right away
- [ ] 6. OAuth 2.0 and OpenID Connect
- [ ] 7. Where to store tokens in the browser
- [ ] 8. XSS
- [ ] 9. CSRF
- [ ] 10. SQL and NoSQL injection
- [ ] 11. SSRF
- [ ] 12. RBAC, ABAC and IDOR
- [ ] 13. Storing passwords
- [ ] 14. Managing secrets
- [ ] 15. OWASP Top 10
- [ ] 16. Security headers
- [ ] Quick questions

---

## 1. What is the difference between authentication and authorization?

**In simple words**
Authentication = who are you? Authorization = what are you allowed to do? You check the first once per request. You check the second for every resource and action.

**Picture it**
Authentication is showing your ID at the hotel front desk. Authorization is your key card only opening **your** room.

**🗣️ Say this in the interview**
> **Authentication** is *who are you?* It means proving identity with a password, a token or SSO.
> **Authorization** is *what are you allowed to do?* Can this user see **this** order, or edit **this** product?
> Authentication usually happens once per request. Authorization has to happen for **every resource and every action**, and always on the server.

**⚠️ Avoid saying**
"The user is logged in, so they can see it." Being logged in doesn't mean the order belongs to them.

---

## 2. Sessions vs JWT: which do you use?

**In simple words**
A session keeps the data on the server, and the browser only has an ID. A JWT carries the data inside the token itself. Sessions are easy to cancel; JWTs are easy to check across many services.

**🗣️ Say this in the interview**
> With **sessions**, the server stores the session data, usually in Redis. The browser only holds a random session ID in a cookie. It's easy to **revoke**: I just delete the session. But every request needs a lookup.
> With a **JWT**, the token itself carries the user info and is **signed**. So any service can check it **without a lookup**. That's great with many services, but I **can't easily revoke** it before it expires.
> For a classic web app, server sessions are simple and secure. For many services and APIs, I use short-lived JWT access tokens plus a refresh token I can revoke.

**⚠️ Avoid saying**
"JWT is always better because it's stateless." Say it's a trade-off: no lookup, but harder to revoke.

---

## 3. What is inside a JWT, and how do you check it properly?

**In simple words**
A JWT has three parts: header, payload, signature. It is signed, not encrypted, so anyone can read it. When you check it, you must check the signature, the algorithm and the claims.

**🗣️ Say this in the interview**
> A JWT has three base64 parts separated by dots. The **header** says the algorithm. The **payload** has the **claims**, like user ID, expiry and roles. Then the **signature**.
> It's **signed, not encrypted**. Anyone can decode and read the payload. So I never put secrets or personal data in it.
> When I verify it, I check the **signature**. I **pin the allowed algorithm**, so an attacker can't switch it to `none` or swap RS256 for HS256. And I check **`exp`** (expiry), **`iss`** (who issued it) and **`aud`** (who it's for).

**They may also ask**
- *HS256 vs RS256?* HS256 uses one **shared secret** to sign and verify, so anyone who can verify can also create fake tokens. RS256 or ES256 use a **private key to sign** and a **public key to verify**. Services can check tokens but can't create them. Public keys are shared via a **JWKS** URL. Use these across services.
- *What is `aud` for?* It stops a token made for service A from being accepted by service B.

**⚠️ Avoid saying**
"The JWT is encrypted, so it's safe to put data in it." It's only signed. Anyone can read it.

---

## 4. What are access tokens and refresh tokens?

**In simple words**
The access token is short-lived and goes with every API call. The refresh token lives longer and is used only to get a new access token.

**Picture it**
The access token is a day pass. The refresh token is the membership card you show at the desk to get a new day pass.

**🗣️ Say this in the interview**
> The **access token** is short-lived, say 5 to 15 minutes, and is sent with every API call. If it leaks, the damage window is small.
> The **refresh token** is long-lived and is used **only** to get new access tokens from the auth server.
> Best practice is **refresh token rotation**. Every refresh returns a **new** refresh token and cancels the old one. If an old refresh token is ever used again, it means it was stolen. So I **revoke the whole token family** and force the user to log in again.

**⚠️ Avoid saying**
"We give access tokens a 30-day expiry so users stay logged in." That's the refresh token's job.

---

## 5. How do you revoke a JWT immediately?

**In simple words**
By design you can't, because the server doesn't store it. If you really need it, you add a small check on the server.

**🗣️ Say this in the interview**
> By design you can't. That's why access tokens should be short-lived.
> If I need instant revocation, like a fired employee or a hacked account, I add a little state back:
> - Revoke their **refresh tokens**, so no new access tokens are issued.
> - Keep a **denylist of token IDs (`jti`)** in Redis until they expire. Or:
> - Store a **token version** on the user. I increase it on logout or password change, and reject tokens with an older version.
>
> It's a conscious trade-off: a small lookup per request, in exchange for real revocation.

**⚠️ Avoid saying**
"We just delete the token on the client." The token still works if someone copied it.

---

## 6. What are OAuth 2.0 and OpenID Connect?

**In simple words**
OAuth 2.0 lets an app use an API on your behalf without seeing your password. OpenID Connect (OIDC) adds "who is this user" on top. "Login with Google" is OIDC.

**🗣️ Say this in the interview**
> **OAuth 2.0** is about **delegated authorization**: letting an app access an API for a user without seeing their password. It gives an **access token**.
> **OpenID Connect** sits on top and adds **authentication**: an **ID token** that says who the user is. "Login with Google" is OIDC.
> For web and mobile apps, the recommended flow is **Authorization Code with PKCE**.

**Authorization Code + PKCE, step by step**
1. The app makes a random `code_verifier` and its hash, the `code_challenge`.
2. It sends the user to the auth server with `client_id`, `redirect_uri`, `scope`, `state` and `code_challenge`.
3. The user logs in and agrees.
4. The auth server sends the user back with a short-lived `code` (and the same `state`).
5. The app checks `state` (this protects against CSRF). Then it swaps `code` + `code_verifier` for tokens.
6. The auth server checks the verifier matches the challenge. Then it returns the access token (plus ID token and refresh token).

> **Why PKCE?** If someone steals the authorization code, it's useless without the original verifier, and the verifier never left the app.

**They may also ask**
- *What about the implicit flow?* Deprecated. Tokens came back in the URL. Use code + PKCE.
- *Client credentials flow?* For machine-to-machine, with no user. A service logs in with its own credentials.
- *SAML vs OIDC?* Both do SSO (single sign-on). SAML is older, uses XML, and is common in big companies. OIDC uses JSON and JWT and fits modern web and mobile better.

**⚠️ Avoid saying**
"OAuth is a login protocol." OAuth is for access; OIDC adds login.

---

## 7. Where should you store tokens in the browser?

**In simple words**
Not in `localStorage`, because any injected script can read it. Use HttpOnly cookies, memory, or keep tokens on a server (BFF).

**🗣️ Say this in the interview**
> I avoid `localStorage` for anything sensitive, because **any XSS can read it**. My preference:
> - The refresh token or session goes in an **HttpOnly, Secure, SameSite cookie**. JavaScript can't read it at all.
> - The short-lived access token stays **in memory** only.
> - Even better for single-page apps: a **Backend-for-Frontend (BFF)**. This is a small server for my frontend. The tokens never reach the browser; the BFF keeps them, and the browser only has a session cookie.
>
> Cookies bring in CSRF risk, so I pair them with `SameSite` and CSRF protection.

**⚠️ Avoid saying**
"`localStorage` is fine because our site has no XSS." You can't promise that; plan for it.

---

## 8. What is XSS, and how do you prevent it?

**In simple words**
XSS (cross-site scripting) is when an attacker gets their JavaScript to run on your site. Then it can steal tokens or act as the user.

**🗣️ Say this in the interview**
> XSS is when an attacker gets **their JavaScript to run on my site**. For example, a product review with a `<script>` tag that gets shown as HTML. Once it runs, it can steal tokens, act as the user, or change the page.
> To prevent it, I **encode output** for where it's shown. React does this by default, so the risky spots are `dangerouslySetInnerHTML` and `href="javascript:..."`. If I must show user HTML, I clean it with DOMPurify.
> I add a **Content Security Policy**, which tells the browser which scripts are allowed to run. And I keep tokens in HttpOnly cookies, so even a successful XSS can't read them.

**They may also ask**
- *What types of XSS are there?* **Stored**: saved in the database, hits every viewer. **Reflected**: in a URL parameter that gets echoed back. **DOM-based**: frontend JavaScript writes untrusted data into the page.

**⚠️ Avoid saying**
"We use React, so XSS is impossible." `dangerouslySetInnerHTML` and `javascript:` links still open the door.

---

## 9. What is CSRF, and how do you prevent it?

**In simple words**
CSRF (cross-site request forgery) tricks the user's browser into sending a request to your site from another site. The browser adds the cookies automatically.

**🗣️ Say this in the interview**
> CSRF tricks the user's **browser** into sending a request to my site from another site. The browser **automatically adds the cookies**. For example, a hidden form on `evil.com` that posts to `bank.com/transfer`.
> My defences: **`SameSite=Lax` or `Strict`** cookies, so cookies aren't sent on cross-site POSTs. Modern browsers default to Lax. **CSRF tokens** for forms. **Checking the `Origin` header** on requests that change data. And never changing data on a GET.

> **XSS vs CSRF in one line:** XSS is the attacker running code **on** your site. CSRF is the attacker making the user's browser send a request **to** your site from somewhere else.

**⚠️ Avoid saying**
"CORS protects us from CSRF." It doesn't. CORS controls who can **read** responses. A CSRF attack doesn't need to read anything.

---

## 10. What is SQL and NoSQL injection?

**In simple words**
Injection is when user input gets treated as code. The fix for SQL is parameterized queries: send the query and the data separately.

**🗣️ Say this in the interview**
> Injection is when user input is treated as **code**. The fix for SQL is always **parameterized queries**. The query and the data are sent separately, so input can never change the query's structure. ORMs do this by default, but raw queries built with string concatenation are still dangerous.
> MongoDB has its own version. Say the login code does `find({ email, password: req.body.password })`. If someone sends `{"$gt": ""}` as the password, it matches any value. The fix is to **validate types**, so the password must be a string, and to strip `$` operators from input.

```javascript
// ❌ Injectable: user input becomes part of the SQL text
db.query(`SELECT * FROM users WHERE email = '${email}'`);

// ✅ Parameterized: the SQL and the value travel separately
db.query('SELECT * FROM users WHERE email = $1', [email]);
```

**⚠️ Avoid saying**
"We escape quotes, so we're safe." Use parameters, not manual escaping.

---

## 11. What is SSRF?

**In simple words**
SSRF (server-side request forgery) is when your server fetches a URL the user gives, and the attacker points it at your internal systems.

**🗣️ Say this in the interview**
> SSRF is when my server fetches a URL the **user** controls, like "import image from URL" or a webhook URL. The attacker points it at **internal** things: `localhost`, internal services, or the cloud metadata endpoint `169.254.169.254`, which can leak credentials.
> My defences: **allow-list** domains where possible. Resolve DNS and **block private and link-local IP ranges**. Don't follow redirects blindly. Use IMDSv2 on AWS. And run the fetcher with as little network access as possible.

**⚠️ Avoid saying**
"We block the string `localhost`." Attackers use other names, IPs and redirects. Check the resolved IP.

---

## 12. What are RBAC, ABAC and IDOR?

**In simple words**
RBAC gives permissions by role. ABAC decides by attributes like owner or region. IDOR is the most common bug: you can see someone else's data just by changing an ID.

**🗣️ Say this in the interview**
> **RBAC** (role-based access control) gives permissions through roles: admin, editor, viewer. It's simple and easy to audit.
> **ABAC** (attribute-based) decides using attributes: the user's department, who owns the resource, the region, the time of day. It's more flexible but harder to reason about. Most real systems use **RBAC plus ownership checks**.
> The most common real-world bug is **IDOR**, also called BOLA. `GET /orders/1235` returns **someone else's** order, because the code checked that I'm logged in, but not that the order is **mine**. It's number one on the OWASP API Top 10.
> The fix is to **always scope queries by owner**: `WHERE id = $1 AND customer_id = $currentUser`.

**⚠️ Avoid saying**
"Order IDs are random, so nobody can guess them." Still check ownership on every request.

---

## 13. How do you store passwords?

**In simple words**
Hash them with a slow, salted algorithm like Argon2id or bcrypt. Never encrypt them, and never use fast hashes like MD5 or SHA-256.

**🗣️ Say this in the interview**
> Passwords are **hashed, never encrypted**, because nobody should ever be able to get them back.
> I use a **slow, salted** password-hashing algorithm: **Argon2id** first, or bcrypt with a proper cost factor. "Slow" is the point: it makes guessing stolen hashes very expensive.
> Never plain SHA-256 or MD5. They're designed to be fast, which helps attackers.

**They may also ask**
- *Salt vs pepper?* A **salt** is random and different for each password, stored with the hash. It defeats precomputed tables. A **pepper** is one secret shared by all passwords, stored **outside** the database, like in a vault.
- *Any bcrypt limit?* It only uses the first 72 bytes of the input.

**⚠️ Avoid saying**
"We encrypt passwords with AES." If you can decrypt them, so can an attacker who gets the key.

---

## 14. How do you manage secrets?

**In simple words**
Secrets never go in code, Git, images or logs. Keep them in a managed vault and load them at runtime. Even better, use cloud identity so no stored secret is needed.

**🗣️ Say this in the interview**
> Secrets never go in **code, Git, Docker images or logs**. They live in a managed store like **AWS Secrets Manager**, **Azure Key Vault** or HashiCorp Vault, and are injected when the app runs.
> Even better is **workload identity**, like an IAM role or Azure Managed Identity. Then the app doesn't need a stored credential at all.
> I also use least privilege, rotation and audit logs.
> And if a secret is ever committed to Git, I **rotate it**. Deleting the commit isn't enough.

**⚠️ Avoid saying**
"I removed the key from the Git history, so it's fine." Someone may already have it. Rotate it.

---

## 15. What is the OWASP Top 10?

**In simple words**
It's the industry's list of the most common web app security risks. Broken access control has been number one for years.

**🗣️ Say this in the interview**
> The OWASP Top 10 is the industry's list of the most common web app risks. **Broken access control** has been number one for years. Here's the 2021 list:

| # | Risk | Simple example |
|---|---|---|
| A01 | Broken Access Control | IDOR: viewing another user's order |
| A02 | Cryptographic Failures | Plain-text passwords, no HTTPS |
| A03 | Injection | SQL injection, XSS |
| A04 | Insecure Design | No rate limit on OTP checks |
| A05 | Security Misconfiguration | Debug mode in production, default passwords |
| A06 | Vulnerable Components | Old library with a known CVE |
| A07 | Auth Failures | No brute-force protection, weak sessions |
| A08 | Software/Data Integrity | Unsigned updates, hacked CI pipeline |
| A09 | Logging/Monitoring Failures | A breach nobody noticed for months |
| A10 | SSRF | Server fetches the internal metadata URL |

*Newer editions (2025) keep access control at number one and give more weight to software supply-chain risk. The ideas are the same.*

**⚠️ Avoid saying**
Don't try to recite all ten in order. Name the top few with an example each.

---

## 16. Which security headers do you set?

**In simple words**
A few HTTP headers tell the browser to be stricter. In Express, the `helmet` package sets most of them.

**🗣️ Say this in the interview**
> The ones I set by default, usually with `helmet`:
> **Content-Security-Policy**: which scripts can run. It's the strongest XSS defence.
> **Strict-Transport-Security**: always use HTTPS.
> **X-Content-Type-Options: nosniff**: don't guess file types.
> **frame-ancestors** or X-Frame-Options: stops clickjacking, where my site is hidden inside another site's frame.
> **Referrer-Policy**: controls how much of the URL is sent to other sites.

**⚠️ Avoid saying**
"Headers alone make us secure." They are an extra layer, not a replacement for safe code.

---

## Quick questions
- **Hashing vs encryption vs encoding?** Hashing is one-way (passwords). Encryption is two-way with a key (stored data). Encoding is just a format (base64) and gives **no** security.
- **Symmetric vs asymmetric?** Symmetric uses one shared key and is fast (AES). Asymmetric uses a public/private key pair (RSA, ECDSA) for key exchange and signatures.
- **What is HMAC?** A hash plus a secret key. It proves a message wasn't changed and came from someone with the key. Used for webhook signatures.
- **What does HTTPS protect?** Privacy and integrity of data in transit, plus proof of the server's identity. It doesn't protect against bugs in your app.
- **What is MFA?** Something you know, plus something you have or are. Prefer TOTP apps or passkeys over SMS where possible.
- **What are passkeys?** A login that resists phishing, using public-key crypto (WebAuthn). There is no shared secret to steal.
- **How do you stop brute-force attacks?** Rate limit per account and IP, add growing delays, lock accounts carefully (don't let attackers lock out real users), and show a CAPTCHA after failures.
- **What is least privilege?** Every user, service and token gets only the permissions it really needs.
- **What about logging and personal data (PII)?** Never log passwords, tokens or full card numbers. Mask emails and phone numbers where possible.

## Remember these 10 things
1. Authentication = who you are. Authorization = what you can do, checked for every resource.
2. Sessions are easy to revoke; JWTs need no lookup but are hard to revoke.
3. A JWT is signed, not encrypted. Pin the algorithm and check `exp`, `iss`, `aud`.
4. Short access token + rotating refresh token. Reused refresh token = revoke the family.
5. Use Authorization Code + PKCE for web and mobile. Implicit flow is deprecated.
6. No tokens in `localStorage`. Use HttpOnly cookies, memory, or a BFF.
7. XSS = their code runs on your site. CSRF = their site sends requests to yours. CORS does not stop CSRF.
8. Always use parameterized queries. Validate types to stop NoSQL `$` tricks.
9. IDOR is the most common bug: always scope queries by owner.
10. Passwords: Argon2id or bcrypt. Secrets: in a vault, and rotate if leaked.
