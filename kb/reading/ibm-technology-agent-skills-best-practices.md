---
type: reading
title: "5 Best Practices for Building AI Agent Skills"
description: Argues a good agent skill has a trigger-ready description, content from real expertise, a lean body, deterministic scripts for fragile steps, and vetting before you run someone else's.
kind: video
resource: https://www.youtube.com/watch?v=qYNs80FKIVc
by:
  - '[[IBM Technology]]'
topic:
  - agent-skills
readStatus: unread
recBy:
addedBy: Jonathan St-Onge
date: "2026-10-15"
tags:
  - agents
  - skills
sources:
  - id: archive
    resource: /sources/ibm-technology-agent-skills-best-practices.md
    title: archived copy
localCopy: '[[sources/ibm-technology-agent-skills-best-practices|archived copy]]'
generated:
  by: claude/claude-sonnet-5-5
  at: '2026-10-15T12:01:59-04:00'
---

# 5 Best Practices for Building AI Agent Skills

> [!agent] Claude summary
> A two-presenter IBM explainer on the SKILL.md format (name and description in YAML, a markdown body, optional `references/` and `scripts/` folders). Its five practices: write the description as the trigger, saying what the skill does and when to use it, and lean "pushy" because models under-trigger; build the body from your own expertise, especially "gotchas" (every manual correction you make to the agent), instead of letting an LLM generate generic filler; keep SKILL.md under about 500 lines or 5,000 tokens and push the rest into `references/` (progressive disclosure); put fragile steps in scripts the agent runs rather than improvises, say explicitly "run this" versus "read this", and trade tests for removing the guess; and treat third-party skills as dependencies, citing an unnamed audit of about 4,000 public skills where over 35% had a security flaw and 13% a critical one. The load-bearing ideas are 2 and 4: skills are worth writing only where the model lacks your context, and the reconciling-totals anecdote (the model added numbers wrong, so a script now adds them) is the clearest illustration. Friction: the "write less" and "be thorough" advice pull against each other and is resolved only by the 500-line rule of thumb; the audit statistic is quoted from memory with no source, so check it before reusing it; and "you can't test your way to trust" is overstated, since tests of the script itself are exactly what would make it trustworthy. The archive is auto-captions with no speaker labels, so attributions to Simon Willison and to each presenter are unreliable.

## Why do I bring this up?

### Jonathan St-Onge

