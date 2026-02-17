# 10 Moats of Vertical Software Framework

## The Framework

The 10 Moats framework categorizes competitive advantages in vertical software into two groups: those being destroyed or weakened by LLMs, and those holding strong or getting stronger.

---

## Moats Destroyed / Weakened by LLMs

### 1. Learned Interfaces
**Definition:** Specialized UIs requiring extensive training, keyboard shortcuts, muscle memory, and "power user" expertise.

**Why it was a moat:**
- Switching costs high when users invested months/years learning complex interfaces
- Training created institutional lock-in
- Competitors needed to replicate entire UX paradigm

**LLM Impact:**
- Natural language replaces specialized UIs
- Users can just ask for what they want
- The interface becomes commoditized

**Examples:**
- Bloomberg Terminal (threatened by conversational finance tools)
- Adobe Creative Suite (vulnerable to text-to-image/video)
- Complex ERP interfaces (SAP, Oracle forms)

**Scoring Rubric:**
- 0 = Simple/no specialized UI (Not applicable)
- 1 = Some complexity but mostly standard web UI
- 2 = Complex interface with shortcuts, power features
- 3 = Highly specialized interface requiring significant training/certification

---

### 2. Custom Workflows & Business Logic
**Definition:** Years of accumulated if/then logic, edge cases, and industry-specific workflows embedded in code.

**Why it was a moat:**
- Rare combination of domain expertise + engineering talent required
- Decades of feature accumulation impossible to replicate quickly
- Workflow knowledge institutionalized in code

**LLM Impact:**
- Domain experts can now encode logic in markdown/prompts
- LLMs handle edge cases through reasoning vs. explicit branching
- Multi-year head start compressed to months

**Examples:**
- Tax software with 10,000+ rules
- Insurance underwriting systems
- Legal practice management with jurisdiction-specific logic

**Scoring Rubric:**
- 0 = Generic workflows, minimal custom logic
- 1 = Some industry-specific features
- 2 = Extensive custom workflows, significant edge case handling
- 3 = Deeply embedded domain expertise, thousands of rules/workflows

---

### 3. Public Data Access
**Definition:** Business models built on aggregating, structuring, and making public data searchable/usable.

**Why it was a moat:**
- Collecting and cleaning public data was expensive
- Search/indexing technology was proprietary
- Data licensing deals created exclusivity

**LLM Impact:**
- LLMs are native parsers of unstructured public data
- Real-time web access makes static databases less valuable
- "Just ask the internet" replaces curated databases

**Examples:**
- Business information providers (ZoomInfo, Crunchbase)
- Legal research databases (Westlaw's public case aggregation)
- News aggregators and research tools

**Scoring Rubric:**
- 0 = No public data component
- 1 = Some public data as supplement
- 2 = Significant public data aggregation is core value
- 3 = Business entirely built on public data curation

---

### 4. Talent Scarcity
**Definition:** Competitive advantage from hiring rare engineers who understand both the domain and software development.

**Why it was a moat:**
- Domain experts couldn't build software without engineers
- Engineers didn't understand domain nuances
- Finding both was extremely difficult

**LLM Impact:**
- Domain experts can build with natural language
- Vibe coding enables non-engineers to create software
- The scarce resource shifts from "engineers who understand X" to just "experts in X"

**Examples:**
- Healthcare IT (engineers who understand HIPAA + clinical workflows)
- Construction tech (engineers who understand job sites)
- Agricultural tech (engineers who understand farming)

**Scoring Rubric:**
- 0 = Generalist software, no domain complexity
- 1 = Moderate domain knowledge helpful
- 2 = Significant domain expertise historically required
- 3 = Extremely rare talent combination was core advantage

---

### 5. Bundling
**Definition:** Expanding market position by bundling adjacent capabilities into a single platform.

**Why it was a moat:**
- Customers preferred single vendor for related tools
- Cross-selling reduced acquisition costs
- Integration created switching costs

**LLM Impact:**
- Agents ARE the bundle—cherry-picking best providers
- Natural language unifies disparate tools
- Point solutions + agent orchestration beats monolithic suites

**Examples:**
- Microsoft 365 (bundled productivity)
- Vertical ERPs expanding into adjacent functions
- All-in-one marketing suites

**Scoring Rubric:**
- 0 = Single product, no bundling
- 1 = 2-3 related products
- 2 = Clear bundling strategy, expanding into adjacencies
- 3 = Platform bundling is primary growth strategy

---

## Moats Holding Strong

### 6. Private/Proprietary Data
**Definition:** Data that cannot be scraped, purchased, or synthesized—unique to the business's operations and relationships.

**Why it's durable:**
- LLMs can't access what isn't publicly available
- Proprietary data becomes MORE valuable in an LLM world (training advantage)
- Network effects from data accumulation compound

**LLM Impact:** POSITIVE
- Proprietary data is key differentiator
- Fine-tuning and RAG on private data creates moat
- LLMs need YOUR data to compete in your vertical

**Examples:**
- Credit bureaus (consumer financial history)
- Healthcare providers (patient records)
- Insurance companies (claims history, risk models)

**Scoring Rubric:**
- 0 = No proprietary data
- 1 = Some operational data, but not unique
- 2 = Significant proprietary dataset with accumulation over time
- 3 = Unique data asset, impossible to replicate

---

### 7. Regulatory/Compliance Lock-in
**Definition:** Structural barriers created by regulatory requirements, certifications, and compliance frameworks.

**Why it's durable:**
- LLMs don't change regulatory requirements
- Compliance creates switching costs (re-certification expensive)
- Regulatory capture protects incumbents

**LLM Impact:** NEUTRAL
- Compliance still required regardless of technology
- May accelerate compliance (AI-assisted) but doesn't remove barrier
- New AI regulations may create additional moats

**Examples:**
- HIPAA-compliant healthcare software
- FDA-regulated medical devices
- SOX-compliant financial systems
- State-by-state licensing (insurance, legal)

**Scoring Rubric:**
- 0 = No regulatory dimension
- 1 = Some compliance helpful but not required
- 2 = Significant regulatory requirements
- 3 = Heavily regulated, certification/audit requirements

---

### 8. Network Effects
**Definition:** Value increases as more users join the platform (direct, indirect, or data network effects).

**Why it's durable:**
- LLMs don't change the fundamental economics of networks
- Multi-sided markets remain sticky
- Data network effects compound with usage

**LLM Impact:** NEUTRAL TO POSITIVE
- May accelerate adoption (better UX)
- Doesn't change core network dynamics
- Could even strengthen through AI-powered matching

**Examples:**
- Marketplaces (more buyers → more sellers → more buyers)
- Payment networks (acceptance ubiquity)
- Collaboration tools (value in who's already there)

**Scoring Rubric:**
- 0 = No network effects
- 1 = Weak network effects (nice to have)
- 2 = Moderate network effects, platform value increases with users
- 3 = Strong network effects, platform dominant due to network size

---

### 9. Transaction Embedding
**Definition:** Software embedded in the flow of money—payments, loans, insurance, claims, payroll.

**Why it's durable:**
- Infrastructure layer that's hard to displace
- Revenue model tied to economic activity
- Switching costs high when integrated with financial flows

**LLM Impact:** NEUTRAL
- LLMs sit ON TOP of transaction infrastructure
- May improve UX but don't replace rails
- Actually INCREASES value of embedded finance (AI-assisted decisions)

**Examples:**
- Stripe (payment processing)
- Toast (restaurant POS + payments)
- Shopify (e-commerce + payments)
- Insurance claims processing

**Scoring Rubric:**
- 0 = No transaction component
- 1 = Facilitates but doesn't embed in transactions
- 2 = Significant transaction facilitation
- 3 = Deeply embedded in money flow, revenue tied to transaction volume

---

### 10. System of Record
**Definition:** The canonical source of truth for a business's critical data—"where things live."

**Why it's durable (for now):**
- Massive switching costs (migration complexity)
- Integration web locks customers in
- Trust and audit trails accumulated over years

**LLM Impact:** WATCH CLOSELY
- Currently safe: agents use systems of record
- Future risk: agents building their own memory/cross-platform views
- Long-term: could be disintermediated by AI-native architectures

**Examples:**
- Salesforce (CRM system of record)
- Workday (HR system of record)
- SAP/Oracle (ERP system of record)
- EMR systems (healthcare records)

**Scoring Rubric:**
- 0 = Not a system of record
- 1 = Secondary system, data duplicated elsewhere
- 2 = Important system for specific function
- 3 = Mission-critical system of record, central to operations

---

## The Three-Question Test

A rapid assessment to evaluate moat durability:

| Question | Yes Means |
|----------|-----------|
| **1. Is the data proprietary?** | Moat holds—LLMs can't access what they can't see |
| **2. Is there regulatory lock-in?** | Switching costs remain regardless of technology |
| **3. Is software embedded in the transaction?** | Infrastructure layer, not replaceable by LLM interface |

**Scoring:**
- **0 Yes:** High risk—moats primarily in categories 1-5
- **1 Yes:** Medium risk—some protection but vulnerable
- **2-3 Yes:** Probably fine—durable moats dominate

---

## Scoring Reference Tables

### Disruption Risk Score (0-100)

For EXISTING businesses—how exposed to LLM disruption:

| Vulnerable Moats (1-5) | Weight | Durable Moats (6-10) | Weight |
|------------------------|--------|----------------------|--------|
| Sum of scores 1-5 | × 8 | Sum of scores 6-10 | × -4 |

**Calculation:** (Vulnerable Sum × 8) - (Durable Sum × 4) = Disruption Risk Score

**Interpretation:**
- **0-25:** Low risk—durable moats dominate
- **26-50:** Medium risk—mixed moat profile
- **51-75:** High risk—vulnerable moats dominate
- **76-100:** Critical risk—primarily categories 1-5

### Moat Durability Score (0-100)

For EXISTING businesses—how much moat portfolio survives:

| Category | Calculation |
|----------|-------------|
| Durable Moats (6-10) | Sum × 6.67 |
| Vulnerable Moats (1-5) | (15 - Sum) × 3.33 |

**Calculation:** (Durable Sum × 6.67) + ((15 - Vulnerable Sum) × 3.33) = Moat Durability

**Interpretation:**
- **0-33:** Weak—moats won't survive LLM era
- **34-66:** Moderate—some moats transferable
- **67-100:** Strong—moat portfolio is durable

### AI Opportunity Score (0-100)

For NEW business ideas—how much whitespace did LLMs create:

| Factor | Weight | How to Score |
|--------|--------|--------------|
| Vulnerable Moats Destroyed in Vertical | 40% | Sum scores for moats 1-5 in target vertical |
| Data Opportunity | 25% | Can you acquire proprietary data? |
| Regulatory Barrier | 20% | Are you protected from fast followers? |
| Transaction Embedding Potential | 15% | Can you embed in money flow? |

**Calculation:** 
- Destroyed Moats Component: (Sum 1-5 / 15) × 40
- Data: (0-3 scale) × 8.33
- Regulatory: (0-3 scale) × 6.67  
- Transaction: (0-3 scale) × 5

---

## Examples

### Example 1: Bloomberg Terminal

| Moat | Score | LLM Impact |
|------|-------|------------|
| 1. Learned Interfaces | 3 | HIGH RISK |
| 2. Custom Workflows | 2 | HIGH RISK |
| 3. Public Data Access | 3 | HIGH RISK |
| 4. Talent Scarcity | 2 | HIGH RISK |
| 5. Bundling | 3 | HIGH RISK |
| 6. Private Data | 3 | PROTECTED |
| 7. Regulatory | 1 | PROTECTED |
| 8. Network Effects | 2 | PROTECTED |
| 9. Transaction Embedding | 1 | PROTECTED |
| 10. System of Record | 2 | PROTECTED |

**Three-Question Test:** Data proprietary? YES (chat, usage patterns). Regulatory? NO. Transaction? NO. **Result: 1/3**

**Scores:**
- Disruption Risk: (13 × 8) - (9 × 4) = 104 - 36 = 68 (High)
- Moat Durability: (9 × 6.67) + ((15-13) × 3.33) = 60 + 7 = 67 (Strong)

**Verdict:** Contradiction! Core data network effect (private message data) protects despite vulnerable UI moats.

---

### Example 2: Toast (Restaurant POS)

| Moat | Score | LLM Impact |
|------|-------|------------|
| 1. Learned Interfaces | 2 | HIGH RISK |
| 2. Custom Workflows | 2 | HIGH RISK |
| 3. Public Data Access | 0 | N/A |
| 4. Talent Scarcity | 1 | HIGH RISK |
| 5. Bundling | 2 | HIGH RISK |
| 6. Private Data | 2 | PROTECTED |
| 7. Regulatory | 1 | PROTECTED |
| 8. Network Effects | 1 | PROTECTED |
| 9. Transaction Embedding | 3 | PROTECTED |
| 10. System of Record | 2 | PROTECTED |

**Three-Question Test:** Data proprietary? YES (transaction data). Regulatory? NO. Transaction? YES (embedded payments). **Result: 2/3**

**Scores:**
- Disruption Risk: (7 × 8) - (9 × 4) = 56 - 36 = 20 (Low)
- Moat Durability: (9 × 6.67) + ((15-7) × 3.33) = 60 + 27 = 87 (Strong)

**Verdict:** Transaction embedding and data moats protect despite commodity UI.

---

### Example 3: Legal Research (Westlaw/Lexis)

| Moat | Score | LLM Impact |
|------|-------|------------|
| 1. Learned Interfaces | 2 | HIGH RISK |
| 2. Custom Workflows | 2 | HIGH RISK |
| 3. Public Data Access | 3 | HIGH RISK (cases are public) |
| 4. Talent Scarcity | 2 | HIGH RISK |
| 5. Bundling | 2 | HIGH RISK |
| 6. Private Data | 1 | PROTECTED (annotations, usage) |
| 7. Regulatory | 0 | N/A |
| 8. Network Effects | 1 | PROTECTED |
| 9. Transaction Embedding | 0 | N/A |
| 10. System of Record | 2 | PROTECTED |

**Three-Question Test:** Data proprietary? NO (cases public). Regulatory? NO. Transaction? NO. **Result: 0/3**

**Scores:**
- Disruption Risk: (11 × 8) - (4 × 4) = 88 - 16 = 72 (High)
- Moat Durability: (4 × 6.67) + ((15-11) × 3.33) = 27 + 13 = 40 (Moderate)

**Verdict:** Classic disruption target—built on public data access (moat #3) which LLMs destroy.

---

## Timeline Indicators

Use these signals to estimate disruption timeline:

| Timeline | Indicators |
|----------|------------|
| **Immediate (0-12 mo)** | Pure interface businesses, simple workflow automation, public data curation |
| **1-2 years** | Complex interfaces, moderate workflow depth, talent-scarcity dependent |
| **3-5 years** | Bundled suites, systems of record with migration friction |
| **5+ years / Never** | Regulatory moats, transaction infrastructure, proprietary data networks |

---

## Strategic Recommendations Framework

### For HIGH Disruption Risk Businesses:
1. **Acquire proprietary data**—fast
2. **Embed in transactions**—become infrastructure
3. **Pursue regulatory certification**—create artificial barriers
4. **Build network effects**—before competitors do
5. **Prepare pricing disruption**—LLM-native entrants will undercut

### For NEW Business Ideas:
1. **Target destroyed moats**—attack incumbents where LLMs leveled the field
2. **Start with proprietary data**—don't build on public data alone
3. **Plan for regulatory moat**—compliance as competitive advantage
4. **Embed early**—design for transaction embedding from day one
5. **Agent-native architecture**—assume your users have AI agents

---

*10 Moats Framework — Moat Analyzer*
*Analysis model: OpenClaw Moat Analyzer v1.0*
