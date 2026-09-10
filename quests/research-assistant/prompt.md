# Prompt — Research Assistant (Automations & Integrations, 25 pts → 38 at 1.5x)

> Quest: commission a sourced briefing on a topic your team genuinely needs — then
> actually share it.

## Topic and why it qualifies

The quest asks for a real audience, not curiosity. This one had a document waiting for
it: an internal program doc for the MCP work I've been brought on to contribute to,
explicitly written as an orientation for *"whoever picks up this project next."* It ends
with a list of deliberately unanswered questions.

So the audience is the program, and the brief was to close one of its open questions.

## Inputs

1. **The internal program doc** — where we stand on MCP, what's planned, what's
   unresolved.
2. **A specific X post** on adding MCP to a product, which I was asked to fold in.
3. Whatever primary sources the research turned up.

## The prompt

```
Act as a research assistant. I need a sourced briefing for my team on MCP
authorization — specifically the question of restricting which AI agents can
connect to a product's MCP server.

Start from this internal doc [link]. It claims our planned approach is "modeled
on how Chargebee handles this for their own MCP." Verify that claim against
primary sources — don't take it at face value.

Structure the output. Separate what is established fact from what is my
interpretation. Rank every source by reliability, and flag anything resting on a
single report or that you could not verify. If you cannot find something, say so
rather than approximating it.
```

The load-bearing instructions were the last two sentences. "Verify that claim" is what
produced the main finding; "if you cannot find something, say so" is what stopped a
requested source being paraphrased from memory when it couldn't be located.

## What happened

**The internal doc's central citation was wrong.** It says the plan is an admin-registered
allowlist with a unique credential per approved agent, "modeled on Chargebee." Chargebee's
actual documented model is close to the inverse: API keys capped at five per server with
no per-tool scoping, or OAuth with explicit guidance to *share one client ID across all
users of the same MCP client* and derive access from the human's permissions. No
named-agent registry exists.

Two fetches established that. The vendor's own MCP overview page contains no
authentication detail at all — which is a plausible route by which the claim got made in
good faith.

**The spec has no agent-identity primitive**, and one part of it pushes against an
allowlist: clients and authorization servers *SHOULD* support Dynamic Client Registration
so clients can obtain client IDs "without user interaction." So the allowlist isn't a
deferred admin feature, it's an authorization-server policy decision — cheaper before
write-capable releases than after.

**The requested X post initially could not be found.** I searched, failed, and said so
rather than reconstructing it. Once the URL was supplied it turned out to be about
something different from what either of us expected — not one-command installation, but
generating an entire MCP surface from an existing API with one agentic prompt. The summary
I'd have guessed at would have been wrong on substance, not just attribution.

And its most useful line argues **against** our own plan: mirror the existing API's
analytics and audit logging from the start rather than deferring them. That came from the
lowest-ranked source in the briefing.

## Spot-checks

Two, per the quest. The first changed the briefing's conclusion; the second surfaced the
Dynamic Client Registration tension that no secondary source had mentioned. Both are
documented in the briefing itself.

## Output

→ [`briefing.md`](briefing.md) — six findings, confidence marked per finding, four-tier
source ranking, rejected sources listed with reasons, and two flagged unverified claims.

→ [`team-post.md`](team-post.md) — the one-paragraph version for the team channel.

## Go Further (not done)

The quest suggests scheduling a monthly refresh. Deliberately skipped: a briefing whose
main value is a one-time correction to an internal document doesn't benefit from
re-running monthly. The MCP spec is versioned and worth re-reading on release, which is an
event trigger, not a calendar one. Noting the decision rather than automating for the
points.
