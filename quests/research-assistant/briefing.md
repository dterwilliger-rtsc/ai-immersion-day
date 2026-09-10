# AI Lead Scoring Service — answers to "Things to consider"

**For:** Team R5, choosing a build project
**Scope:** the five questions the project brief raises, answered, with the research underneath
**Delivered:** 2026-09-10, during the team's decision

---

## The five answers

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
the top third, then score pairs that differ only on an attribute linked to a
protected ground and confirm the scores do not separate.

---

# The research behind those answers

## Why there are no benchmarks to weight signals against

The original plan was to weight signals using the industry's published conversion data.
That data does not survive a sourcing check.

Every recurring claim in this space comes from a company selling an AI leasing product:
speed-to-lead as the top predictor, the five-minute response window, a 65 to 80 percent
drop-off after the first hour, a 44.8 percent conversion lift. The vendors involved
include EliseAI, Zuma, Perq, Leasey.AI, Dyverse and ApartmentList.

I fetched the one piece of trade press promising a review of the research (Propmodo,
"What the Research on Lead Conversion Tells Us About AI's Role in Leasing"). It attributes
its figures to Zuma and EliseAI with no publication date, no methodology and no sample
size. The 65 to 80 percent figure carries no citation at all. The article raises no
caveats about methodology, selection bias or confounders.

This is why answer 2 says to keep weights simple, readable and hand-set, and why answer 5
defines usefulness as a test you design rather than a benchmark you hit. There is no
credible external number to calibrate against, so the evaluation has to be internal and
explicit.

## Why answer 4 is a design constraint rather than a compliance note

Prioritising leads is a regulated activity, and the regulation reaches the vendor.

HUD's 2024 guidance on the Fair Housing Act and AI covers the targeting and delivery of
housing opportunities, including practices that unlawfully discriminate by "limiting or
denying consumers information about housing opportunities." Deprioritising a lead limits
that person's information about a housing opportunity, which places lead scoring inside
the guidance rather than adjacent to it.

Three specifics shaped answer 4:

- **Responsibility follows the tool.** Housing providers "remain responsible for ensuring
  their decisions comply with the FHA ... even where they have outsourced screening to a
  third-party screening company," and HUD holds that "both housing providers and tenant
  screening companies have a responsibility to avoid using AI in a discriminatory manner."
  A tool Rentsync builds for clients puts Rentsync in a named category of responsible
  party.
- **Effect, not intent.** The standard is disparate impact. HUD's framing asks that
  "algorithms are similarly predictive across protected class groups" with adjustments
  "to correct for any disparities in predictiveness," which is considerably stronger than
  not using protected attributes as features.
- **Testing is expected.** The guidance calls for "regular end-to-end testing ... to
  ensure that any discriminatory outcomes are detected" and assessment of "less
  discriminatory alternatives." That expectation is the origin of the second half of
  answer 5.

**The Canadian position is more direct, not softer.** Under the Ontario Human Rights Code,
receipt of public assistance is itself a protected ground in housing, and the protection
extends to access to rental opportunities rather than only to tenancy decisions. A model
that downranks assistance-linked signals is exposed under Ontario law without needing a
disparate-impact argument at all.

## Why the reasons matter more than the score

The case most often cited here is SafeRent Solutions. A Black applicant holding a housing
voucher, with 16 years of on-time payments, was scored out. The reported cause was a
design flaw: the model did not properly account for housing vouchers. Because voucher
holders are disproportionately Black and Hispanic, a neutral omission produced a racially
disparate outcome.

Nobody set out to discriminate. Someone failed to model a payment source, and nothing in
the output made that visible. That is the failure mode available to a prototype that ranks
leads on plausible-sounding signals, and it is why answer 4 ends with "always show the
reasons." An unexplained score cannot be debugged.

## Why synthetic data helps here

The brief encourages synthetic data. For this project it is better than historical data
for one specific reason: you can construct pairs of leads that differ on exactly one
attribute, which is the only clean way to run the parity check in answer 5. Historical
data will not give you that. It also keeps client information out of a hackathon
prototype.

---

## Source reliability

**Primary regulatory sources.** HUD's 2024 guidance on the FHA and AI, read via a law
firm's summary that quotes it directly. The Ontario Human Rights Commission's policy on
human rights and rental housing, read directly, and the more applicable of the two for a
Canadian product.

**Legal and civil-rights commentary.** Georgetown Law's Poverty Journal and The Leadership
Conference on AI and tenant screening. Both argue a position, and both are consistent with
each other on the SafeRent facts.

**Trade press.** Propmodo, cited only as evidence about the state of the evidence, never
for a figure.

**Rejected.** Vendor blogs from EliseAI, Zuma, Perq, Leasey.AI, Dyverse and ApartmentList.
Every conversion statistic traces to one of them, and each sells a product the statistic
promotes. They are the subject of the first section, not a source for it.

### Unverified or single-source, flagged
- "Speed-to-lead is the single biggest predictor of conversion." Repeated everywhere,
  sourced nowhere I could find. Plausible and unproven. Do not build a demo claim on it.
- The five-minute and 65 to 80 percent figures. No citation located anywhere, including in
  the trade-press review. Likely origin is business-to-business sales research rather than
  rental housing, but I could not confirm that either way.
- SafeRent specifics (16 years of payments, the voucher-handling flaw). Consistently
  reported across independent outlets; I did not read primary filings.
- HUD's exact wording. I read a law firm's summary presenting the quotes as verbatim
  rather than HUD's own documents.

### Spot-checks
1. **Chased the conversion statistics to their origin.** Found vendor attribution, no
   methodology, and one central figure with no citation. This reversed the original plan,
   which was to use those benchmarks.
2. **Checked whether the fair-housing guidance covers lead prioritisation or only
   application screening.** It covers targeting and delivery, and names third-party
   technology providers. Then checked the Canadian equivalent, which proved more directly
   applicable.

---

## Honest assessment

The deliverable is five answers to the project's own questions. Everything above them is
the reasoning, and two parts of it are load-bearing but second-hand: I read a summary of
HUD's guidance rather than the guidance, and I have not read the SafeRent filings. Both are
sound enough to choose a project and shape a prototype. Neither is sound enough for a
client conversation without checking the originals.

The most useful result was negative. I set out to find which signals predict rental lead
conversion and found that the entire visible literature is vendors quoting themselves.
Saying so is more useful than passing their numbers along, and it changed two of the five
answers.

## Sources

- [HUD 2024 guidance on the FHA, tenant screening and algorithmic advertising, summarised with quotations](https://www.consumerfinancialserviceslawmonitor.com/2024/05/hud-issues-guidance-on-applicability-of-the-fair-housing-act-to-tenant-screening-and-housing-related-advertising-that-relies-upon-algorithms-and-ai/)
- [Ontario Human Rights Commission, Policy on human rights and rental housing](https://www.ohrc.on.ca/en/policy-human-rights-and-rental-housing)
- [OHRC, Prohibited grounds of discrimination](https://www.ohrc.on.ca/en/human-rights-and-rental-housing-ontario-background-paper/prohibited-grounds-discrimination)
- [Georgetown Law Poverty Journal, The Discriminatory Impacts of AI-Powered Tenant Screening Programs](https://www.law.georgetown.edu/poverty-journal/blog/the-discriminatory-impacts-of-ai-powered-tenant-screening-programs/)
- [The Leadership Conference, AI + Tenant Screening](https://civilrights.org/resource/ai-tenant-screening/)
- [Propmodo, What the Research on Lead Conversion Tells Us About AI's Role in Leasing](https://propmodo.com/what-the-research-on-lead-conversion-tells-us-about-ais-role-in-leasing/), cited for its sourcing rather than its figures
