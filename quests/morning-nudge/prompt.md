# Morning Nudge (Automations & Integrations, 50 pts → 75 at 1.5x)

> Quest: a scheduled task that Slacks you every morning with every unanswered mention
> and DM — set to run tomorrow.

## Order of operations

The quest is emphatic about this and it's right: **one-off first, schedule second. Never
schedule a prompt you haven't tuned.** The tuning is the quest; the cron expression is
the easy part.

## Pass 1 — the naive version

First attempt used Slack search for mentions and messages addressed to me. Two problems
surfaced immediately:

1. **Search alone under-reports.** A search for my user ID returned nothing, and a
   `to:me` search returned only messages I had *sent*. Inbound messages with no keyword
   match don't surface. The fix: enumerate DM and group-DM channels directly and read
   each one's recent history, using search only as a supplement.
2. **Everything visible looked like a hit.** Join notices, bot DMs, already-answered
   threads and messages addressed to other people all came back as "unanswered."

## Pass 2 — counting the false positives

Against one real day of Slack, the naive pass produced **1 true positive and 5
categories of false positive.** Each false positive became an explicit exclusion rule:

| False positive | Rule added |
|---|---|
| A DM containing only "X accepted your invitation to join Slack" | Exclude Slack system and join notices — not messages |
| A group-DM message @-mentioning a different person | Exclude messages addressed to someone else that I merely see |
| Two DMs I had already replied to | Exclude anything where my latest message is newer than theirs |
| Bot/app DMs (Slackbot and two integrations) | Exclude bot and app accounts |
| Broadcast announcements with no ask directed at me | Exclude pure FYIs and @here with no request to me |

And one inclusion rule that the naive pass nearly *missed*, which turned out to be the
only real item:

> **An open question put to a group I'm in, that others answered and I didn't, counts.**

That was the actual find — a question about which project the team should pick, asked
in a group DM, answered by three other people, never answered by me, and live at the
moment the digest ran. A rule that only counted direct @-mentions would have skipped it.

## The tuned schedule

- **Cadence:** weekdays, 8:00 AM `America/St_Johns` (`0 8 * * 1-5`)
- **First run:** the next morning — so the proof is waking up to it working
- **Destination:** my own Slack DM. Private, and nothing posts anywhere else
- **Monday's run** covers the whole weekend, not just 24h
- **Output:** count first ("2 things waiting on you"), one line per item with who /
  where / age / what they need / permalink, `⏰` on anything over 24h, capped at 12
  lines. If nothing qualifies it says so in one line rather than padding.
- **Explicit guardrails in the prompt:** post only to that one channel ID, and never
  reply to any message it finds.

Full task prompt: [`task-prompt.md`](task-prompt.md)

## What I'd watch for

The rules are tuned against a **single day** of my Slack — and my second day at the
company, so the volume was tiny. The filters are the right shape but they're
under-tested; a normal week with real channel traffic will produce false positives these
five rules don't cover. The honest status is "tuned enough to schedule, not proven."

## Operational notes worth knowing

- Scheduled tasks run **while the app is open.** If it's closed when the task is due, it
  runs on next launch — so a missed morning isn't a broken task.
- Tool approvals granted during a run are remembered for later runs. Running it once
  manually first pre-approves the Slack connector, so future runs don't stall waiting on
  a permission prompt.
