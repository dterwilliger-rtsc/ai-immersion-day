# Prompt — Sheet Whisperer (Automations & Integrations, 25 pts → 38 at 1.5x)

> Quest: point Claude at a live Google Sheet in your Drive — no upload — and have it
> tell you something you didn't know.

## The sheet

An internal integrations knowledge base owned by another team — a flat log, one row per
captured piece of knowledge, 21 columns, 48 entries, built up over roughly ten weeks.
Chosen because it's my own team's subject matter, so anything it surfaced would be
useful past the quest.

Read live through the Google Drive connector. Nothing exported, nothing uploaded.

## Prompts used

Opening question — deliberately open, per the quest's "open questions beat directed
ones" framing:

```
Read the [named sheet] in my Drive. Don't chart anything and don't summarize the
contents back to me. Tell me what I don't know about this file — what's odd,
inconsistent, or quietly broken about it.
```

Then, pushing on what came back:

```
Which rows? Show me the counts. How many entries are missing required fields?
```

```
There are no formulas in this file. So what's the actual fragility here?
```

## Why the follow-ups mattered more than the first answer

The opening question produced plausible observations. The **counts** were what made
them real — and one of them collapsed under checking.

The connector's first read returned a *sample* of ten rows. Reasoning from that sample
would have produced a confidently wrong claim: that the file has ten entries, all from
a two-week window, and had been abandoned since. The real file has 48 entries spanning
ten weeks. Every interesting finding below is in the 38 rows the sample never showed.

Pulling the full file and counting properly is what turned this from a plausible
narrative into findings. That step — **verify the sample is the whole thing** — was the
actual lesson.

## The formula angle

The quest suggests pointing at the ugliest formula in the file and asking what it does.
**There are no formulas here.** It's a flat log; the only logic is dropdown validation
sourced from a reference tab. Recorded as a finding rather than manufacturing a formula
to explain: this file's fragility is entirely schema discipline and data hygiene, not
computation.

## Findings

→ [`findings-redacted.md`](findings-redacted.md)
