# Prompt — Quest 1: Meeting Prep

Run verbatim in Claude Code (Claude Opus 5) with Google Calendar, Gmail, Google
Drive and Slack connectors authorized.

The first three paragraphs are the quest's supplied prompt. The fourth line is
my own addition — the quest's "Go Further" hint is about scheduling, but I
wanted the week-ahead sweep in the same pass.

```
Act as my chief of staff. Look at my next meeting on my calendar.

Brief me: who's attending, what it's about, what I should be ready to answer,
and what I should ask.

If you can see Slack or my Drive, pull anything relevant to this meeting and
give me the two-line version of it.

And flag anything else on my calendar this week that needs prep I haven't done.
```

## What made it work

- **Say the role, not the task.** "Act as my chief of staff" got a different
  shape of answer than "summarize my next meeting" would have — it produced
  questions to ask, not just facts to know.
- **Name the connectors explicitly.** "If you can see Slack or my Drive" is what
  pushed it past the calendar entry into the attached deck and the Slack history.
- **Ask for what you haven't done.** The week-ahead line is what surfaced the
  missing RSVP. Asking "what's on my calendar" would not have.

## Next iteration

Schedule it as a recurring 8am task so the brief arrives before the day starts
rather than being pulled on demand.
