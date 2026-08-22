# MODEL V2 — External Reference Transfer (LightGBM)

Internal frozen identifier: **V2-B-REFONLY**. Preserve that string in artifacts, scripts, tests, and tags.

This document describes the frozen selected WYH personal model. It is not an official captain-repository submission.

## 1. Status

| Item | Value |
|---|---|
| Identity | Frozen selected WYH personal model |
| Public name | External Reference Transfer (LightGBM) |
| Internal frozen ID | V2-B-REFONLY / C0 |
| External labels | Approved Zenodo MERFISH spinal-cord deposit (cleaned) |
| Evaluation | Honest external-reference validation on the 5000 competition-train cells |
| Evaluation partition | `experiments/team_folds_5_seed42.csv` |
| Primary accuracy | **0.8212** (4106 / 5000 = 82.12%) |
| Macro-F1 | 0.7936 |
| Official hidden-test score | No verified official hidden-test score is attributable specifically to MODEL V2 in this personal repository |
| Official leaderboard score | **Not submitted.** No verified official leaderboard score is attributable specifically to MODEL V2 in this personal repository |
| Submission candidate | `outputs/submissions/model_v2_candidate.csv` |
| Tag | `model-v2` |

MODEL V2 is the selected architecture. It does not blend MODEL V1, V2-A, or V2-B probabilities. LightGBM is fit **only on the 136,574 cleaned reference rows**. Competition training labels are **not** the boosting target. The recorded **0.8212** figure is honest external-reference validation on the 5000 competition-train cells under the documented team-compatible partition. It is **not** conventional competition-label OOF.

No verified official leaderboard score is attributable specifically to MODEL V2 in this personal repository. Do not invent captain-submission provenance.

The candidate file has exactly 5000 test predictions in `meta_test.csv` order, uses the official columns `Cell_ID,MERFISH_cell_type_annotation.y`, and is produced only from `outputs/probabilities/V2-B-REFONLY_test_probabilities_seg.csv.gz`.

## 2. Problem and motivation

University of Rochester Biomedical Data Science Hackathon Summer 2026: 60-class MERFISH cell-type classification. Official metric: overall accuracy.

MODEL V1 (Hierarchical Signature Specialists) reached **0.7598** on the frozen personal 3-fold competition-label OOF protocol (3799 / 5000). Remaining errors concentrated in the large metadata-missing / glial-non-neuronal regime: 2958 / 5000 training cells lack `(Region, E/I, Segment)`, and most V1 mistakes sit in that bucket. Closely related oligodendrocyte / OPC / astrocyte subtypes are the dominant confusions.

MODEL V2 therefore added two families in order:

1. **Spatial modeling (V2-A)** on competition train+test features only, using neighbor graphs and fold-safe neighbor-label histograms.
2. **Same-study external reference (V2-B)** after organizers permitted the public Zenodo deposit. Reference cells supply additional labeled examples of the same 60 classes without using hidden test labels.

## 3. Model evolution

| Model | Method | Protocol | Accuracy | Decision |
|---|---|---|---:|---|
| MODEL V1 / YW-004 | Hierarchical Signature Specialists | personal 3-fold competition-label OOF | 0.7598 | Frozen historical baseline |
| BRIDGE-YW004-5F | Same V1 architecture on the team 5-fold partition | team 5-fold partition | 0.7596 | Bridge, not a new version |
| V2-A Spatial LightGBM + E/I | Competition-only spatial LightGBM | team 5-fold partition | 0.7690 | Accepted spatial family; not retuned |
| **V2-B-REFONLY** | **LightGBM fit on cleaned Zenodo reference** | **external-reference validation on the 5-fold partition** | **0.8212** | **Selected MODEL V2** |
| V2-C best fixed blend C1 | 0.25 V2-A + 0.75 V2-B | team 5-fold partition | 0.8224 | **Rejected** |

C1 was rejected despite a slightly higher aggregate score:

- net gain versus V2-B was only **+6 cells** (+0.12 pp);
- fold behavior was MIXED: folds 0–2 improved, folds 3–4 **regressed** (−0.40 / −0.30 pp);
- macro-F1 **decreased** (0.7936 → 0.7906);
- 88 predictions changed to harvest those 6 net cells;
- the extra spatial-blend complexity is not justified by a robust, fold-stable gain.

Equal three-expert averaging (C4) was worse than V2-B (0.8132). No other predeclared blend was eligible. The simpler V2-B-REFONLY architecture is therefore MODEL V2.

## 4. External reference provenance

| Item | Value |
|---|---|
| Source | Zenodo record **18039571** (Wang … Meltzer, same MERFISH spinal-cord study) |
| File | `work/external/MERFISH_spinal_cord_resolved_0718.h5ad` |
| MD5 | `ce06f62c0ec4973581dae17bb76f0cd9` |
| Raw shape | 146,621 cells × 500 genes |
| Label column | `obs["MERFISH cell type annotation"]` |

Independent exclusion (same-team policy, reproduced locally):

- remove 5,000 competition train Cell_IDs;
- remove 5,000 competition test Cell_IDs;
- align the 200 competition genes in exact official column order;
- remove 47 rows whose aligned 200-gene count vector exactly duplicates a competition train or test cell.

**Final usable reference: 136,574 cells.** All 200 competition genes are present and ordered as in `counts_train.csv`. After normalizing spaces/hyphens to underscores, all 60 official classes are covered; no unmapped labels remain in the usable set.

The raw `.h5ad` lives under `work/external/`, which is **gitignored**. It is not distributed in Git. Reproduction requires placing that file locally and verifying the MD5 above.

## 5. MODEL V2 architecture

MODEL V2 is V2-B-REFONLY: a single LightGBM multiclass model **fit only on the 136,574 usable reference rows**. Competition training labels are never the boosting target.

### Underlying modeling-component provenance

The LightGBM specification / defaults, extended-universe feature layout, and related reference-prep scripts are documented as deriving from the `team/main` `ext_refonly.py` family (`reports/V2-B-REFONLY_methodology.json`: `read_only_source` = `team/main via git show`; scripts `work/ext_refonly.py`, `work/prep_ext.py`, `work/common_ext.py`, `work/ext_post.py`, `work/build_reference_ids.py`).

WYH does **not** claim to have originated that reference-LightGBM modeling idea or those defaults.

WYH **does** own the personal MODEL V2 freeze: the honest fold-safe external-reference evaluation contract, leakage-control implementation, independent exclusion reproduction, artifacts, reproducibility, and the model-selection evidence that kept V2-B-REFONLY over the rejected C1 blend.

### Features

Features follow the same-team extended-universe layout on all 146,621 deposit cells (graphs need the full sections):

- library-size log1p of the 200 competition genes (scale = median total of the 10,000 competition cells);
- PCA50 of those genes (`random_state=0`);
- within-`Section ID` spatial kNN and PCA50 expression kNN;
- mean PCA50 of 10 spatial neighbors;
- metadata: log total, log volume, density, coordinates, Region, E/I, Segment (Laminae recoded 1:1 on train), AP, gender, mouse, dataset;
- fold-safe neighbor-label histograms (15 spatial / 25 expression labeled neighbors, weights `1/(1+d)`).

LightGBM specification (from `team/main` `ext_refonly.py` defaults, with **fixed 700 rounds** as in `refonly_full`; no early stopping):

```text
objective=multiclass, num_class=60
learning_rate=0.05
num_leaves=127
min_data_in_leaf=30
feature_fraction=0.5
bagging_fraction=0.8
bagging_freq=1
lambda_l2=1.0
max_bin=127
seed=0
num_boost_round=700
```

Post-processing evaluated in order: raw probabilities, then E/I constraint, then Segment mask (Laminae→Segment 1:1 on TRAIN). On this run, raw = +E/I = +Segment = **0.8212** (Segment changed no argmax). The released test probabilities are the Segment-masked full-reference predictions.

MODEL V2 does **not** use V2-A or BRIDGE probabilities, class-weight search, pseudo-labels, or early stopping on competition loss.

## 6. Validation protocol

All MODEL V2 numbers use the team-compatible 5-fold file `experiments/team_folds_5_seed42.csv` as the common **evaluation / robustness partition**:

```text
StratifiedKFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)
```

When predicting competition fold *f*:

- fold-*f* competition labels are invisible in model fitting, neighbor-label histograms, routing, masks, feature selection, and post-process selection;
- reference labels remain visible as permitted external labeled data;
- other training-fold labels may enter neighbor histograms only;
- hidden test labels are never read.

This is honest **external-reference validation** of a classifier fit on cleaned Zenodo rows, scored on the 5000 competition-train cells. It is **not** conventional competition-label OOF: competition labels are not the LightGBM fitting target.

This contract is stricter than the same-team `ext_refonly.py` all-train-visible evaluation. The 0.8212 figure is slightly pessimistic relative to test-time histograms (all 5000 train labels visible in those histograms).

The protocol-matched V1 architecture on the same 5-fold file is **BRIDGE-YW004-5F = 0.7596**. The directly protocol-matched improvement is:

**0.8212 − 0.7596 = +0.0616 (+6.16 percentage points).**

The descriptive **cross-protocol** difference between MODEL V1 3-fold OOF 0.7598 and MODEL V2 external-reference validation 0.8212 is **+0.0614 / +6.14 percentage points**. That +6.14 pp figure is not the protocol-matched improvement.

## 7. Final results

**External-reference validation accuracy = 82.12% = 4106 / 5000.** Macro-F1 = 0.7936.

| Fold | Accuracy |
|---|---:|
| 0 | 0.822 |
| 1 | 0.831 |
| 2 | 0.812 |
| 3 | 0.821 |
| 4 | 0.820 |
| Overall | **0.8212** |

| Slice | n | Accuracy |
|---|---:|---:|
| Hard metadata-missing bucket | 2958 | 0.7569 |
| Neuron | 1858 | 0.9198 |
| Glial / non-neuronal | 3142 | 0.7629 |

Deltas:

- **+0.0616** vs BRIDGE-YW004-5F (0.7596) — **protocol-matched**
- **+0.0522** vs V2-A +E/I (0.7690) — same 5-fold partition
- +0.0614 / +6.14 pp vs MODEL V1 3-fold OOF (0.7598) — descriptive **cross-protocol** difference between 0.7598 and 0.8212; not the protocol-matched improvement

## 8. Error analysis

Remaining errors are still concentrated among closely related glial subtypes. Top confusion pairs on the 5000 competition-train evaluation cells:

- `oligodendrocyte_1` → `oligodendrocyte_progenitor_2` (98)
- `oligodendrocyte_progenitor_2` → `oligodendrocyte_1` (55)
- `oligodendrocyte_progenitor_2` → `oligodendrocyte_2` (40)
- `oligodendrocyte_2` → `oligodendrocyte_progenitor_2` (36)
- `astrocyte_1` → `oligodendrocyte_progenitor_2` (31)
- `endothelial` → `astrocyte_1` (30)
- `meninges_2` → `meninges_1` (21)

No new specialists are introduced in MODEL V2.

## 9. Expert complementarity (diagnostic only)

The following is **diagnostic only**. It is not a deployable model score and was not used to select MODEL V2. Diagnostic oracle coverage is a retrospective coverage ceiling requiring ground-truth knowledge. It is not deployable accuracy.

Three saved experts (BRIDGE, V2-A +E/I, V2-B) were compared by argmax:

| Statistic | Count | Rate |
|---|---:|---:|
| Three-expert diagnostic oracle (at least one expert correct) | 4414 / 5000 | **0.8828** |
| MODEL V2 / V2-B correct | 4106 / 5000 | 0.8212 |
| Oracle headroom | 308 cells | — |
| Only BRIDGE correct | 111 | — |
| Only V2-A correct | 90 | — |
| Only V2-B correct | 241 | — |
| All three wrong | 586 | — |

Fixed convex blends (C0–C4 only; no weight search) did not convert that headroom into a stable gain. That diagnostic later motivated the V3 research program. The V3 program completed; MODEL V3 was **not created**. No V3 score is claimed.

## 10. Leakage / overfitting controls

- Independent exclusion of all competition IDs and exact 200-gene duplicates from the reference.
- Hidden test target never used.
- Frozen team 5-fold file; personal 3-fold `experiments/folds.csv` untouched.
- No lucky-seed search; LightGBM seed 0; 700 fixed rounds.
- V2-C evaluated only five predeclared blends; no grid, scipy, stacking, or post-hoc retuning.
- C1 rejected despite a slightly higher aggregate score.
- MODEL V1 sources, tag, and candidate immutable.
- Official data manifest verified.

## 11. Reproduction

Place the approved deposit at `work/external/MERFISH_spinal_cord_resolved_0718.h5ad` and confirm MD5 `ce06f62c0ec4973581dae17bb76f0cd9`. Do not commit that file.

```bash
# BRIDGE (YW-004 on team 5-fold partition)
.venv/bin/python scripts/07_bridge_yw004_5f.py

# V2-A competition-only spatial LightGBM
.venv/bin/python scripts/08_v2a_spatial_lgbm.py

# V2-B-REFONLY (requires local h5ad)
.venv/bin/python scripts/09_v2b_refonly.py

# V2-C fixed-blend evaluation (saved probabilities only; no retraining)
.venv/bin/python scripts/11_v2c_select.py

.venv/bin/pytest -q tests/
.venv/bin/python scripts/90_validate_submission.py outputs/submissions/model_v2_candidate.csv
.venv/bin/python scripts/10_official_manifest.py --verify
```

## 12. Repository artifacts

| Kind | Path |
|---|---|
| V2-B training | `scripts/09_v2b_refonly.py`, `src/merfish60/reference.py`, `src/merfish60/ext_universe.py` |
| V2-C blend (no fit) | `scripts/11_v2c_select.py`, `src/merfish60/v2c_blend.py` |
| Folds | `experiments/team_folds_5_seed42.csv` |
| Registry | `experiments/registry_v2.csv` |
| V2-B metrics / audits | `outputs/metrics/V2-B-REFONLY_*.json`, `reports/V2-B-REFONLY_methodology.json` |
| V2-C scoreboard | `outputs/metrics/V2-C-scoreboard.json`, `outputs/metrics/V2-C-complementarity.json` |
| Competition-train evaluation labels | `outputs/oof/V2-B-REFONLY_oof.csv` (filename retains historical `oof` identifier) |
| Selected test probabilities | `outputs/probabilities/V2-B-REFONLY_test_probabilities_seg.csv.gz` |
| Candidate | `outputs/submissions/model_v2_candidate.csv` |
| Release metrics | `outputs/metrics/model_v2_metrics.json` |

## 13. Limitations

External Reference Transfer (LightGBM) still confuses closely related glial subtypes. Static fixed blending of hierarchical, spatial, and reference experts did not provide a robust gain. The three-expert diagnostic oracle (0.8828) is coverage headroom only.

The later V3 research program tested complementary-expert utilization, including source-diverse references, and did not promote a successor. MODEL V2 remains the frozen selected WYH personal model. No future accuracy is claimed. No verified official leaderboard score is attributable specifically to MODEL V2 in this personal repository.
