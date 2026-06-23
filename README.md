# Monetary Unit Sampling (MUS) — Audit Tool

A single-page web app that helps financial statement auditors plan, draw and evaluate a
**Monetary Unit Sample**. Everything runs in the browser — no data leaves the page.

Open [`index.html`](index.html) in any modern browser. *(It loads React and Babel from a CDN,
so an internet connection is needed the first time.)*

## What it does

**Step 1 — Estimate the sample size**
Pick the expected error level (Low / Moderate EE$), confidence level and materiality. The required
sample size is read from the VAGO DST guidance tables, with the relevant cell highlighted.

**Step 2 — Select the sample (probability-proportional-to-size)**
Paste or upload your population (CSV/Excel), choose the **amount column** sampling is based on, and the
tool computes the **sampling interval** (population ÷ sample size — or enter your own). A random start is
generated (and editable, for reproducibility). Fixed-interval MUS selection draws the sample; any item
whose book value ≥ the interval is flagged as a **key item** examined in full. Export the selection to CSV.

**Step 3 — Evaluate & project errors**
Enter the audited value beside each book value. For each sample item the tool computes the **tainting** and
**projected misstatement**; key items use their actual misstatement. It reports the **most likely error**,
**basic precision** and the **upper misstatement limit** (Stringer bound), and compares the UML with
materiality to reach a conclusion. Export the evaluation to CSV.

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

Because it is a single static file named `index.html`, it can be published with **GitHub Pages**
(Settings → Pages → deploy from the default branch) and shared as a link.
