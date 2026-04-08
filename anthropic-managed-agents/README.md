# Anthropic Managed Agents

Design, scope, and set up Anthropic Claude Managed Agents — the hosted agent runtime where Anthropic runs Claude in cloud containers with tools, MCP connectors, and skills. This skill is a primer + setup coach that turns a rough idea or concrete spec into a setup blueprint you hand to your coding agent.

**Use it when you want to:**
- Turn a repetitive workflow into a hosted agent
- Design an in-app agent for a SaaS product
- Evaluate whether Managed Agents is the right fit vs. rolling your own with the Agent SDK
- Get a primer on how Managed Agents works (concepts, quickstart, MCP, skills, auth, limits)

## Install

```bash
npx skills add https://github.com/BadTechBandit/skills --skill anthropic-managed-agents
```

Browse on skills.sh: https://skills.sh/BadTechBandit/skills/anthropic-managed-agents

### Manual install

```bash
git clone https://github.com/BadTechBandit/skills.git
ln -s $(pwd)/skills/anthropic-managed-agents ~/.claude/skills/anthropic-managed-agents
ln -s $(pwd)/skills/anthropic-managed-agents ~/.codex/skills/anthropic-managed-agents
```

## How it works

Invoke with no arguments to get a menu (explain, brainstorm, fit check, or bring your own idea). Invoke with a concrete idea to jump straight to a structured blueprint: fit check → agent design → auth plan → setup steps → open questions → next action.

```
anthropic-managed-agents/
├── SKILL.md              # Mode selector + workflow + guardrails
└── references/
    └── primer.md         # Full primer: concepts, quickstart, MCP, skills, auth, limits, doc links
```

Detailed content lives in `references/primer.md` and is loaded on demand so the skill stays lean.

## Source

Based on the official [Anthropic Managed Agents docs](https://platform.claude.com/docs/en/managed-agents/overview).

## License

MIT
