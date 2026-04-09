---
name: humanizer
description: Remove common signs of AI-generated writing and rewrite text so it sounds more natural, specific, and human. Use when editing drafts that feel generic, over-polished, overly promotional, repetitive, or obviously AI-written.
---

# Humanizer

Rewrite text so it sounds like a real person wrote it. Keep the meaning intact, preserve the intended tone, and remove the patterns that make writing feel synthetic.

## Core job

When given text to humanize:

1. Identify obvious AI writing patterns
2. Rewrite the weak sections
3. Preserve the original meaning
4. Match the intended voice and context
5. Make the writing sound like it has an actual person behind it

Do not make the writing louder just to make it feel human. The goal is not "more dramatic." The goal is more natural, more specific, and less artificial.

## What to fix

### 1. Inflated importance

Watch for language that makes ordinary facts sound historic, profound, or symbolic.

Examples to cut or simplify:
- serves as
- stands as
- is a testament to
- pivotal moment
- significant milestone
- underscores the importance
- reflects a broader trend
- marks a shift
- evolving landscape
- lasting legacy
- key turning point

Bad:
> This update marks a pivotal moment in the evolution of team productivity.

Better:
> This update adds batch actions and cuts a few repetitive steps.

### 2. Fake depth with "-ing" phrases

Cut trailing phrases that pretend to add insight without saying much.

Watch for:
- highlighting
- underscoring
- emphasizing
- ensuring
- reflecting
- symbolizing
- contributing to
- fostering
- showcasing

Bad:
> The redesign improves navigation, ensuring users can move through the interface more efficiently.

Better:
> The redesign makes navigation simpler.

### 3. Promotional language

Remove ad copy unless the user explicitly wants ad copy.

Watch for:
- vibrant
- rich
- stunning
- remarkable
- groundbreaking
- powerful
- seamless
- intuitive
- must-visit
- breathtaking
- world-class
- cutting-edge

Bad:
> The platform delivers a seamless and powerful experience.

Better:
> The platform is faster and easier to use.

### 4. Vague attribution

Replace fuzzy authority claims with specifics, or cut them.

Watch for:
- experts say
- some critics argue
- observers note
- reports suggest
- many believe
- industry experts

Bad:
> Experts believe this approach will change the industry.

Better:
> The company says this cut onboarding time by 28 percent.

If no source is available, remove the claim instead of padding it.

### 5. Overused AI vocabulary

These words are not banned, but they’re overrepresented in AI writing and should trigger a closer look:

- additionally
- crucial
- delve
- enhance
- fostering
- highlight
- intricate
- key
- landscape
- pivotal
- showcase
- testament
- underscore
- valuable
- vibrant

Prefer simpler language when it says the same thing better.

### 6. Copula avoidance

AI often dodges simple words like "is" and "has" in favor of puffed-up phrasing.

Bad:
> The gallery serves as a hub for contemporary work and features over 3,000 square feet.

Better:
> The gallery is a contemporary art space with 3,000 square feet.

### 7. Negative parallelisms

Cut patterns like:
- not just X, but Y
- not merely X, but Y
- it’s not about X, it’s about Y

Bad:
> It’s not just a tool, it’s a revolution in how teams collaborate.

Better:
> It’s a collaboration tool with better search and fewer clicks.

### 8. Forced rule-of-three phrasing

AI loves neat little trios.

Bad:
> The event offers insight, inspiration, and innovation.

Better:
> The event includes talks, panels, and time to meet people.

If three items are real and useful, keep them. If they exist only to sound polished, rewrite.

### 9. Synonym cycling

AI often keeps changing labels to avoid repetition.

Bad:
> The founder built the product. The entrepreneur refined the platform. The creator launched the company.

Better:
> The founder built the product, refined it, and launched the company.

### 10. Em dash overuse

Replace most em dashes with periods, commas, or parentheses.

Bad:
> The feature is useful, especially for small teams, because it reduces setup time.

Better:
> The feature is especially useful for small teams because it reduces setup time.

### 11. Chatty assistant residue

Remove lines that sound like chatbot conversation rather than actual writing.

Watch for:
- Great question
- Of course
- I hope this helps
- Let me know if you want
- Here’s an overview
- You’re absolutely right

### 12. Filler and hedging

Trim phrases like:
- in order to
- due to the fact that
- at this point in time
- it is important to note
- has the ability to
- could potentially
- might perhaps

Bad:
> In order to improve performance, the system has the ability to cache responses.

Better:
> To improve performance, the system can cache responses.

### 13. Generic upbeat endings

Cut vague wrap-ups that say nothing.

Bad:
> The future looks bright as the company continues its journey.

Better:
> The company plans to open two more locations this year.

## Add actual voice

Removing AI patterns is only half the job. If the result is sterile, it still feels fake.

Improve the writing by:

- varying sentence length naturally
- using specific nouns and verbs instead of abstractions
- allowing a little personality when the context supports it
- keeping some edge, opinion, or texture if it fits the author’s voice
- choosing clarity over polish

Human writing can be clean without sounding machine-made.

Bad:
> The results were interesting and raised important questions.

Better:
> The results were messy, but hard to ignore.

## Rewrite rules

- Keep the original meaning
- Keep the right tone for the context
- Do not invent facts, sources, or statistics
- Do not make formal writing casual unless the user asks
- Do not add humor unless it fits
- Do not make everything sound like marketing
- Prefer concrete details over abstract claims
- Prefer simple sentence construction when it reads better

## Workflow

1. Read the text once for meaning
2. Mark the phrases that sound synthetic
3. Rewrite those sections, not just individual words
4. Read the result out loud mentally
5. Tighten anything that still sounds canned or overbuilt

## Output format

Provide:
1. The rewritten text
2. A short list of the biggest changes, if useful

## Quality check

Before finalizing, judge the rewrite on these five dimensions:

| Dimension | What to check | Score |
|---|---|---|
| Directness | Says what it means without throat-clearing | /10 |
| Rhythm | Sentence lengths vary naturally | /10 |
| Trust | Respects the reader and avoids hand-holding | /10 |
| Authenticity | Sounds like a person, not a content machine | /10 |
| Refinement | No obvious filler left | /10 |
| Total |  | /50 |

Guideline:
- 45-50 = solid
- 35-44 = needs another pass
- below 35 = rewrite again
