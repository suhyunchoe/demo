# Deep dive: CLEAR — auditable radiology foundation model (issue #3)

- Paper: [`recommended/2026-08-19/clear-auditable-radiology-foundation-model/paper.json`](../../recommended/2026-08-19/clear-auditable-radiology-foundation-model/paper.json)
- DOI: https://doi.org/10.1038/s41551-026-01741-4
- Issue: https://github.com/suhyunchoe/demo/issues/3
- Approved plan: [`plan.md`](./plan.md)
- Physician's question (issue #3): *"우리처럼 결절 증례가 적은 코호트에서도 이 방법이 통할까요? 특히 놓친 결절(거짓음성)이 걱정입니다."* ("Would this method hold up in a cohort like ours with few nodule cases? I'm especially worried about missed nodules / false negatives.")

## What the paper reports

CLEAR (Concept‑Level Embeddings for Auditable Radiology) is pretrained on **0.87 million image‑report pairs from 239,391 patients**, and externally validated on **four large, physician‑annotated datasets from the US, Europe, and Asia**. It reports state‑of‑the‑art multi‑label chest X‑ray classification and auditable zero‑shot pathology detection, including concept‑bottleneck models built from data‑driven concepts (source: paper abstract, linked above).

## What our data shows

All numbers below were pulled with `mcpscripts query_medical_db` against the masked `llm.hospital` view. Cohort‑level aggregates only — no row-level identifiers are reported.

**Cohort size**
```sql
select count(*) from llm.hospital;               -- 272
select count(distinct patient_key) from llm.hospital; -- 153
```

**Institution totals**
```sql
select institution_code, count(*) from llm.hospital group by institution_code order by institution_code;
```
| Institution | Studies |
|---|---|
| INST01 | 62 |
| INST02 | 56 |
| INST03 | 54 |
| INST04 | 48 |
| INST05 | 52 |

**Findings-label buckets by institution** (bucketing rule: any label containing "Nodule" → Nodule; else exact "No Finding"; else contains "Infiltration"/"Atelectasis"; else Other)
```sql
select institution_code,
  case when findings_label like '%Nodule%' then 'Nodule'
       when findings_label='No Finding' then 'No Finding'
       when findings_label like '%Infiltration%' then 'Infiltration'
       when findings_label like '%Atelectasis%' then 'Atelectasis'
       else 'Other' end as bucket,
  count(*)
from llm.hospital group by 1,2 order by 1,2;
```
See `figure.svg` for the stacked chart. No Finding dominates every institution (23–36 of 48–62 studies); Nodule is the smallest bucket everywhere (1–4 per institution).

**Nodule subgroup — total, by institution, by device**
```sql
select count(*) from llm.hospital where findings_label like '%Nodule%';                 -- 12
select institution_code, count(*) from llm.hospital where findings_label like '%Nodule%' group by institution_code order by institution_code;
select device_id, count(*) from llm.hospital where findings_label like '%Nodule%' group by device_id order by device_id;
```
| Institution | Nodule studies |
|---|---|
| INST01 | 4 |
| INST02 | 2 |
| INST03 | 1 |
| INST04 | 2 |
| INST05 | 3 |

Nodule appears on 7 of 9 devices, at most 4 studies on any single device (DEV09), 1 on several others.

**Pure vs. combined Nodule cases**
```sql
select case when findings_label='Nodule' then 'pure' else 'combined' end, count(*)
from llm.hospital where findings_label like '%Nodule%' group by 1;
-- pure: 7, combined: 5
```
Combined-label breakdown:
```sql
select findings_label, count(*) from llm.hospital
where findings_label like '%Nodule%' and findings_label <> 'Nodule' group by findings_label;
```
| Combined label | n |
|---|---|
| Atelectasis, Nodule | 1 |
| Effusion, Infiltration, Nodule | 1 |
| Effusion, Infiltration, Nodule, Pleural_Thickening | 1 |
| Effusion, Nodule | 1 |
| Fibrosis, Nodule | 1 |

## What we infer (hypothesis-generating only)

- **Institution-level generalization is not testable here in CLEAR's sense.** CLEAR's external validation used cohorts an order of magnitude larger per site. Our per-institution totals (48–62) and, critically, per-institution Nodule counts (1–4) cannot support a calibrated sensitivity/false-negative-rate estimate — any such number would have a denominator too small to be statistically meaningful.
- **The physician's false-negative concern cannot be quantified with this data.** With 12 Nodule-positive studies split 4/2/1/2/3 across institutions, at best a qualitative, case-by-case review of these 12 studies is feasible — not a calibrated miss rate.
- **Combined-finding Nodule cases (5/12, 42%) are a plausible extra risk factor** worth flagging to the radiologist reviewing this PR, since co-occurring findings (Effusion, Infiltration, Atelectasis, Fibrosis, Pleural_Thickening) could make nodules harder to localize — but we cannot verify this claim against ground truth without report-level review, which is outside the scope of this masked, cohort-aggregate analysis.
- We also lack the 239,391-patient, report-text pretraining corpus CLEAR relies on for its concept embeddings — so even setting aside the small-N issue, our data cannot reproduce CLEAR's *auditability* mechanism, only motivate whether it would be worth external validation on a larger version of this cohort.

## Limitations

- **Sample size**: 272 studies / 153 patients total; only 12 Nodule-positive studies (4.4%). This is far below the scale needed to estimate a false-negative rate with any precision.
- **No report text / pretraining data**: `report_text` exists in the schema but was not used to reproduce CLEAR's concept-embedding training — we only queried structured labels (`findings_label`, `institution_code`, `device_id`), not free text, per the masking rules for this analysis.
- **No ground-truth adjudication**: We used the existing `findings_label` field as-is; we did not (and could not, under the masking rules) review images or radiologist reports to confirm whether any Nodule case was itself a missed/borderline call.
- **Label bucketing is a simplification**: the "Infiltration"/"Atelectasis"/"Other" buckets in the stacked chart use substring matching on a multi-label field for visualization only; the underlying multi-label detail is preserved in the co-occurrence table above.
- **Findings here are hypothesis-generating, not confirmatory.** No inferential statistics (e.g., confidence intervals) are reported because the subgroup sizes do not support them.
