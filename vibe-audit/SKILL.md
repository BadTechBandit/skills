---
name: vibe-audit
description: |
  Comprehensive security, performance, and code quality audit for vibe-coded applications.
  Checks 34 common issues that make apps vulnerable, slow, or fragile — from hardcoded API keys
  to missing rate limiting to absent error boundaries. Produces a structured report folder with
  pass/fail scorecard and self-contained implementation plans for every failing check.
  Use when: "audit my app", "check my code", "is this ready to ship", "security check",
  "vibe audit", "pre-launch check", "review my project", "what's wrong with my app",
  or any request to evaluate code quality and app readiness before deployment.
---

# Vibe Audit

Audit a project against 34 checks across 4 categories. Produce a report folder with a summary scorecard and self-contained fix files for every failure.

## Phase 1: Reconnaissance

Before checking anything, identify the project stack. This determines which checks apply and which file patterns to use.

Detect:
- **Framework**: Next.js (app router or pages?), Express, Fastify, Nuxt, SvelteKit, generic Node
- **Database**: Postgres, MySQL, MongoDB, SQLite, Prisma, Drizzle, Mongoose, or none
- **Auth**: NextAuth/Auth.js, Clerk, Supabase Auth, Firebase Auth, Passport, custom, or none
- **Payments**: Stripe, Paddle, LemonSqueezy, or none
- **Hosting**: Vercel, AWS, Railway, Fly.io, self-hosted, unknown
- **TypeScript**: yes/no (check for tsconfig.json)
- **Package manager**: npm/pnpm/yarn/bun (check lock files)

Detection method:
1. Read `package.json` — dependencies reveal framework, DB, auth, payments
2. Check for `next.config.*`, `nuxt.config.*`, `svelte.config.*`, `vite.config.*`
3. Check for `tsconfig.json`
4. Check for `.env*` files (DO NOT output env values, only note existence)
5. Check for `vercel.json`, `fly.toml`, `railway.json`, `Dockerfile`

Store stack results as context for Phase 2.

## Phase 2: Audit

Run checks from 4 categories. Read the reference file for each category before checking.

| Category | Reference File | Checks |
|----------|---------------|--------|
| Security | `references/security.md` | 16 checks: auth, keys, CORS, rate limiting, sessions, CSP, IDOR, open redirects, JWT secrets, password hashing, error leakage, session invalidation |
| Database & Performance | `references/database-performance.md` | 4 checks: indexing, pooling, pagination, validation |
| Infrastructure & Ops | `references/infrastructure.md` | 10 checks: webhooks, env vars, logging, backups, secrets, file uploads, root process, exposed DB ports |
| Code Quality | `references/code-quality.md` | 4 checks: error boundaries, TypeScript, request limits, npm audit |

### Execution strategy

If the agent supports parallel execution (subagents, background tasks, parallel tool calls), dispatch one task per category simultaneously. Each task:
1. Reads its reference file for detection patterns and pass/fail criteria
2. Receives the stack context from Phase 1
3. Runs all checks in its category
4. Returns results per check: ID, pass/fail/skip, severity, files involved, one-line finding

If parallel execution is not available, process categories sequentially — read one reference file at a time.

### Skip logic

Skip checks that do not apply to the detected stack:
- No database detected → skip all database-performance checks
- No Stripe/payment processor detected → skip webhook signature check
- Hosting is Vercel or Cloudflare → skip HTTPS redirect check (handled automatically)
- No admin routes found → skip admin role portion of S8
- No email sending found → skip synchronous email check
- Managed auth (Clerk, Supabase Auth, Firebase Auth) → skip password reset expiry and password hashing checks (handled by provider)
- No JWT usage found → skip JWT secret check
- No custom password handling → skip password hashing check
- No file uploads found → skip MIME validation check
- Serverless hosting (Vercel, Cloudflare Workers, Lambda) → skip root process and exposed DB port checks
- No Dockerfile or server process config → skip root process check

Mark skipped checks as `SKIP` with the reason in the report.

## Phase 3: Report

Create a `vibe-audit-report/` folder in the project root:

```
vibe-audit-report/
├── SUMMARY.md
├── priority-1-critical/
│   └── {nn}-{issue-slug}.md
├── priority-2-high/
│   └── {nn}-{issue-slug}.md
└── priority-3-medium/
    └── {nn}-{issue-slug}.md
```

Only create issue files for FAILING checks. Passing and skipped checks appear only in SUMMARY.md. Only create priority folders that contain failures.

### SUMMARY.md format

```markdown
# Vibe Audit Report

**Project:** {name from package.json}
**Date:** {YYYY-MM-DD}
**Stack:** {framework} + {database} + {auth} + {hosting}

## Scorecard

| # | Check | Severity | Status | Finding |
|---|-------|----------|--------|---------|
| S1 | Rate limiting on API routes | CRITICAL | FAIL | No rate limiting on 12 API routes |
| S2 | Auth token storage | CRITICAL | PASS | Using httpOnly cookies via Clerk |
| ... | | | | |

## Stats

- **Passed:** X/34
- **Failed:** Y/34
- **Skipped:** Z/34
- **Critical failures:** N
- **High failures:** N
- **Medium failures:** N

## Next Steps

Fix files are in `priority-1-critical/`, `priority-2-high/`, and `priority-3-medium/`.
Each file is self-contained — hand it to any coding agent to implement the fix.
Start with Priority 1 (critical security and data loss risks).
```

### Issue file format

Each issue file must be fully self-contained. A user hands this single file to any coding agent and gets the fix implemented without needing additional context.

```markdown
# {Check Name}

**Severity:** {CRITICAL | HIGH | MEDIUM}
**Category:** {Security | Database & Performance | Infrastructure | Code Quality}
**Effort:** {Small | Medium | Large}

## Problem

{2-3 sentences in plain English. What can go wrong. Why it matters.}

## Found In

- `path/to/file.ts:42` — {what's wrong here}
- `path/to/other-file.ts:15` — {what's wrong here}

## Fix

### Steps

1. {First step with key code snippet}
2. {Second step}
3. {Verification — how to confirm the fix works}
```

### Severity classification

| Severity | Criteria | Examples |
|----------|----------|---------|
| CRITICAL | Security breach, data loss, or financial exposure | Hardcoded keys, no auth on admin, SQL injection, no backups |
| HIGH | Performance failure at scale or operational blindness | No indexing, no logging, no connection pooling, no CSP |
| MEDIUM | Best practice violation, quality concern | No error boundaries, no health check, no TypeScript |

## Chat Output

After creating the report folder, output to the user:
1. The full SUMMARY.md scorecard table
2. Count of issues by priority
3. Note: "Fix files saved to `vibe-audit-report/` — hand any file to your coding agent to implement."
