# Validation protocols

This repository uses **two** frozen evaluation contracts. They are not the
same kind of score. Do not treat MODEL V1, MODEL V2, and V3 experimental
numbers as interchangeable OOF.

| Identity | File | What the number is |
|---|---|---|
| MODEL V1 | `experiments/folds.csv` | Conventional 3-fold **competition-label OOF** = **0.7598** |
| BRIDGE-YW004-5F | `experiments/team_folds_5_seed42.csv` | Same V1 architecture on the 5-fold partition = **0.7596** |
| MODEL V2 | `experiments/team_folds_5_seed42.csv` | Honest **external-reference validation** = **0.8212** |
| V3 experimental candidates | same 5-fold file unless a frozen report says otherwise | External-validation or analysis-only; not a formal MODEL version |

Future runs **load** these files. They do not call `StratifiedKFold` again to
create a new split. Do not overwrite either file.

Diagnostic oracle coverage is defined in §5. It is not deployable accuracy.

---

# 1. MODEL V1 — competition-label 3-fold OOF

Protocol:

```text
StratifiedKFold(n_splits=3, shuffle=True, random_state=20260819)
```

Persisted assignments: `experiments/folds.csv` with columns `Cell_ID,fold`
and fold IDs `0,1,2`. Every training Cell_ID appears exactly once. Test
Cell_IDs are excluded.

## Fitting target

Competition training labels **are** the fitting target. Signature maps,
specialists, and the global fallback are estimated from the two training
folds, then applied to the held-out fold.

## Genuinely OOF predictions

For each training cell, the recorded prediction comes from a model fitted on
the **other two folds only**. Concatenating the three validation predictions
yields one OOF label per training cell. No cell is scored by a model that
trained on that cell.

OOF accuracy is overall accuracy of that concatenated vector (5000 scored
cells). Fold accuracies are the same metric on each validation fold.

**MODEL V1 result: 3799 / 5000 = 0.7598 (75.98%).**

## Why 3-fold stratification

- The official metric is overall accuracy on a 60-class problem with a long
  tail (minimum training count = 3). Stratification keeps class prevalence
  similar across folds, including the rarest class, which can appear in
  every fold because `n = 3` equals `n_splits`.
- Three folds is enough to get a mean and a range without shrinking training
  sets too far (each model sees about 3333 cells).
- The official train/test split is a within-section mixed split with closely
  matched marginals. Stratified random folds on cells match that mixed
  regime more closely than holding out entire sections or mice.

## V1 leakage rules

1. Fit on the training portion of the fold only. Normalization, encoders,
   signature-to-label maps, neighbors, and models are estimated from the two
   training folds, then applied to the validation fold.
2. Do not use test labels. `meta_test.MERFISH_cell_type_annotation` is fully
   hidden. Do not treat `prediction/prediction.csv` as labels.
3. Do not use the public leaderboard to choose models.
4. Cell_ID must stay a string. Float conversion corrupts joins and fold
   alignment.
5. `(Region, Excitatory_vs_Inhibitory, Segment)` maps that look deterministic
   in the full training set are only observed-deterministic there. Rebuild
   any such map inside each training fold.
6. Spatial neighbors: train and test cells are mixed inside the same 108
   sections. Neighbor features are allowed only from cells that are in the
   current training portion. Validation-fold labels must not enter those
   features.
7. `Excitatory_vs_Inhibitory` is strongly target-correlated metadata,
   potentially annotation-derived. It is present on test, so it is
   competition-legal, but it is not a gene measurement. Sprint 1 baselines
   do not use it.

Sprint 1 baselines on this protocol: `YW-000` (per-training-fold majority)
and `YW-001` (log1p 200-gene L2 logistic regression). Selected MODEL V1
architecture is YW-004. Results live in `outputs/metrics/`, `outputs/oof/`,
and `experiments/registry.csv`.

---

# 2. Protocol-matched V1 bridge

**BRIDGE-YW004-5F = 0.7596.**

This is the frozen YW-004 (MODEL V1) architecture scored on
`experiments/team_folds_5_seed42.csv`. It is **not** a new formal MODEL
version.

It exists so that the V1 architecture can be compared with MODEL V2 under
the **same** 5-fold evaluation partition. MODEL V1’s 0.7598 is a different
protocol (3-fold competition-label OOF) and must not be used as the
directly protocol-matched baseline for 0.8212.

Script: `scripts/07_bridge_yw004_5f.py`.

---

# 3. MODEL V2 — external-reference validation

File: `experiments/team_folds_5_seed42.csv`

```text
StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
```

This file is the common **evaluation / robustness partition** for MODEL V2
and later V3 comparisons. Fold IDs are `0,1,2,3,4`. Every training Cell_ID
appears exactly once. Test Cell_IDs are excluded.

## Training source

MODEL V2 LightGBM is fit **only on the 136,574 cleaned Zenodo reference
rows**. Competition train labels are **not** used to fit the LightGBM
classifier.

When scoring competition fold *f*:

- fold-*f* competition labels are invisible in model fitting, neighbor-label
  histograms, routing, masks, feature selection, and post-process selection;
- reference labels remain visible as permitted external labeled data;
- other training-fold labels may enter neighbor histograms only;
- hidden test labels are never read.

## Validation set

Predictions on the **5000 competition-train cells** form the validation set.
Accuracy 4106 / 5000 = **0.8212 (82.12%)** is honest **external-reference
validation**, not conventional competition-label OOF.

The 5-fold partition is used for robustness / evaluation slicing
(per-fold accuracies 0.822 / 0.831 / 0.812 / 0.821 / 0.820) and for
fold-safe neighbor-label histograms. It does not convert the score into
competition-label OOF.

This contract is stricter than the `team/main` `ext_refonly.py`
all-train-visible evaluation.

## Protocol-matched improvement

```text
0.8212 − 0.7596 = +0.0616
= +6.16 percentage points versus BRIDGE-YW004-5F
```

The descriptive **cross-protocol** difference between MODEL V1 3-fold OOF
0.7598 and MODEL V2 external-reference validation 0.8212 is **+0.0614 /
+6.14 percentage points**. That figure is 0.8212 − 0.7598 and is **not**
the protocol-matched improvement.

Public name: External Reference Transfer (LightGBM). Internal ID:
`V2-B-REFONLY`. Details: `docs/versions/model_v2.md`.

---

# 4. V3 experimental evaluation

Canonical **analysis partition** for V3 comparisons is the same 5-fold file
as MODEL V2: `experiments/team_folds_5_seed42.csv`.

Authoritative program summary: `reports/v3/v3_research_program_summary.md`.

Semantics used across V3:

- **External-validation candidates.** M2 / S0 / SNI (and related students)
  are honest external-validation predictions unless a frozen report says
  otherwise. Competition labels do not enter those boosting or
  student-training objectives. That is not identical to conventional
  competition-label OOF.
- **Canonical fold slices.** Folds 0–2 versus folds 3–4 were used as a
  **retrospective stability partition**. Canonical folds 3–4 were viewed
  across V3 stages and are **not** a pristine untouched holdout.
- **Tune vs holdout.** YHH V7 was described in E00T as using folds 0–2 for
  gating and folds 3–4 as holdout, but cell-level OOF artifacts were
  unavailable; YHH was omitted from the honest oracle pool.
- **Native teammate protocols.** LZH Prior-H is a frozen comparator with a
  **native 3-fold** protocol. Cell-level predictions can still be used
  descriptively if each cell is genuinely held out of that 3-fold, but
  canonical folds 3–4 are not LZH’s original holdout. E00T recorded MEDIUM
  validation confidence because Prior-H was selected on the same OOF used
  for reporting.
- **Promotion restrictions.** E07D compared M2 with frozen MODEL V2 under
  criteria frozen before seeing the three-cell gain: paired McNemar,
  bootstrap intervals, section/class stability, and predeclared STRONG
  checks (`net_ge_25`, `acc_ge_0.8262`, `folds_3_4_net_positive`,
  `sni_capture_ge_15`). M2 (4109 / 5000 = 0.8218) failed those checks.
  MODEL V3 was not created.

Do not promote a V3 experimental number to a formal MODEL version without
meeting that evidence bar.

---

# 5. Diagnostic-only statistics

These quantities require true labels. They are **not** model accuracy.

## Diagnostic oracle coverage

Pairwise and multi-expert oracle coverage asks whether **at least one**
saved expert was already correct on a cell. Computing it uses ground truth
and a perfect selector. Examples:

- V2 three-expert (BRIDGE, V2-A, V2-B): 4414 / 5000 = 0.8828
- V3 two-expert (LZH Prior-H + WYH MODEL V2): 4215 / 5000 = 0.8430
- V3 five-expert (LZH + WYH + S0 + SNI + M2): 4393 / 5000 = 0.8786

**Diagnostic oracle coverage is a retrospective coverage ceiling requiring
ground-truth knowledge and is not deployable accuracy.**

Do not report 0.8786 or 0.8828 as OOF, test, leaderboard, or MODEL V3
accuracy.

## Complementarity

Exclusive-correct counts, all-model-wrong counts, and unique recoveries
(for example S0 96, SNI 53, M2 29) describe whether an expert answers cells
that others miss. They are coverage diagnostics, not standalone scores.

## Rescue / harm diagnostics

E03A confidence/margin/entropy rescue audits and E05A directional H1/H2/H3
patches measure whether complementary answers can be selected without
ground truth. Net-negative patches are evidence against deployable rules,
not a MODEL V3 score.

## McNemar / statistical comparisons

E07D used paired McNemar, cell-level bootstrap, and section-cluster
bootstrap on M2 versus MODEL V2. Those tests informed the promotion
decision. They do not create a new version identity.

---

# Metrics

| Role | Metric |
|---|---|
| **Primary, model selection** | overall accuracy (matches the hackathon) |
| Diagnostic | per-fold accuracy |
| Diagnostic | protocol-matched deltas (same fold file) |
| Diagnostic | macro-F1 |
| Diagnostic | per-class recall |
| Diagnostic only | oracle coverage, unique recoveries, rescue/harm nets |

Macro-F1 and per-class recall are reported so rare types are visible. They
are not the selection rule.

There is no automated “accept if the gain exceeds fold standard deviation”
gate. Formal version decisions are recorded in experiment registries and
frozen reports.
