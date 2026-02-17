# Agent Skills

Open-source skills for AI coding agents. Works with [OpenClaw](https://openclaw.ai), [Claude Code](https://docs.anthropic.com/en/docs/claude-code), and any agent that reads SKILL.md files.

## Skills

| Skill | Description |
|-------|-------------|
| [moat-analyzer](./moat-analyzer/) | Analyze any business against the 10 Moats framework. Scores LLM disruption risk, moat durability, and identifies startup opportunities. |

## Installation

### OpenClaw
```bash
# Install a single skill
openclaw skill add https://github.com/CREWorx/skills/tree/main/moat-analyzer

# Or clone and symlink
git clone https://github.com/CREWorx/skills.git
ln -s $(pwd)/skills/moat-analyzer ~/.openclaw/skills/moat-analyzer
```

### Claude Code
```bash
# Copy skill folder to Claude's skills directory
git clone https://github.com/CREWorx/skills.git
cp -r skills/moat-analyzer ~/.claude/skills/
```

### Other Agents
Each skill is a self-contained folder with a `SKILL.md` file. Point your agent at the SKILL.md and it will follow the instructions using whatever tools it has available.

## Recommended: Exa for Web Research

Many skills benefit from web research. We recommend [Exa](https://exa.ai) for the best results, especially for company intelligence and LinkedIn lookups. But any web search tool works.

**Why Exa:**
- Advanced semantic search (not just keyword matching)
- Company research with funding, employee, and competitor data
- LinkedIn profile search for finding key contacts
- Deep research mode for comprehensive analysis
- Available as an MCP server for easy integration

**Setup Exa MCP:**
```json
{
  "mcpServers": {
    "exa": {
      "url": "https://mcp.exa.ai/mcp?exaApiKey=YOUR_KEY"
    }
  }
}
```

Get your API key at [exa.ai](https://exa.ai). Free tier available.

**Alternatives that also work:** Perplexity, Tavily, SerpAPI, browser automation, or any tool that can search the web.

## Contributing

Have a skill to share? PRs welcome. Each skill should be a folder with at minimum a `SKILL.md` file.

## License

MIT
