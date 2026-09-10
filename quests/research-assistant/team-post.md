# The one-paragraph version (for the team channel)

The quest's last step: *"Ask for the one-paragraph version and post it in your team
channel. A briefing nobody reads is a quest half-done."*

Published here in redacted form. The version posted internally names the document and
the release it affects.

---

> Spent some time today digging into the MCP authorization question our program doc
> leaves open — which AI agents are allowed to connect. Two things worth flagging. First,
> the doc says our planned approach (an admin registers each approved agent, unique
> credential each) is modeled on how Chargebee does it. Chargebee actually does close to
> the opposite: API keys capped at five per server with no per-tool scoping, or OAuth
> where they explicitly tell you to *share one client ID across all users of the same MCP
> client* and let the signed-in user's own permissions do the limiting. There's no
> named-agent registry. Worth correcting before someone builds from it. Second, and more
> useful: the MCP spec has no concept of agent identity at all, and it *recommends*
> Dynamic Client Registration so clients can self-register without user interaction —
> which is roughly what an allowlist exists to prevent. So this isn't a deferred admin
> screen, it's a policy decision at the authorization server (do we allow open DCR or
> require pre-registration?), and it's much cheaper to decide before write-capable
> releases than after. Separately: the spec forbids passing an inbound client token
> through to upstream APIs — worth a five-minute check that the MVP does token exchange
> instead. Full briefing with sources and confidence levels is linked; happy to walk
> anyone through it.

---

## Why it's shaped like that

- **Leads with the correction**, because that's the part with a deadline — someone could
  build from the current wording.
- **Names the specific quote** ("share one client ID across all users") so it's checkable
  rather than something to take on trust.
- **Ends with one concrete ask** — the token-passthrough check — scoped to five minutes,
  so there's an action rather than just information.
- **No conclusion about what we should do.** I'm two days in; flagging a sourced
  discrepancy is useful, and telling the team what to build instead would not be.
