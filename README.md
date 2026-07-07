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
Enter the audited value beside each book value. For each sample item the tool computes the **tainting** and
**projected misstatement**; key items use their actual misstatement. It reports the **most likely error**,
**basic precision** and the **upper misstatement limit** (Stringer bound), and compares the UML with
materiality to reach a conclusion. Export the evaluation to CSV.

**Documentation (ASA)**
A concise reference page (the **ASA · Documentation** tab) on what to record so the work meets the Australian
Auditing Standards issued by the AUASB — how to evidence (a) the sample-size estimate, (b) the sample selection
(the tool used and how it selects), and (c) the projection of errors to the population — with a **worked example
throughout for detailed substantive testing of supplies and services expenses**. References ASA 530 (Audit
Sampling), ASA 230 (Audit Documentation), and ASA 320 / 450 / 330 / 500.

Each of Steps 1–3 also includes a live **Example documentation** working-paper note (under the methodology
section) that updates as you change inputs — modelling how to evidence the sample-size estimate, the selection,
and the projection on the audit file.

## Saving & recalling samples

Enter your name in the **Performed by** field (top right) — it is remembered and used in default file names.
After a sample is selected you can **save** it; the default name is `username_ddmmyy_HHMM_positive|negative`.
Saved samples are kept in the browser's local storage and listed under **Saved samples** in Steps 2 and 3, where
you can **Load** one back in before extrapolating errors, **Delete** it, or **Export** it to a JSON file (and
**Import** it on another machine) for your working papers. CSV/JSON exports default to the same name.

## Methodology notes

- **Selection:** fixed-interval (systematic) MUS. Each item is selected with probability proportional to its
  book value; items ≥ the sampling interval are certainty selections.
- **Reliability factors** are the Poisson upper confidence limits, rounded up to two decimals to match the
  standard published audit tables (e.g. at 95%: 3.00, 4.75, 6.30, 7.76 …).
- **Projection (Stringer bound):**
  `UML = interval × [ R(0) + Σ taintᵢ × (R(i) − R(i−1)) ] + key-item misstatements`,
  with taints ranked largest first. Overstatements and understatements are evaluated separately.

Outputs are a calculation aid — apply professional judgement and your firm/office methodology.

## Hosting

Because it is a single static file, it can be published with **GitHub Pages**
(Settings → Pages → deploy from the default branch) and shared as a link — the page will be at
`/Sampling.html`. *(Rename the file back to `index.html` if you want it served at the site root.)*
