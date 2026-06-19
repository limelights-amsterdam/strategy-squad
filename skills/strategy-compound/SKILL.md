---
name: strategy-compound
description: >-
  Bank reusable strategic insights so the next strategy-squad run starts sharper instead of cold. Reads a finished run (or the current conversation) and writes durable, structured insight files to docs/strategy-insights/, deduping against what's already there. Use after a strategy-squad run to capture what was learned, or when someone says "remember this for next time", "save this insight", "compound this". Adapts EveryInc's ce-compound pattern for strategy.
---

# Strategy Compound

You turn a finished strategy run into **durable, reusable insight** so future runs build on it. This is
the "compounding" layer: each run makes the next one sharper.

## How to work
1. Read the schema: `references/schema.md` (relative to this skill).
2. Gather the source: the run's workspace files (`decision-memo.md`, `02-challenges.md`,
   `03-blue-ocean.md`, etc.) and/or the current conversation. Ask the user for today's date if you
   don't have it — never guess a date.
3. **Read before writing.** Scan existing `docs/strategy-insights/**` for overlap. If a new insight
   clearly overlaps an existing file, UPDATE that file (add `last_updated:`) instead of creating a
   duplicate.
4. Extract 1-5 genuinely reusable insights (not case trivia). Good candidates: a market pattern, a
   recurring competitor blind spot, a positioning move that worked, a facilitation question that
   landed, a fatal-assumption type that keeps recurring.
5. Write each to `docs/strategy-insights/<category>/<slug>.md` with valid frontmatter + body sections
   (Context · Insight · Why it matters · When to apply · Evidence/caveats · Related).
6. Anonymize for a public repo: no client secrets, no named individuals. Confidence field is mandatory
   and honest.

## Output
- The files written/updated (paths), one line each, plus a one-line "next-run hint": what a future
  strategy-squad run should read first for a similar case.

## Rules
- One insight per file. Dedup against existing files — update, don't duplicate.
- Reusable over specific: if it only applies to this exact case, it's not an insight, it's a note.
- Honest confidence — a strategy insight is a bet, not a proof.
