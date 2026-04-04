# Code Quality Checks

4 checks for error handling, type safety, request limits, and dependency hygiene.

---

## C1: No Error Boundaries in the UI

**Severity:** MEDIUM

One unhandled error in any component crashes the entire page to a white screen. User sees nothing, can do nothing, leaves forever.

### Detection

**Next.js App Router:** Check for error boundary files:
```
find error.tsx, error.ts, error.jsx, error.js in app/ directory
find global-error.tsx, global-error.ts in app/ directory
```

If no `error.tsx` files exist → FAIL.
If `app/error.tsx` exists but no `app/global-error.tsx` → partial FAIL.

**React (generic):** Search for ErrorBoundary usage:
```
ErrorBoundary|componentDidCatch|getDerivedStateFromError|react-error-boundary
```

Check `package.json` for `react-error-boundary`.

If no error boundary implementation found → FAIL.

### Pass criteria

**Next.js:** At minimum `app/error.tsx` and `app/global-error.tsx` exist. Ideally error boundaries at critical route segments.
**React:** ErrorBoundary wraps major UI sections.

### Fix approach

**Next.js App Router:** Create `app/error.tsx` with `'use client'`, error display, and reset button. Create `app/global-error.tsx` for root layout errors. Add `error.tsx` to critical route segments. **Effort: Small**

---

## C2: No TypeScript on AI-Generated Code

**Severity:** MEDIUM

AI writes confident, wrong code. Without TypeScript, type errors hide until runtime — and users find them before you do.

### Detection

Check for TypeScript:
```
find tsconfig.json (exclude node_modules)
```

If no `tsconfig.json` → FAIL.

If TypeScript exists, check strictness:
```
strict|noImplicitAny|strictNullChecks
```
in `tsconfig.json`.

If `strict: false` or `strict` not present → partial FAIL (TypeScript exists but not strict).

Check for `.js`/`.jsx` files in source directories alongside TypeScript:
```
find *.js, *.jsx in app/, pages/, src/, lib/, components/ directories
```

If significant JS files exist in a TypeScript project → partial FAIL (incomplete migration).

### Pass criteria

`tsconfig.json` exists with `strict: true`. Source files use `.ts/.tsx`. Minimal `@ts-ignore` or `any` usage.

### Fix approach

**No TypeScript:** Run `npx tsc --init` with `strict: true`. Rename `.js` → `.ts`, `.jsx` → `.tsx`. Fix type errors.
**Not strict:** Set `"strict": true` in `tsconfig.json`, fix errors incrementally. **Effort: Large**

---

## C3: No Request Size Limits

**Severity:** HIGH

Without size limits, an attacker sends a 1GB POST body and crashes your server or exhausts memory.

### Detection

**Next.js App Router:** Search route files for:
```
bodySizeLimit|export.*config.*bodyParser
```
Also check `next.config.*` for body size configuration.

**Next.js Pages Router:** Search `pages/api/**` for:
```
config.*bodyParser.*sizeLimit
```

**Express:** Search for:
```
express.json.*limit|bodyParser.*limit|urlencoded.*limit
```

**Generic:** Search for:
```
content-length|maxBodySize|bodyLimit|payload.*limit
```

If no request size limits found → FAIL.

### Pass criteria

Request body size limits configured globally or per-route. Typical default: 1-10MB. File upload routes have explicit, higher limits where needed.

### Fix approach

**Next.js Pages Router:** Add `export const config = { api: { bodyParser: { sizeLimit: '1mb' } } }` to route files.
**Next.js App Router:** Configure in route segment config or `next.config.ts`.
**Express:** `app.use(express.json({ limit: '1mb' }))`. **Effort: Small**

---

## C4: npm Packages Not Audited

**Severity:** MEDIUM

Known vulnerabilities sitting in `node_modules` because nobody ran `npm audit` since initial setup. One critical CVE in a transitive dependency and you're exposed.

### Detection

Check if the project has a lock file:
```
package-lock.json|yarn.lock|pnpm-lock.yaml|bun.lockb
```

If lock file exists, run or simulate an audit check:
- Search for `npm audit`, `yarn audit`, or `pnpm audit` in CI/CD workflows (`.github/workflows/*`, `Makefile`, `package.json` scripts)
- Check for automated dependency tools: `dependabot.yml`, `renovate.json`, `.snyk`

If no audit in CI AND no automated dependency update tool configured → FAIL.

Also check lock file age — if `package-lock.json` hasn't been modified in 90+ days and no dependency management tool is configured → FAIL.

### Pass criteria

`npm audit` (or equivalent) runs in CI pipeline. Or: automated dependency update tool (Dependabot, Renovate, Snyk) is configured and active. Known critical/high vulnerabilities are addressed.

### Fix approach

**Quick:** Run `npm audit` and fix critical/high issues with `npm audit fix`. **Sustainable:** Add Dependabot (`.github/dependabot.yml`) or Renovate (`renovate.json`) for automated PR creation on dependency updates. Add `npm audit --audit-level=high` to CI pipeline. **Effort: Small**
