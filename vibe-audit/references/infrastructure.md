# Infrastructure & Ops Checks

10 checks for payment security, environment config, observability, and operational resilience.

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

## I3: Images Uploaded Directly to Server + No MIME Validation

**Severity:** MEDIUM (storage) / HIGH (MIME validation)

Local file storage means no CDN, no optimization, massive bandwidth costs, and lost files on redeploy. No MIME validation means attackers can upload executable files disguised as images.

### Detection

**Storage check:**
Check for file upload handling:
```
multer|formidable|busboy|writeFile.*upload|createWriteStream|fs.write
```

Check for cloud storage:
```
@vercel/blob|@aws-sdk/client-s3|cloudinary|uploadthing|supabase.*storage|firebase.*storage
```
in `package.json`.

If file uploads exist but no cloud storage → FAIL (storage).

Also check for `public/uploads` or similar directories in the project.

**MIME type validation check:**
If file uploads exist, search for MIME type / file extension validation:
```
mimetype|mime-type|content-type|fileFilter|accept.*image|allowedTypes|allowedExtensions
file\.type|\.endsWith|\.extension|magic-bytes|file-type
```

Check multer config for `fileFilter` function.
Check formidable config for `filter` or file type checks.

If file uploads exist WITHOUT MIME type validation → FAIL (MIME).

If no file uploads found → SKIP.

### Pass criteria

File uploads go to cloud storage with CDN delivery. Upload handlers validate MIME type and file extension server-side. Only explicitly allowed file types accepted. No files saved to local filesystem in production.

### Fix approach

**Storage:** Use `@vercel/blob` (Vercel), Cloudinary, or S3. Serve via CDN. Use `next/image` for optimization.
**MIME:** Add `fileFilter` to multer or equivalent validation. Check both `file.mimetype` and extension. Use an allowlist (`image/jpeg`, `image/png`, etc.), never a blocklist. Consider using `file-type` package for magic byte validation. **Effort: Medium**

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

---

## I9: Server Running as Root

**Severity:** HIGH

A compromised process running as root owns the entire machine. Any RCE vulnerability becomes a full system takeover.

### Detection

Check for Docker configuration:
```
Dockerfile|docker-compose.*
```

If Dockerfile found, search for:
```
USER|--chown|gosu|su-exec
```

If Dockerfile exists WITHOUT a `USER` directive (defaults to root) → FAIL.
If `docker-compose.*` runs containers with `privileged: true` → FAIL.

Check for process manager configuration:
```
pm2|ecosystem.config|supervisor
```
If PM2 config found, check for `uid`/`gid` settings.

If hosting is serverless (Vercel, Cloudflare Workers, AWS Lambda) → SKIP (sandboxed by platform).
If no Dockerfile or server process config found → SKIP.

### Pass criteria

Application runs as a non-root user. Dockerfile includes `USER appuser` or equivalent. No containers run in privileged mode.

### Fix approach

Add to Dockerfile: `RUN addgroup -S app && adduser -S app -G app` then `USER app` before the `CMD` directive. Remove `privileged: true` from docker-compose. **Effort: Small**

---

## I10: Database Port Exposed to Public Internet

**Severity:** CRITICAL

An exposed database port (5432, 3306, 27017) means anyone on the internet can attempt to connect. Combined with weak credentials, it's a direct path to full data exfiltration.

### Detection

Check `docker-compose.*` for database port mappings:
```
ports:.*5432|ports:.*3306|ports:.*27017|ports:.*6379
```

If database ports are mapped to `0.0.0.0` or without host binding (e.g., `"5432:5432"` instead of `"127.0.0.1:5432:5432"`) → FAIL.

Check for firewall or network configuration:
```
fly.toml|railway.json|security-group|firewall
```

Check database connection strings for public hostnames vs internal/private networking:
```
DATABASE_URL|MONGO_URL|REDIS_URL
```
in `.env*` files. If connecting to `localhost` or private network IPs → likely PASS. If connecting to a public hostname → check if the provider handles network isolation (Neon, PlanetScale, Supabase handle this).

If hosting is serverless with managed DB (Vercel + Neon, etc.) → SKIP (handled by provider).
If no database → SKIP.

### Pass criteria

Database ports not exposed to public internet. Docker-compose binds DB ports to `127.0.0.1` only. Managed databases use private networking or provider-managed access controls.

### Fix approach

**Docker:** Change `"5432:5432"` to `"127.0.0.1:5432:5432"`. **Self-hosted:** Configure firewall to block external access to DB ports. **Cloud:** Use private networking / VPC peering. **Effort: Small**
