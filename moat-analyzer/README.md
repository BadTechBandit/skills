# 🏰 Moat Analyzer

Analyze any business against the **10 Moats of Vertical Software** framework. Assess which competitive advantages survive the LLM era, which are collapsing, and where the startup opportunities are.

## What It Does

Enter a company name (or describe a business idea) and get:

- **Disruption Risk Score** (0-100) - How exposed is this business to LLM disruption?
- **Moat Durability Score** (0-100) - How much of the moat portfolio survives?
- **Three-Question Test** - Quick binary assessment of core moat strength
- **10 Individual Moat Scores** - Detailed breakdown with evidence and LLM impact
- **5 Disruption Opportunities** - Concrete startup ideas to attack the weakest moats
- **Strategic Recommendations** - What to do about it (defend or attack)

## The 10 Moats

**Vulnerable (LLM-Exposed):**
1. Learned Interfaces
2. Custom Workflows
3. Public Data Access
4. Talent Scarcity
5. Bundling

**Durable (LLM-Resistant):**
6. Proprietary Data
7. Regulatory Lock-in
8. Network Effects
9. Transaction Embedding
10. System of Record

## Install

```bash
npx skills add https://github.com/BadTechBandit/skills --skill moat-analyzer
```

Browse on skills.sh: https://skills.sh/BadTechBandit/skills/moat-analyzer

### Manual install

```bash
git clone https://github.com/BadTechBandit/skills.git
ln -s $(pwd)/skills/moat-analyzer ~/.claude/skills/moat-analyzer
ln -s $(pwd)/skills/moat-analyzer ~/.codex/skills/moat-analyzer
```

Or point any agent directly at `SKILL.md` — it contains the complete framework, scoring system, and output format.

## Usage

Just tell your agent:
- "Analyze moats for Salesforce"
- "Moat analysis: Stripe"
- "Evaluate Bloomberg Terminal for LLM disruption risk"
- "Check moats for [my business idea]"

## Research Tools

The skill works with any web search tool your agent has. For best results, we recommend [Exa](https://exa.ai):

- Semantic company research (not just keyword matching)
- LinkedIn profile search for finding key decision-makers
- Deep research mode for comprehensive analysis

Any alternative works too: Perplexity, Tavily, SerpAPI, browser, etc.

## Example Output

```
Moat Analysis Report: Westlaw (Thomson Reuters)

Disruption Risk:  72/100 (HIGH)
Moat Durability:  40/100 (Moderate)
Risk Tier:        HIGH
Timeline:         1-2 years

Three-Question Test: 0/3 (all No)
- Data proprietary? No (cases are public)
- Regulatory lock-in? No
- Transaction embedded? No

Top vulnerability: Public Data Access (3/3)
LLMs are native parsers of public legal data,
making curated databases significantly less valuable.
```

## Files

- `SKILL.md` - The skill instructions (what your agent reads)
- `framework.md` - Detailed moat definitions, scoring rubrics, and worked examples
- `README.md` - This file

## License

MIT
