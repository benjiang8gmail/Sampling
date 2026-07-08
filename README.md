# Monetary Unit Sampling (MUS) — Audit Tool

A single-page web app that helps financial statement auditors plan, draw and evaluate a
**Monetary Unit Sample**. Everything runs in the browser — no data leaves the page.

Open [`Sampling.html`](Sampling.html) in any modern browser. *(It loads React and Babel from a CDN,
so an internet connection is needed the first time.)* Styled with the Mosaic / Fluent 2 design
tokens used across the VAGO EDMS pages.

## What it does

**Step 1 — Estimate the sample size**
Pick the expected error level (Low / Moderate EE$), confidence level and materiality. The required
sample size is read from the VAGO DST guidance tables, with the relevant cell highlighted.

**Step 2 — Select the sample (probability-proportional-to-size)**
Paste or upload your population (CSV/Excel), choose the **amount column**, and pick the **sample basis —
positive or negative values** (e.g. to sample credit notes / refunds; negatives are sampled on their absolute
amounts). The tool computes the **sampling interval** (population ÷ sample size — or enter your own). A random
start is generated (and editable, for reproducibility). Fixed-interval MUS selection draws the sample; any item
whose value ≥ the interval is flagged as a **key item** examined in full. The CSV export records the sample
basis (and the original signed amounts).

**Step 3 — Evaluate & project errors**
Enter the audited value beside each book value (or bulk-paste a column from Excel). Every selected item has a
**status** — tested, not yet tested, **unable to test** (treated as fully misstated per ASA 530.11), or
**anomaly** (excluded from projection with a mandatory justification per ASA 530.13, shown both ways). The
**conclusion is withheld until every item is resolved**; meanwhile a best/worst-case UML range is shown. For
each sample item the tool computes the **tainting** (capped at ±100% for the Stringer maths, with any excess
disclosed); **key items use their actual misstatement uncapped**. It evaluates **both directions**: the
**most likely overstatement** and **upper misstatement limit (UML)**, and — evaluated separately — the
**most likely understatement** and **lower misstatement limit (LML)**, plus the **minimum error cushion**
(basic precision — the buffer both limits carry with zero errors found) and the **net most likely error**
(carried to the schedule of misstatements). Each limit is displayed as an equation that foots:
*most likely + minimum error cushion + incremental allowance = limit*, where the **incremental allowance**
is the extra sampling-risk margin that grows as errors are found. Both limits are compared with tolerable
misstatement. Because MUS
has **low power against understatement**, the LML carries a plain-English caution — a low LML is not assurance
over completeness (plan a separate completeness test), though an LML above materiality is a genuine red flag.
The evaluation CSV includes the ranked-taint **workings for both limits** so a reviewer can re-add either bound
by hand.

**Documentation (ASA)**
A concise reference page (the **ASA · Documentation** tab) on what to record so the work meets the Australian
Auditing Standards issued by the AUASB — how to evidence (a) the sample-size estimate, (b) the sample selection
(the tool used and how it selects), and (c) the projection of errors to the population — with a **worked example
throughout for detailed substantive testing of supplies and services expenses**. References ASA 530 (Audit
Sampling), ASA 230 (Audit Documentation), and ASA 320 / 450 / 330 / 500.

Each of Steps 1–3 also includes a live **Example documentation** working-paper note (under the methodology
section) that updates as you change inputs — modelling how to evidence the sample-size estimate, the selection,
and the projection on the audit file.

## Files, sessions & reperformance

Enter your name in the **Performed by** field (top right) — it is remembered and used in default file names.
The whole session **auto-saves to the browser** (an accidental refresh loses nothing; *Reset session* in the
footer starts fresh). One **Files** panel (Steps 2 and 3) saves the current sample (browser + CSV), **opens**
any previously saved sample or evaluation CSV (type auto-detected), and lists what's saved with Load / Export
CSV / Delete. (On the Evaluate step the Save controls sit beside *Download evaluation (CSV)* at the foot of
the results.) Every save **adds a new entry** (names are auto-uniquified, never overwritten); the browser list
keeps the **three most recent** — older entries drop off with a notice, and the CSV copy written at save time is
unaffected. Populations can be uploaded as **Excel (.xlsx)** or CSV.

Every export is stamped with the **tool version**, run date/time, column choices, exclusion counts and a
**population fingerprint** (row count, total and a checksum), so a reviewer can prove "same file". An optional
**sort by reference before selecting** makes the selection order reconstructable from the data itself, and the
one-click **Reperform check** re-runs a saved sample's selection against the loaded population and reports
match/mismatch item by item. The Documentation tab includes a live **model workpaper (Part B)** built from the
current session, downloadable as a **Word document**.

## Methodology notes

- **Selection:** fixed-interval (systematic) MUS. Each item is selected with probability proportional to its
  book value; items ≥ the sampling interval are certainty selections.
- **Reliability factors** are the Poisson upper confidence limits, rounded up to two decimals to match the
  standard published audit tables (e.g. at 95%: 3.00, 4.75, 6.30, 7.76 …).
- **Projection (Stringer bound):**
  `UML = interval × [ R(0) + Σ taintᵢ × (R(i) − R(i−1)) ] + key-item misstatements`,
  with taints ranked largest first. Overstatements and understatements are evaluated **separately**, each
  producing a most likely error and a limit (the **lower misstatement limit** is the same formula applied to
  understatement taints). `R(0) × interval` is the **minimum error cushion** (basic precision) — the floor both
  limits share when no errors are found.

Outputs are a calculation aid — apply professional judgement and your firm/office methodology.

## Hosting

Because it is a single static file, it can be published with **GitHub Pages**
(Settings → Pages → deploy from the default branch) and shared as a link — the page will be at
`/Sampling.html`. *(Rename the file back to `index.html` if you want it served at the site root.)*
