# AI Immersion Day — Quest Log

Single log for Rentsync's AI Immersion Day (2026-09-10). One folder per quest under
[`quests/`](quests/): the prompt used, the output produced, and an honest note on what
the AI actually added — including where it fell short.

Started on day two of the job, which is why several quests use onboarding as their
real-work subject matter.

---

## Quests

| Quest | Category | Pts | Folder | Status |
|---|---|---|---|---|
| [Meeting Prep](quests/meeting-prep/) | Automations & Integrations | 25 | `quests/meeting-prep/` | ✅ Logged |
| [Prompt Like a Pro](quests/prompt-like-a-pro/) | Discovery | 10 | `quests/prompt-like-a-pro/` | ✅ Logged |
| [Sheet Whisperer](quests/sheet-whisperer/) | Automations & Integrations | 38 | `quests/sheet-whisperer/` | ✅ Logged |

**Running total: 73 pts** (a 1.5x multiplier was live for Sheet Whisperer — 25 base → 38)

### Meeting Prep — 25 pts
Claude read the next real meeting via Google Calendar, opened the PDF attached to the
invite to extract the actual agenda, cross-referenced Slack and Drive on the attendees,
and flagged an un-RSVP'd session later in the week.

The context layer is what mattered: the calendar alone would have produced a
restatement of the invite. It also **got a status claim wrong** — misattributing an
RSVP between two events — which is corrected in place and kept visible, because that
turned out to be the most transferable lesson of the quest.

→ [prompt](quests/meeting-prep/prompt.md) · [output](quests/meeting-prep/brief-redacted.md)

### Sheet Whisperer — 38 pts
Pointed Claude at a live integrations knowledge base in Drive — no export, no upload —
and asked the open question rather than a directed one: *what don't I know about this
file?*

It found that **26 of 48 entries are missing the Entry ID the sheet's own instructions
mark as required**, that ID'd and un-ID'd rows split perfectly along a second axis
(original source text preserved vs. dropped) revealing two ingestion paths, that the
date column holds both ISO strings and raw spreadsheet serials, and that Root Cause —
the most valuable column in a troubleshooting KB — is filled on 7 of 48 rows.

The near-miss is the lesson: the connector's first read was a ten-row **sample**, and
the confident story available from it was wrong on every count. Pulling the full file
changed the conclusions.

→ [prompt](quests/sheet-whisperer/prompt.md) · [findings](quests/sheet-whisperer/findings-redacted.md)

### Prompt Like a Pro — 10 pts
Same task (a first-day Slack intro) run twice: one line, then role + context + task +
example + constraints. The example field did more work than the role line.

Both outputs still contain placeholders, so the structured prompt produced a better
*scaffold*, not a finished post — noted rather than glossed over.

→ [prompt](quests/prompt-like-a-pro/prompt.md) · [one-line output](quests/prompt-like-a-pro/output-one-line.md) · [structured output](quests/prompt-like-a-pro/output-structured.md)

---

## Ground rules I set for myself

**Real work, not demo scenarios.** Every quest runs against something actual — my real
calendar, my real inbox. That means raw output contains colleague names, email
addresses and internal detail, so every artifact here is redacted before it lands, and
unredacted working files are gitignored (`*.local.md`). Structure and reasoning are
untouched; only identifying and internal-confidential detail is generalized.

**Failures stay in.** Where the AI got something wrong, the correction is visible rather
than rewritten away. A log of only successes wouldn't be much use to anyone reading it
later, including me.

---

## Setup notes for anyone else running these

- **Connectors:** Google Calendar, Gmail, Drive and Slack were already authorized on my
  account — check before spending time on setup.
- **The main hackathon project needs no Git repo.** Per `#ai-immersion-day`, the core
  project publishes to Harbour with contributors added there. This repo is for the
  individual side quests.
- **Homebrew on a fresh Mac:** if `gh` and friends aren't found, `brew shellenv` in
  `~/.zprofile` only loads for *login* shells. Add it to `~/.zshenv` too.
