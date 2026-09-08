# Move 01 — The Audit

## Goal / Time

Figure out what you actually got done, and who never heard about it. 15 minutes.

The audit goes first because every other move uses what it finds. Five questions. Question 5 is the one that stings.

## Track switch

- **Boss track:** question 5 names the boss's boss, peers, senior leaders who care about this work, and the other teams the work touches.
- **Client track:** question 5 names the client's CEO, the person who decides whether to renew, and people who send referrals.

## Inputs to collect

- **Persona:** `senior leader` (boss) or `fractional or independent operator` (client).

That's it. The remaining inputs are the user's free-text answers to the five audit questions below. Ask them all together, in this exact form:

1. What are the 3 biggest things I or my team got done this quarter? Names, numbers, outcomes.
2. For each one, what was the call I made that other people would have gotten wrong?
3. What did each of those things make possible that wasn't possible before?
4. Who already knows about each of these? List names and titles.
5. Who should know but currently doesn't? List names and titles.

If the user can't fill question 5, that itself is the useful answer — surface it.

## Prompt

Run verbatim, with the persona substituted:

> I'm a [senior leader / fractional or independent operator] doing a quarterly audit of what I delivered and who knows about it. I'm going to answer 5 questions and I want you to push back if my answers are vague, generic, or missing the call I made. After my answers, summarize my top 3 accomplishments for the quarter in 1 sentence each, focused on the call I made and what changed because of it, not just what got done.
>
> 1. What are the 3 biggest things I or my team got done this quarter? Names, numbers, outcomes.
> 2. For each one, what was the call I made that other people would have gotten wrong?
> 3. What did each of those things make possible that wasn't possible before?
> 4. Who already knows about each of these? List names and titles.
> 5. Who should know but currently doesn't?
>    - **Boss track:** your boss's boss, peers, senior leaders who care about this work, the other teams your work touches.
>    - **Client track:** the client's CEO, the person who decides whether to renew, people who send you referrals.
>    List names and titles.

## Insight as guardrail

The gap between question 4 (who already knows) and question 5 (who should know) **is the to-do list**. Before returning the summary, confirm the user's question 5 is non-empty and contains real names and titles — not "various stakeholders" or "the leadership team". If it is empty, push back: that itself is the answer.

The summary must be three sentences, each focused on the call made and what changed — not a task list.

## Chains into

Move 02 (Send It Up). Paste the three-sentence summary as the accomplishments slot.