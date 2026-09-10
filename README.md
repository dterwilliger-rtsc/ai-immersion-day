# AI Immersion Day — Quest Log

Evidence repo for Rentsync's AI Immersion Day hackathon.
One directory per quest: the prompt used, the connectors involved, the output
produced, and an honest note on what the AI actually added.

Started 2026-09-10, day two on the job.

---

## Ground rule I set for myself

Every quest gets run against **real work**, not a demo scenario. That means the
raw output usually contains colleague names, email addresses and internal
detail — so every artifact in this repo is redacted before it lands here, and
unredacted working files are gitignored. The structure and reasoning are
untouched; only identifying and internal-confidential detail is generalized.

---

## Quests

### 1. Meeting Prep — 25 pts · Automations & Integrations
**Goal:** the connector habit with the highest repeat value — ten minutes saved
before every meeting, and the follow-up drafted after.

| | |
|---|---|
| Prompt | [`prompts/01-meeting-prep.md`](prompts/01-meeting-prep.md) |
| Output | [`output/meeting-brief-redacted.md`](output/meeting-brief-redacted.md) |
| Connectors | Google Calendar, Gmail, Google Drive, Slack |
| Model | Claude Opus 5 via Claude Code |
| Status | ✅ Complete |

**One line:** Claude read my next meeting, opened the PDF attached to the invite
to extract the real agenda, cross-referenced Slack and Drive for who I'd be
talking to, and flagged a reporting-line contradiction across three systems plus
an un-RSVP'd session next week.

**Honest assessment:** the calendar layer alone would have produced a
restatement of the invite. The value was entirely in the Drive/Gmail/Slack
context layer — reading the attachment, and dating the organizer's own start
from a Slack announcement.

It also got something wrong: it misattributed an RSVP status between two events
on the same calendar. Corrected in the output, with the correction left visible
rather than rewritten away. See the closing sections of the output for both.

---

## Setup notes

No connector configuration was needed — Google Calendar, Gmail, Drive and Slack
were already authorized on the account. Worth knowing for anyone else running
these quests: check before you spend time on setup.
