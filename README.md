# manageup

The **Managing Up** playbook as an invocable agent skill, plus the source document it was built from.

- `managing_up_five_moves.md` — the source playbook (five moves, boss vs client tracks, paste-ready prompts).
- `plan.md` — the approved plan for converting the source into a skill.
- `.agents/skills/managing-up/` — the `managing-up` skill for Warp / Warp-compatible agents. Invoke with `/managing-up` or describe the intent (e.g. "update my boss", "recap a meeting", "agenda for my 1:1") and the skill routes to the right move.

## Skill layout

```
.agents/skills/managing-up/
├── SKILL.md                 # router + workflow (track-first, move selection, chaining)
├── assets/tracks.md         # boss vs client track switcher
└── references/
    ├── move-01-audit.md
    ├── move-02-send-it-up.md
    ├── move-03-note-before-meeting.md
    ├── move-04-recap.md
    └── move-05-agenda-first.md
```

Move prompts are byte-identical to the source. The skill adds the track switch, the bracketed-input collection, the insight guardrail on the output, and the chain to the next move.

## Use

```
/managing-up boss update             # Move 02, boss track
/managing-up client recap            # Move 04, client track
/managing-up agenda for my 1:1       # Move 05
```

Or describe the intent in plain language — the skill's description is pushy enough to fire on any work-visibility request.