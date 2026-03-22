# Database & Performance Checks

4 checks for database configuration, query patterns, and input validation.

Skip all checks in this category if no database is detected.

---

## D1: No Database Indexing on Queried Fields

**Severity:** HIGH

Without indexes, every query scans the entire table. Works at 100 rows, completely dies at 10,000.

### Detection

**Prisma:** Check schema for indexes:
```
@@index|@unique|@@unique
```
in `prisma/schema.prisma`. Then identify queried fields by searching for `where: {` patterns in source code. Compare queried fields against indexed fields.

**Drizzle:** Search for `index(` or `uniqueIndex(` in schema definitions.

**Mongoose:** Search for `.index(` or `.createIndex(` on schema fields.

**Raw SQL:** Search for `CREATE INDEX` in migration files.

If database exists but no index definitions found beyond primary keys → FAIL.

### Pass criteria

Fields used in WHERE clauses, ORDER BY, foreign key lookups, and frequently queried filters have indexes defined in schema or migrations.

### Fix approach

**Prisma:** Add `@@index([fieldName])` to models, run `npx prisma migrate dev`. **Mongoose:** Add `.index()` to schema fields. **SQL:** `CREATE INDEX idx_name ON table(column)`. Priority targets: user lookups (email, username), foreign keys, status fields, date ranges. **Effort: Medium**

---

## D2: No Pagination on Database Queries

**Severity:** HIGH

A single API call fetches your entire table into memory. At scale, this kills the server and the database simultaneously.

### Detection

Search for unbounded collection queries:

**Prisma:** `findMany(` without `take:` or `cursor:`
**Mongoose:** `.find(` without `.limit(`
**Drizzle:** `.select(` without `.limit(`
**Raw SQL:** `SELECT * FROM` without `LIMIT`

If list/collection queries exist without limits → FAIL.

### Pass criteria

All queries returning collections have explicit limits. API endpoints returning lists accept pagination parameters (page/limit or cursor).

### Fix approach

Add `take: 50` (Prisma), `.limit(50)` (Mongoose/Drizzle), or `LIMIT 50` (SQL) to all collection queries. Implement pagination on API endpoints — accept `page`/`limit` or `cursor` params. **Effort: Small**

---

## D3: No Database Connection Pooling

**Severity:** HIGH

Without pooling, every request opens a new database connection. First traffic spike exhausts connection limits and crashes everything.

### Detection

**Prisma:** Search for `new PrismaClient` — if it appears inside request handlers or in multiple files without a singleton pattern → FAIL. Check for:
```
globalThis.*prisma|global.*prisma
```
If no global singleton → FAIL.

**pg (node-postgres):** If using `new Client()` per request instead of `new Pool()` → FAIL.

**Mongoose:** If `mongoose.connect()` called inside request handlers → FAIL.

**Serverless drivers:** Check for `@neondatabase/serverless`, `@planetscale/database` — these handle pooling internally. If used correctly → PASS.

### Pass criteria

Database client is a singleton (created once, reused). Connection pooling configured. Serverless functions use appropriate pooling strategy.

### Fix approach

**Prisma:** Create global singleton: `globalThis.prisma ??= new PrismaClient()`. **pg:** Use `new Pool()` instead of `new Client()`. **Serverless:** Use `@neondatabase/serverless` or Prisma Accelerate. **Effort: Small**

---

## D4: No Request Validation Middleware

**Severity:** HIGH

API routes accept any shape of data. Invalid types cause runtime crashes, malformed input corrupts your database.

### Detection

1. Check `package.json` for validation libraries:
   `zod`, `joi`, `yup`, `class-validator`, `ajv`, `valibot`, `typebox`
2. If none found → likely FAIL
3. If found, check usage on API routes — search for:
   `.parse(`, `.safeParse(`, `.validate(`, `.validateSync(`, `validateRequest`, `validateBody`
   in route files (exclude test files)
4. Count API routes vs routes with validation. If <50% use schema validation → FAIL

### Pass criteria

Validation library installed AND used on the majority of API routes. Every route accepting POST/PUT/PATCH validates the request body against a schema.

### Fix approach

Install `zod`. Define a schema per route's expected input. Validate with `schema.safeParse(body)` at the top of every handler. Return 400 with error details on failure. **Effort: Medium**
