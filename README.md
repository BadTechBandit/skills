# Agent Skills

Open-source skills for AI coding agents. Works with Claude Code, Codex, Cursor, Windsurf, and any agent that reads SKILL.md files.

## Install

```bash
npx skills add BadTechBandit/skills
```

Or install a single skill:

```bash
npx skills add BadTechBandit/skills --skill vibe-audit
```

## Skills

| Skill | Description |
|-------|-------------|
| [vibe-audit](./vibe-audit/) | Audit your vibe-coded app against 25 security, performance, and code quality checks. Generates a report with pass/fail scorecard and self-contained fix files you hand right back to your coding agent. |
| [moat-analyzer](./moat-analyzer/) | Analyze any business against the 10 Moats framework. Scores LLM disruption risk, moat durability, and identifies startup opportunities. |

## How it works

Each skill is a folder with a `SKILL.md` file and optional `references/`, `scripts/`, and `assets/` directories. Your coding agent reads the SKILL.md and follows the instructions using whatever tools it has available.

```
vibe-audit/
├── SKILL.md                          # Main instructions
└── references/
    ├── security.md                   # 10 checks
    ├── database-performance.md       # 4 checks
    ├── infrastructure.md             # 8 checks
    └── code-quality.md               # 3 checks
```

## Manual install

If you prefer not to use the skills CLI:

```bash
git clone https://github.com/BadTechBandit/skills.git
ln -s $(pwd)/skills/vibe-audit ~/.claude/skills/vibe-audit
ln -s $(pwd)/skills/vibe-audit ~/.codex/skills/vibe-audit
```

## License

MIT
