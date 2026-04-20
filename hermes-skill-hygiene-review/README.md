# Hermes skill hygiene review

Review the active custom skills a Hermes agent has created over time and surface only the ones that look stale, overly narrow, or better merged into broader skills. This keeps the live skills directory useful instead of letting it turn into a pile of fossilized one-off fixes.

**Use it when you want to:**
- Review which agent-created skills probably should not stay active
- Surface archive or merge candidates without automatically touching anything
- Add a lightweight weekly hygiene pass to a Hermes setup that creates skills autonomously

## Install

```bash
npx skills add https://github.com/BadTechBandit/skills --skill hermes-skill-hygiene-review
```

Browse on skills.sh: https://skills.sh/BadTechBandit/skills/hermes-skill-hygiene-review

### Manual install

```bash
git clone https://github.com/BadTechBandit/skills.git
ln -s $(pwd)/skills/hermes-skill-hygiene-review ~/.claude/skills/hermes-skill-hygiene-review
ln -s $(pwd)/skills/hermes-skill-hygiene-review ~/.codex/skills/hermes-skill-hygiene-review
```

## How it works

```
hermes-skill-hygiene-review/
├── README.md
└── SKILL.md
```

Run it manually or from cron. The skill inspects only active agent-created skills, asks three simple recurrence and reuse questions, and returns a short list of archive or merge candidates for a human to review.

## License

MIT
