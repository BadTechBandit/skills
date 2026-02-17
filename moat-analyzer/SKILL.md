---
name: moat-analyzer
description: Analyze any business or new venture against the 10 Moats framework. Scores LLM disruption risk, moat durability, and AI opportunity. Identifies concrete startup ideas to disrupt the target company or vertical.
metadata:
  openclaw:
    emoji: "🏰"
    requires:
      tools: ["web_search"]
---

# Moat Analyzer

Analyze any business against the 10 Moats of Vertical Software framework. Assess which competitive advantages survive the LLM era, which are collapsing, and where the startup opportunities are.

**Trigger:** "analyze moats", "moat analysis", "evaluate [company]", "check moats for [idea]", "disruption analysis"

---

## Input

Accept ONE of:
- **Company:** Name, ticker, or URL
- **Business idea:** Description of a new venture targeting a vertical
- **Competitive comparison:** "Company A vs Company B"

---

## Workflow

### 1. Research

Gather intel on the target using your available web search tools.

**Recommended: [Exa](https://exa.ai)** for best results (company intel + LinkedIn search). Any web search tool works.

**For existing companies, search for:**
- `"[company]" business model revenue competitive advantage`
- `"[company]" proprietary data network effects`
- `"[company]" regulatory compliance certifications`
- `"[company]" AI disruption threat competitors`

**For new ideas:**
- `"[industry]" vertical software incumbents market leaders`
- `"[industry]" AI disruption opportunity whitespace`

Identify: vertical served, product offerings, revenue model, key competitors, known moat evidence.

### 2. Score the 10 Moats (0-3 each)

See `framework.md` for detailed scoring rubrics and examples.

**Vulnerable Moats (LLM-Exposed):**

| # | Moat | What to Look For |
|---|------|-----------------|
| 1 | **Learned Interfaces** | Complex UIs, keyboard shortcuts, required training/certification, steep learning curve |
| 2 | **Custom Workflows** | Deep domain logic, rule engines, if/then branches, years of coded business processes |
| 3 | **Public Data Access** | Business built on making public information searchable/accessible |
| 4 | **Talent Scarcity** | Competitive advantage from rare engineer+domain expert combination |
| 5 | **Bundling** | Growth via adjacent product modules, suite strategy, all-in-one platform |

**Durable Moats (LLM-Resistant):**

| # | Moat | What to Look For |
|---|------|-----------------|
| 6 | **Proprietary Data** | Unique datasets that can't be scraped, licensed, or synthesized |
| 7 | **Regulatory Lock-in** | HIPAA, FDA, SOX, industry certifications creating switching costs |
| 8 | **Network Effects** | Platform value increases with more participants |
| 9 | **Transaction Embedding** | Software sits in the money flow (payments, loans, claims) |
| 10 | **System of Record** | Canonical source of truth for critical business data |

**Scale:**
- 0 = Not applicable
- 1 = Weak presence
- 2 = Moderate presence
- 3 = Strong presence

### 3. Three-Question Test

| Question | Implications |
|----------|-------------|
| Is the data proprietary? | If no, accessibility layer is collapsing |
| Is there regulatory lock-in? | If no, switching costs are dissolving |
| Is software embedded in the transaction? | If no, you're replaceable |

- **0 Yes = HIGH RISK**
- **1 Yes = MEDIUM RISK**
- **2-3 Yes = LOWER RISK**

### 4. Calculate Scores

**Disruption Risk (0-100):**
```
Vulnerable = sum of moats 1-5 (max 15)
Durable = sum of moats 6-10 (max 15)
Risk = (Vulnerable × 8) - (Durable × 4), clamped 0-100
```

**Moat Durability (0-100):**
```
Durability = (Durable × 6.67) + ((15 - Vulnerable) × 3.33), clamped 0-100
```

### 5. Risk Tier & Timeline

**Tier:**
- HIGH: Risk > 50 or Three-Question = 0/3
- MEDIUM: Risk 25-50 or Three-Question = 1/3
- LOW: Risk < 25 and Three-Question >= 2/3

**Timeline:**
- Immediate: Public data moat >= 2 or learned interface = 3 with simple workflows
- 1-2 years: Talent scarcity dependent or bundling-only strategy
- 3-5 years: System of record with migration friction
- 5+ years: Strong regulatory, transaction, or network moats

### 6. Disruption Opportunities

Identify the top 5 AI-agent startup ideas that could attack this company's weakest moats or exploit the whitespace LLMs created in their vertical.

For each opportunity:
1. **Name & One-Liner**
2. **Attack Vector** - Which destroyed moat(s) does this exploit?
3. **Target Market** - Who buys this? Be specific.
4. **How It Works** - What does the AI agent actually do?
5. **Entry Strategy** - How do you get the first 10 customers?
6. **Why Now** - What changed that makes this viable today?
7. **Revenue Model** - How does it make money?
8. **Estimated Build Effort** - Small team timeline to MVP

### 7. Strategic Recommendations

**For existing businesses (defend):**
- Immediate actions to protect against disruption
- Medium-term moat diversification strategy
- Long-term positioning in an agent-native world

**For new ideas (attack):**
- Which destroyed moats create your entry point
- How to build durable moats from day one
- How to avoid being disrupted yourself

---

## Output Format

```markdown
# Moat Analysis Report: [Company/Idea]

## Executive Summary
[2-3 sentences: what this company does, overall risk assessment, key finding]

## Risk Profile
| Metric | Score | Rating |
|--------|-------|--------|
| Disruption Risk | X/100 | [Low/Medium/High/Critical] |
| Moat Durability | X/100 | [Weak/Moderate/Strong] |
| Risk Tier | - | [HIGH/MEDIUM/LOW] |
| Disruption Timeline | - | [Immediate/1-2yr/3-5yr/5+yr] |

## Three-Question Test
| Question | Answer | Evidence |
|----------|--------|----------|
| Data proprietary? | Yes/No | [brief evidence] |
| Regulatory lock-in? | Yes/No | [brief evidence] |
| Transaction embedded? | Yes/No | [brief evidence] |
| **Result** | **X/3** | **[Risk interpretation]** |

## Moat Analysis

### Vulnerable Moats (LLM-Exposed)
#### 1. Learned Interfaces - X/3
[Evidence and LLM impact analysis]
[...continue for all 10 moats]

## Disruption Opportunities
### 1. [Startup Name] - [One-liner]
[Full details for each of 5 opportunities]

## Strategic Assessment
[Final synthesis]
```

---

## Reference

See `framework.md` for detailed moat definitions, scoring rubrics, calculation breakdowns, and worked examples (Bloomberg, Toast, Westlaw).
