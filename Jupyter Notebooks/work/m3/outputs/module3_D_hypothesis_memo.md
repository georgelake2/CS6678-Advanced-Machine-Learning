# EDA Hypothesis Memo — Module 3.D (NYC 311)

**Top 3 hypotheses to pursue next**

_Scoring weights_: impact=0.35, falsifiable=0.35, robust=0.2, cost=0.1

## H11 (priority score: 5.0·impact + 5.0·falsifiable + 5.0·robust + 5.0·cost = 5.00)
- **Observation:** Median resolution time differs dramatically by agency (e.g., NYPD very fast, HPD very slow)
- **Hypothesis:** Resolution time is primarily determined by the type of work each agency performs
- **Prediction:** Within a single agency, the variance of resolution time will be much smaller than the full dataset
- **Primary confounder:** Complaint type mix inside each agency
- **Next check (one plot / one table):** Compute resolution-time spread (IQR or p95) per agency and compare to global spread

## H12 (priority score: 5.0·impact + 5.0·falsifiable + 4.0·robust + 5.0·cost = 4.80)
- **Observation:** Heat/Hot Water and similar categories have very high p50 and p95 compared to noise or parking
- **Hypothesis:** Long resolution times are caused by complaint types that require multi-step or physical interventions
- **Prediction:** Most cases above the global p95 threshold will belong to a small number of complaint types
- **Primary confounder:** These complaint types are mostly handled by one agency
- **Next check (one plot / one table):** Look at the complaint-type distribution for only the slow (>= p95) cases

## H7 (priority score: 4.0·impact + 5.0·falsifiable + 4.0·robust + 5.0·cost = 4.45)
- **Observation:** Top correlations against resolution_hours change noticeably across different slices/time windows.
- **Hypothesis:** Many 'top' correlations are unstable (feature fishing / multiple comparisons) rather than consistent signals.
- **Prediction:** Rank correlation tables in two splits (first half vs second half of the week) will show low overlap in top-k features.
- **Primary confounder:** A real process change or a volume composition shift happened within the week.
- **Next check (one plot / one table):** Compute top-k correlations in two splits; measure overlap and rank correlation; identify features that remain top across both splits.

---
**Notes for reviewers:**
- Each hypothesis names a metric (median/p95/rate), a grouping (agency/type/borough/time), and a direction.
- Confounders are concrete, not generic (composition, censoring, logging, timing).
- Next checks are intentionally cheap: a single stratified plot or summary table.
- If a check fails, the hypothesis should be downgraded or discarded.