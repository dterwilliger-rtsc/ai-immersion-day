# Briefing — AI lead scoring for rental leads

**For:** Team R5, deciding today which build project to take on
**Question:** if we build the AI Lead Scoring Service, what should we know before we start?
**Commissioned & delivered:** 2026-09-10, same hour — the team is choosing now

---

## Bottom line

Build it. But two things change how:

1. **Ignore the industry's conversion statistics.** The evidence base is almost entirely
   vendors marketing their own leasing products, and the numbers don't survive contact
   with a citation check.
2. **Score behaviour, not people.** A tool that decides which renters get followed up is
   regulated in a way the project brief doesn't mention — and if Rentsync ships it to
   clients, Rentsync is in scope, not just the landlord.

The second point is the one worth the team's attention today, because it's cheap to design
for now and expensive to bolt on.

---

## Finding 1 — The published evidence is vendor marketing, nearly all of it

**Confidence: high.** This is a claim about sourcing, and I checked the sourcing.

Search results for what predicts rental lead conversion are dominated by companies that
sell AI leasing assistants — EliseAI, Zuma, Perq, Leasey.AI, Dyverse, ApartmentList. The
recurring claims:

- "Speed-to-lead is the single biggest predictor of conversion"
- "Leads contacted within five minutes are dramatically more likely to tour"
- "Roughly a 65–80% drop-off in conversion likelihood after the first hour"
- "44.8% higher lead-to-lease conversion" · "85% of operators increased conversion"

I fetched the one piece of trade press that promised to review the research
("What the Research on Lead Conversion Tells Us About AI's Role in Leasing", Propmodo).
It attributes its figures to **Zuma** and **EliseAI** — both vendors — with, in each case,
no publication date, no methodology and no sample size. The 65–80% drop-off figure carries
**no citation at all.** The article raises **no caveats** about methodology, selection bias
or confounders.

The "80% of sales require five or more follow-ups / 44% of reps quit after one" pairing
also appears with no attribution, and is generic sales-training folklore rather than
anything about renting apartments.

**So:** treat every conversion-lift percentage in this space as a marketing claim. Not
necessarily false — plausibly directionally right — but not a number to design against or
quote to a client.

**What this means for a one-day prototype:** we cannot calibrate a model against
"industry benchmarks," because there aren't credible public ones. Which is fine, and
actually simplifies the build — see Finding 4.

## Finding 2 — Prioritising leads is a regulated activity, and it reaches the vendor

**Confidence: high.** Primary regulatory sources.

The project brief frames this as an efficiency problem: effort goes to the wrong people.
True. But the mechanism — some prospects get faster, better follow-up than others — is
exactly what US fair-housing regulators have been looking at.

**HUD's 2024 guidance** on the Fair Housing Act and AI covers not only application
screening but the **targeting and delivery of housing opportunities**, with violations
arising where these functions *"unlawfully discriminate on the basis of protected
characteristics, such as limiting or denying consumers information about housing
opportunities."*

Deprioritising a lead *is* limiting that person's information about a housing opportunity.
That places lead scoring inside the guidance's scope, not adjacent to it.

Three specifics that matter for how we'd build:

- **Liability follows the tool, not just the user.** *"Housing providers remain
  responsible … even where they have outsourced screening to a third-party screening
  company,"* and HUD asserts *"both housing providers and tenant screening companies have
  a responsibility to avoid using AI in a discriminatory manner."* If Rentsync builds this
  for clients, Rentsync is a named category of responsible party.
- **Disparate impact, not intent.** A practice can be unlawful for its *effect*, whatever
  was meant. HUD's framing requires ensuring *"algorithms are similarly predictive across
  protected class groups and making adjustments to correct for any disparities in
  predictiveness"* — a much stronger standard than "we didn't use race as a feature."
- **Testing is an expectation, not a nicety.** The guidance calls for *"regular end-to-end
  testing of advertising systems to ensure that any discriminatory outcomes are detected"*
  and assessment of *"less discriminatory alternatives."*

**The Canadian angle is sharper, not softer.** Our market is Canadian. Under the **Ontario
Human Rights Code**, **receipt of public assistance** is itself a protected ground in
housing, and the protection explicitly extends to **access to rental opportunities** — not
only to tenancy decisions. So a scoring model that quietly downranks assistance-linked
signals is exposed under Ontario law directly, without needing a disparate-impact theory.

## Finding 3 — The cautionary case is a bug, not malice — and that's the point

**Confidence: medium-high.** Widely reported; I did not read the court filings.

The most-cited example in this area is **SafeRent Solutions**. A Black applicant with a
housing voucher and 16 years of on-time payments was scored out. The reported cause was a
**design flaw**: the algorithm didn't properly account for housing vouchers. Because voucher
recipients are disproportionately Black and Hispanic, a facially neutral omission produced
a racially disparate outcome.

Nobody set out to discriminate. Someone failed to model a payment source. That is precisely
the failure mode available to a hackathon prototype that ranks leads on plausible-sounding
signals, and it's why Finding 4 is a design constraint rather than a compliance appendix.

## Finding 4 — What this implies for the build

**Confidence: this is my recommendation, not a finding.** Marked as such.

The brief asks *"How could the assessment inform follow-up without making inappropriate
assumptions?"* — and it's the question with the most design leverage in it.

**Score actions the prospect chose to take. Don't score who they appear to be.**

| Use — chosen behaviour | Avoid — attributes and proxies |
|---|---|
| Requested a tour, proposed a specific time | Income, employment, credit, benefits |
| Replied to outreach; replied again | Neighbourhood, postal code, building searched |
| Asked a specific question (parking, pets, lease length) | Name, language of inquiry, inferred demographics |
| Viewed multiple units, returned across sessions | Household size, family status, age |
| Completed a form rather than abandoning it | Price band as a stand-in for means |

Postal code deserves a flag of its own: it's the classic proxy, and in housing it carries
almost the full signal of protected characteristics.

Three practices worth building in from the first commit, all cheap at prototype scale:

1. **Reasons, always.** The brief already asks the score to explain what drove it. Treat
   the explanation as the product. A score you can't explain is one you can't defend and
   can't debug — and it's what makes the SafeRent failure discoverable.
2. **Floor, don't gate.** Frame output as *ordering* the queue, never as suppressing a
   lead. Every lead still gets contacted; scoring changes sequence, not eligibility. This
   is a one-line product decision that removes most of the regulatory exposure.
3. **Test predictiveness parity, not just accuracy.** Generate synthetic cohorts that
   differ *only* on a protected-ground-linked attribute and confirm scores don't separate.
   That directly answers *"How would you know if the scoring is useful?"* — usefulness is
   accuracy **and** parity.

On the brief's other questions:

- **New lead, limited information** → return "insufficient signal," not a low score. A
  cold lead and a bad lead are different states, and collapsing them penalises everyone
  who just arrived.
- **Should signals carry different weights?** Start with transparent additive weights we
  can read and argue about. A learned model on synthetic data would only be learning our
  own assumptions with extra steps.
- **How do we judge it?** Decide before generating data — the brief says so too. Cheapest
  credible approach: hand-label ~30 synthetic leads by intent, then check the ranking
  agrees, plus the parity test above.

## Finding 5 — Synthetic data is a feature of this project, not a compromise

**Confidence: high** (project brief) / **recommendation** (the rest).

The brief encourages synthetic data explicitly. Worth stating plainly why that's a real
advantage here rather than a shortcut: we can construct cohorts that differ on exactly one
variable, which is the only clean way to run the parity test in Finding 4 — and something
you cannot do with historical data. It also means no client PII in a hackathon prototype,
which is its own reason.

---

## Source reliability ranking

### Tier 1 — Primary regulatory sources
1. **HUD 2024 guidance on the FHA, algorithms and AI** (as reported with direct quotation
   by Consumer Financial Services Law Monitor). Basis for Finding 2. **Caveat: I read a law
   firm's summary quoting the guidance, not HUD's original documents.** The quotes are
   presented as verbatim; I did not verify them against the source PDFs.
2. **Ontario Human Rights Commission — policy on human rights and rental housing.** The
   authority for receipt of public assistance being a protected ground and for protection
   extending to access to rental opportunities. Primary, and the most directly applicable
   source for a Canadian product.

### Tier 2 — Legal and civil-rights commentary
3. Georgetown Law Poverty Journal; The Leadership Conference (civilrights.org) on AI and
   tenant screening. Advocacy-aligned — directionally reliable on the SafeRent facts,
   which are widely and consistently reported, but they argue a position.

### Tier 3 — Trade press
4. **Propmodo.** Used as *evidence about the evidence base* — its own sourcing is what
   Finding 1 rests on. Not cited for any factual claim about conversion.

### Rejected — not used for any claim
5. **Vendor blogs** (EliseAI, Zuma, Perq, Leasey.AI, Dyverse, ApartmentList). Every
   conversion statistic traces back to one of these, and each sells a product whose value
   the statistic demonstrates. Named in Finding 1 as the subject, never used as a source.

### ⚠️ Flagged: unverified or single-source
- **"Speed-to-lead is the single biggest predictor of conversion."** Repeated everywhere,
  sourced nowhere I could find. Plausible; unproven. **Do not build the demo's headline
  around it.**
- **The "5 minutes" and "65–80% drop-off" figures.** No citation located in any source,
  including the trade-press review. Probable origin is B2B sales-response research, not
  rental housing — but I could not confirm that either way, so it stays flagged rather
  than asserted.
- **SafeRent specifics** (16 years of payments, the voucher-handling flaw). Consistently
  reported across independent outlets, but I did not read primary filings.
- **HUD quotations** — see Tier 1 caveat above.

### Spot-checks performed
Two, as the quest requires:
1. **Chased the conversion statistics to their source.** Fetched the trade-press article
   that promised a research review; found vendor attribution, no methodology, no sample
   sizes, and one central figure with no citation at all. This produced Finding 1 and
   changed the briefing's recommendation — the original plan was to *use* those benchmarks.
2. **Checked whether US fair-housing guidance covers lead prioritisation or only
   application screening.** It covers targeting and delivery of housing opportunities, and
   names third-party technology providers as responsible parties. Then checked the
   Canadian equivalent, which turned out to be more directly applicable, not less.

---

## Honest assessment

The useful output here is a **reframe**, not a data set. I set out to find what signals
predict rental lead conversion so the team could weight them. That question has no credible
public answer — the entire visible literature is vendors quoting themselves. Reporting the
absence is more valuable than laundering their numbers into a briefing.

The genuinely actionable finding is regulatory, and it wasn't what I went looking for: lead
prioritisation sits inside fair-housing scope, liability reaches the vendor, and in Ontario
the relevant protected ground applies to *access to opportunities* directly. That turns
"explain the score" from a nice-to-have into the core of the design.

**Weakest points, stated plainly:** I read a law firm's summary of HUD's guidance rather
than HUD's own documents, and I have not read the SafeRent filings. Both are load-bearing
for Finding 2 and Finding 3, and both should be checked before any of this reaches a client
conversation. For choosing a hackathon project this afternoon, they're sound enough.

**What I'd want and don't have:** any non-vendor study of rental lead conversion, and
whether Rentsync already has sanitized historical lead data — which would change the
prototype's evaluation approach considerably.

---

## Sources

- [HUD 2024 guidance on the Fair Housing Act, tenant screening and algorithmic advertising — summary with quotations](https://www.consumerfinancialserviceslawmonitor.com/2024/05/hud-issues-guidance-on-applicability-of-the-fair-housing-act-to-tenant-screening-and-housing-related-advertising-that-relies-upon-algorithms-and-ai/)
- [Ontario Human Rights Commission — Policy on human rights and rental housing](https://www.ohrc.on.ca/en/policy-human-rights-and-rental-housing)
- [OHRC — Prohibited grounds of discrimination](https://www.ohrc.on.ca/en/human-rights-and-rental-housing-ontario-background-paper/prohibited-grounds-discrimination)
- [Georgetown Law Poverty Journal — The Discriminatory Impacts of AI-Powered Tenant Screening Programs](https://www.law.georgetown.edu/poverty-journal/blog/the-discriminatory-impacts-of-ai-powered-tenant-screening-programs/)
- [The Leadership Conference — AI + Tenant Screening](https://civilrights.org/resource/ai-tenant-screening/)
- [Propmodo — What the Research on Lead Conversion Tells Us About AI's Role in Leasing](https://propmodo.com/what-the-research-on-lead-conversion-tells-us-about-ais-role-in-leasing/) *(cited as evidence about sourcing, not for its figures)*
