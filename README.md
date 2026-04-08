# Agent Skills

Open-source skills for AI coding agents. Works with Claude Code, Codex, Cursor, Windsurf, and any agent that reads `SKILL.md` files.

Some of these skills I found and kept. Some I built at 2 a.m. when nothing else fit. If it's here, it worked.

— Roman

## Skills

| Skill | Description |
|-------|-------------|
| [anthropic-managed-agents](./anthropic-managed-agents/) | Primer + setup coach for Anthropic Claude Managed Agents — the hosted agent runtime with tools, MCP connectors, and skills. Turns an idea or spec into a setup blueprint. |
| [vibe-audit](./vibe-audit/) | Audit your vibe-coded app against 34 security, performance, and code quality checks. Generates a report with pass/fail scorecard and self-contained fix files. |
| [moat-analyzer](./moat-analyzer/) | Analyze any business against the 10 Moats framework. Scores LLM disruption risk, moat durability, and identifies startup opportunities. |
| [marketing-skills-for-agents](./marketing-skills-for-agents/) | 34 marketing skills — CRO, copywriting, SEO, GEO, analytics, paid ads, email sequences, sales enablement, and growth engineering. |

Every skill folder has its own `README.md` with full description and manual install fallback.

## Install a single skill

### anthropic-managed-agents

```bash
npx skills add https://github.com/BadTechBandit/skills --skill anthropic-managed-agents
```

### vibe-audit

```bash
npx skills add https://github.com/BadTechBandit/skills --skill vibe-audit
```

### moat-analyzer

```bash
npx skills add https://github.com/BadTechBandit/skills --skill moat-analyzer
```

### marketing-skills-for-agents

```bash
npx skills add https://github.com/BadTechBandit/skills@marketing-skills-for-agents
```

## Install the whole repo

```bash
npx skills add BadTechBandit/skills
```

## Manual install (any skill)

```bash
git clone https://github.com/BadTechBandit/skills.git
ln -s $(pwd)/skills/<skill-name> ~/.claude/skills/<skill-name>
ln -s $(pwd)/skills/<skill-name> ~/.codex/skills/<skill-name>
```

## How it works

Each skill is a folder with a `SKILL.md` file and optional `references/`, `scripts/`, and `assets/` directories. Your coding agent reads `SKILL.md` and follows the instructions using whatever tools it has available.

```
vibe-audit/
├── README.md                         # Landing page + install
├── SKILL.md                          # Main instructions (what your agent reads)
└── references/
    ├── security.md                   # 10 checks
    ├── database-performance.md       # 4 checks
    ├── infrastructure.md             # 8 checks
    └── code-quality.md               # 3 checks
```

---

## For agents: how to add a new skill to this repo

When Roman asks you to add a new skill to this repository, follow this exact workflow. No back-and-forth — everything you need is here.

### 1. Choose a skill name

- Lowercase, kebab-case (`my-new-skill`)
- Descriptive and specific — name matches the frontmatter `name:` field and the folder name
- Avoid personal/project branding; prefer generic names that describe the capability (e.g. `anthropic-managed-agents`, not `roman-agents`)

### 2. Create the skill folder

At the repo root:

```
skills/<skill-name>/
├── README.md          # Landing page for anyone who opens just this folder on GitHub
├── SKILL.md           # The actual skill (frontmatter + instructions your agent reads)
└── references/        # Optional: detail files loaded on demand
    └── *.md
```

Use `scripts/` or `assets/` subdirectories only when genuinely needed (executable code or output templates). Most skills don't need them.

### 3. Write the SKILL.md

Frontmatter must have exactly two fields:

```yaml
---
name: <skill-name>
description: <what it does + explicit "use when" triggers — this is the only thing the agent reads to decide whether to load the skill>
---
```

Rules for the body:
- Imperative voice ("Do X", not "You should do X")
- Keep under ~500 lines — move detail into `references/*.md`
- No "when to use this" sections in the body (put that in frontmatter description)
- No README-style preamble, changelog, or setup instructions inside SKILL.md

### 4. Scrub PII

Before committing, check for personal names, company names, project names, emails, and internal product names. Replace with generic examples ("a SaaS product", "an internal tool"). Run:

```bash
grep -riE "roman|gotmoat|curo|flyworx|creworx|glitch" skills/<skill-name>/
```

Zero hits = safe to commit.

### 5. Write the per-skill README.md

Every skill needs its own `README.md` following this exact template:

```markdown
# <Skill Display Name>

<One-paragraph description of what the skill does. Mirror the SKILL.md frontmatter but in human-friendly prose.>

**Use it when you want to:**
- <Concrete use case 1>
- <Concrete use case 2>
- <Concrete use case 3>

## Install

​```bash
npx skills add https://github.com/BadTechBandit/skills --skill <skill-name>
​```

Browse on skills.sh: https://skills.sh/BadTechBandit/skills/<skill-name>

### Manual install

​```bash
git clone https://github.com/BadTechBandit/skills.git
ln -s $(pwd)/skills/<skill-name> ~/.claude/skills/<skill-name>
ln -s $(pwd)/skills/<skill-name> ~/.codex/skills/<skill-name>
​```

## How it works

<File tree + 1-3 sentences on the workflow.>

## License

MIT
```

### 6. Update the main README

Two places to edit at the top of this file:

**a. Add a row to the `## Skills` table:**

```
| [skill-name](./skill-name/) | <Short description, ~1-2 sentences> |
```

**b. Add an install block under `## Install a single skill`:**

```markdown
### skill-name

​```bash
npx skills add https://github.com/BadTechBandit/skills --skill skill-name
​```
```

Keep both lists in the same order. Newest skills at the top of the table = newest install block at the top of the install section.

### 7. Do not push

Stop after files are written and the table is updated. Show Roman a summary of what changed. He reviews and pushes.
