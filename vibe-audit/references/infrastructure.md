# Infrastructure & Ops Checks

8 checks for payment security, environment config, observability, and operational resilience.

---

## I1: Stripe Webhooks Without Signature Verification

**Severity:** CRITICAL

Without signature verification, anyone can send fake payment events to your webhook. Free products, fake subscriptions, revenue manipulation.

### Detection

First check if Stripe is used — search `package.json` for `stripe`. If not found → SKIP.

If Stripe found, locate webhook handlers:
```
webhook|stripe.*event|constructEvent
```

Check for signature verification:
```
constructEvent|stripe.webhooks.constructEvent|verifySignature|STRIPE_WEBHOOK_SECRET|whsec_
```

If webhook handler exists WITHOUT `constructEvent` or signature verification → FAIL.

### Pass criteria

All Stripe webhook endpoints verify the signature using `stripe.webhooks.constructEvent()` with the webhook signing secret.

### Fix approach

Use `stripe.webhooks.constructEvent(body, sig, process.env.STRIPE_WEBHOOK_SECRET)`. Get signing secret from Stripe Dashboard → Developers → Webhooks. **Effort: Small**

---

## I2: No Environment Variable Validation at Startup

**Severity:** HIGH

Missing env vars cause silent failures in production. Features break with cryptic errors or fall back to insecure defaults.

### Detection

Check `package.json` for env validation packages:
`@t3-oss/env-nextjs`, `@t3-oss/env-core`, `envalid`, `env-var`, `znv`

Search for validation patterns:
```
z.object.*env|createEnv|envalid
if.*!process.env.|throw.*env|missing.*env|required.*env
```

If no env validation at app startup → FAIL.

### Pass criteria

Environment variables validated at startup (not at usage time). Missing required vars cause immediate, clear failures with the variable name in the error message.

### Fix approach

Install `@t3-oss/env-nextjs` (Next.js) or `envalid` (generic). Define all required vars with types. Import at app startup so it fails fast. **Effort: Small**

---

## I3: Images Uploaded Directly to Server

**Severity:** MEDIUM

Local file storage means no CDN, no optimization, massive bandwidth costs, and lost files on redeploy in serverless environments.

### Detection

Check for file upload handling:
```
multer|formidable|busboy|writeFile.*upload|createWriteStream|fs.write
```

Check for cloud storage:
```
@vercel/blob|@aws-sdk/client-s3|cloudinary|uploadthing|supabase.*storage|firebase.*storage
```
in `package.json`.

If file uploads exist but no cloud storage → FAIL.

Also check for `public/uploads` or similar directories in the project.

If no file uploads found → SKIP.

### Pass criteria

File uploads go to cloud storage (Vercel Blob, S3, Cloudinary, etc.) with CDN delivery. No files saved to local filesystem in production.

### Fix approach

Use `@vercel/blob` (Vercel), Cloudinary, or S3. Serve via CDN. Use `next/image` for optimization. **Effort: Medium**

---

## I4: Emails Sent Synchronously in Request Handlers

**Severity:** MEDIUM

One slow SMTP server and your entire app hangs. Users wait 10+ seconds while an email sends.

### Detection

Check for email sending in route handlers:
```
sendMail|resend.*send|sgMail|transporter.send|nodemailer
```
in `app/**/route.*`, `pages/api/**`

Check for async/background patterns:
```
waitUntil|after(|queue|background.*email|Promise.allSettled.*mail
```

If email sending is directly in request handlers without background processing → FAIL.
If no email sending found → SKIP.

### Pass criteria

Email sending happens in background jobs, queues, or `waitUntil`/`after()` callbacks — not blocking the response.

### Fix approach

**Next.js (Vercel):** Use `after()` (Next.js 15+) or `waitUntil()`. **Generic:** Use a job queue (BullMQ, Inngest, Vercel Queues). **Effort: Medium**

---

## I5: No Health Check Endpoint

**Severity:** MEDIUM

Without a health check, your app goes down silently. You find out from angry users, not from monitoring.

### Detection

Search for health check routes:
```
health|healthcheck|health-check|readiness|liveness
```
in route/API files.

Check common paths:
```
find files matching */health* or */healthcheck* in route directories
```

Check deployment config (`vercel.json`, `fly.toml`) for health check settings.

If no health endpoint found → FAIL.

### Pass criteria

A `/health` or `/api/health` endpoint exists returning 200. Ideally checks database connectivity too.

### Fix approach

Create `app/api/health/route.ts` returning `{ status: 'ok', timestamp: Date.now() }`. For deeper checks, add DB ping. **Effort: Small**

---

## I6: No Logging in Production

**Severity:** HIGH

When something breaks in production, you have zero forensic data. No logs means guessing.

### Detection

Check `package.json` for logging libraries:
`pino`, `winston`, `bunyan`, `loglevel`, `@axiomhq/js`, `@logtail/js`, `sentry`, `@sentry/nextjs`

Search for structured logging usage:
```
logger.|log.(info|warn|error|debug)
```
(exclude `console.` matches)

Check for error tracking:
```
Sentry|captureException|captureMessage|LogRocket|Bugsnag
```

If only `console.log` and no structured logging or error tracking → FAIL.

### Pass criteria

Structured logging library installed and used. Error tracking configured (Sentry, etc.). Logs shipped to a queryable service in production.

### Fix approach

Install `pino` for structured logging or `@sentry/nextjs` for error tracking. Replace `console.log` with structured calls. Configure log drain. **Effort: Medium**

---

## I7: No Backup Strategy

**Severity:** CRITICAL

One bad migration, one accidental DELETE, and all user data is gone permanently.

### Detection

Check database provider:
- **Neon:** Point-in-time recovery on paid plans → check plan level
- **Supabase:** Daily backups on paid plans
- **PlanetScale:** Automatic backups
- **Vercel Postgres (Neon):** Check plan
- **Self-hosted:** Search for backup scripts:
  ```
  pg_dump|mysqldump|mongodump|backup
  ```
  in shell scripts, cron configs, CI workflows

If using managed DB with automatic backups → PASS (note plan level).
If self-hosted with no backup scripts → FAIL.
If no database → SKIP.

### Pass criteria

Database has automated backups. Point-in-time recovery available. Restoration tested at least once.

### Fix approach

**Managed DB:** Verify backup settings in dashboard. Upgrade plan if needed. **Self-hosted:** Set up `pg_dump` cron, store in S3. Test restoration. **Effort: Medium**

---

## I8: Hardcoded Secrets in Codebase

**Severity:** CRITICAL

Secrets committed to git live in repository history forever — even after deletion. Anyone with repo access has your keys.

### Detection

Search source files (exclude node_modules, .git):
```
password\s*=\s*['"][^'"]{8,}
secret\s*=\s*['"][^'"]{8,}
```
in `*.ts`, `*.js`, `*.json`, `*.yaml`, `*.yml`

Check if `.env` files are tracked by git:
```
git ls-files | grep \.env (exclude .example, .sample, .template)
```

Check `.gitignore` for env file exclusions:
```
grep \.env .gitignore
```

If `.env` files not in `.gitignore` → FAIL.
If secrets found in source files → FAIL.

### Pass criteria

`.env*` files (except `.env.example`) in `.gitignore`. No secrets in source code. No `.env` files in git history.

### Fix approach

Add `.env`, `.env.local`, `.env*.local` to `.gitignore`. Create `.env.example` with placeholders. If secrets were previously committed, rotate them immediately — removing from code does not remove from git history. **Effort: Small**
