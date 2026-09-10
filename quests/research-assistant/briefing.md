# Briefing — MCP authorization, agent allowlisting, and one-command connection

**Commissioned:** 2026-09-10 · **Audience:** the team picking up our MCP program
**Status:** internal briefing. Findings are sourced; speculation is marked as such.

> **Redaction note:** this is the public copy. The internal program document that prompted
> the briefing is not linked or quoted beyond what's needed to state the finding. Release
> contents and roadmap specifics are generalized.

---

## Why this briefing

An internal program doc — explicitly written as an orientation for "whoever picks up this
project next" — defers one operational question past MVP:

> which AI agents are allowed to connect. In MVP the server is open to any AI client with
> the account's URL and valid auth. Post-MVP, the plan is an allowlist: an admin registers
> each approved agent and gets a unique credential for it — **modeled on how Chargebee
> handles this for their own MCP.**

That last clause is a checkable claim about a named vendor. So I checked it.

---

## Finding 1 — The cited precedent does not implement the design it's cited for

**Confidence: high.** Verified directly in Chargebee's own documentation.

Chargebee offers external AI clients two authentication paths, and neither is an
agent registry:

| Path | What it actually does |
|---|---|
| **API key** | Capped at **5 keys per server**. A key "grants access to all enabled tools in this server" — no per-tool scoping. Sent as `Authorization: Bearer`. Positioned for "server-to-server integrations, internal automations, trusted development environments." |
| **OAuth** | Access is scoped by "the signed-in Chargebee user's access level in Chargebee" — i.e. by the *human*, not the agent. |

The OAuth guidance is the direct contradiction. Chargebee's docs say:

> "Use the same OAuth client ID for users of the same MCP client. For example, if you have
> 100 Claude Code users, you can generate one OAuth client ID and share it with all 100
> users."

That is deliberately **one shared credential per client type**, with authorization derived
from each human's existing permissions. The internal doc proposes the inverse — a unique
credential per registered agent. Chargebee's model is per-*user* authorization; the doc
describes per-*agent* identity.

There is no documented admin capability to register or allowlist specific named agents.

**A secondary observation worth flagging:** Chargebee's own docs are inconsistent about
this. The MCP overview page contains no authentication detail at all — its only security
guidance is "connect only trusted clients and review actions before they run." The auth
detail lives on individual server pages. Anyone who read the overview and stopped would
come away with a different impression than the one the per-server pages support. That is a
plausible route by which the internal doc's claim got made in good faith.

## Finding 2 — MCP has no concept of agent identity, and one part of the spec works against it

**Confidence: high.** Normative language, primary source.

The specification models a protected MCP server as an **OAuth 2.1 resource server**. Its
requirements are about *audience binding*, not about who or what the client is:

- MCP servers **MUST** implement OAuth 2.0 Protected Resource Metadata (RFC 9728).
- MCP clients **MUST** implement Resource Indicators (RFC 8707) and send a `resource`
  parameter identifying the target server — "regardless of whether authorization servers
  support it."
- MCP servers **MUST** validate that tokens were issued specifically for them, and **MUST
  NOT** accept or transit any other tokens.

Nothing in that identifies the *agent application*. A token proves a user authorized
access to this server. It does not say "and Claude Code was the thing asking."

Worse for an allowlist, the spec pushes the opposite way:

> MCP clients and authorization servers **SHOULD** support the OAuth 2.0 Dynamic Client
> Registration Protocol (RFC 7591) to allow MCP clients to obtain OAuth client IDs
> **without user interaction.**

Self-registration without user interaction is close to the definition of what an allowlist
exists to prevent. The escape hatch is one sentence in the same section:

> "Authorization servers can implement their own registration policies."

**So the conclusion is architectural, not a feature request:** an agent allowlist is not
something MCP provides and not really an "admin screen" either. It is a policy decision at
the authorization server — specifically, declining to support open Dynamic Client
Registration and requiring pre-registration instead. That belongs to whoever owns the
authorization server, and it is materially cheaper to decide before write-capable releases
than to retrofit after.

## Finding 3 — Token passthrough is a named vulnerability that applies directly here

**Confidence: high.** Primary source, normative.

If the MCP server sits in front of existing internal APIs — which is the obvious way to
build one on top of an existing platform — the spec is explicit:

> "If the MCP server makes requests to upstream APIs, it may act as an OAuth client to
> them. The access token used at the upstream API is a **separate** token, issued by the
> upstream authorization server. The MCP server **MUST NOT** pass through the token it
> received from the MCP client."

The spec names the failure mode this prevents: the **confused deputy problem**, where a
downstream API wrongly trusts a forwarded token as validated. It also requires that proxy
servers using static client IDs obtain user consent for each dynamically registered client.

**Concrete thing to verify in the MVP:** that inbound client tokens are exchanged for
separate upstream credentials rather than forwarded. This is cheap to confirm now and
expensive to discover later — and it is a read-only-release question too, not only a
write-release one.

## Finding 4 — Read-only first is well-supported by the spec's own posture

**Confidence: medium-high.** Interpretation of primary sources, not a direct quote.

The staged approach — a small read-only slice first, writes later — lines up with where the
specification concentrates its warnings. The security considerations are overwhelmingly
about token misuse, audience confusion and privilege escalation, and every one of those
gets more consequential the moment tools can mutate data. Chargebee's own framing agrees
from the operator side: "You are responsible for any actions AI clients perform through
Chargebee MCP servers, **including write actions.**"

This is the one finding where I am reasoning rather than quoting. Recorded as such.

## Finding 5 — Connecting customers: the obvious distribution tool is the wrong one

**Confidence: high** on both halves.

**MCPB (MCP Bundles) does not apply.** It is the official one-click install format — zip
archives with a `manifest.json`, modeled on `.crx`/`.vsix`, built via `mcpb init` and
`mcpb pack`. But it is **local-only**: "zip archives containing a local MCP server," with
no remote or HTTP support. For a hosted, per-account product MCP, MCPB is a dead end.
Useful negative result — it's the first thing you'd reach for.

**The real one-command path for a remote server is client configuration, not packaging.**
`npx add-mcp <url>` writes MCP config across 24 coding agents (Claude Code, Cursor, VS
Code, Codex, Copilot CLI, Zed, Windsurf and others) and explicitly supports remote
streamable-HTTP and SSE transports:

```
npx add-mcp https://mcp.example.com/mcp
npx add-mcp https://mcp.example.com/mcp -a cursor -a claude-code
```

**Cheap, high-leverage recommendation:** publish the canonical server URL plus a
copy-pasteable one-liner in the customer-facing docs. Note the spec's guidance that clients
**SHOULD** send the most specific canonical URI, without a trailing slash — so document the
exact string rather than letting customers guess. This is a documentation task, not
engineering work, and it is the difference between "there is an MCP" and "customers connect
to it."

## Finding 6 — The "single prompt" claim: what it gets right, and where it collides with Findings 1–3

**Confidence: mixed — see the flags.** Primary source for the author's own method; unverified as an engineering claim.

A widely-shared X post from a bootstrapped-SaaS practitioner (2026-08-23, ~53K views)
argues that *"the complexity of adding an MCP to your product (if you already have some
sort of API) is a single prompt,"* and publishes the prompt verbatim. It is not about
one-command *installation* — it's about generating the entire MCP surface from an existing
API with one agentic coding instruction. Five things in it map directly onto our program:

**Where it's genuinely useful:**

1. **"Aiming for feature parity with our existing API implementation."** This is the same
   logic as carving releases out of an existing tool inventory, and it's a good sanity
   check: if a tool exists in the API and not the MCP, that gap should be deliberate.
2. **"Create MCP management, analytics, and log features analogous to our API
   implementation."** This is the most valuable line in the post for us, and it directly
   contradicts our own plan. Our doc defers usage limits and audit logging as
   "day two" concerns. He treats them as **in-scope from the start, by mirroring what the
   existing API already has.** That reframing is strong: if the platform already has API
   analytics and logging, the MCP not having them isn't a deferral, it's a regression —
   and building alongside is cheaper than retrofitting.
3. **"Pay special attention to how tools need to be configured, described, and tagged" for
   plugin-store review.** A distribution channel our doc doesn't mention at all. If a
   first-class listing in a model vendor's tool store is ever wanted, tool naming and
   description conventions are cheaper to get right before Release 1 ships than after
   customers depend on the names.
4. **"A dedicated landing page for agentic website visitors that has an easy path for them
   to quickly implement the MCP."** Independent arrival at the same recommendation as
   Finding 5 — the adoption bottleneck is documentation and a copy-pasteable path, not
   packaging.
5. **"First create a scope document and have me verify it, then implement."** Plan-then-
   execute, with an explicit instruction to ask questions when uncertain rather than guess.

**Where applying it here would go wrong:**

The prompt says *"use the OAuth implementation that is closest to our existing
authentication stack."* For most products that's pragmatic. For an MCP server it is not
sufficient, because the spec imposes requirements that "closest to what we already have"
will not satisfy by accident — serving RFC 9728 Protected Resource Metadata, validating
RFC 8707 audience claims, returning `WWW-Authenticate` on 401, and **not** passing the
inbound token through to upstream APIs (Findings 2 and 3). An agent following that
instruction against an existing session-auth stack can produce something that authenticates
users correctly and still fails the spec's MUSTs.

It also can't decide the allowlist question. "Closest to our existing stack" has no opinion
on whether to support open Dynamic Client Registration — which Finding 2 identifies as the
actual decision point.

**Net:** the post is a good scaffold and a genuinely useful corrective on logging and
analytics. It is not a substitute for the authorization design, and the two findings
should be read together — his prompt for scope and sequencing, the spec for the auth layer.

---

## Source reliability ranking

As the quest requires — ranked, with single-source claims flagged.

### Tier 1 — Normative primary sources
1. **MCP specification, Authorization (2025-06-18)** — `modelcontextprotocol.io`. RFC-backed
   normative language (MUST/SHOULD). The authority for Findings 2, 3 and part of 5.
   Underlying: OAuth 2.1 draft-13, RFC 8707, RFC 9728, RFC 7591, RFC 9068.
2. **`modelcontextprotocol/mcpb`** — official MCP-org repository. Authority for MCPB being
   local-only.

### Tier 2 — Vendor primary documentation
3. **Chargebee MCP docs (per-server pages)** — authoritative *about Chargebee*, and the
   basis for Finding 1. Caveat applied: **internally inconsistent** across pages, and
   vendor docs describe intent as much as behaviour. I did not test the API.

### Tier 3 — Third-party tooling
4. **`neondatabase/add-mcp`** — a real, specific repository, but a vendor-authored tool.
   Its client list and transport support are taken **from its own README and not
   independently tested.** Treat the "24 clients" figure as a claim, not a measurement.

### Tier 4 — Practitioner opinion
5. **Arvid Kahl, X post, 2026-08-23** — a credible practitioner (bootstrapped SaaS
   founder, builds with agentic tooling daily) publishing his own method. It is a
   **primary source for what he does** and a reasonable scaffold. It is **not evidence**:
   a single self-reported anecdote, no data, no named product outcome, and the author's own
   sign-off is "(Dictated but not read 🤣)". Weight it as an experienced opinion, which is
   how it's used in Finding 6 — for scope and sequencing, never for the auth layer.

### Rejected — not cited
- **StackOne's Chargebee connector page** — third-party wrapper marketing. It was the
  source of the phrase "each user is isolated via `origin_owner_id`," which describes
  *StackOne's* product, not Chargebee's MCP. Discarded to avoid attributing a reseller's
  architecture to the vendor.
- **Several "MCP Server Complete Guide (2026)" blog results** — SEO content farms with no
  identifiable authorship. Not used for any claim.

### ⚠️ Flagged: single-source and unverified
- **"OAuth 2.1 authentication support is coming soon to Chargebee."** Appeared in a search
  result summary. **I could not find it on the Chargebee page itself.** Single-source,
  unverified — do not plan around it.
- **"95% of the way there."** The X post's central quantitative claim about how much of an
  MCP implementation a single prompt delivers. **Self-reported, single-source, no
  supporting detail, and not verified against any shipped product.** The author himself
  scopes it: "The rest is some manual testing, deploying, and telling your customers you
  have an MCP now." Do not plan a schedule around this number.
- **Search-and-retrieval note:** my own initial search for this post **failed** — I could
  not find it from a description alone, and reported that rather than paraphrasing a
  half-remembered version. It entered the briefing only once the exact URL was supplied.
  Worth recording, because "the researcher could not find the thing you remembered" is a
  real outcome, and inventing a plausible summary would have been the easy failure here.

### Spot-checks performed
Two, as the quest requires — and the first one changed the briefing:
1. **Chargebee auth model.** Fetched the vendor docs directly rather than trusting the
   search summary. The overview page turned out to contain none of the relevant detail;
   the per-server page contradicted the internal doc's characterization. This is Finding 1.
2. **MCP authorization spec.** Fetched the normative page rather than relying on
   recollection. It confirmed there is no agent-identity primitive and surfaced the
   Dynamic Client Registration tension, which no secondary source had mentioned.

---

## What I'd do with this

1. **Correct the internal doc's Chargebee reference.** Not a nitpick — the design it
   currently points at is the opposite of what the precedent does, and someone will build
   from it.
2. **Reframe the allowlist as an authorization-server policy decision**, and decide it
   before write-capable releases rather than after.
3. **Verify the MVP does token exchange, not passthrough.** One conversation, and the spec
   is unambiguous that passthrough is forbidden.
4. **Ship the one-liner in customer docs.** Near-zero cost, direct effect on adoption.
5. **Open questions I could not close from public sources:** whether per-tool (not just
   per-server) scoping is achievable on the intended auth path, and what audit-logging
   granularity is expected — the internal doc lists usage limits and audit logging as "day
   two" concerns, and nothing in the spec mandates either.

## Honest assessment

The strongest result is a **correction**, not a discovery: an internal doc cites a vendor
precedent that does not support the design attributed to it. That took two source fetches
to establish and would have survived indefinitely otherwise, because the claim is plausible
and the vendor's own overview page is silent enough to seem consistent with it.

The second strongest is a **correction in the other direction** — against our own plan.
Finding 6 makes a better case than our doc does for building MCP analytics and audit
logging alongside Release 1 rather than deferring them, on the straightforward grounds
that the existing API already has them.

The weakest part is Finding 4, which is my reading of where the spec puts its emphasis
rather than anything it states. Marked accordingly.

Two process notes worth keeping, since both are about the limits of this kind of research:

- **I could not find the requested X post from a description.** I said so and excluded it
  rather than paraphrasing something plausible. It was only incorporated once the exact URL
  was provided — and when it arrived, it turned out to be about something different from
  what either of us expected (generating an MCP from an existing API, not one-command
  installation). The guess I would have written would have been wrong on the substance,
  not just the citation.
- **The most useful single line in the whole briefing** — mirror the existing API's
  analytics and logging instead of deferring them — came from the Tier 4 source, the
  lowest-ranked one. Source ranking governs how much weight a claim carries, not whether
  it's worth reading.

---

## Sources

- [MCP Specification — Authorization (2025-06-18)](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization)
- [modelcontextprotocol/mcpb](https://github.com/modelcontextprotocol/mcpb)
- [Chargebee MCP Servers (overview)](https://www.chargebee.com/docs/billing/2.0/ai-in-chargebee/chargebee-mcp)
- [Chargebee Data Lookup MCP Server (auth detail)](https://www.chargebee.com/docs/billing/2.0/ai-in-chargebee/data-lookup-agent)
- [neondatabase/add-mcp](https://github.com/neondatabase/add-mcp)
- [Arvid Kahl on X, 2026-08-23 — "adding an MCP to your product ... is a single prompt"](https://x.com/arvidkahl/status/2091598550279037169)
- RFCs referenced normatively by the spec: [8707](https://www.rfc-editor.org/rfc/rfc8707.html) · [9728](https://datatracker.ietf.org/doc/html/rfc9728) · [7591](https://datatracker.ietf.org/doc/html/rfc7591) · [8414](https://datatracker.ietf.org/doc/html/rfc8414) · [OAuth 2.1 draft-13](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-v2-1-13)
