# The short version (for the team channel)

Quest step 4: post the short version where the team will see it.

The project brief raises five questions under "Things to consider." This answers each in
one sentence, which is what a team mid-decision can actually read. Everything else lives
in [`briefing.md`](briefing.md).

---

**What behaviours are good indicators of serious intent?**
Actions that cost the person effort or commit them to a time, such as proposing a tour
slot, replying a second time, or asking about parking or lease length; single page views
and generic availability questions carry almost no information.

**Should some signals carry more weight than others?**
Yes, ordered by how much effort each action takes, kept simple and additive so anyone can
read and change them, with a cap so no single signal can carry a lead on its own.

**How should the system handle a new lead with limited information?**
Return "insufficient signal" as its own state rather than a low score, and put new leads
in the normal queue so the system does not quietly favour whoever has had the most time to
browse.

**How could the assessment inform follow-up without making inappropriate assumptions?**
Score what people did rather than what they look like (no postal code, price band, or
income proxies), make the output order the queue rather than filter anyone out, and always
show the reasons.

**How would you know if the scoring is useful?**
Hand-label about 30 synthetic leads before scoring them and check the serious ones rank in
the top third, then score pairs that differ only on an attribute linked to a protected
ground and confirm the scores do not separate.

---

## Why it is shaped like this

Earlier drafts were a briefing on lead scoring generally: the state of the evidence, the
regulatory position, a cautionary case. All sourced, none of it usable by a team picking a
project in the next few minutes.

The project brief already states the five questions it wants answered. Answering those in
one sentence each is more useful than research that arrives in its own shape and expects
the reader to map it across. The reasoning still exists and is still sourced; it sits
underneath rather than in front.
