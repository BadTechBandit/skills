# Marketing Skills for AI Agents

A collection of 34 AI agent skills for marketing — conversion optimization, copywriting, SEO, GEO, analytics, and growth engineering. Works with Claude Code, OpenAI Codex, Cursor, Windsurf, and any agent that supports the [Agent Skills spec](https://agentskills.io).

Originally based on [coreyhaines31/marketingskills](https://github.com/coreyhaines31/marketingskills) (MIT), with additional skills added.

## Install

```bash
# Install all marketing skills
npx skills add BadTechBandit/skills@marketing-skills-for-agents

# Install specific skills
npx skills add BadTechBandit/skills@marketing-skills-for-agents --skill page-cro copywriting seo-geo

# List available skills
npx skills add BadTechBandit/skills@marketing-skills-for-agents --list
```

This automatically installs to your `.agents/skills/` directory (and symlinks into `.claude/skills/` for Claude Code compatibility).

### Alternative: Clone and Copy

```bash
git clone https://github.com/BadTechBandit/skills.git
cp -r skills/marketing-skills-for-agents/skills/* ~/.agents/skills/
```

### Alternative: Git Subtree (track updates)

```bash
cd your-project
git subtree add --prefix=marketing-skills https://github.com/BadTechBandit/skills.git main --squash

# Pull updates later:
git subtree pull --prefix=marketing-skills https://github.com/BadTechBandit/skills.git main --squash
```

## What are Skills?

Skills are markdown files that give AI agents specialized knowledge and workflows for specific tasks. When you add these to your project, your agent can recognize when you're working on a marketing task and apply the right frameworks and best practices.

## How Skills Work Together

Skills reference each other and build on shared context. The `product-marketing-context` skill is the foundation — every other skill checks it first to understand your product, audience, and positioning before doing anything.

```
                            ┌──────────────────────────────────────┐
                            │      product-marketing-context       │
                            │    (read by all other skills first)  │
                            └──────────────────┬───────────────────┘
                                               │
    ┌──────────────┬─────────────┬─────────────┼─────────────┬──────────────┬──────────────┐
    ▼              ▼             ▼             ▼             ▼              ▼              ▼
┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────────┐ ┌──────────┐ ┌─────────────┐ ┌───────────┐
│  SEO &   │ │   CRO    │ │Content & │ │  Paid &    │ │ Growth & │ │  Sales &    │ │ Strategy  │
│ Content  │ │          │ │   Copy   │ │Measurement │ │Retention │ │    GTM      │ │           │
├──────────┤ ├──────────┤ ├──────────┤ ├────────────┤ ├──────────┤ ├─────────────┤ ├───────────┤
│seo-audit │ │page-cro  │ │copywritng│ │paid-ads    │ │referral  │ │revops       │ │mktg-ideas │
│ai-seo    │ │signup-cro│ │copy-edit │ │ad-creative │ │free-tool │ │sales-enable │ │mktg-psych │
│seo-geo   │ │onboard   │ │cold-email│ │ab-test     │ │churn-    │ │launch       │ │           │
│site-arch │ │form-cro  │ │email-seq │ │analytics   │ │ prevent  │ │pricing      │ │           │
│programm  │ │popup-cro │ │social    │ │            │ │          │ │competitor   │ │           │
│schema    │ │paywall   │ │lead-mag  │ │            │ │          │ │             │ │           │
│content   │ │          │ │          │ │            │ │          │ │             │ │           │
└────┬─────┘ └────┬─────┘ └────┬─────┘ └─────┬──────┘ └────┬─────┘ └──────┬──────┘ └─────┬─────┘
     │            │            │              │             │              │              │
     └────────────┴─────┬──────┴──────────────┴─────────────┴──────────────┴──────────────┘
                        │
         Skills cross-reference each other:
           copywriting ↔ page-cro ↔ ab-test-setup
           revops ↔ sales-enablement ↔ cold-email
           seo-audit ↔ schema-markup ↔ ai-seo ↔ seo-geo
```

See each skill's **Related Skills** section for the full dependency map.

## Available Skills

| Skill | Description |
|-------|-------------|
| [ab-test-setup](skills/ab-test-setup/) | Plan, design, or implement A/B tests and experiments |
| [ad-creative](skills/ad-creative/) | Generate, iterate, or scale ad creative for any paid platform |
| [ai-seo](skills/ai-seo/) | Optimize content for AI search engines — get cited by LLMs |
| [analytics-tracking](skills/analytics-tracking/) | Set up, improve, or audit analytics tracking and measurement |
| [churn-prevention](skills/churn-prevention/) | Reduce churn — cancel flows, save offers, dunning, payment recovery |
| [cold-email](skills/cold-email/) | Write B2B cold emails and follow-up sequences that get replies |
| [competitor-alternatives](skills/competitor-alternatives/) | Create competitor comparison and alternative pages |
| [content-strategy](skills/content-strategy/) | Plan content strategy, decide what to create, topic clusters |
| [copy-editing](skills/copy-editing/) | Edit, review, and improve existing marketing copy |
| [copywriting](skills/copywriting/) | Write marketing copy for any page — homepage, landing, pricing, features |
| [email-sequence](skills/email-sequence/) | Create or optimize email sequences, drip campaigns, lifecycle emails |
| [form-cro](skills/form-cro/) | Optimize lead capture, contact, demo request, and application forms |
| [free-tool-strategy](skills/free-tool-strategy/) | Plan and build free tools for lead generation and SEO |
| [launch-strategy](skills/launch-strategy/) | Plan product launches, feature announcements, go-to-market |
| [lead-magnets](skills/lead-magnets/) | Create, plan, or optimize lead magnets for email capture |
| [marketing-ideas](skills/marketing-ideas/) | 139 proven marketing ideas for SaaS and software products |
| [marketing-psychology](skills/marketing-psychology/) | Apply psychological principles and mental models to marketing |
| [onboarding-cro](skills/onboarding-cro/) | Optimize post-signup onboarding, activation, time-to-value |
| [page-cro](skills/page-cro/) | Optimize any marketing page for conversions |
| [paid-ads](skills/paid-ads/) | Google Ads, Meta, LinkedIn, Twitter/X campaign strategy |
| [paywall-upgrade-cro](skills/paywall-upgrade-cro/) | Create or optimize in-app paywalls, upgrade screens, upsells |
| [popup-cro](skills/popup-cro/) | Create or optimize popups, modals, overlays for conversion |
| [pricing-strategy](skills/pricing-strategy/) | Pricing decisions, packaging, monetization strategy |
| [product-marketing-context](skills/product-marketing-context/) | Create or update your product marketing context document |
| [programmatic-seo](skills/programmatic-seo/) | Create SEO-driven pages at scale using templates and data |
| [referral-program](skills/referral-program/) | Create, optimize, or analyze referral and affiliate programs |
| [revops](skills/revops/) | Revenue operations, lead lifecycle, marketing-to-sales handoff |
| [sales-enablement](skills/sales-enablement/) | Sales decks, one-pagers, objection handling, demo scripts |
| [schema-markup](skills/schema-markup/) | Add, fix, or optimize schema markup and structured data |
| [seo-audit](skills/seo-audit/) | Audit, review, or diagnose technical and on-page SEO issues |
| [seo-geo](skills/seo-geo/) | SEO & GEO — optimize for both traditional and AI search engines |
| [signup-flow-cro](skills/signup-flow-cro/) | Optimize signup, registration, account creation flows |
| [site-architecture](skills/site-architecture/) | Plan page hierarchy, navigation, URL structure, internal linking |
| [social-content](skills/social-content/) | Create and optimize social media content across platforms |

## Skill Categories

### SEO & Discovery
- `seo-audit` - Technical and on-page SEO
- `ai-seo` - AI search optimization (AEO, GEO, LLMO)
- `seo-geo` - Combined SEO + GEO for traditional and AI search engines
- `programmatic-seo` - Scaled page generation
- `site-architecture` - Page hierarchy, navigation, URL structure
- `competitor-alternatives` - Comparison and alternative pages
- `schema-markup` - Structured data

### Conversion Optimization
- `page-cro` - Any marketing page
- `signup-flow-cro` - Registration flows
- `onboarding-cro` - Post-signup activation
- `form-cro` - Lead capture forms
- `popup-cro` - Modals and overlays
- `paywall-upgrade-cro` - In-app upgrade moments

### Content & Copy
- `copywriting` - Marketing page copy
- `copy-editing` - Edit and polish existing copy
- `cold-email` - B2B cold outreach emails and sequences
- `email-sequence` - Automated email flows
- `social-content` - Social media content
- `lead-magnets` - Lead magnets for email capture

### Paid & Distribution
- `paid-ads` - Google, Meta, LinkedIn ad campaigns
- `ad-creative` - Bulk ad creative generation and iteration

### Measurement & Testing
- `analytics-tracking` - Event tracking setup
- `ab-test-setup` - Experiment design

### Retention
- `churn-prevention` - Cancel flows, save offers, dunning, payment recovery

### Growth Engineering
- `free-tool-strategy` - Marketing tools and calculators
- `referral-program` - Referral and affiliate programs

### Strategy & Monetization
- `marketing-ideas` - 139 SaaS marketing ideas
- `marketing-psychology` - Mental models and psychology
- `launch-strategy` - Product launches and announcements
- `pricing-strategy` - Pricing, packaging, and monetization

### Sales & RevOps
- `revops` - Lead lifecycle, scoring, routing, pipeline management
- `sales-enablement` - Sales decks, one-pagers, objection docs, demo scripts

## Usage

Once installed, just ask your agent to help with marketing tasks:

```
"Help me optimize this landing page for conversions"
→ Uses page-cro skill

"Write homepage copy for my SaaS"
→ Uses copywriting skill

"Run a GEO audit on my site"
→ Uses seo-geo skill

"Create a 5-email welcome sequence"
→ Uses email-sequence skill
```

You can also invoke skills directly:

```
/page-cro
/seo-geo
/email-sequence
/seo-audit
```

## Credits

Marketing skills originally created by [Corey Haines](https://github.com/coreyhaines31/marketingskills) under the MIT license. Additional skills (`seo-geo`) added by [BadTechBandit](https://github.com/BadTechBandit).

## License

[MIT](LICENSE)
