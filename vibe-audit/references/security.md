# Security Checks

16 checks for authentication, authorization, input handling, and transport security.

---

## S1: No Rate Limiting on API Routes

**Severity:** CRITICAL

API routes without rate limiting let attackers spam your backend into a massive bill overnight.

### Detection

1. Check `package.json` for rate limiting libraries:
   `express-rate-limit`, `rate-limiter-flexible`, `@upstash/ratelimit`, `next-rate-limit`, `limiter`
2. If none found → FAIL
3. If found, verify usage on API routes:

**Next.js App Router:** Search `app/**/route.{ts,js}` for `rateLimit`, `Ratelimit`
**Next.js Pages Router:** Search `pages/api/**` for rate limit usage
**Express:** Search for `rateLimit` middleware in route definitions

4. Count total API routes vs routes with rate limiting. If <50% covered → FAIL

### Pass criteria

Rate limiting library installed AND applied to API routes — especially auth, payment, and cost-incurring endpoints (AI calls, email sends).

### Fix approach

Install `@upstash/ratelimit` + `@upstash/redis` (serverless) or `express-rate-limit` (Express). Apply to all public-facing API routes. **Effort: Medium**

---

## S2: Auth Tokens in localStorage

**Severity:** CRITICAL

localStorage is accessible to any JavaScript on the page. One XSS attack exposes every user's session.

### Detection

Search all frontend code:
```
localStorage.setItem.*token|auth|jwt|session|access_token|refresh_token
localStorage[.*token
localStorage.token
localStorage.jwt
```
File patterns: `*.ts`, `*.tsx`, `*.js`, `*.jsx`

### Pass criteria

No auth/session tokens in localStorage. Tokens stored in httpOnly cookies or handled by auth library (Clerk, NextAuth, Supabase Auth manage this automatically).

### Fix approach

Switch to httpOnly cookies. Set server-side with `httpOnly: true, secure: true, sameSite: 'lax'`. **Effort: Medium**

---

## S3: No Input Sanitization on Forms

**Severity:** CRITICAL

Unsanitized user input enables SQL injection, XSS, and command injection. AI-generated code rarely adds sanitization.

### Detection

1. Check `package.json` for validation/sanitization libraries:
   `zod`, `joi`, `yup`, `class-validator`, `validator`, `dompurify`, `sanitize-html`, `xss`
2. If none found → likely FAIL
3. Check API routes for request body usage without validation:
   Search for `req.body`, `request.json()`, `await request.json()` in route files
4. Compare against validation usage: `.parse(`, `.validate(`, `.safeParse(`
5. If API routes access request bodies without validation → FAIL
6. **SQL injection specifically:** Search for raw SQL string concatenation:
   ```
   `SELECT.*\$\{|`INSERT.*\$\{|`UPDATE.*\$\{|`DELETE.*\$\{
   "SELECT " +|"INSERT " +|"UPDATE " +|"DELETE " +
   query\(.*\+.*req\.|query\(.*\+.*body
   ```
   If SQL queries are built via string concatenation or template literals with user input → FAIL

### Pass criteria

Validation library installed AND used on API routes that accept user input. Server-side validation required — client-side only does not count.

### Fix approach

Install `zod`. Create schemas for every API route's expected input. Use `.safeParse()` before processing. **Effort: Medium**

---

## S4: Hardcoded API Keys in Frontend

**Severity:** CRITICAL

API keys in source code are discoverable within hours via browser dev tools or source maps.

### Detection

Search all source files (exclude `.env*`, `node_modules`, `.git`):
```
sk_live_|sk_test_
AKIA[0-9A-Z]
AIzaSy
ghp_[a-zA-Z0-9]
glpat-
xoxb-|xoxp-
sk-[a-zA-Z0-9]{20,}
```
File patterns: `*.ts`, `*.tsx`, `*.js`, `*.jsx`, `*.json` (exclude node_modules)

Also check for inline key assignments:
```
apiKey:\s*['"][a-zA-Z0-9_-]{20,}['"]
```

Check for secrets in client-exposed env vars:
```
NEXT_PUBLIC_.*SECRET|NEXT_PUBLIC_.*KEY.*=.*sk_|NEXT_PUBLIC_.*PRIVATE
```
in `.env*` files.

### Pass criteria

No API keys/secrets in source code. All secrets in `.env*` files via `process.env`. No secrets in `NEXT_PUBLIC_*` variables.

### Fix approach

Move all keys to `.env.local`. Access via `process.env.KEY_NAME` server-side only. Never prefix secrets with `NEXT_PUBLIC_`. Add `.env*.local` to `.gitignore`. **Effort: Small**

---

## S5: Sessions That Never Expire

**Severity:** CRITICAL

A stolen session token grants permanent access. No expiry means no recovery.

### Detection

Search for JWT/session configuration:
```
jwt.sign(       — check if expiresIn option is present
sign(            — check for exp claim
maxAge           — in cookie/session config
session.*{       — check for maxAge or expires
```

If using NextAuth/Auth.js, check session config for `maxAge` setting.
If JWT signing found WITHOUT `expiresIn` or `exp` → FAIL.
If using managed auth (Clerk, etc.) → likely PASS (check docs for defaults).

### Pass criteria

All JWT tokens have expiration. Session cookies have `maxAge`. Auth library session duration explicitly configured.

### Fix approach

Add `expiresIn: '24h'` to JWT sign calls. Set `maxAge` on session cookies. Review auth library defaults. **Effort: Small**

---

## S6: Password Reset Links That Don't Expire

**Severity:** CRITICAL

An old reset email in someone's inbox becomes a permanent account takeover vector.

### Detection

Search for password reset logic:
```
reset.*password|forgot.*password|passwordReset|reset_token|resetToken
```

If files found, check for token expiration:
```
expire|ttl|expiresAt|expiresIn|validUntil|Date.now
```

If reset token generated without expiry → FAIL.
If using managed auth (Clerk, Supabase, Firebase) → SKIP (handled automatically).

### Pass criteria

Reset tokens expire (typically 1-24 hours). Expiry checked server-side before allowing reset. Used tokens deleted immediately.

### Fix approach

Store tokens with `expiresAt = Date.now() + 3600000` (1 hour). Validate before accepting. Delete after use. **Effort: Small**

---

## S7: No CORS Policy

**Severity:** HIGH

Without CORS restrictions, any website can make authenticated requests to your API using your users' cookies.

### Detection

**Next.js:** Search `next.config.*`, `middleware.*`, `proxy.*`, and route handlers for:
```
Access-Control-Allow-Origin|cors
```

**Express:** Search for `cors(` middleware or `cors` in package.json.

**Generic:** Search all source for `Access-Control-Allow-Origin`.

If no CORS configuration found → FAIL.
If using `Access-Control-Allow-Origin: *` with credentials → FAIL.

### Pass criteria

CORS policy explicitly configured with specific allowed origins (not wildcard with credentials).

### Fix approach

**Next.js:** Add CORS headers in `next.config.ts` or per-route. **Express:** `npm install cors`, configure specific origins. **Effort: Small**

---

## S8: Admin Routes Without Role Checks + Unprotected API Routes

**Severity:** CRITICAL

Any authenticated user can access admin functionality. Unprotected internal API routes let anyone call sensitive endpoints.

### Detection

**Admin routes:**
Find admin-related routes:
```
find files matching */admin* in route/page/api directories
```

If admin routes found, check for role/permission checks:
```
role|isAdmin|admin.*check|authorize|permission|rbac|guard
```

If admin routes exist WITHOUT role verification → FAIL.
If no admin routes found → SKIP admin portion.

**All API routes (auth middleware coverage):**
Count total API routes and check how many have auth middleware:
```
auth|getSession|getServerSession|currentUser|requireAuth|withAuth|protect|middleware
```
in `app/**/route.*`, `pages/api/**`, or Express route files.

If >25% of non-public API routes lack auth checks → FAIL.
Exclude intentionally public routes (health check, webhooks with their own verification).

### Pass criteria

All admin routes verify user role/permissions server-side. Auth middleware applied to all non-public API routes. Role checks happen in middleware or at the top of handlers, not just UI hiding.

### Fix approach

Add role check middleware for admin. Add auth middleware to all protected API routes. Verify role from session/JWT server-side. Never trust client-provided role claims. **Effort: Medium**

---

## S9: No Content Security Policy (CSP)

**Severity:** HIGH

Without CSP, browsers allow any script, style, or frame to load — making XSS trivially exploitable.

### Detection

Search for CSP configuration:
```
Content-Security-Policy|contentSecurityPolicy|csp
```
in `next.config.*`, `middleware.*`, `proxy.*`, `vercel.json`, route files.

Check for `helmet` in package.json (Express).
Check for `next-safe` or CSP packages.

If no CSP configuration found → FAIL.

### Pass criteria

CSP headers configured in production. At minimum: `default-src 'self'`, `script-src` restricted.

### Fix approach

**Next.js:** Add CSP in `next.config.ts` headers or middleware. **Express:** Use `helmet()`. Start with report-only mode, then tighten. **Effort: Medium**

---

## S10: No HTTPS Redirect Enforcement

**Severity:** HIGH

HTTP traffic sends cookies, tokens, and data in plaintext. Anyone on the same network intercepts everything.

### Detection

First check hosting:
- Vercel → SKIP (automatic HTTPS enforcement)
- Cloudflare → SKIP (automatic with default settings)

Otherwise search for:
```
x-forwarded-proto|https.*redirect|force-ssl|requireHTTPS
Strict-Transport-Security|hsts
```
in middleware, proxy, server files, and config.

If not on auto-HTTPS platform AND no redirect/HSTS found → FAIL.

### Pass criteria

HTTPS enforced via hosting platform, redirect middleware, or HSTS headers.

### Fix approach

Add HTTPS redirect in middleware/proxy. Set `Strict-Transport-Security: max-age=31536000; includeSubDomains`. Most modern hosting handles this automatically. **Effort: Small**

---

## S11: Weak or Default JWT Secrets

**Severity:** CRITICAL

A JWT secret of "secret", "password", or any value copied from a tutorial lets anyone forge valid tokens and impersonate any user.

### Detection

Search for JWT secret configuration:
```
JWT_SECRET|JWT_KEY|TOKEN_SECRET|AUTH_SECRET|NEXTAUTH_SECRET
```
in `.env*` files. DO NOT output the actual values — only check characteristics.

Check for weak/default values:
```
secret|password|123456|changeme|your-secret|my-secret|test|development|jwt_secret
```

Also search source code for hardcoded JWT secrets:
```
sign\(.*['"]secret['"]\)|sign\(.*['"]password['"]\)|SECRET.*=.*['"]secret
```

If JWT secret matches common defaults or is fewer than 32 characters → FAIL.
If no JWT usage found → SKIP.

### Pass criteria

JWT secret is a strong, randomly generated value (32+ characters). Stored in environment variables, not source code. Different between environments.

### Fix approach

Generate a strong secret: `openssl rand -base64 32`. Store in `.env.local`. Use different secrets per environment. **Effort: Small**

---

## S12: Error Responses Leaking Internal Details

**Severity:** HIGH

Stack traces, database table names, file paths, and SQL errors in API responses give attackers a blueprint of your system.

### Detection

Search for error handling in API routes that passes raw errors to responses:
```
res.json\(.*err|res.send\(.*err|Response.json\(.*error.message|stack|stackTrace
catch.*\{.*res.*error\.(message|stack)
```

Check for development error handlers in production:
```
NODE_ENV.*development.*stack|showStack|verbose.*error
```

Search for database errors being forwarded:
```
catch.*\{.*res.*(500|error).*e\b
```

If error responses include stack traces, file paths, or raw database errors → FAIL.

### Pass criteria

Production error responses return generic messages ("Something went wrong") with an error ID for internal correlation. Stack traces, SQL errors, and file paths never reach the client. Detailed errors logged server-side only.

### Fix approach

Create a global error handler that catches all errors, logs them with a correlation ID, and returns a sanitized response. In Next.js, use `error.tsx` boundaries and sanitized API error responses. **Effort: Small**

---

## S13: Passwords Hashed with Weak Algorithms

**Severity:** CRITICAL

MD5 and SHA1 are broken for password hashing — rainbow tables crack them in seconds. Even SHA256 without salting is inadequate.

### Detection

Search for password hashing:
```
createHash.*md5|createHash.*sha1|createHash.*sha256
md5\(|sha1\(|SHA1|MD5
```

Check `package.json` for proper hashing libraries:
`bcrypt`, `bcryptjs`, `argon2`, `scrypt`

If password hashing uses MD5, SHA1, or unsalted SHA256 → FAIL.
If using managed auth (Clerk, Supabase Auth, Firebase Auth, NextAuth with providers) → SKIP.
If no custom password handling found → SKIP.

### Pass criteria

Passwords hashed with bcrypt (cost factor ≥10), argon2, or scrypt. No MD5, SHA1, or plain SHA256 for passwords.

### Fix approach

Install `bcryptjs`. Replace `createHash('md5')` with `bcrypt.hash(password, 12)`. Migrate existing hashes by re-hashing on next login. **Effort: Medium**

---

## S14: IDOR on Resource Endpoints

**Severity:** CRITICAL

If `/api/users/123/orders` works for any authenticated user, anyone can enumerate and access other users' data by changing the ID.

### Detection

Search for resource endpoints that use URL parameters without ownership checks:
```
params.id|params.userId|params.orderId|req.params|request.nextUrl.searchParams
```
in API route handlers.

Then check if those handlers verify the requesting user owns the resource:
```
session.user.id.*params|currentUser.*params|where.*userId.*session
```

If resource endpoints use URL/query params to fetch data WITHOUT checking ownership against the authenticated user → FAIL.

### Pass criteria

All resource endpoints verify the authenticated user owns or has permission to access the requested resource. Queries include `WHERE userId = session.user.id` or equivalent ownership filter.

### Fix approach

Add ownership checks to every resource endpoint: `where: { id: params.id, userId: session.user.id }`. For shared resources, implement explicit permission checks. Never trust client-provided user IDs for authorization. **Effort: Medium**

---

## S15: Open Redirects in Callback URLs

**Severity:** HIGH

If your login callback accepts an arbitrary `redirect` or `callbackUrl` parameter, attackers can redirect users to phishing sites via your trusted domain.

### Detection

Search for redirect parameters in auth flows and route handlers:
```
callbackUrl|redirect_uri|returnTo|next=|redirect=|return_url|goto=
```

Check if redirect targets are validated:
```
allowedRedirects|validRedirects|whitelist.*url|startsWith.*\/|isRelativeUrl|new URL.*origin
```

If redirect parameters are used without validating the destination is a same-origin or allowlisted URL → FAIL.

### Pass criteria

All redirect parameters validated against an allowlist of domains or restricted to relative paths (same-origin). No open redirects to arbitrary external URLs.

### Fix approach

Validate redirect URLs: only allow relative paths (`/dashboard`) or a strict allowlist of domains. Use `new URL(redirect).origin === new URL(request.url).origin` check. Strip any redirect that doesn't match. **Effort: Small**

---

## S16: Sessions Not Invalidated on Logout

**Severity:** HIGH

If logout only clears the client-side cookie/token but doesn't invalidate the session server-side, a stolen token remains valid indefinitely after the user "logs out."

### Detection

Search for logout handlers:
```
logout|signOut|sign-out|logOut|log-out
```

Check if logout destroys the server-side session or adds the token to a blocklist:
```
session.destroy|delete.*session|revoke|blocklist|blacklist|invalidate.*token
```

If logout only clears cookies client-side (`res.clearCookie` or `cookies().delete()`) without server-side invalidation → FAIL.
If using managed auth (Clerk, Supabase Auth) that handles invalidation → PASS.
If using stateless JWTs without a refresh token revocation strategy → FAIL.

### Pass criteria

Logout invalidates the session server-side. For JWTs: refresh tokens are revoked and access tokens are short-lived (≤15 min). For session-based auth: session destroyed in the store.

### Fix approach

**Session-based:** Call `session.destroy()` on logout. **JWT:** Implement short-lived access tokens (15 min) + revocable refresh tokens. On logout, delete the refresh token from the database. **Effort: Medium**
