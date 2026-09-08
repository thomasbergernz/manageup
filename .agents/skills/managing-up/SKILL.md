---
name: managing-up
description: Runs the Managing Up five-moves playbook: Audit, Send It Up, Note Before the Meeting, Recap, Send the Agenda First. Use this whenever the user wants to update their boss, write a status email, prep for a meeting, recap a meeting, send a 1:1 or client agenda, or asks for help being more visible at work. Triggers on phrases like "manage up", "update my boss", "write a recap", "agenda for my 1:1", "make my work visible", or any request that maps to one of the five moves. Use it even when the user doesn't name a move — if the user describes a work-visibility problem, this likely fixes it. Don't use for general email drafting, performance reviews, or HR-style feedback.
---

# Managing Up

The default failure mode this skill prevents: the agent writes a vague "here's what I did" list, frames accomplishments as tasks instead of calls made, picks the wrong recipient, or ships a recap that nobody outside the room ever sees. The playbook insists on call-you-made framing, a named recipient one level up, and sending to people who weren't in the room.

## Workflow

### Step 1 — Pick the track

Every move has two tracks. **Ask one question first** if the user hasn't named it:

- **Boss track** — they have a direct manager.
- **Client track** — independent, fractional, or juggling multiple engagements.

If the user said "boss", "my manager", "my CEO", or named a client, skip the question. Otherwise ask. The track decides recipient, tone, and which bracketed options to fill in the prompt. Read `assets/tracks.md` for the persona phrasing snippets.

### Step 2 — Pick the move

Map the request to exactly one of the five. If ambiguous, ask which move they want — do not guess.

| User intent                                              | Move |
|----------------------------------------------------------|------|
| "What did I actually get done this quarter?"             | 01 Audit |
| "Update my boss's boss" / "quarterly update" / "make my work visible" | 02 Send It Up |
| "Shape a meeting I'm not running" / "send a note before a meeting"    | 03 Note Before the Meeting |
| "Recap a meeting" / "write the post-meeting email"                  | 04 Recap |
| "Agenda for my 1:1" / "agenda for my client check-in"               | 05 Send the Agenda First |

### Step 3 — Run the move

Read the matching `references/move-NN-*.md` for that move only. Then:

1. **Collect every bracketed input** the move's prompt lists. Ask in a single grouped prompt — the playbook already enumerates them, so don't ask one question per slot. First resolve every slot the track already decides against the table in `assets/tracks.md`; only the rows marked **ask** there are questions for the user.
2. **Substitute the track** into the prompt before running it. Boss or client phrasing goes in the `[bracketed]` slots.
3. **Run the move's prompt verbatim** against the user's inputs. The playbook is the contract — do not rewrite the prompts.
4. **Apply the insight as a hard guardrail** before returning output. Every move has an "Insight" check in its reference file. If the draft fails it, rewrite until it passes.

### Step 4 — Chain

After delivering a move, surface one next move in a single sentence. Natural chains:

- Move 01 (Audit) → Move 02 (Send It Up): paste the audit summary as the accomplishments slot.
- Move 04 (Recap) → Move 05 (Agenda First): the recap's "what's next" feeds next week's agenda wins.
- Move 03 (Note Before the Meeting) → Move 04 (Recap): same meeting, post-event.

### Output contract

Every move returns three things, in this order:

1. **The draft artifact** — the email, note, recap, agenda, or audit summary.
2. **The insight applied** — one sentence naming the guardrail the draft passed (or was rewritten to pass).
3. **One next-move suggestion** — single sentence, one of the chains above.

## When not to use

- General email drafting with no managing-up intent.
- Performance reviews, salary negotiation, HR complaints — different playbooks.
- Sales outreach or marketing copy.

## References

- `assets/tracks.md` — boss vs client track switcher.
- `references/move-01-audit.md`
- `references/move-02-send-it-up.md`
- `references/move-03-note-before-meeting.md`
- `references/move-04-recap.md`
- `references/move-05-agenda-first.md`