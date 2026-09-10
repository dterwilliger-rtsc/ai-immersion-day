# The short version (for the team channel)

Quest step 4: *"Ask for the one-paragraph version and post it in your team channel."*

Written to be read on a phone by teammates mid-decision, and to land the one point that
changes what gets built rather than summarising everything.

---

> Did some quick research on lead scoring since a few of us are keen — two things worth
> knowing before we start.
>
> **The industry's numbers are all vendor marketing.** "Respond in 5 minutes," "65–80%
> drop-off after an hour," "44.8% conversion lift" — every one of those traces back to a
> company selling an AI leasing tool. I chased the one article promising a research
> review; it cites two vendors with no methodology or sample size, and its headline
> statistic has no citation at all. Not necessarily wrong, but not something to build our
> demo's claims on.
>
> **More important: deciding who gets followed up is a regulated activity.** US fair
> housing guidance from 2024 explicitly covers "limiting or denying consumers information
> about housing opportunities" — which is what deprioritising a lead does — and it names
> third-party tech providers as responsible, not just landlords. In Ontario it's more
> direct: receipt of public assistance is a protected ground, and it covers *access to
> rental opportunities*. The well-known cautionary case (SafeRent) wasn't malice, it was a
> bug — the model didn't account for housing vouchers, and voucher holders skew heavily to
> Black and Hispanic renters.
>
> Two design choices that cost us nothing today and remove most of that risk: **score
> actions people chose to take** (asked for a tour, replied twice, asked about parking)
> **not attributes or proxies** (postal code, price band, income) — and frame the output as
> **ordering the queue, never filtering anyone out**. Everyone still gets contacted; the
> score changes the order. That also makes "explain why this lead scored high" the core
> feature rather than a nice-to-have, which is already in the project brief.
>
> Full notes and sources are linked if useful. Happy to own the fairness-testing piece if
> we go this way.

---

## Why it's shaped like that

- **Leads with the thing that changes the build**, not with a summary of everything found.
- **Names specific numbers as unreliable** so nobody puts them on a demo slide.
- **The regulatory point is framed as design guidance, not as a warning** — two concrete
  choices, both free, rather than "we should be careful."
- **Ends by volunteering for a specific piece of work.** Second day; better to offer to own
  something narrow than to arrive with opinions about what everyone else should do.
- **Doesn't tell the team which project to pick.** Four people already want this one. The
  briefing informs the decision; making it isn't mine.
