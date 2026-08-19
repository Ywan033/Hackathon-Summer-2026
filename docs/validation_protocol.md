# Validation protocol

Frozen for all experiments on `ywan/ml-pipeline` unless a later sprint explicitly replaces this file and `experiments/folds.csv` together.

## Protocol

```text
StratifiedKFold(n_splits=3, shuffle=True, random_state=20260819)
```

Persisted assignments: `experiments/folds.csv` with columns `Cell_ID,fold` and fold IDs `0,1,2`. Every training Cell_ID appears exactly once. Test Cell_IDs are excluded.

Future runs **load** this file. They do not call `StratifiedKFold` again to create a new split.

## Why 3-fold stratification

- The official metric is overall accuracy on a 60-class problem with a long tail (minimum training count = 3). Stratification keeps class prevalence similar across folds, including the rarest class, which can appear in every fold because `n = 3` equals `n_splits`.
- Three folds is enough to get a mean and a range without shrinking training sets too far (each model sees about 3333 cells).
- The official train/test split is a **within-section mixed split with closely matched marginals**. Stratified random folds on cells match that mixed regime more closely than holding out entire sections or mice. A grouped split may be added later as a stress test; it is not the selection protocol.

## Why folds are persisted

Model comparisons are only interpretable if every method is scored on the same cells. Regenerating folds (different seed, library version, or row order) would confound method with split. `experiments/folds.csv` is the lock file for that split. Tests check that the saved file still matches the frozen `StratifiedKFold` call.

## Out-of-fold (OOF) predictions

For each training cell, the recorded prediction comes from the model fitted on the **other two folds only**. Concatenating the three validation predictions yields one OOF label per training cell, with no cell scored by a model that trained on that cell.

OOF accuracy is the overall accuracy of that concatenated vector (5000 scored cells). Fold accuracies are the same metric on each validation fold separately.

## Metrics

| Role | Metric |
|---|---|
| **Primary, model selection** | overall accuracy (matches the hackathon) |
| Diagnostic | per-fold accuracy |
| Diagnostic | mean / OOF accuracy |
| Diagnostic | macro-F1 |
| Diagnostic | per-class recall |

Macro-F1 and per-class recall are reported so rare types are visible. They are not the selection rule.

There is no automated “accept if the gain exceeds fold standard deviation” gate. Decisions are recorded in `experiments/registry.csv` with the actual fold numbers.

## Leakage rules for this protocol

1. **Fit on the training portion of the fold only.** Normalization, encoders, signature-to-label maps, neighbors, and models are estimated from the two training folds, then applied to the validation fold.
2. **Do not use test labels.** `meta_test.MERFISH_cell_type_annotation` is fully hidden. Do not treat `prediction/prediction.csv` as labels.
3. **Do not use the public leaderboard to choose models.** Leaderboard scores are the official test set.
4. **Cell_ID must stay a string.** Float conversion corrupts joins and fold alignment.
5. **`(Region, Excitatory_vs_Inhibitory, Segment)` maps that look deterministic in the full training set are only observed-deterministic there.** Rebuild any such map inside each training fold.
6. **Spatial neighbors:** train and test cells are mixed inside the same 108 sections. Neighbor features are allowed only from cells that are in the current training portion. Validation-fold labels must not enter those features.
7. **`Excitatory_vs_Inhibitory` is strongly target-correlated metadata, potentially annotation-derived.** It is present on test, so it is competition-legal, but it is not a gene measurement. Sprint 1 baselines do not use it.

## Sprint 1 baselines

- `YW-000`: per-training-fold majority class (no features).
- `YW-001`: `log1p` of the 200 raw gene counts, L2 logistic regression, no metadata, no coordinates, no spatial features, no class weighting.

Results live in `outputs/metrics/`, `outputs/oof/`, and `experiments/registry.csv`.
