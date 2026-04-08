# Vibe Audit

Comprehensive security, performance, and code quality audit for vibe-coded applications. Checks 34 common issues that make apps vulnerable, slow, or fragile — from hardcoded API keys to missing rate limiting to absent error boundaries. Produces a structured report folder with a pass/fail scorecard and self-contained implementation plans for every failing check that you hand right back to your coding agent.

**Use it when you want to:**
- Pre-launch audit a vibe-coded project before shipping
- Security-check an app you didn't originally write
- Get a prioritized punch list of what to fix, with ready-to-run fix files

## Install

```bash
npx skills add https://github.com/BadTechBandit/skills --skill vibe-audit
```

Browse on skills.sh: https://skills.sh/BadTechBandit/skills/vibe-audit

### Manual install

```bash
git clone https://github.com/BadTechBandit/skills.git
ln -s $(pwd)/skills/vibe-audit ~/.claude/skills/vibe-audit
ln -s $(pwd)/skills/vibe-audit ~/.codex/skills/vibe-audit
```

## How it works

```
vibe-audit/
├── SKILL.md                          # Main instructions + phases
└── references/
    ├── security.md                   # 10 checks
    ├── database-performance.md       # 4 checks
    ├── infrastructure.md             # 8 checks
    └── code-quality.md               # 3 checks
```

Run by asking your agent to "vibe audit my app" or "audit this project". The skill recons the stack, runs the relevant checks, scores them, and writes a report folder with fix files.

## License

MIT
