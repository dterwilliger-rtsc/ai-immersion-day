# Prompt — Research Assistant (Automations & Integrations, 25 pts → 38 at 1.5x)

> Quest: commission a sourced briefing on a topic your team genuinely needs — then
> actually share it.

## Topic, and why it qualified

The quest insists on a real audience rather than curiosity. This had the most real
audience available: **my team was choosing its build project in Slack while I researched.**

Four of six members had said they wanted the AI Lead Scoring Service within about ten
minutes, one had proposed a vote, and nobody had looked into the problem itself. A
briefing delivered inside that window is decision input. The same briefing delivered
tomorrow is trivia.

## The prompt

```
Act as a research assistant. My team is deciding right now whether to build an AI
lead scoring service for rental leads — it ranks incoming renter enquiries by how
serious they look, so staff follow up on the best ones first.

Research what we should know before starting. Two things specifically:
what behavioural signals actually predict that a rental lead converts, and what
could go wrong with a system that decides which renters get followed up.

Search for current sources — don't answer from memory. Chase every statistic to
its origin and tell me who published it and whether they were selling something.
Rank sources by reliability, separate established fact from your own
recommendation, and flag anything you couldn't verify. If the evidence for
something is weak, say that instead of repeating it.
```

The instruction that did all the work: **"chase every statistic to its origin and tell me
who published it and whether they were selling something."** That single clause inverted
the briefing's conclusion.

## What happened

**The plan was to collect the industry's conversion benchmarks and weight signals by
them. There are no credible public benchmarks.** Every statistic — speed-to-lead as the
top predictor, the five-minute window, a 65–80% drop-off after an hour, 44.8% conversion
lift — traces back to a company selling an AI leasing product. The one piece of trade
press promising a research review attributed its numbers to two vendors with no
methodology or sample size, left its central figure uncited entirely, and raised no
caveats.

Reporting that absence turned out to be more useful than repeating the numbers would have
been.

**The finding I wasn't looking for was the regulatory one.** US fair-housing guidance from
2024 covers the targeting and delivery of housing opportunities — violations include
"limiting or denying consumers information about housing opportunities" — which is a
description of deprioritising a lead. It names third-party technology providers as
responsible parties, not only landlords. And the Canadian position is more directly
applicable, because receipt of public assistance is itself a protected ground in Ontario
housing, with protection extending to *access to rental opportunities*.

That reframes the build: "explain what drove the score" stops being a nice feature and
becomes the thing that makes the system defensible and debuggable.

## Spot-checks

Two, per the quest, and the first one changed the recommendation:

1. **Chased the conversion statistics.** Fetched the trade-press "research" review and
   found vendor attribution with no methodology. Became Finding 1 and killed the original
   plan.
2. **Checked scope of the fair-housing guidance** — whether it covers lead prioritisation
   or only application screening. It covers targeting and delivery. Then checked the
   Canadian equivalent, which was sharper rather than softer.

## Output

→ [`briefing.md`](briefing.md) — one-sentence answers to each of the project brief's five
"Things to consider" questions, with the sourced reasoning underneath, source reliability,
and four flagged unverified claims.

→ [`team-post.md`](team-post.md) — the same five answers, sized for a team channel.

## Two rewrites, and what they were for

The first attempt researched a different topic entirely and produced something dense that
the team had no use for. The second was on the right topic but organised around what I had
found: state of the evidence, then regulation, then a cautionary case, then implications.
Sourced, accurate, and still not usable by six people picking a project in the next few
minutes.

The project brief already lists the five questions it wants answered. Answering those
directly, one sentence each, was worth more than a briefing that arrived in its own shape
and expected the reader to do the mapping. The research did not change between the second
and third versions. Only the order did, and only the third one was any use.

## Note on a discarded first attempt

I originally ran this quest on a different topic — MCP authorization — and produced a
longer briefing that was genuinely dense and not what the team needed. It was scrapped and
replaced with this. The lesson is the quest's own framing: *"it needs a real audience, not
just curiosity."* The first topic was interesting to me. This one was in front of six
people making a decision that hour.

## Go Further (not done)

The quest suggests a monthly refresh. Skipped deliberately: this briefing's value is
time-boxed to a decision being made today. The regulatory half is worth re-checking when
guidance changes — an event trigger, not a calendar one. Recording the decision rather
than scheduling something to earn the mention.
