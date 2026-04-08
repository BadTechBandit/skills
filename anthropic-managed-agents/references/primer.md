# Managed Agents — Primer

Distilled reference for Anthropic Claude Managed Agents. Source: https://platform.claude.com/docs/en/managed-agents/overview (crawled 2026-04-08).

**Beta status:** Currently in beta. Every API request requires `anthropic-beta: managed-agents-2026-04-01` (SDKs set this automatically).

## What it is

Claude Managed Agents is a hosted agent harness where Anthropic runs Claude as an autonomous agent inside secure cloud containers on their infrastructure. Instead of building your own agent loop, tool execution layer, sandbox, and runtime, you define an agent configuration once and launch sessions against it via API. The platform handles container provisioning, prompt caching/compaction, event streaming, and stateful session history.

## When to use it

**Good fit:**
- Long-running tasks (minutes to hours) with many tool calls
- You want cloud-hosted, isolated containers without managing infra
- Asynchronous workloads — agent runs while your app streams back results
- Stateful sessions with persistent file systems and conversation history
- Multi-agent orchestration (research preview — request access)

**Bad fit:**
- You need fine-grained control over the agent loop or custom retry logic → use **Agent SDK**
- Simple one-shot prompts with no tool use → use the **Messages API** directly
- Tight latency requirements where container provisioning overhead matters
- Workloads that require custom sandboxing or bring-your-own infra

## Core concepts

**Agent** — A versioned, reusable configuration object: model, system prompt, tools, MCP servers, skills. Create once, reference by ID. Updates increment the version; sessions can be pinned to a specific version. Docs: `/managed-agents/agent-setup`

**Environment** — A cloud container template: pre-installed packages (pip, npm, apt, cargo, gem, go), networking rules (unrestricted or limited with explicit allowlists), and mounted files. Multiple sessions share an environment definition but each gets its own isolated container instance. Docs: `/managed-agents/environments`

**Session** — A running agent instance. Binds an agent + environment, maintains conversation history, tracks state: `idle` → `running` → `idle` (or `rescheduling` / `terminated` on errors). Sessions do not share filesystem state. Docs: `/managed-agents/sessions`

**Events** — The communication layer, streamed over SSE.
- You send: `user.message`, `user.interrupt`, `user.custom_tool_result`, `user.tool_confirmation`
- You receive: `agent.message`, `agent.tool_use`, `agent.tool_result`, `agent.mcp_tool_use`, `session.status_idle`, `session.error`, and span events for observability
- Docs: `/managed-agents/events-and-streaming`

**MCP Servers** — External tool providers connected via the Model Context Protocol. Declared by URL on the agent; credentials supplied separately at session creation via vaults. Docs: `/managed-agents/mcp-connector`

**Skills** — Reusable, filesystem-based knowledge bundles that load on demand. Two types: Anthropic pre-built (`xlsx`, `pdf`, `pptx`, `docx`) and custom (org-authored). Max 20 skills per session. Docs: `/managed-agents/skills`

## Quickstart

Four-step minimal flow:

**1. Create an environment**
```python
environment = client.beta.environments.create(
    name="quickstart-env",
    config={"type": "cloud", "networking": {"type": "unrestricted"}},
)
```

**2. Create an agent**
```python
agent = client.beta.agents.create(
    name="Coding Assistant",
    model="claude-sonnet-4-6",
    system="You are a helpful coding assistant.",
    tools=[{"type": "agent_toolset_20260401"}],
)
```

**3. Start a session**
```python
session = client.beta.sessions.create(
    agent=agent.id,
    environment_id=environment.id,
)
```

**4. Send a message and stream**
```python
with client.beta.sessions.events.stream(session.id) as stream:
    client.beta.sessions.events.send(session.id, events=[{
        "type": "user.message",
        "content": [{"type": "text", "text": "Write a hello world Python script."}]
    }])
    for event in stream:
        if event.type == "agent.message":
            print(event.content[0].text)
        elif event.type == "session.status_idle":
            break
```

**CLI alternative:** `brew install anthropics/tap/ant`, then `ant beta:agents create`, `ant beta:sessions create`, etc.

**SDKs available:** Python, TypeScript, Go, Java, C#, Ruby, PHP.

## MCP connectors

MCP configuration is split into two stages so secrets stay out of reusable agent definitions.

**Stage 1 — Declare on the agent (no auth)**
```python
agent = client.beta.agents.create(
    mcp_servers=[{"type": "url", "name": "github", "url": "https://api.githubcopilot.com/mcp/"}],
    tools=[
        {"type": "agent_toolset_20260401"},
        {"type": "mcp_toolset", "mcp_server_name": "github"},
    ],
)
```

**Stage 2 — Supply credentials via vault at session creation**
```python
session = client.beta.sessions.create(
    agent=agent.id,
    environment_id=environment.id,
    vault_ids=[vault.id],
)
```

**What's supported:** Any remote MCP server with an HTTP endpoint using MCP streamable HTTP transport. No "blessed" first-party named connectors are enumerated in the docs — you wire any MCP-compatible server by URL yourself. Anthropic handles OAuth token refresh when credentials are stored in vaults.

**Default permission policy for MCP tools:** `always_ask` — the agent emits `agent.mcp_tool_use` and waits for `user.tool_confirmation` before executing. Override in agent config if you want auto-approve.

**Networking:** In `limited` networking mode, set `allow_mcp_servers: true` to permit outbound traffic to MCP endpoints beyond the `allowed_hosts` list.

## Skills in Managed Agents

Similar to Claude Code skills (filesystem bundles, progressive disclosure), but differences:

- **Attached at agent creation** (not per-session or per-message), in the `skills` array
- **Two types:**
  - `anthropic` pre-built: use short IDs (`xlsx`, `pdf`, `pptx`, `docx`)
  - `custom`: org-authored, use `skill_*` IDs returned at upload; pin to a version or use `latest`
- **Max 20 skills per session**
- **No per-skill config** visible in the docs — attach or don't

```python
agent = client.beta.agents.create(
    skills=[
        {"type": "anthropic", "skill_id": "xlsx"},
        {"type": "custom", "skill_id": "skill_abc123", "version": "latest"},
    ],
)
```

Authoring custom skills: see https://platform.claude.com/docs/en/skills/overview (separate section, not Managed Agents-specific).

## Auth model — vaults

Auth uses a **vaults** abstraction:

- Register OAuth credentials or API keys into a vault once → vault gets a stable ID
- At session creation, pass `vault_ids: [vault.id]` to bind credentials to that session's MCP calls
- Anthropic manages token refresh automatically
- Secrets never appear in agent definitions (agent configs are credential-free)
- Each session can reference different vaults → **per-user auth is achievable** (one vault per end user)
- Invalid credentials: session still creates, then `session.error` fires describing the MCP auth failure. Auth retries on the next `idle → running` transition.

## Limits & pricing

**Rate limits (org-level):**

| Operation | Limit |
|---|---|
| Create endpoints (agents, sessions, environments, etc.) | 60 req/min |
| Read endpoints (retrieve, list, stream, etc.) | 600 req/min |

Org-level spend limits and tier-based rate limits also apply.

**Pricing:** No dedicated pricing page under `/managed-agents/`. Standard per-token pricing for the chosen model appears to apply. Whether container compute time adds cost on top of tokens is undocumented. See Open Questions.

**Other:** Max 20 skills per session. No documented concurrent session limit. Agent versions: no documented cap; archiving is the lifecycle end.

## Further reading

- **Overview** — https://platform.claude.com/docs/en/managed-agents/overview
- **Agent setup / versioning** — https://platform.claude.com/docs/en/managed-agents/agent-setup
- **Environments** — https://platform.claude.com/docs/en/managed-agents/environments (packages, networking, lifecycle)
- **Sessions** — https://platform.claude.com/docs/en/managed-agents/sessions (create, pin versions, archive, delete)
- **Events and streaming** — https://platform.claude.com/docs/en/managed-agents/events-and-streaming (full SSE event reference, interrupts, tool confirmations, span events)
- **Tools reference** — https://platform.claude.com/docs/en/managed-agents/tools (built-in toolset, disabling tools, custom tool definition)
- **MCP connector** — https://platform.claude.com/docs/en/managed-agents/mcp-connector
- **Skills** — https://platform.claude.com/docs/en/managed-agents/skills
- **Authoring custom skills** — https://platform.claude.com/docs/en/skills/overview
- **Anthropic CLI (`ant`)** — https://github.com/anthropics/anthropic-cli

## Open questions

These were undocumented or unreachable during research. If the user hits one of these, link them to the overview and say so explicitly instead of guessing.

1. **Vault creation API.** Sessions and MCP docs reference "Authenticate with vaults" but the page 404s. Full vault creation flow is undocumented in crawlable pages.
2. **Pricing specifics.** No dedicated pricing page. Unknown whether container compute time costs extra on top of per-token model pricing.
3. **Built-in MCP connector catalog.** Docs don't enumerate first-party "blessed" MCP servers. GitHub is used as an example but appears user-configured.
4. **Container specs.** Base OS, exact language/runtime versions, CPU/RAM per container aren't clearly documented.
5. **Observability / tracing.** Span events exist, but full integration story (Datadog, structured log export, retention) isn't detailed.
6. **Security / compliance.** No dedicated security page under Managed Agents. Container isolation guarantees, session history retention, SOC 2 / HIPAA status undocumented here.
7. **Concurrency limits.** No documented limit on concurrent running sessions per organization.
8. **`callable_agents` / multi-agent.** Listed as a research preview on the agent config page; no dedicated documentation page found.
