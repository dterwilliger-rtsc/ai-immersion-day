<!-- Redacted for publication: Slack user/channel IDs and my email address are
     replaced with placeholders. Everything else is the live task prompt verbatim,
     copied from ~/.claude/scheduled-tasks/morning-nudge/SKILL.md -->

---
name: morning-nudge
description: Weekday 8am Slack digest of mentions and DMs from the last day that still need a reply from me.
---

Build my "Morning Nudge" digest and post it to my own Slack DM.

## Who I am
Slack user ID: <MY_SLACK_USER_ID> (<my work email>). Timezone: America/St_Johns (NDT).

## Step 1 — Gather
Using the Slack connector, collect everything addressed to me in the last 24 hours (since 8:00 AM the previous weekday, so Monday's run covers the whole weekend):

- Direct messages to me. Enumerate my DM and group-DM channels (slack_list_user_channels with types="im,mpim") and read each one's recent messages rather than relying on search alone — search misses inbound messages that have no keyword match.
- Channel messages that @-mention me directly, and any thread I'm participating in that has new replies.

## Step 2 — Filter: does it actually need a reply from ME?
Include an item ONLY if a person is waiting on a response from me. Apply these exclusions — each one came from a real false positive:

1. EXCLUDE anything I already replied to. If my most recent message in that DM or thread is newer than theirs, it's handled. This is the most common false positive.
2. EXCLUDE Slack system and join notices ("X accepted your invitation to join Slack", "has joined the conversation", channel join/leave). These are not messages.
3. EXCLUDE bot and app DMs — Slackbot and any other app/bot account.
4. EXCLUDE messages addressed to someone else that I merely happen to see. In a group DM, a message that @-mentions a different person, or answers someone else's question, is not mine to answer.
5. EXCLUDE pure FYIs, announcements and broadcast @here/@channel with no question or request directed at me.
6. EXCLUDE threads someone else already resolved — if a later reply from anyone answers the question, I'm not the blocker.
7. INCLUDE, always, a direct question or request to me that I have not answered — including an open question put to a group I'm a member of that I never responded to while others did.

Prefer a short accurate list over a long one. If nothing qualifies, say so plainly.

## Step 3 — Post
Post to my own Slack DM: channel ID <MY_SELF_DM_CHANNEL_ID>.

Format:

*🔔 Morning Nudge — <weekday, Month D>*

Then one bullet per item, most urgent first:
• *<who>* in <#channel or "DM">, <how long ago> — <what they need in one line> — <permalink>

Rules for the message:
- Lead with the count: "2 things waiting on you" / "Nothing waiting on you."
- Age each item in hours or days. Anything over 24h, flag with ⏰.
- If an item is time-sensitive (a decision being made now, a deadline today), say so.
- Keep the whole message under 12 lines. This is a nudge, not a report.
- If nothing qualifies, post just: "*🔔 Morning Nudge — <date>* — Nothing waiting on you. 🎉"

Do not post anything anywhere other than channel <MY_SELF_DM_CHANNEL_ID>. Do not reply to any of the messages you find.