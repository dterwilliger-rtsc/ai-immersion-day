# Chief-of-Staff Meeting Brief — generated output (redacted)

**Quest:** Meeting Prep · Automations & Integrations · 25 pts
**Run:** 2026-09-10 · **Target meeting:** next real calendar event, the following day
**Connectors used:** Google Calendar, Gmail, Google Drive, Slack

> **Redaction note:** colleague names, email addresses, meeting links and internal
> HR/benefits specifics have been removed or generalized. Structure, reasoning and
> the flagged action items are unchanged. The unredacted brief stays local.

---

## The meeting
- **What:** New Hire HR Orientation (video call), 60 minutes
- **Organizer:** HR Generalist
- **Also invited:** HR lead (optional, no response)
- **Attachment:** orientation slide deck (PDF) — Claude read it and pulled the agenda

## Who's in the room
| Person | Role | Context Claude surfaced |
|---|---|---|
| Organizer | HR Generalist | Joined the company ~10 weeks ago — also relatively new. Runs the session. |
| Optional attendee | HR lead | The organizer's manager. Company-wide instruction is to cc the organizer on anything sent to them. |
| Me | Platform team, start date 2026-09-10 | Three different sources named three different reporting signals — flagged as a question to ask. |

## What it's about — agenda extracted from the attached deck
Company overview & history · work hours · benefits · Slack · recognition &
rewards platform · HR system · key contacts.

Substantive items: 8h day + unpaid break, defined core hours in Eastern,
flexible start/finish by agreement with manager, no timesheets. Benefits
activate after a probationary period, with third-party enrollment email
expected within days of start.

## What I should be ready to answer
- **Preferred working hours.** I'm in Newfoundland, 1.5h ahead of Eastern, so
  company core hours shift for me. This is the one real decision in the deck
  and it's mine to make.
- Confirmation that laptop, Google, password manager and Slack all work — my
  team onboarding doc shows all four already complete.
- Payroll details, if not already in the HR system.

## What I should ask
1. Exact benefits start date, and whether the enrollment email has been triggered
   (nothing from the provider in my inbox yet).
2. Health/wellness spending account amounts — the deck names them but not the figures.
3. **Who is my manager on paper?** IT's email, my welcome doc, and my first team
   calendar invite each pointed somewhere different. Also: who runs my 90-day review?
4. Probation length and what the check-in looks like.
5. Time zone in practice — is anyone else on my team outside Eastern?
6. Where time off gets submitted, and my balance.

## Two-line context from Drive & Slack
> **Drive:** My team welcome doc shows all setup complete, with a Week 1 plan, a
> running questions doc, and a weekly to-do list still open — HR orientation is
> the formal layer on top of the team onboarding already underway.
>
> **Slack:** The organizer was introduced company-wide ~10 weeks ago as the new
> HR Generalist with an explicit "include her on anything you'd send [the HR lead]"
> instruction, so she's the correct first stop — and the company has announced new
> hires nearly monthly all year, so this is a well-worn track, not an improvised one.

## Rest of the week — prep flags
| When | What | Status |
|---|---|---|
| Today | AI Immersion Day (team session) | ✅ Accepted. One other attendee is still unresponded. |
| Today | Hackathon, 11–6 | 🟡 In progress. Invite promised "more info to come" and never delivered — worth asking the organizer about submission format. |
| Next Thu | [Optional] AI Open Forum, drop-in | ⚠️ **No RSVP from me.** Explicitly for showcasing AI wins — **good venue to demo this.** |

Nothing else on the calendar for the following 10 days.

**Also unaddressed:** two unread Slack account-setup emails from the last 24h.

---

## Correction (same day)

The first version of this brief claimed I hadn't RSVP'd to today's team session.
That was wrong — I had accepted; the unresponded attendee was someone else on the
invite. The genuinely un-RSVP'd event was next week's optional AI forum. Caught by
checking the calendar UI directly against the API response.

Leaving the correction visible rather than quietly rewriting it, because "verify the
brief against the source before you act on it" is the actual lesson of this quest.

## What actually earned its keep

Things I would not have had without the connectors:

1. **It read the attached PDF.** The agenda above isn't guessed from the meeting
   title — it came out of the deck attached to the invite.
2. **It cross-referenced Slack to date the organizer's own start.** Knowing the
   person running your orientation is ten weeks in changes how you read the session.
3. **It caught a reporting-line contradiction across three systems** (IT email,
   welcome doc, calendar invite) that no single source would have shown.
4. **It caught a missing RSVP** on next week's optional AI forum — the one session
   where this work would actually get seen.
5. **The timezone math.** Core hours are stated in Eastern; I'm not in Eastern.
   It converted, then flagged it as the one decision actually mine to make.

Two failure modes worth noting:

- Without the Drive and Gmail layer, this brief would have been a restatement of
  the calendar invite. The context layer is what turned it from a summary into
  something with action items.
- It misattributed an RSVP status between two events on the same calendar (see
  the correction above). The per-attendee detail was right there in the API
  response and still got read onto the wrong event — so the status lines are
  exactly the part worth spot-checking before acting.
