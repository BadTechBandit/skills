# Humanizer

Humanizer helps your coding agent rewrite text so it sounds more natural, specific, and human. It removes common AI writing habits like inflated significance, vague authority claims, fake depth, promotional language, repetitive structure, and over-polished phrasing.

**Use it when you want to:**
- Clean up a draft that sounds obviously AI-written
- Rewrite stiff or generic text without changing the meaning
- Make marketing, product, or editorial copy sound more natural

## Install

```bash
npx skills add https://github.com/BadTechBandit/skills --skill humanizer
```

Browse on skills.sh: https://skills.sh/BadTechBandit/skills/humanizer

### Manual install

```bash
git clone https://github.com/BadTechBandit/skills.git
ln -s $(pwd)/skills/humanizer ~/.claude/skills/humanizer
ln -s $(pwd)/skills/humanizer ~/.codex/skills/humanizer
```

## How it works

```text
humanizer/
├── README.md
└── SKILL.md
```

Ask your agent to humanize a draft, and it will identify AI-sounding patterns, rewrite the text, and keep the original meaning and tone intact.

## License

MIT
