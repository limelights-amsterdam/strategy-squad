# Changelog

## Unreleased
- Championship judges now **write their verdict to `02-judge-<lens>.md`** instead of only replying in
  chat — a teammate's plain-text chat reply doesn't reliably reach the partner, so verdicts were getting
  lost. The partner reads the three files to tally.
- Firm lenses (`mckinsey`/`bcg`/`bain`) gain an **output-hygiene rule**: no wrapping the whole document
  in a code fence, no stray/trailing ``` fences (fixes Bain leaving dangling fences).
- Partner synthesis + Copilot deliverables: **never invent numbers** — unknown figures/owners/dates use
  an explicit `[TO FILL: …]` placeholder rather than a guessed value or a bare `X`/`€Y`.

## 0.1.0 — initial
- Big 3 orchestrator (`strategy-squad`): McKinsey × BCG × Bain rival associates → idea championship
  (≥2-fatal tally) → Blue Team → train-the-trainer (provocative questions + pitch) → decision memo +
  facilitator pack.
- Firm-lens skills + house-style cheatsheets: `mckinsey-lens`, `bcg-lens`, `bain-lens`.
- `blue-team`, `facilitator-prep`, `pitch-coach`.
- Copilot single-agent version (no Agent Teams / no terminal).
- Loop mode (`strategy-loop`) and compounding store (`strategy-compound`) — human-gated.
