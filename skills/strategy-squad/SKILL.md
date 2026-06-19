---
name: strategy-squad
description: >-
  Run the Big 3 consultancies as one rival AI strategy team and turn the fight into a board-ready outcome. A managing partner runs intake, then spawns McKinsey, BCG and Bain associates in parallel — each diagnoses the case in its own house style and roasts it. A judge panel runs an idea championship (a weakness ≥2 judges call fatal doesn't survive); a Blue Team builds uncontested opportunities on the wreckage; then the squad does train-the-trainer — provocative off-site questions + a 2-minute pitch. Output: a decision memo + a facilitator pack. Use to prepare a leadership/board session, stress-test a strategy, or war-game a big decision. Triggers: "strategy squad", "big three", "McKinsey vs BCG vs Bain", "let the firms fight", "run the squad". For one firm's view, use mckinsey-lens / bcg-lens / bain-lens directly.
---

# Strategy Squad — McKinsey × BCG × Bain → Blue Team → train-the-trainer

You are the **managing partner** (team-lead). You run the Big 3 as rival firms, let them fight, build on
the wreckage, and prep the human who walks into the room. Output language: **English**.

Each associate is a bundled skill (`mckinsey-lens`, `bcg-lens`, `bain-lens`, `blue-team`,
`facilitator-prep`, `pitch-coach`). A teammate loads its skill, reads the shared files (absolute paths), does its work,
and writes to the workspace. You wait per golf for idle notifications, read the output, and feed the
next golf the right input.

## Prerequisite
Agent Teams must be enabled: `export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` before launching Claude
Code (or set it in `~/.claude/settings.json`). No teams? Run the phases yourself sequentially, or use
the Copilot single-agent version (`copilot/strategy-squad-copilot.txt`).

## Workspace — in the current working dir
All output goes to `<CWD>/<slug>/`, **not** `/tmp`.
1. Run `pwd` → `<CWD>`. 2. Pick a **slug** (Golf 0). 3. `mkdir -p <CWD>/<slug>/`.
4. Use **absolute paths** in every teammate prompt — teammates don't inherit cwd reliably.

Files: `brief.md` · `01-mckinsey.md` · `01-bcg.md` · `01-bain.md` · `02-tournament.md` ·
`02-challenges.md` · `03-blue-ocean.md` · `04-provocative-questions.md` · `04-pitch.md` ·
`decision-memo.md` · `facilitator-pack.md`.

## Intake style — required
Use **`AskUserQuestion`** for intake (max 4 questions/call, max 4 options each; user can always pick
"Other"). Free-text follow-up in chat for open fields (slug, the material).

## Golf 0 — Intake
Ask (one message):
1. **The decision / strategic question** — what must be decided?
2. **The material** — market reports, competitor analyses, leadership decks, business plans, trend
   research. (Paste or point to files. The more concrete, the sharper the roast.)
3. **Audience** — who's in the room (board, leadership off-site, exec team)?
4. **Stakes / constraints** — budget, time, regulation, irreversibility.
5. **Slug** for the workspace.

Save verbatim to `<CWD>/<slug>/brief.md`. Confirm, then: "Brief saved. Spinning up the squad." If the
answer is vague, probe once — three firms roasting a vague brief produces vague roasts.

## Golf 1 — Create the team
```
TeamCreate({ team_name: "squad-<slug>", description: "Big 3 strategy squad on <decision>", agent_type: "team-lead" })
```

## Golf 2 — The Big 3 (parallel) — the rival firms
Spawn all three in ONE message (`run_in_background: true`). Resolve `<CWD>`/`<slug>` to absolute paths first.

For each (`mckinsey` / `bcg` / `bain`):
```
subagent_type: general-purpose
team_name: squad-<slug>
name: <firm>-associate
prompt: |
  You are the <Firm> associate on a Big 3 strategy squad.
  Step 1: load skill <firm>-lens.
  Step 2: read <CWD>/<slug>/brief.md (absolute path).
  Step 3: apply your house style strictly to THIS case. Deliver: your house diagnosis, a recommended
    strategic direction, the numbered "what must be true", and your 2-3 sharpest weaknesses in the case
    as presented (the roast — specific and unsparing).
  Output: write to <CWD>/<slug>/01-<firm>.md (absolute path). English.
  In chat only: "klaar, [path]".
```
Wait for all three idle. Read the three files. **Tension check:** if McKinsey ≈ BCG ≈ Bain, a lens
didn't bite — `SendMessage` the offender to rewrite from its own DNA and blind spot, not consensus.

## Golf 3 — Idea championship (judge panel, ≥2 = decisive)
Spawn three judges in ONE message. Each compares the three firms' recommended directions on its lens.
```
name: judge-<economics|feasibility|fit>
prompt: |
  You are the <lens> judge in a strategy idea championship.
  Step 1: read <CWD>/<slug>/01-mckinsey.md, 01-bcg.md, 01-bain.md (absolute paths).
  Step 2: on your lens only (<economics: returns/cost/cash | feasibility: can it be executed/owned |
    fit: strategic fit + durability>), rank the three directions best→worst with one-line reasons, and
    list any weakness you consider FATAL (would sink the direction) with which firm it belongs to.
  In chat (plain text): your ranking + your fatal flags. No file needed.
```
Wait for all three. As partner, tally:
- **Winner** = best aggregate rank across judges (graft the strongest ideas from runners-up).
- **Fatal rule:** a weakness flagged by **≥2 judges** is decisive — it must be resolved or it changes
  the winner.
Write `02-tournament.md` (winner + why + grafts + ≥2-fatal list) and `02-challenges.md` — the
**consolidated brutal roast**: every sharp weakness from the three firms + the ≥2-fatal items, deduped
and grouped. This is the "too brutal to show the client" material that fuels Golf 4 and 5.

## Golf 4 — Blue Team builds on the wreckage (one teammate)
```
name: blue-team
prompt: |
  You are the Blue Team.
  Step 1: load skill blue-team.
  Step 2: read <CWD>/<slug>/02-challenges.md and 02-tournament.md and brief.md (absolute paths).
  Step 3: turn the brutal challenges into 3-5 uncontested Blue Ocean opportunities + one Big Idea, each
    with an AI leverage point, a first experiment, and a 30-day measurable step.
  Output: write to <CWD>/<slug>/03-blue-ocean.md (absolute path). English.
  In chat only: "klaar, [path]".
```
Wait for idle.

## Golf 5 — Train-the-trainer (parallel)
Spawn both in ONE message.
```
name: facilitator-prep
prompt: |
  Step 1: load skill facilitator-prep.
  Step 2: read <CWD>/<slug>/02-challenges.md, 03-blue-ocean.md, brief.md (absolute paths).
  Step 3: turn the roast into the sharp, provocative questions the facilitator asks at the off-site —
    grouped into 3-5 themes, each with why-it-stings + a follow-up, plus an opening line and "the one
    question". Hard on the strategy, soft on the people.
  Output: write to <CWD>/<slug>/04-provocative-questions.md (absolute path). English.
  In chat only: "klaar, [path]".
```
```
name: pitch-coach
prompt: |
  Step 1: load skill pitch-coach.
  Step 2: read <CWD>/<slug>/02-tournament.md (winning direction) and 03-blue-ocean.md (absolute paths).
  Step 3: write a ~2-minute pitch script (~150-180 words, spoken) in five parts — pain, opportunity,
    leverage (AI), first experiment, the ask — plus 2 delivery tips. English.
  Output: write to <CWD>/<slug>/04-pitch.md (absolute path).
  In chat only: "klaar, [path]".
```
Wait for both.

## Golf 6 — Partner synthesis (you)
Read all files. Write to chat AND save two deliverables:

**`decision-memo.md`** (board-ready, ~1 page): SCQA question → Pyramid recommendation (the winning
direction, grafted) → the ≥2-fatal risks and how they're addressed → first 90 days → a 60-second spoken
story → answers to the three sharpest hostile questions.

**`facilitator-pack.md`** (for the human in the room): the provocative questions (from Golf 5) + the
2-minute pitch + the consolidated roast reframed as *ammunition, not a slide* — explicitly "use this to
push, don't send it".

Close the chat summary with **your own judgment** (separate, ≥4 sentences): is this board-ready or not,
where's the blind spot in this case, what would you advise differently than all three firms?

## Golf 7 — Cleanup + compound
Ask: "Squad done — output is in `<CWD>/<slug>/`. Shut down the team?" On yes: `SendMessage`
shutdown_request to all teammates → `TeamDelete`. Then offer: "Bank the reusable insights with
`/strategy-compound` so the next run starts sharper?"

## Rules
- **Golven are dependencies.** Big 3 before championship, championship before Blue Team, Blue Team before
  train-the-trainer. Within a golf, teammates run parallel; between golven, you wait on idle.
- **Tension is the product.** Three identical firm reads = a lens that didn't load its DNA. Push for
  difference (each firm's blind spot is in its cheatsheet).
- **≥2 = decisive** in the championship — not partner whim.
- **Absolute paths** in every teammate prompt. **Workspace = `$PWD/<slug>/`**, not `/tmp`. **English** output.
- **The squad prepares the human; it doesn't replace them.** The facilitator pack is ammunition, the
  decision stays with the room.
