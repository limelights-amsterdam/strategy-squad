# strategy-squad

A Claude Code **plugin** that runs the Big 3 management consultancies as one rival *agent team*. A
managing partner runs intake, then spawns **McKinsey, BCG and Bain** associates in parallel — each
diagnoses the case in its own house style and **roasts** it. A judge panel runs an **idea
championship** (a weakness ≥2 judges call fatal doesn't survive); a **Blue Team** builds uncontested
opportunities on the wreckage; then the squad does **train-the-trainer** — it hands the facilitator the
provocative questions to ask in the room plus a 2-minute pitch.

Output: a board-ready **decision memo** + a **facilitator pack** (the questions + pitch + the
"too-brutal-to-show" roast, framed as ammunition — not a slide to send the client).

> The squad prepares the human; it doesn't replace them. AI does the analysis; walking into the room
> and surviving the politics is still you. The recipe is free — the restaurant is what matters.

## What's inside

```
strategy-squad/
├── .claude-plugin/
│   ├── plugin.json          # plugin manifest
│   └── marketplace.json     # lets you install from a local clone (before/without GitHub)
├── skills/
│   ├── strategy-squad/      # the orchestrator (managing partner, runs the team in waves)
│   ├── mckinsey-lens/       # firm lens — structure: MECE, Pyramid/SCQA, 7S, Three Horizons
│   ├── bcg-lens/            # firm lens — economics: Growth-Share, Experience Curve, Strategy Palette
│   ├── bain-lens/           # firm lens — results: RAPID, NPS/Elements of Value, Repeatable Models
│   ├── blue-team/           # Blue Ocean builder (builds on the roast)
│   ├── facilitator-prep/    # train-the-trainer: roast → provocative off-site questions
│   ├── pitch-coach/         # the 2-minute pitch of the winning direction
│   ├── strategy-loop/       # autonomous, human-gated loop mode (deepen until dry, then stop)
│   └── strategy-compound/   # bank reusable insights to docs/strategy-insights/ (gets sharper over time)
├── references/              # the three firm house-style cheatsheets (also the Copilot context files)
├── copilot/                 # single-agent version for Claude Desktop / Copilot (no terminal, no flag)
└── docs/strategy-insights/  # the compounding knowledge store
```

The associates are bundled skills — teammates **load them by name** and read the firm cheatsheets as
files. The three firms come out genuinely different because each cheatsheet names that firm's *blind
spot*, and the other firms attack it.

## Credits

- **Oria AI** — the McKinsey- and BCG-style framework write-ups that inform two of the firm lenses:
  [oria.one/resources/21-strategy-skills-for-claude](https://www.oria.one/resources/21-strategy-skills-for-claude)
  and [oria.one/resources/bcg-style-strategy-skills-for-claude](https://www.oria.one/resources/bcg-style-strategy-skills-for-claude).
  Huge thanks — bedankt! 🙏 Discovered via the **AI for Strategy** community.
- **Bain & Company** — the Bain lens is authored from their public methodology (RAPID, Net Promoter
  System / Elements of Value, Repeatable Models, Founder's Mentality, Micro-Battles) at
  [bain.com](https://www.bain.com); no public Oria Bain pack exists yet.
- **Kieran Klaassen / EveryInc** — the
  [compound-engineering-plugin](https://github.com/EveryInc/compound-engineering-plugin) (MIT) is the
  model for the plugin packaging, the `/lfg`-style autonomous loop, and the `ce-compound`
  knowledge-store pattern that `strategy-compound` adapts for strategy.
- The orchestration, the firm cheatsheets, the Blue Team, the championship and the facilitator layer are
  original to this project. Built by **Tim van den Bosch (Sitelane)** with **Joost de Leij**.

## Prerequisites

- **Claude Code** with **Agent Teams enabled** for the full team run. Quickest way — run this in your
  terminal before you start Claude:
  ```sh
  export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
  ```
  To enable it everywhere, set it in `~/.claude/settings.json` instead:
  ```json
  { "env": { "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1" } }
  ```
- No Agent Teams? Use the **Copilot single-agent version** (`copilot/strategy-squad-copilot.txt`) — same
  squad, run sequentially, works in Claude Desktop with no flag.

## Install

**From GitHub (once published):**
```
/plugin marketplace add limelights-amsterdam/strategy-squad
/plugin install strategy-squad
```

**From a local clone (test before publishing):**
```
/plugin marketplace add /path/to/strategy-squad
/plugin install strategy-squad
```

## Use

Start a session and say, for example:

> *"Use the strategy squad to prepare a leadership decision on whether we do [X] or [Y]. Here's the
> material: …"* (attach market reports, competitor decks, leadership slides)

The managing partner runs a short intake, then the pipeline runs in waves:
**Big 3 diagnose + roast → idea championship (≥2 = fatal) → Blue Team builds → train-the-trainer
(questions + pitch) → decision memo + facilitator pack.**

A full run spans several live teammate sessions — noticeably more tokens than a normal chat. Use it for
real decisions and leadership sessions, not quick questions.

### Loop mode (autonomous, human-gated)

Point it at a case + your connected context (Notion, meeting notes, Slack, analytics via MCP), describe
the artifact you want, and run `/strategy-loop` inside `/loop`. It re-runs the squad and **deepens until
it stops finding new fatal weaknesses**, then **halts at a human-review gate** and banks the insights.
It never finalizes or "ships" a strategy unattended — strategy has no test suite, so the decision always
returns to you.

## Not for this

- One firm's view on its own → use `mckinsey-lens`, `bcg-lens`, or `bain-lens` directly.
- A quick question that doesn't justify a multi-agent run → use a single chat.
- A decision you want fully automated → it won't. By design it hands the call back to a human.

## License

MIT — see [`LICENSE`](LICENSE). This covers the orchestration, the firm cheatsheets, and the docs in
this repo. The McKinsey/BCG framework material is informed by Oria AI and the compounding/loop patterns
by EveryInc (see [Credits](#credits)); please keep that attribution when reusing.
