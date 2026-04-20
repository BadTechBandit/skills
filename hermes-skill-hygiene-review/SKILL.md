---
name: hermes-skill-hygiene-review
description: Review active skills a Hermes agent created over time, then surface only questionable archive or merge candidates so the live skills directory stays useful instead of filling with stale one-off fixes. Use when: "review our skills", "audit stale skills", "what should we archive", "skill hygiene", "clean up agent-created skills", or any request to identify active skills that probably do not deserve to stay loaded.
---

# Hermes skill hygiene review

Review only the active custom skills a Hermes agent created over time. Surface only questionable archive or merge candidates. Touch nothing automatically.

## First principles

- Keep repeatable workflows as active skills.
- Store reusable facts or lessons in memory.
- Archive one-off incidents instead of leaving them active.

## Review scope

- Inspect active skills under `~/.hermes/skills/`.
- Ignore official, community, or package-installed skills.
- Ignore explicit user-curated long-term workflow skills.
- Ignore anything already under `~/.hermes/skills-archive/`.
- If you are not confident a skill belongs to the agent-created subset, skip it.

## Evidence order

1. Read the skill file.
2. Check recent session history only if recurrence is unclear.
3. Check memory or Honcho only if ongoing relevance is still unclear.
4. Do not pull in extra sources unless the decision is stuck.

## Review questions

Ask only:

1. Did this come up more than once?
2. Is it tied to an ongoing project or system?
3. Is it a reusable procedure instead of a one-off diagnosis?

## Surface rule

Surface a skill only if two or more answers are false or unclear.
Do not list obvious keepers.

## Recommendation rule

Use only:

- `archive candidate`
- `merge candidate`

Do not recommend deletion.
Do not move, patch, archive, or delete anything.
Leave the human in the loop.

## Output

If nothing is questionable:

- for manual use, reply exactly: `No questionable Hermes-created skills this week.`
- for scheduled cron use, reply exactly: `[SILENT]`

If something is questionable, reply in this format:

```text
Skills in question this week

1. /full/path/to/skill
   Recommendation: archive candidate|merge candidate
   Why: one short reason
```

Keep the report short.
Only list skills that are actually in question.
