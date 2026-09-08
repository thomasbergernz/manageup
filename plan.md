# Plan: Convert `managing_up_five_moves.md` into a Skill

## Problem statement

`/Users/thomasb/managing-up/managing_up_five_moves.md` contains the "Managing Up: Five Moves" playbook — five repeatable communication moves (Audit, Send It Up, Note Before the Meeting, Recap, Send the Agenda First), each with a paste-ready prompt for any LLM. It's currently a flat read-only document; converting it into a proper agent skill makes the moves invocable, lets the agent pick the right track (boss vs. client) automatically, and lets later moves chain on the output of earlier ones.

## Current state

- Repo: `thomasbergernz/manageup` (public, on `main`).
- Local branch: `main`, clean working tree.
- Source doc: `managing_up_five_moves.md` (~150 lines) — 5 moves, each with Goal / Time / Prompt / Insight, plus a shared intro that defines two parallel tracks (boss vs. client).
- The doc is well-structured but is **prompt-only**; it doesn't tell the agent *how* to run a move end-to-end (which track, how to fill the brackets, how to chain into the next move).

## Proposed changes

Build a single Warp skill, `managing-up`, that lets the agent run any of the five moves from a natural-language request.

### 1. Skill location

Place at `.agents/skills/managing-up/SKILL.md` inside `thomasbergernz/manageup`. Warp's recommended location, auto-discovered, works locally and as `thomasbergernz/manageup:managing-up` for cloud agents.

Directory layout:
```
.agents/skills/managing-up/
├── SKILL.md                 # frontmatter + workflow
├── references/
│   ├── move-01-audit.md
│   ├── move-02-send-it-up.md
│   ├── move-03-note-before-meeting.md
│   ├── move-04-recap.md
│   └── move-05-agenda-first.md
└── assets/
    └── tracks.md            # boss vs client track switcher + reusable persona phrasing
```

The moves get split into `references/` because the SKILL.md body should stay under ~150 lines, and each move has its own prompt, input shape, and validation — putting them in one body would bloat the always-loaded context.

### 2. `SKILL.md` frontmatter (router)

Name: `managing-up`.

Description draft (must pass the trigger test — pushy, names the moves, names the failure it prevents):

> Runs the Managing Up five-moves playbook: Audit, Send It Up, Note Before the Meeting, Recap, Send the Agenda First. Use this whenever the user wants to update their boss, write a status email, prep for a meeting, recap a meeting, send a 1:1 or client agenda, or asks for help being more visible at work. Triggers on phrases like "manage up", "update my boss", "write a recap", "agenda for my 1:1", "make my work visible", or any request that maps to one of the five moves. Use it even when the user doesn't name a move — describes a work-visibility problem and this likely fixes it. Don't use for general email drafting, performance reviews, or HR-style feedback.

### 3. `SKILL.md` body

Sections, all imperative, all specific to the playbook (not generic email-coach filler):

1. **Opening** — one paragraph: the failure mode the skill prevents (invisible work between meetings; the agent by default writes vague "here's what I did" lists instead of the call-you-made framing the playbook requires).
2. **Track first** — ask the user **one** question before doing anything: boss or client track. Both tracks exist for every move; wrong track = wrong recipient and wrong tone.
3. **Move selection** — match the request to one of the five moves; if ambiguous, ask. Numbered, with explicit mappings (e.g. "I need to update my boss" → Move 02; "recap a meeting" → Move 04).
4. **For the chosen move** — read the matching `references/move-NN-*.md`, then:
   - Collect the bracketed inputs from the user (one prompt, not three — the playbook's prompts already enumerate them).
   - Run the move's prompt verbatim against the user's inputs, with the track baked in.
   - Apply the move's insight as a hard guardrail on the output (e.g. Move 02 "if it sounds like a press release, rewrite it"; Move 04 "send to people in the meeting + 1–3 who weren't").
5. **Chaining** — Move 01 output is the input slot for Move 02, Move 04 often feeds Move 05. After delivering a move, proactively offer the next logical step in one sentence.
6. **Output contract** — every move returns: (a) the draft artifact (email, note, recap, agenda), (b) the 1–3 sentence insight applied, (c) one explicit next-move suggestion.
7. **When not to use** — generic email writing, performance reviews, salary negotiation, HR complaints. These share vocabulary but have different playbooks.

### 4. `references/move-NN-*.md` (one per move)

Each file is a self-contained runbook for one move, structured identically so the agent can pattern-match:

1. **Goal / Time** — straight from the source doc.
2. **Track switch** — which recipient and tone for boss vs client (lifted from the source, no invention).
3. **Inputs to collect** — list every `[bracketed]` slot from the source prompt; the agent must collect all of them before running the prompt, not invent them.
4. **Prompt** — the verbatim prompt from the source, with track already substituted in. No editorial rewrites; the source is the contract.
5. **Insight as guardrail** — the source's "What I wish I'd done sooner" line, phrased as a check the output must pass before being returned to the user.
6. **Chains into** — which move(s) this one's output feeds next.

### 5. `assets/tracks.md`

A short reference the agent loads once at the start of any run:

- Boss track: direct manager in the loop; recipient is the boss's boss or peer leaders; tone is calm-informational.
- Client track: no employer rhythm; recipient is the client CEO or renewal decider; tone is calm-confident; the insight is that work between calls is invisible by default.
- Persona phrasing snippets the agent can lift into prompts (e.g. "senior leader / fractional or independent operator", "[my boss's boss / my client's CEO or the person who decides whether to renew]").

### 6. Validation

- Frontmatter parses, `name` matches folder, `description` < 1024 chars.
- `SKILL.md` body ≤ ~150 lines; each `references/move-*.md` ≤ ~80 lines.
- Every prompt in `references/` is **byte-identical** to the source doc's prompt (the playbook is the contract).
- No secrets, no invented URLs, no invented tool names — only tools named in the source (Otter, Fireflies, Granola, Fathom, Wispr Flow).
- Smoke test: invoke the skill with `/managing-up boss update` and `/managing-up client recap` locally; confirm the agent picks Move 02 / Move 04, asks the track question (if not given), collects bracketed inputs, returns the artifact + next-move suggestion.

### 7. Repo hygiene

- Add `.gitignore` entry for any future `evals/` and `workspace/` scratch dirs.
- Update README with a one-paragraph "What's here" pointing at the skill and the source doc.
- Commit + push to `main` on `thomasbergernz/manageup`.

## Out of scope

- Quantitative evals (`evals/evals.json`) — playbook outputs are subjective writing; skip per the create-skill guidance.
- Description-optimizer script — defer until after a real run shows under/over-triggering.
- A second skill for the client track alone — the track is a flag, not a separate workflow.