---
name: virtual-user-test
description: "Simulate virtual users with personal characteristics to test whether a website converts. Hybrid approach: a real-navigation sample detects friction, then mass scoring estimates conversion rate. Use when the user wants to test if a site sells, run virtual user testing, simulate personas browsing, or asks 'would this convert'. Triggers on 'virtual user test', 'test if it sells', 'simulate users', 'persona test', 'conversion test', 'synthetic user testing', 'UX test', 'would users buy'."
version: 1.0.0
author: facucorona
license: MIT
tags: [virtual-user-testing, synthetic-users, persona-testing, conversion-rate, ux-testing, website-testing, cro, opencode-skill, claude-code-skill]
category: testing
---

# Virtual User Test (OpenCode-native)

Hybrid virtual-user testing: a real-navigation sample detects friction, then mass scoring estimates conversion rate. Native to OpenCode (uses `browse` + `subagent`) — no Claude Code Workflow engine required.

## Parameters

- `url` (required): target site to test.
- `total` (default 50): number of personas to generate. Scalable to 200+.
- `navPercent` (default 15): % of personas that navigate the real site. Minimum 3.
- `segments` (optional): comma-separated segments to force. Otherwise auto-distributed.

## Phases

### Phase 1 — Generate personas (1 subagent)
Generate `total` personas as JSON. Each persona:
- id, name, age, role, company, budget, techComfort (1-5), attentionSpan (seconds)
- goals: what they want to achieve on the site
- redFlags: what makes them leave
- convertsIf: what would make them buy/contact
- segment

Distribute across segments. If `segments` given, force them; else auto-distribute across plausible segments for the site's domain.

### Phase 2 — Capture site (1 subagent + browse)
Navigate to `url`. Capture:
- Full-page screenshot (desktop 1440)
- Mobile screenshot (390)
- Text content of: hero, portfolio/work, lists/services, CTA, footer
- scrollHeight, presence of navbar/footer/CTA

### Phase 3 — Real navigation sample (N subagents in parallel)
Select `ceil(total * navPercent)` personas, at least 1 per segment. Each subagent:
- Opens a new tab with `browse tabs new`
- Navigates to `url`
- Scrolls through the page (top → bottom), timing each section
- Clicks 1-2 CTAs (or attempts to)
- Reports: frictionPoints[], timeToCTA (seconds), confusion, wouldContinue (bool), notes

Important: each subagent uses its own tab id to avoid conflicts.

### Phase 4 — Mass scoring in batch (1-2 subagents)
For all `total` personas, score against the site (captures + real friction points from sample). Each persona produces:
- verdict: "buy" | "contact" | "leave" | "unsure"
- conversionBarrier: main reason they wouldn't convert (or "none")
- objection: specific doubt
- recommendation: what would change their mind

Batch in groups of 25 per subagent to avoid token overflow.

### Phase 5 — Consolidation (1 subagent)
Aggregate:
- simulatedConversionRate: % buy+contact
- topFrictionPoints: top 5 from nav sample + scoring
- topRecommendations: top 5 actionable changes
- segmentBreakdown: conversion rate per segment
- representativeQuotes: 3-5 quotes from personas (in character)
- verdict: one-line summary

## Rules

- Never invent testimonials, metrics, clients, certifications or prices. Only use what's on the site.
- Personas must be diverse (age, role, budget, tech comfort, attention span).
- Real-navigation sample must include at least one persona per segment.
- If `browse` fails (no browser), fall back to Phase 4 only (scoring against captures).
- Report must label results as "simulated, not real user evidence".

## Output format

Human-readable summary with:
1. Conversion rate estimate (simulated)
2. Top 5 friction points
3. Top 5 recommendations
4. Segment breakdown
5. 3-5 representative quotes
6. Caveats (simulated data, not real users)
