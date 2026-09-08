# Tracks: Boss vs Client

Load this once at the start of any move. Every move has two parallel versions; the track decides recipient, tone, and which bracketed options to substitute.

## Bracket resolution by track

The track decides most bracketed slots. Resolve them from the track — **do not ask the user a
question the track already answers.** Only the "ask" rows below are real questions.

| Move | Slot | Boss track | Client track |
|------|------|-----------|--------------|
| 01 | persona | `senior leader` | `fractional or independent operator` |
| 02 | recipient | `my boss's boss` | `my client's CEO or the person who decides whether to renew` |
| 02 | Version A/B | default **A** (warm, not asking) | default **B** (calls made, what changed) |
| 03 | Scenario A/B/C | **ask**: A (runs it) or B (doesn't run it) | **C** — unless the client runs the meeting, then B |
| 04 | Situation A/B/C | **ask**: A (ran it or presented) or B (owns what happens next) | **C** |
| 05 | recipient / meeting | `my boss` / `1:1` | `my client` / `check-in` |

Version A/B in Move 02 is a tone choice the track already implies — the boss tone is A's
"friendly and informational, not asking for anything", the client tone is B's "calm and
confident, focused on the calls I made". State the default in one line and let the user
override it; don't pose it as an open question.

Scenario C in Moves 03 and 04 is client-shaped by definition. Never pair boss track with C,
and never pair client track with A. If the user's answer contradicts their track, name the
contradiction and re-confirm the track.

## Boss track

**Situation:** the user has a direct manager. The implicit rhythm of an org still applies — updates flow upward, the work is supposed to be visible to senior leaders.

**Recipient one level up:** the boss's boss, peer senior leaders who care about this work, or the other teams the work touches.

**Tone:** calm and informational, not asking for anything. Wins and what's next, no asks.

**Persona phrasing to lift into prompts:**

- `[senior leader / fractional or independent operator]` → use `[senior leader]`
- `[my boss's boss / my client's CEO or the person who decides whether to renew]` → use `[my boss's boss]`
- `[my boss / my client]` → use `[my boss]`

## Client track

**Situation:** the user is independent, fractional, or juggling multiple engagements. No employer rhythm forces them to send updates, so the work between calls becomes invisible by default. The whole point of the playbook on this track is that nobody is making the user send the Friday update anymore — fix that.

**Recipient one level up:** the client's CEO, the person who decides whether to renew, people who send referrals.

**Tone:** calm and confident, focused on the calls made and what changed because of them. A list of updates reads like a vendor; a strategic question reads like a partner.

**Persona phrasing to lift into prompts:**

- `[senior leader / fractional or independent operator]` → use `[fractional or independent operator]`
- `[my boss's boss / my client's CEO or the person who decides whether to renew]` → use `[my client's CEO or the person who decides whether to renew]`
- `[my boss / my client]` → use `[my client]`