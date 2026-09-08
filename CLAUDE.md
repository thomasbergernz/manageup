# CLAUDE.md

## What this repo is

A prose/prompt repo, not a code repo. It holds the "Managing Up: Five Moves"
playbook and packages it as an invocable agent skill. No build, no tests, no
dependencies, no runtime.

- `managing_up_five_moves.md` — the source playbook. **Source of truth.**
- `messy_prompt.txt` — the raw original draft the playbook was cleaned up from.
  Historical; do not edit or cite it.
- `plan.md` — the approved conversion plan, including the validation rules below.
- `.agents/skills/managing-up/` — the skill.

Skills live under `.agents/skills/`, not `.claude/skills/`. That is the Warp
convention and is deliberate — the skill is consumed as
`thomasbergernz/manageup:managing-up`.

## Hard rules when editing the skill

1. **Move prompts must stay byte-identical to `managing_up_five_moves.md`.**
   The playbook is the contract. To change a prompt, change the source doc
   first, then propagate. Never rewrite a prompt in `references/` alone.
2. **Line budgets:** `SKILL.md` ≤ 150 lines (currently 70), each
   `references/move-*.md` ≤ 80 lines. `SKILL.md` is always-loaded context.
3. **Reference files share one fixed shape.** Six sections, same order in every
   file: Goal / Time, Track switch, Inputs to collect, Prompt, Insight as
   guardrail, Chains into. The agent pattern-matches on it.
4. **Boss vs client is a flag, not a second skill.** Every move has both tracks.
   New behaviour goes in `assets/tracks.md`, not a parallel skill.
5. **No invented content.** No new tools, URLs, or persona phrasings that aren't
   in the source doc. Named tools are only Otter, Fireflies, Granola, Fathom,
   Wispr Flow.

## Verifying a change

No test runner. Check by hand:

```sh
# every prompt line in every reference still appears in the source doc
for f in .agents/skills/managing-up/references/move-*.md; do
  sed -n 's/^> //p' "$f" | while IFS= read -r l; do
    grep -qF -- "$l" managing_up_five_moves.md || echo "$f MISS: $l"
  done
done

# NOTE: containment only — catches edited lines, not deleted or reordered ones.
# For byte-identity, extract the source's `## Move NN` block and diff it.

# line budgets
wc -l .agents/skills/managing-up/SKILL.md .agents/skills/managing-up/references/*.md
```

The router smoke test (`/managing-up boss update` → Move 02,
`/managing-up client recap` → Move 04, each asking for the track when it isn't
given) runs in **Warp**. Claude Code does not discover `.agents/skills/`, so
from here either read the router table in `SKILL.md` directly, or symlink
`.claude/skills/managing-up` → `.agents/skills/managing-up` to invoke it.

## Scratch

`evals/`, `workspace/`, and `*-workspace/` are gitignored. Put iteration
scratch there.
