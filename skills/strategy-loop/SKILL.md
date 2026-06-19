---
name: strategy-loop
description: >-
  Run the strategy-squad autonomously in a loop — point it at a case plus connected context (Notion, meeting notes, Slack, analytics via MCP), describe the artifact you want, walk away. It deepens the analysis round after round until it stops surfacing new fatal weaknesses (loop-until-dry), banks the insights, and HALTS at a human-review gate. It never finalizes or ships a strategy unattended. Use with /loop for hands-off, long-running strategy prep. Triggers: "run the squad in a loop", "strategy loop", "deepen this until it's done", "walk-away strategy run".
---

# Strategy Loop (autonomous, human-gated)

You run the strategy-squad repeatedly, going deeper each round, and **stop yourself** when the analysis
stops improving — then hand the decision back to a human. You never declare a strategy "final" or act
on it unattended. Strategy has no test suite; the human owns the call.

## Setup (confirm before looping)
1. **The case + the artifact.** What decision, and what final artifact do they want (decision memo,
   facilitator pack, board narrative)? Save the brief to `<CWD>/<slug>/brief.md`.
2. **Connected context.** If MCP sources are connected (Notion, Slack, Google Drive, analytics), pull
   the relevant context into the brief first. Note which sources you used and which you couldn't reach.
3. **Prior insights.** Run the `strategy-insights-researcher` agent over `docs/strategy-insights/` and
   fold relevant prior insights into the brief, so you start sharp.
4. **Convergence threshold.** Default: stop after **2 consecutive rounds** that surface **no new fatal
   weakness**. Confirm or let the user override.

## The loop (use `/loop` dynamic mode — self-paced)
Each round:
1. Run a strategy-squad pass on the current brief + everything in the workspace (interactively spawn the
   Big 3 + championship + Blue Team, or run the phases inline if Agent Teams is off).
2. Track **seen challenges** across rounds (a running list in `<CWD>/<slug>/_seen-challenges.md`).
   Compare this round's fatal weaknesses against it.
3. **Loop-until-dry:** if this round found a *new* fatal weakness, address it (feed it back into the next
   round's brief), reset the dry counter, and loop. If it found nothing new, increment the dry counter.
4. Stop when the dry counter hits the threshold — OR a hard cap of **5 rounds** (log that the cap was
   hit). Never loop forever.

Use `ScheduleWakeup` only if you're genuinely waiting on slow external context (a report, a data pull);
otherwise iterate in-session. Log one progress line per round so the user can see it on return.

## On convergence — HALT at the human gate
1. Write the final `decision-memo.md` + `facilitator-pack.md`.
2. Bank the run via `strategy-compound` into `docs/strategy-insights/`.
3. Output a clear stop signal: `<promise>READY-FOR-REVIEW</promise>` plus a 5-line summary — what
   converged, the decisive risks that remain, and **the explicit decisions left to the human**.
4. **Do not** publish, send, present, or otherwise act on the strategy. Stop and wait.

## Rules
- **Never auto-finalize.** Converged ≠ correct. The gate is mandatory.
- **Bounded.** Threshold + 5-round hard cap; log if the cap is hit (means it's still contested — flag
  that to the human, don't paper over it).
- **Honest about context gaps.** If you couldn't reach a connected source, say so in the summary; don't
  silently proceed as if you had it.
- **English** output. Workspace = `<CWD>/<slug>/`, not `/tmp`.
