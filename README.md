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
A sourced briefing on MCP authorization — specifically, how you restrict which AI agents
can connect to a product's MCP server. Commissioned against a real internal program doc
that leaves exactly that question open.

The instruction that did the work was *"verify that claim — don't take it at face value."*
The internal doc cites a named vendor as the model for its planned design; two source
fetches showed the vendor does close to the **opposite**, and the vendor's own overview
page omits authentication entirely, which is plausibly how the claim got made in good
faith. A second finding: the MCP spec has no agent-identity primitive at all, and
*recommends* Dynamic Client Registration so clients self-register without user
interaction — so an allowlist isn't a deferred admin feature, it's an authorization-server
policy decision.

Two things worth recording about the method. A source I was asked to include **couldn't be
found** from a description — I said so and left it out rather than paraphrasing something
plausible; supplied as a URL later, it turned out to be about a different topic than either
of us expected, so the guess would have been wrong on substance. And the most useful line
in the briefing came from the **lowest-ranked** source: source ranking should govern how
much weight a claim carries, not whether it's worth reading.

→ [briefing](quests/research-assistant/briefing.md) · [prompt & method](quests/research-assistant/prompt.md) · [one-paragraph team version](quests/research-assistant/team-post.md)

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
