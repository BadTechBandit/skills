---
name: anthropic-managed-agents
description: Design, scope, and set up Anthropic Claude Managed Agents — the hosted agent runtime where Anthropic runs Claude in cloud containers with tools, MCP connectors, and skills. Use when the user wants to (1) turn a repetitive workflow into a hosted agent, (2) design an in-app agent for a SaaS product, (3) evaluate whether Managed Agents is the right fit vs. rolling a custom agent with the SDK, or (4) get a primer on how Managed Agents works. Triggers on "managed agent", "hosted agent", "claude agent for X", "automate this with an agent", or the slash command /anthropic-managed-agents.
---

# Managed Agents

Guide the user from a rough idea or a concrete spec to a working Anthropic Managed Agents configuration. This skill is a primer + setup coach, not a code generator. Hand off actual implementation to Claude Code / Codex once the design is locked.

**Authoritative docs:** https://platform.claude.com/docs/en/managed-agents/overview
**Beta header required on every API request:** `anthropic-beta: managed-agents-2026-04-01`

## Mode selection

Check how the skill was invoked:

- **No arguments / vague opener** → run **Mode A: Menu**.
- **Arguments or a concrete idea provided up front** → run **Mode B: Direct walkthrough**.

### Mode A — Menu

Present exactly these four options and wait for the user to pick one:

1. **I have a specific agent idea** — user provides it; go to Mode B.
2. **Brainstorm from a rough problem** — ask 3-5 targeted questions to extract: what repetitive task, who triggers it, what systems it touches, what "done" looks like, how often it runs. Then go to Mode B.
3. **Fit check an existing workflow** — user describes a current process; assess whether Managed Agents is the right tool (see `references/primer.md` → "When to use it"). Recommend Managed Agents, Agent SDK, or plain Messages API.
4. **Just explain Managed Agents to me** — load `references/primer.md` and summarize core concepts in under 300 words. Offer to dive into MCP, skills, auth, or limits as follow-ups.

### Mode B — Direct walkthrough

Produce a structured blueprint in this exact order. Skip sections only if clearly not applicable.

1. **Fit check** — Is Managed Agents the right tool? 1-2 sentences with reasoning. If it's a bad fit, stop and recommend the alternative (Agent SDK, Messages API, n8n, etc.).
2. **Agent design**
   - Model choice (default: `claude-sonnet-4-6`; use Opus only if the task needs deep reasoning)
   - System prompt (draft 3-8 lines, imperative voice)
   - Tools — built-in `agent_toolset_20260401` on/off, plus any custom tools
   - MCP connectors needed (list external systems → map each to an MCP server URL)
   - Skills to attach (Anthropic pre-built: `xlsx`, `pdf`, `pptx`, `docx`; or custom skill IDs)
   - Environment needs (networking: unrestricted vs. limited + allowlist; preinstalled packages)
3. **Auth plan** — Which vaults are needed? Per-user or shared? Flag where OAuth credentials come from.
4. **Setup steps** — Minimal ordered list: create environment → create agent → create vault(s) → create session → send first event. Reference `references/primer.md` §Quickstart for code shape; don't re-render unless asked.
5. **Open questions & gaps** — Anything the docs don't cover for this use case. Pull from `references/primer.md` §Open questions if relevant. Link to the exact doc page the user should read.
6. **Next action** — One concrete next step (e.g., "Hand this blueprint to Claude Code and have it scaffold the Python client").

## Reference material

All detailed content lives in `references/primer.md` — a single-file primer covering what Managed Agents is, when to use it, core concepts (agents, environments, sessions, events, MCP, skills), quickstart code, MCP connector wiring, skills attachment, auth via vaults, limits, further-reading links, and known documentation gaps. **Load it when:**

- The user picks Mode A option 4 (explain it)
- Mode B needs quickstart code, MCP wiring details, or auth specifics
- The user asks any factual question about Managed Agents behavior

Do not duplicate primer content into SKILL.md — read from the reference file instead.

## Guardrails

- **Don't guess.** If the primer lists something in "Open questions" (pricing specifics, vault creation API, container specs), say so and point to the overview URL rather than inventing an answer.
- **Don't generate full implementations.** This skill outputs blueprints. Implementation is handed off.
- **Flag auth early.** Any agent touching external systems needs a vault plan before setup steps — surface it in Mode B step 3, not as an afterthought.
- **Default to Sonnet 4.6** for agent model unless the task genuinely needs Opus reasoning.
- **When in doubt, link out.** Every open question should have a doc URL next to it.
