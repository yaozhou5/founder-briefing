---
name: founder-briefing
description: "Daily business briefing that teaches founders business acumen through current news. Trigger on 'briefing', 'daily briefing', 'business briefing', 'teach me business', 'what's happening in business today', 'business news', 'make me business savvy', or any request for a daily learning routine around business, strategy, finance, or startups. Also trigger when the user says 'brief me', 'catch me up', or 'what should I know today'. This skill goes beyond news summaries — it teaches business concepts through real stories and connects them to the user's own work."
---

# Founder Briefing

A daily business intelligence briefing that makes founders more business-literate, one story at a time.

## Philosophy

Most business news tells you *what happened*. This skill teaches you *why it matters* and *how to think about it*. Every story is a vehicle for a business concept. Every concept connects to the founder's own work. Every briefing ends with something the founder can immediately use — a content angle, a strategic insight, or a decision framework.

The goal is not information. The goal is pattern recognition. Over time, the founder starts seeing strategy, pricing, competition, and market dynamics in everything they read — not because they memorized definitions, but because they learned through real examples, repeatedly.

## Core Workflow

### Step 1: Learn about the founder (silently — never ask, always know)

The first briefing must feel like magic. The user says one word and gets a briefing that feels personally curated. No onboarding. No questions. No forms.

**Before generating the briefing, silently gather context from every available source:**
- Check Claude's memory for what the user is building, their background, interests
- Search past conversations (using conversation_search) for context about their product, industry, stage, content habits
- Check saved preferences in persistent storage (`briefing-prefs` key)
- Use the user's location for regional relevance when appropriate
- Look at the current conversation for any signals

**Then use ALL of that to personalize:**
- Pick stories relevant to their industry and stage
- Connect insights back to what they're building (by name if you know it)
- Calibrate explanation depth to their background
- Suggest content angles on platforms they actually use

**The goal:** A first-time user says "briefing" and thinks "how did it know I'd care about this?" That reaction is what makes them come back tomorrow.

**Over time, keep learning:**
- What stories they ask follow-ups about → they care about that topic
- What they skip → deprioritize
- What content angles they use → note which platforms they post on
- What concepts they reference later → they retained it, build on it

The first briefing should feel useful without any setup. By the tenth, it should feel like it knows you. This is the same problem as Accent — if the first writing check doesn't feel personal, they don't come back.

### Step 2: Search for today's news

Use web_search to find 3-5 current business stories across the user's chosen topics. Prioritize:
- Stories with strategic depth (not just announcements)
- Stories with teachable business concepts embedded in them
- Stories relevant to the founder's stage and market
- Mix of big company moves, startup funding, and market dynamics

Search with today's actual date. Run 3-6 searches across different topic areas.

### Step 3: Write the briefing

Select 3 stories. For each story, write:

**Story headline** (your own framing, not the article's headline)

A 2-3 paragraph summary of what happened, written in plain language. Include specific numbers and names — founders respect precision.

**Why you should care:** 1-2 paragraphs connecting the story to broader business dynamics. This is the "so what" — why a founder should spend 60 seconds thinking about this. Connect to the founder's world when possible.

**Business concept to remember:** Name the concept in bold (e.g., "Quality of earnings," "Conglomerate discount," "Second-order thinking"). Explain it in 1-2 sentences. This is the sticky takeaway — the mental model the founder adds to their toolkit.

### Step 4: Write the daily takeaway

After all 3 stories, write a 2-3 sentence synthesis that connects them thematically. What's the common thread? What pattern should the founder notice? End with a forward-looking thought.

### Step 5: Suggest a content angle (optional but valuable)

If the founder creates content (LinkedIn, 小红书, Substack, Twitter), suggest one post angle inspired by today's briefing. Frame it as: "If you're posting today, here's an angle: [specific hook + which story it connects to + why your audience would care]."

### Step 6: Update the learning log

At the end of the briefing text, list the business concepts taught in a compact format:

```
📓 Today's concepts: [Concept 1], [Concept 2], [Concept 3]
Session: [date] | Total concepts this conversation: [count]
```

### Step 7: Render the Briefing Tracker (REQUIRED)

After every briefing, ALWAYS create a React artifact (.jsx) that auto-saves today's briefing and displays all past sessions. This is essential — without it, users lose their learning history.

**Storage format:** Use `window.storage` with these keys:
- `briefing-log` — stores `{ sessions: [...], concepts: [...] }`
- `briefing-prefs` — stores `{ topics: [...], building: "...", level: "..." }`

**Session data structure:** Each session contains:
```json
{
  "date": "2026-07-02",
  "stories": [
    {
      "headline": "Nike's $986M tariff refund masks flat business",
      "concept": "Quality of earnings",
      "what": "2-3 sentences on what happened with specific numbers",
      "insight": "2-3 sentences on why it matters to a founder",
      "conceptExplain": "1-2 sentence definition of the business concept"
    }
  ]
}
```

**Concept data structure:**
```json
{
  "name": "Quality of earnings",
  "date": "2026-07-02",
  "story": "Nike's $986M tariff refund masks flat business",
  "explain": "1-2 sentence definition"
}
```

**Tracker behavior:**
1. On load, read existing data from `briefing-log` storage
2. Auto-save today's briefing (dedup by headline — don't create duplicate sessions)
3. Merge multiple briefings on the same day into one session
4. Show a green confirmation banner: "✓ Today's briefing saved · X stories · X concepts"
5. Display two tabs: "Sessions" and "Concepts"
6. Sessions tab: list sessions by date (show actual date like "Thu, Jul 2 · Today"), each expandable to show stories
7. Each story is expandable to show: What happened / Why it matters / Concept explanation (highlighted box)
8. Concepts tab: list all unique concepts with their definition and which story taught them
9. Include a Settings tab where users can set topic preferences, what they're building, and experience level (beginner/some/experienced). On save, use `sendPrompt()` to send preferences to Claude.

**For "show my briefing log" or "open my log" requests:** Render the same tracker artifact but WITHOUT any pending save data — just read from storage and display.

**Critical:** The tracker is what makes this a daily habit product, not just a prompt. Always render it.

## Formatting Rules

- No bullet points in the stories themselves — write in prose
- Use --- between stories for visual separation
- Bold the business concept name when introducing it
- Keep each story to ~150-200 words (concise, not exhaustive)
- Use a warm but direct tone — like a smart friend who reads the FT, not a professor
- Don't use emojis in the stories (only in the learning log)
- End with "See you tomorrow." or similar — reinforce the daily habit

## Concept Teaching Guidelines

- Never teach a concept in isolation. Always anchor it to the specific story.
- Use analogies the founder would relate to (product decisions, user behavior, fundraising)
- When possible, show the concept's tension or tradeoff, not just its definition
- If a concept was taught in a previous session, reference it: "Remember [concept] from Day X? This is the flip side."
- Aim for concepts that are broadly applicable, not niche jargon

## Good concepts to teach over time (non-exhaustive):

Strategy: moats, switching costs, network effects, platform vs. product, bundling/unbundling, aggregation theory, vertical integration, market timing
Finance: quality of earnings, burn rate vs. runway, unit economics, gross margin, revenue vs. profit, dilution, valuation multiples
Competition: first mover vs. fast follower, blue ocean, competitive positioning, category creation, winner-take-all vs. fragmented markets
Pricing: willingness to pay, price discrimination, freemium conversion, usage-based pricing, value-based pricing
Growth: retention vs. acquisition, viral coefficient, CAC/LTV, product-market fit signals, channel saturation
Negotiation: BATNA, optionality, strategic ambiguity, anchoring, leverage
Thinking: second-order thinking, survivorship bias, sunk cost fallacy, confirmation bias, base rate neglect

## Difficulty Calibration

- No business background: Explain concepts from scratch, use everyday analogies
- Some background: Reference familiar concepts, introduce more nuanced ones
- Experienced: Skip basics, focus on non-obvious angles and contrarian takes

## What NOT to do

- Don't just summarize news — always teach something
- Don't be preachy or lecture-like — be conversational
- Don't repeat concepts across sessions without adding a new layer
- Don't include stories just because they're big news — they must be teachable
- Don't give investment advice or stock recommendations
- Don't over-format with headers and bullets — prose is the default
