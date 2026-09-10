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
| [Morning Nudge](quests/morning-nudge/) | Automations & Integrations | 75 | `quests/morning-nudge/` | ✅ Logged |
| [Research Assistant](quests/research-assistant/) | Automations & Integrations | 38 | `quests/research-assistant/` | ✅ Logged |

**Running total: 186 pts** (a 1.5x multiplier was live for the later quests — Sheet Whisperer 25→38, Morning Nudge 50→75, Research Assistant 25→38)

### Meeting Prep — 25 pts
Claude read the next real meeting via Google Calendar, opened the PDF attached to the
invite to extract the actual agenda, cross-referenced Slack and Drive on the attendees,
and flagged an un-RSVP'd session later in the week.

The context layer is what mattered: the calendar alone would have produced a
restatement of the invite. It also **got a status claim wrong** — misattributing an
RSVP between two events — which is corrected in place and kept visible, because that
turned out to be the most transferable lesson of the quest.

→ [prompt](quests/meeting-prep/prompt.md) · [output](quests/meeting-prep/brief-redacted.md)

### Research Assistant — 38 pts
A sourced briefing on AI lead scoring for rental leads, commissioned and delivered inside
the hour my team was choosing its build project in Slack — four of six had said they
wanted that project and nobody had looked into the problem yet.

The load-bearing instruction was *"chase every statistic to its origin and tell me who
published it and whether they were selling something."* It inverted the conclusion. The
plan was to weight signals by the industry's conversion benchmarks; there are no credible
public ones. Speed-to-lead, the five-minute window, the 65–80% hourly drop-off, a 44.8%
lift — every figure traces to a company selling an AI leasing product, and the one trade
article promising a research review cites two vendors with no methodology and leaves its
headline number uncited. Reporting the absence beat laundering the numbers.

The finding I wasn't looking for was regulatory: US fair-housing guidance covers
*"limiting or denying consumers information about housing opportunities"* — which is what
deprioritising a lead does — and names third-party technology providers as responsible
parties. In Ontario it's more direct still, since receipt of public assistance is itself a
protected ground covering access to rental opportunities. That turns "explain the score"
from a feature into the design's foundation.

→ [briefing](quests/research-assistant/briefing.md) · [prompt & method](quests/research-assistant/prompt.md) · [team version](quests/research-assistant/team-post.md)

### Morning Nudge — 75 pts
A scheduled task that DMs me on Slack every weekday at 8am with every mention and DM
still waiting on a reply. First run set for the next morning, so the proof is waking up
to it working.

The cron expression was the easy part. The quest is the **tuning**, and two things came
out of it: Slack *search alone under-reports* (a search for my own user ID returned
nothing, and `to:me` returned only messages I'd sent), so the task enumerates DM
channels and reads them directly. And the naive pass produced 1 true positive against 5
categories of false positive — join notices, bot DMs, already-answered threads,
messages addressed to someone else, and broadcast FYIs — each of which became an
explicit exclusion rule.

The rule that nearly got missed found the only real item: **an open question put to a
group, answered by others and not by me, counts.** A filter that only matched direct
@-mentions would have skipped it.

Tuned against a single quiet day, so the filters are the right shape but under-tested —
noted in the writeup rather than claimed as proven.

→ [writeup](quests/morning-nudge/prompt.md) · [live task prompt](quests/morning-nudge/task-prompt.md)

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
