# Plan: CLEAR: an auditable foundation model for radiology grounded in clinical concepts

- Issue: https://github.com/suhyunchoe/demo/issues/3
- Paper: `recommended/2026-08-19/clear-auditable-radiology-foundation-model/paper.json`
- DOI: https://doi.org/10.1038/s41551-026-01741-4
- Physician focus: "Would this method hold up in a cohort like ours with few nodule cases? I'm especially worried about missed nodules (false negatives)."

## Proposed presentation

Dashboard entry goal: help the physician judge whether CLEAR's multi-institution,
multi-label generalization claims are even testable on our 272-study, 5-institution
cohort, with special attention to the Nodule subgroup (12 studies, 7 "pure" Nodule
+ 5 Nodule combined with another finding) where false negatives are the stated
concern.

1. **Stacked bar: findings_label distribution by institution (INST01–05)**
   X-axis = institution, Y-axis = study count, stacked/colored by top labels
   (No Finding, Infiltration, Atelectasis, Nodule, others). This is the chart type
   that best answers "does institution matter for the labels we care about" —
   a single number (272 total) hides that Nodule cases are unevenly spread across
   institutions (4/2/1/2/3), which is exactly the kind of institutional skew CLEAR's
   external-validation claim is meant to be robust to, and exactly what could hide
   or inflate a false-negative rate if we ever tested it here.

2. **Bar chart: Nodule-subgroup counts by institution and by device**
   Y-axis = count of studies with any Nodule label, X-axis = INST01–05 (second
   panel: DEV01–09). Rationale: the physician's question is about a *specific
   small subgroup*, not the whole cohort, so the subgroup needs its own explicit
   visualization    rather than being folded into the whole-cohort chart — with n=1–4
   per institution/device, per-site zero-shot recall claims cannot be estimated
   reliably, which is the visual the physician needs to see directly.

3. **Text section: "Why small-n matters here"** — plain-language note that with
   12 Nodule-positive studies total (4.4% of the cohort) split across 5
   institutions and 7 devices, any per-institution false-negative rate has a
   denominator of 1–4 and is not statistically meaningful; a deep dive can at
   best report qualitative/ anecdotal misses, not a calibrated sensitivity.

4. **Table: label co-occurrence for Nodule cases** (Nodule alone vs. Nodule +
   Effusion/Infiltration/Atelectasis/Fibrosis/Pleural_Thickening) — supports
   the physician's "missed nodule" concern because combined-finding cases are
   plausibly harder to localize/detect than isolated nodules, and the table
   shows this is more than half (5/12) of our nodule cases.

Layout order: (1) cohort-wide institution/label distribution → (2) nodule
subgroup by institution/device → (3) small-n caveat text → (4) co-occurrence
table. This mirrors the physician's reasoning: start from "is generalization
even testable here," narrow to "how small is the nodule subgroup," then "how
hard are these particular nodule cases."

## Open questions

- Does the physician want a **prospective small-scale zero-shot check** (running
  a concept-bottleneck-style classifier on our 12 Nodule studies) even knowing
  the subgroup is too small for a calibrated false-negative rate, or only a
  **descriptive/qualitative review** of those 12 cases?
- Should "Nodule" cases combined with other findings (5 of 12) be analyzed
  separately from "pure" Nodule cases (7 of 12), since combined-finding images
  may have different miss risk?
- Is there a target false-negative-rate benchmark from CLEAR's four external
  validation cohorts that the physician wants used as a qualitative reference
  point, given our sample cannot reproduce a comparable metric?
- Should institution- or device-level splits be used for any deep dive, given
  both are similarly small (max 4 nodule cases per institution, max 4 per device)?
