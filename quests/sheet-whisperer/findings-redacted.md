# Sheet Whisperer — findings (redacted)

**Sheet:** internal integrations knowledge base, another team's file, read live via the
Google Drive connector · 21 columns · 48 populated rows · entries span 2026-06-25 → 2026-09-04

> **Redaction note:** client names, account numbers, vendor portal hostnames, internal
> app URLs and the author's name are removed, and PMS vendors are referred to
> generically. All counts, ratios and structural findings are exact and unmodified.

---

## 1. Over half the knowledge base has no Entry ID — and it's a required field

The sheet's own "How to use" tab states: *"One row = one entry. Required: Entry ID,
Created, Last Updated, Author, Scope, Client, Integration/PMS, Category, Title,
Summary, Detail."*

| | Count |
|---|---|
| Rows with an Entry ID (`KL-0001`–`KL-0022`) | 22 |
| Rows with content but **no** Entry ID | **26** |
| Total populated rows | 48 |

This isn't cosmetic. The file already cross-references itself by ID — one entry cites
another as *"Instance: KL-0001"*. Twenty-six entries cannot be cited that way, linked
to, or pointed at in a ticket. More than half the knowledge base is unaddressable.

## 2. There are two ingestion paths, and the correlation is perfect

| | Has Raw Capture | No Raw Capture |
|---|---|---|
| **Has Entry ID** | 22 | 0 |
| **No Entry ID** | 0 | 26 |

Zero mixed rows. Every ID'd row has the original submitted text preserved; no
unlabelled row does. That's not drift — that's two different processes writing into one
sheet, one of which drops both the identifier and the original source text.

The second path is the newer one. So the file is getting *less* traceable over time,
not more.

## 3. The `Created` column holds two incompatible types

| Format | Rows |
|---|---|
| ISO date strings (`2026-06-25`) | 40 |
| Raw spreadsheet serial numbers (`46224`) | 8 |

The serials decode to real dates — `46224` → 2026-07-21, `46231` → 2026-07-28 — so no
data is lost. But any sort, date filter, or "what's gone stale" logic silently
mis-orders: eight entries from late July sort as text-vs-number rather than by date.
The whole late-July block is the affected one.

## 4. An exact duplicate, and a near-duplicate

- **Exact:** two rows share an identical title, identical summary, identical tags and
  the same `Created` value. Same entry, entered twice. Both are in the unlabelled set,
  which is presumably how it went unnoticed — with no Entry ID, there's nothing to
  collide.
- **Near:** two ID'd entries describe the same underlying mechanism, one scoped
  globally and one to a specific client. Defensible by design, but the second explicitly
  names the first as an instance, so it reads as one fact filed twice.

## 5. The analytical columns are mostly empty

| Column | Filled |
|---|---|
| Tags / Summary / Detail | 48 / 48 |
| Related Integrations | 35 / 48 |
| Source Links | 14 / 48 |
| Problem / Symptom | 11 / 48 |
| Solution / Workaround | 11 / 48 |
| Impact | 9 / 48 |
| **Root Cause** | **7 / 48** |

Descriptive fields are complete; diagnostic fields are under a quarter filled. The
schema was designed to capture *why things break and what fixed them*, and is being
used to capture *what happened*. Root Cause — arguably the highest-value column in a
troubleshooting KB — is populated on 15% of entries.

## 6. Bus factor of one

All 48 entries have the same author. Ten weeks of accumulated institutional knowledge,
one contributor. No second person has written into the file the tab describes as the
"single source of truth."

## 7. Concentration

One PMS vendor accounts for **17 of 48 entries (35%)** — more than three times the next
most common. `Feed Mapping` is the single largest category (11), ahead of
`Incoming Integration` and `Process/SOP` (9 each). If this log is a proxy for where
integration effort actually goes, a third of it goes to one vendor.

## 8. The credential rule is holding

The "How to use" tab says *"Never store credentials. Note where the secret lives
instead."* One entry stores a full outbound webhook payload — with the auth key
replaced by a placeholder and a note to keep real keys in a secrets manager. The one
row where that rule was load-bearing, and it was followed.

---

## What I'd actually do with this

Ranked by effort-to-value, if this were my file to fix:

1. **Backfill the 26 missing Entry IDs.** Mechanical, and it's the prerequisite for
   every other fix — including detecting the duplicate.
2. **Normalize the eight serial dates to ISO.** One-minute fix, removes a silent
   sorting bug.
3. **Delete the exact duplicate.**
4. **Find out why the second ingestion path drops Entry ID and Raw Capture** — this is
   the one that keeps generating problems 1 and 4 rather than being a one-off cleanup.
5. **Decide whether Root Cause is actually wanted.** 7/48 says either the column should
   be enforced or it should be dropped. A column that's empty 85% of the time trains
   people to ignore the schema.

## Honest note on this quest

The interesting findings are structural, not domain insights. Claude didn't tell me
something new about property-management integrations — it told me the file recording
them is half-unlabelled, double-typed and singly-authored. That's a fair outcome for
"what don't I know about this?", but worth stating plainly: this was a data-hygiene
audit, not analysis.

And the near-miss is the part worth keeping. The first read was a ten-row sample, and
the confident story available from that sample — small file, brief burst of activity,
long since abandoned — was wrong on all three counts. The file is nearly five times
bigger and still active as of last week. Every finding above lives in rows the sample
didn't return.
