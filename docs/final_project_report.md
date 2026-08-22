# Final Project Report

Repository: `Ywan033/Hackathon-Summer-2026`
Development source branch: `ywan/ml-pipeline`

This report covers the WYH personal modeling contribution. Team-final
integration, captain-submission provenance, and official leaderboard results
are outside its scope. Detailed experiment tables remain in the linked reports.

## 1. Problem and Scope

The task is 60-class cell-type annotation of MERFISH spinal-cord cells.
Official inputs are 5000 labeled training cells and 5000 hidden-label test
cells, each with 200 competition genes, spatial coordinates, and metadata.
The official metric is overall accuracy.

This repository records:

- a reproducible validation and data-contract layer;
- frozen MODEL V1 (Hierarchical Signature Specialists);
- frozen MODEL V2 (External Reference Transfer, LightGBM);
- a completed V3 research program that did **not** create MODEL V3.

It does not record the complete team solution, YHH or LZH models, integration
on the team repository’s `main` branch (`team/main`), or the team final
ensemble. `prediction/prediction.csv` is the organizer example submission and
is not a WYH model.

## 2. Data and Evaluation

Official files, hashes, and Cell_ID rules: [`data_provenance.md`](data_provenance.md),
[`data_audit.md`](data_audit.md). Cell_ID is a 19-digit string and must not be
cast through floating point.

There are **two** frozen evaluation contracts
([`validation_protocol.md`](validation_protocol.md)):

| Identity | File | What the number is |
|---|---|---|
| MODEL V1 | `experiments/folds.csv` | Conventional 3-fold **competition-label OOF** = 0.7598 |
| BRIDGE-YW004-5F | `experiments/team_folds_5_seed42.csv` | Same V1 architecture on the 5-fold partition = 0.7596 |
| MODEL V2 | `experiments/team_folds_5_seed42.csv` | **External-reference validation** = 0.8212 |
| V3 candidates | same 5-fold file unless a frozen report says otherwise | External-validation or analysis-only |

These numbers are not interchangeable OOF scores.

Diagnostic oracle coverage asks whether at least one saved expert was already
correct. It requires ground truth and a perfect selector. It is not deployable
accuracy.

## 3. Reproducible Baseline Infrastructure

Before modeling, the repository established join-safe official CSVs, lossless
Cell_ID loading, persisted fold files, a submission validator, and an official
SHA-256 manifest (`experiments/official_data_manifest.json`).

Entry points: `scripts/00_validate_data.py`, `scripts/10_official_manifest.py`,
`scripts/90_validate_submission.py`. Library: `src/merfish60/`. Tests:
`tests/test_data_contract.py`, `tests/test_official_contract.py`,
`tests/test_folds.py`, `tests/test_submission_contract.py`.

This layer is not a model score. It is the contract used by later experiments.

## 4. MODEL V1

Public name: Hierarchical Signature Specialists.
Architecture: frozen YW-004 full-train per-signature specialists.
Tag: `model-v1`.
Card: [`versions/model_v1.md`](versions/model_v1.md).

Routing uses `(Region, Excitatory_vs_Inhibitory, Segment)`. Single-class
signatures are deterministic. Ambiguous signatures use per-signature
multinomial L2 logistic regression on `log1p` of the official 200-gene counts.
Fallback is a global gene-only logistic regression.

Result: **3799 / 5000 = 0.7598 (75.98%)** under conventional 3-fold
competition-label OOF. Status: frozen historical baseline.

Sprint 1–3 experiments (YW-000…YW-007) are recorded in
`experiments/registry.csv`. YW-004 was selected; nested-invalid or
no-gain ablations were not promoted.

Remaining errors concentrate in the metadata-missing glial / non-neuronal
bucket (OOF accuracy 0.6815 there). Dominant confusions are oligodendrocyte
↔ progenitor pairs and astrocyte subtypes.

No verified official leaderboard score is attributable specifically to
MODEL V1 in this personal repository.

## 5. MODEL V2

Public name: External Reference Transfer (LightGBM).
Internal frozen ID: **`V2-B-REFONLY`**.
Tag: `model-v2`.
Card: [`versions/model_v2.md`](versions/model_v2.md).

LightGBM is fit **only** on 136,574 cleaned cells from the approved Zenodo
MERFISH spinal-cord deposit (record 18039571; MD5
`ce06f62c0ec4973581dae17bb76f0cd9`). Competition training labels are not the
boosting target. The 5000 competition-train cells are the validation set.

Result: **4106 / 5000 = 0.8212 (82.12%)**. Macro-F1 approximately 0.7936.
Fold accuracies: 0.822 / 0.831 / 0.812 / 0.821 / 0.820.
Hard metadata-missing bucket: 0.7569; neuron: 0.9198; glial / non-neuronal:
0.7629.

Protocol-matched improvement versus BRIDGE-YW004-5F (0.7596):

`0.8212 − 0.7596 = +0.0616` (**+6.16 percentage points / +308 correct cells**).

The +6.14 pp difference between MODEL V1 0.7598 and MODEL V2 0.8212 is a
cross-protocol descriptive comparison, not that protocol-matched delta.

A competition-only spatial family (V2-A-SPATIAL-LGBM, 0.7690) was accepted as
a family and not selected. A predeclared V2-C blend C1 (0.8224) was evaluated
and rejected (+6 net cells, mixed folds, folds 3–4 regression, lower
macro-F1). MODEL V2 remains the simpler V2-B architecture.

Underlying LightGBM defaults are documented as deriving from
`ext_refonly.py` on the team repository’s `main` branch (`team/main`). WYH
owns the personal freeze, external-reference evaluation contract, exclusion
reproduction, artifacts, reproducibility, and selection evidence—not
origination of those defaults.

No verified official leaderboard score is attributable specifically to
MODEL V2 in this personal repository.

## 6. V3 Research Program

Authoritative summary:
[`../reports/v3/v3_research_program_summary.md`](../reports/v3/v3_research_program_summary.md).
Contribution record:
[`contributions/wyh_v3_contribution.md`](contributions/wyh_v3_contribution.md).

Status: **completed**. MODEL V3 was **not created**. There is no public
modeling stage after V3.

The program asked whether complementary experts and additional external
sources could produce a stable standalone successor to MODEL V2 without
violating leakage or promotion discipline. Seven predeclared stages
(V3-E00T … V3-E07D) showed that complementary coverage is real, then that
converting that coverage into a robust deployable gain was the bottleneck.

Best standalone experimental candidate: **E06M M2 = 4109 / 5000 = 0.8218**.
Versus MODEL V2: net +3 cells / +0.06 percentage points.

Diagnostic oracle coverage (for example five-expert 4393 / 5000 = 0.8786) is
a retrospective ceiling. It is not OOF accuracy, not test accuracy, and not a
MODEL V3 score.

Negative results were retained: privileged-gene distillation did not beat the
200-gene student; confidence gating was unsafe; directional patches were
net-negative; naive multi-reference pooling regressed.

## 7. Model Selection and Promotion Decision

Formal version identity is tied to predeclared evidence, not to a higher
single validation number. The table below mixes native MODEL V1 3-fold
competition-label OOF with later external-validation scores. Those protocols
are not interchangeable.

| Candidate | Result | Evaluation | Decision |
|---|---:|---|---|
| MODEL V1 / YW-004 | 0.7598 | 3-fold competition-label OOF | Frozen historical baseline |
| V2-C C1 | 0.8224 | External-reference validation (5-fold partition) | Rejected |
| E06M M2 | 0.8218 | V3 external-validation protocol | Not promoted |
| MODEL V2 / V2-B-REFONLY | 0.8212 | External-reference validation (5-fold partition) | **Selected personal model** |

E07D compared M2 with frozen MODEL V2 under criteria frozen before seeing the
three-cell gain: paired McNemar p = 0.8895; bootstrap 95% CI includes 0;
canonical folds 3–4 net −9; STRONG checks (`net_ge_25`, `acc_ge_0.8262`,
`folds_3_4_net_positive`, `sni_capture_ge_15`) failed.

MODEL V2 remains the selected frozen WYH personal model. MODEL V3 was not
created.

## 8. Contribution and Team Boundaries

WYH-owned work documented in this repository: validation contracts; MODEL V1;
MODEL V2 personal freeze and evaluation; V3 E00T–E07D; leakage / exclusion
implementation; promotion evidence; personal development documentation on
`ywan/ml-pipeline`; and documented mature handoff of V1/V2 checkpoints toward
`team/wyh`.

Not WYH-owned: LZH Prior-H (`depth_masked_prior_h_anchor`); YHH models;
integration on the team repository’s `main` branch (`team/main`); the team
final ensemble; `prediction/prediction.csv`.

YHH cell-level OOF was recorded as unavailable during the V3 program. That
historical statement is not rewritten here.

Map: [`contributions/wyh_contribution_summary.md`](contributions/wyh_contribution_summary.md).

## 9. Reproducibility and Data Governance

Reproduction commands, required inputs, and current verification status:
[`reproducibility.md`](reproducibility.md).

Official CSVs are tracked and hashed. External `.h5ad` files are gitignored
and must be placed locally. MODEL V2 requires Zenodo 18039571 with MD5
`ce06f62c0ec4973581dae17bb76f0cd9`. SNI (`SNI_merged_0917.h5ad`) is a V3
source only.

## 10. Limitations

Supported by frozen reports:

- closely related cell-type / subtype confusion, especially oligodendrocyte,
  OPC, and astrocyte labels in the metadata-missing regime;
- MODEL V2 reliance on a matched same-study external reference;
- possible domain-shift sensitivity (documented on the V3 SNI source; not
  assumed absent for the Zenodo transfer either);
- V3 standalone gains too small and insufficiently stable for promotion;
- canonical folds 3–4 in V3 are a retrospective stability partition, not a
  pristine holdout;
- internal validation is distinct from an official hidden-test or
  leaderboard score.

Diagnostic oracle coverage is a retrospective ceiling, not a limitation of
a deployed classifier.

## 11. Final Project State

| Identity | Status | Result |
|---|---|---|
| MODEL V1 | Frozen historical baseline | 3799 / 5000 = 0.7598 |
| MODEL V2 | Frozen **selected WYH personal model** | 4106 / 5000 = 0.8212 |
| V3 program | Completed | Best experimental standalone 0.8218 |
| MODEL V3 | **Not created** | — |

**MODEL V2 is the selected frozen WYH personal model. No MODEL V3 was
created.**

No official competition result is asserted. No captain-submission provenance
for MODEL V1 or MODEL V2 is established in this personal repository.
