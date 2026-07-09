---
name: rigorous-evaluation
description: "Use when evaluating or reporting a model's performance, choosing metrics, or picking a decision threshold. Covers the metric that matters per use case, calibration, thresholds chosen on validation (never test), confusion matrices, ROC/PR curves, confidence intervals, and honest reporting. Triggers on 'evaluate', 'metrics', 'confusion matrix', 'ROC/AUC', 'which threshold', 'is this result good', 'report results'."
---

# Rigorous Evaluation

The goal is a number you can defend, not the biggest number.

## Pick the metric that matters
- Imbalanced / screening: **AUC + recall (sensitivity) at a fixed operating point**, PR-AUC. Accuracy alone is misleading.
- Detection: **mAP@[IoU]**. Segmentation: **Dice / IoU**. Regression: **MAE/RMSE + R²**. Ranking: **NDCG/MAP**.
- High-stakes (medical, safety): report **sensitivity AND specificity** at the chosen operating point, plus calibration.

## Thresholds & selection - on VALIDATION only
- Choosing the decision threshold using **test** labels is leakage. Select it on validation, then apply unchanged to test.
- Report at least two operating points when relevant: **Youden J** (balanced) and a **high-sensitivity** point (e.g., recall ≥ 0.90) for screening use.

## Calibration (for decision support)
Raw softmax is often over-confident. Apply **temperature scaling** (fit on val) so a displayed probability is trustworthy. Report reliability, not just discrimination.

## Uncertainty & fairness
- Report **confidence intervals** (bootstrap, or DeLong for AUC) when the test set is small.
- Train each model a few times / use CV; report mean ± std. A single run's val-best may not be the test-best.
- Check performance across **subgroups** where it matters (site, device, demographic).

## Honest reporting
- Show the **confusion matrix** and per-class metrics, not just the headline.
- If a result looks too good, **re-run the leakage audit** (`data-rigor-and-leakage`) before believing it.
- State the comparison honestly: same split as the papers? same metric? Note when a result is *not* comparable.
- Never report a number you have not actually computed from a real run.
