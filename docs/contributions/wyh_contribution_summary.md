# WYH Contribution Summary — MERFISH Hackathon Summer 2026

Evidence map for work owned on personal branch `ywan/ml-pipeline` in
`Ywan033/Hackathon-Summer-2026`.

This is an archival attribution document. It is not résumé copy, not a
leaderboard claim, and not a MODEL V3 freeze.

Oracle values below are diagnostic coverage ceilings. They are not
deployable model accuracy.

---

## Ownership boundary (read first)

**WYH-owned**

- validation contracts and Cell_ID integrity
- MODEL V1 / YW-004 implementation and freeze
- MODEL V2 personal freeze, evaluation contract, artifacts, and
  reproducibility
- external-reference exclusion / leakage safeguards as implemented here
- V3 E00T–E07D research and analysis
- SNI / M2 experimentation where evidenced on this branch
- contribution / reproducibility infrastructure
- personal-branch documentation

**Not WYH-owned**

- LZH Prior-H (`depth_masked_prior_h_anchor`)
- YHH models
- `team/main` integration and the `ext_refonly.py` modeling family
- team final ensemble
- `prediction/prediction.csv`

YHH cell-level OOF was recorded as unavailable during V3 (E00T). That
historical statement is not rewritten here.

---

## A. Reproducible validation / data-contract infrastructure

| Field | Evidence |
|---|---|
| Objective | Make official CSVs, Cell_IDs, folds, and submission files join-safe and leakage-safe |
| WYH-owned implementation | Official contract, lossless Cell_ID loaders, persisted fold files, submission validator, data audit |
| Code | `src/merfish60/io.py`, `src/merfish60/official_contract.py`, `src/merfish60/validate_submission.py`, `src/merfish60/cv.py`, `src/merfish60/team_cv.py`, `scripts/00_validate_data.py`, `scripts/10_official_manifest.py`, `scripts/90_validate_submission.py` |
| Artifacts / reports | `docs/data_audit.md`, `docs/data_provenance.md`, `docs/validation_protocol.md`, `experiments/official_data_manifest.json`, `experiments/folds.csv`, `experiments/team_folds_5_seed42.csv`, `tests/test_data_contract.py`, `tests/test_official_contract.py`, `tests/test_folds.py`, `tests/test_submission_contract.py` |
| Evaluation protocol | Not a model score. Contracts apply to all later experiments |
| Result | 5000/5000 train and test IDs; disjoint ID sets; Cell_ID kept as 19-digit string; official SHA256 manifest |
| Promotion / decision | Foundation for MODEL V1 and MODEL V2; never replaced |
| Ownership boundary | Organizer-provided CSVs and `prediction/prediction.csv` are not WYH models |

---

## B. MODEL V1 — Hierarchical Signature Specialists

| Field | Evidence |
|---|---|
| Objective | Competition-only 60-class classifier using official 200 genes and `(Region, E/I, Segment)` routing |
| Research question | Can fold-safe signature specialists beat gene-only logistic regression without leakage? |
| WYH-owned implementation | YW-000…YW-007 experiments; selected YW-004 full-train specialists |
| Code | `src/merfish60/model_v1.py`, `src/merfish60/signatures.py`, `src/merfish60/models.py`, `scripts/01_baseline.py`, `scripts/02_sprint2_experiments.py`, `scripts/04_sprint3_experiments.py`, `scripts/06_model_v1.py` |
| Artifacts / reports | `docs/versions/model_v1.md`, `experiments/registry.csv`, `outputs/metrics/YW-004_metrics.json`, `outputs/metrics/model_v1_metrics.json`, `outputs/submissions/model_v1.csv`, `reports/sprint2_comparison.md`, `reports/sprint3_comparison.md`, tag `model-v1` |
| Evaluation protocol | Conventional frozen 3-fold competition-label OOF (`experiments/folds.csv`; StratifiedKFold n=3, seed 20260819) |
| Result | **3799 / 5000 = 0.7598 (75.98%)** |
| Promotion / decision | Frozen historical baseline. YW-005 excluded (not nested). YW-006/007 did not replace YW-004 |
| Ownership boundary | Entire V1 line is WYH-owned. No verified official leaderboard score is attributable specifically to MODEL V1 in this personal repository |

---

## C. MODEL V2 — External Reference Transfer (LightGBM)

Internal frozen identifier: **V2-B-REFONLY**.

| Field | Evidence |
|---|---|
| Objective | Transfer labels from the approved same-study Zenodo MERFISH reference onto official 200-gene competition cells |
| Research question | Does an excluded, fold-safe external-reference LightGBM beat the V1 architecture under a common 5-fold partition? |
| WYH-owned implementation | Personal freeze of V2-B-REFONLY: independent exclusion reproduction, honest 5-fold evaluation contract (stricter than all-train-visible team eval), leakage audits, artifacts, C0 vs C1 selection, documentation, tag `model-v2` |
| Code | `scripts/09_v2b_refonly.py`, `src/merfish60/reference.py`, `src/merfish60/ext_universe.py`, `scripts/11_v2c_select.py`, `src/merfish60/v2c_blend.py` |
| Artifacts / reports | `docs/versions/model_v2.md`, `reports/V2-B-REFONLY_methodology.json`, `outputs/metrics/model_v2_metrics.json`, `outputs/metrics/V2-B-REFONLY_*.json`, `outputs/oof/V2-B-REFONLY_oof.csv`, `outputs/probabilities/V2-B-REFONLY_test_probabilities_seg.csv.gz`, `outputs/submissions/model_v2_candidate.csv`, `experiments/V2-B-REFONLY_exclusion_manifest.json` |
| Evaluation protocol | Honest **external-reference validation** on the 5000 competition-train cells. LightGBM fit only on 136,574 cleaned reference rows. `experiments/team_folds_5_seed42.csv` is the evaluation / robustness partition. **Not** conventional competition-label OOF |
| Result | **4106 / 5000 = 0.8212 (82.12%)** |
| Promotion / decision | Selected frozen WYH personal model. V2-C C1 (0.8224) rejected |
| Official score | No verified official leaderboard score is attributable specifically to MODEL V2 in this personal repository |

### Underlying component provenance vs WYH freeze

`reports/V2-B-REFONLY_methodology.json` records:

- `read_only_source`: `team/main via git show`
- scripts: `work/ext_refonly.py`, `work/prep_ext.py`, `work/common_ext.py`, `work/ext_post.py`, `work/build_reference_ids.py`
- LightGBM defaults from that family, with fixed 700 rounds as in `refonly_full` (no early stopping)

WYH does **not** claim to have originated the reference-LightGBM modeling
idea or those defaults.

WYH **does** own the personal MODEL V2 freeze: evaluation contract,
leakage-control implementation, exclusion reproduction (usable 136,574;
MD5 `ce06f62c0ec4973581dae17bb76f0cd9`), artifacts, reproducibility, and
the selection evidence that kept V2-B-REFONLY.

---

## D. Protocol-matched bridge evaluation

| Field | Evidence |
|---|---|
| Objective | Compare the frozen V1 architecture with MODEL V2 under one partition |
| WYH-owned implementation | BRIDGE-YW004-5F |
| Code | `scripts/07_bridge_yw004_5f.py`, `src/merfish60/yw004_cv.py` |
| Artifacts | `experiments/registry_v2.csv`; numbers in `docs/versions/model_v2.md` and `outputs/metrics/model_v2_metrics.json` (`delta_vs_bridge` = 0.0616) |
| Evaluation protocol | Same 5-fold file as MODEL V2: `experiments/team_folds_5_seed42.csv` |
| Result | **BRIDGE-YW004-5F = 0.7596** |
| Promotion / decision | Bridge only; not a new MODEL version |
| Ownership boundary | Re-evaluation of WYH MODEL V1 architecture; not a teammate model |

Protocol-matched improvement:

`0.8212 − 0.7596 = +0.0616` (**+6.16 pp**)

The descriptive **cross-protocol** difference between MODEL V1 3-fold OOF
0.7598 and MODEL V2 external-reference validation 0.8212 is **+0.0614 /
+6.14 pp**. That figure is not the protocol-matched improvement.

Related 5-fold family on the same file: V2-A-SPATIAL-LGBM = 0.7690
(`scripts/08_v2a_spatial_lgbm.py`). Accepted as a spatial family; not
selected as MODEL V2.

---

## E. V3 multi-reference research

| Field | Evidence |
|---|---|
| Objective | Test whether a methodologically distinct path could replace MODEL V2 without violating leakage or promotion discipline |
| Research question | Can source-diverse external references (privileged 500-gene signal; SNI) be internalized into a stable standalone successor? |
| WYH-owned implementation | V3-E02D, V3-E04S, V3-E06M, V3-E07D on `ywan/ml-pipeline` |
| Code | `experiments/v3/v3_e02d_privileged_gene_distillation.py`, `experiments/v3/v3_e04s_sni_source_expert.py`, `experiments/v3/v3_e06m_source_balanced_multireference.py`, `experiments/v3/v3_e07d_final_deployable_decision_audit.py` |
| Artifacts / reports | `reports/v3/v3_research_program_summary.md`, corresponding `reports/v3/v3_e*.md`, `outputs/v3/`, `docs/contributions/wyh_v3_contribution.md` |
| Evaluation protocol | Same 5-fold partition as MODEL V2. M2 / S0 / SNI are honest external-validation candidates unless a frozen report says otherwise. Canonical folds 3–4 are a retrospective stability partition, not a pristine holdout |
| Result | Best standalone experimental candidate **M2 = 4109 / 5000 = 0.8218**. Naive pool M1 = 0.8172. SNI standalone = 0.5682 |
| Promotion / decision | **MODEL V3 PROMOTION NOT JUSTIFIED.** Net +3 vs MODEL V2; McNemar p = 0.8895; folds 3–4 net −9; bootstrap 95% CI includes 0; STRONG checks failed. MODEL V2 remains selected. MODEL V3 was not created |

There is no public modeling stage after V3.

---

## F. Complementarity / oracle diagnostics (V3)

| Field | Evidence |
|---|---|
| Objective | Measure whether auditable experts already cover different errors |
| Research question | Is routing-only over the current honest pool sufficient, or is independent information required? |
| WYH-owned implementation | V3-E00T, V3-E03A, V3-E05A analyses (no new deployable model) |
| Code | `experiments/v3/v3_e00t_team_expert_audit.py`, `experiments/v3/v3_e03a_rescue_audit.py`, `experiments/v3/v3_e05a_directional_complementarity_audit.py` |
| Artifacts / reports | `reports/v3/v3_e00t_team_expert_audit.md`, `reports/v3/v3_e03a_rescue_audit.md`, `reports/v3/v3_e05a_directional_complementarity_audit.md`, `outputs/v3/v3_research_program_metrics.csv` |
| Evaluation protocol | Cell_ID-aligned comparison on the 5000 competition-train cells; canonical 5-fold slices where used. LZH Prior-H native protocol is 3-fold |
| Result | Two-expert diagnostic oracle 4215 / 5000 = 0.8430; S0 unique recoveries 96; SNI unique recoveries 53; four-expert 4364 / 5000 = 0.8728; M2 unique recoveries 29; five-expert 4393 / 5000 = 0.8786. Earlier V2 three-expert diagnostic oracle 4414 / 5000 = 0.8828 |
| Promotion / decision | Coverage is real; conversion into deployable accuracy failed under tested gates and patches. Oracles were not used as model scores |
| Ownership boundary | LZH Prior-H is a frozen teammate comparator (0.8266), not WYH-owned. YHH omitted in V3 for missing cell-level OOF |

**Diagnostic oracle coverage is a retrospective coverage ceiling requiring
ground-truth knowledge and is not deployable accuracy.**

---

## G. Model-promotion discipline and negative results

| Field | Evidence |
|---|---|
| Objective | Keep version identity tied to predeclared evidence, including failures |
| WYH-owned implementation | V2-C C1 rejection; V3 STRONG / PROMISING / rejection labels that were not lowered after seeing results |
| Artifacts / reports | `outputs/metrics/model_v2_metrics.json` (`v2c_rejection`); `outputs/v3/v3_e07d_decision.json`; `reports/v3/v3_research_program_summary.md` §8 and §10 |
| Evaluation protocol | Predeclared blends only for V2-C. Predeclared STRONG checks for M2 (`net_ge_25`, `acc_ge_0.8262`, `folds_3_4_net_positive`, `sni_capture_ge_15`) plus McNemar / bootstrap |
| Result | C1 0.8224 rejected. Privileged-gene KD did not beat S0. S0 confidence gating unsafe. H1/H2/H3 nets −30 / −8 / −14. M2 not promoted |
| Promotion / decision | MODEL V2 remains selected. MODEL V3 not created. Negative results retained |
| Ownership boundary | Discipline is WYH process. It does not transfer teammate model ownership |

---

## H. Leakage controls and data provenance

| Field | Evidence |
|---|---|
| Objective | Keep hidden test labels unused; keep competition IDs and exact-vector duplicates out of external training |
| WYH-owned implementation | Official manifest verification; Zenodo MD5 gate; ID and exact-vector exclusion; SNI overlap audits in E04S |
| Code | `src/merfish60/reference.py`, `src/merfish60/official_contract.py`, E04S / E06M dataset audits |
| Artifacts / reports | `docs/data_provenance.md`, `experiments/V2-B-REFONLY_exclusion_manifest.json`, `outputs/metrics/V2-B-REFONLY_leakage_audit.json`, `outputs/v3/v3_e04s_dataset_audit.json` |
| Evaluation protocol | Applies to every scored experiment |
| Result | Zenodo usable 136,574 after removing 5000 train IDs, 5000 test IDs, and 47 exact 200-gene duplicates. SNI usable 55,193 after documented exclusions. `prediction/prediction.csv` unmodified by personal modeling |
| Promotion / decision | Leakage failures are disqualifying; none of the frozen personal versions rely on test labels |
| Ownership boundary | Organizer data and Zenodo record identity are external. WYH owns the local exclusion / audit implementation |

---

## I. Mature handoff to `team/wyh`

| Field | Evidence |
|---|---|
| Objective | Deliver mature WYH checkpoints to the team collaboration branch without making `team/wyh` the daily research line |
| WYH-owned implementation | Personal source of truth remains `ywan/ml-pipeline`. Contract: only mature validated checkpoints are for `team/wyh` (`AGENTS.md` repository-role rules) |
| Supporting evidence on this branch | V3-E00T recorded `team/wyh` at `da127c6` (`docs(wyh): document Model V1 and Model V2 releases`) as mature WYH delivery and used the personal frozen V2 artifacts instead of treating that branch as the modeling source |
| Evaluation protocol | Not a model score |
| Result | Remote `team/wyh` exists as the documented team delivery branch. MODEL V1 and MODEL V2 personal artifacts remain on `ywan/ml-pipeline` |
| Promotion / decision | Handoff is delivery of mature V1/V2 checkpoints, not transfer of teammate-model ownership |
| Ownership boundary | This document does **not** claim that every V3 experiment was merged to `team/wyh`. Commits on other local/remote branches are not treated as ancestors of this HEAD unless they are |

Do not describe `team/wyh`, `team/main`, or `prediction/prediction.csv` as
the WYH personal model.

---

## Numbers that must not be restated as personal accuracy

| Number | Allowed description |
|---|---|
| 0.7598 | MODEL V1 3-fold competition-label OOF |
| 0.7596 | BRIDGE-YW004-5F protocol-matched V1 architecture |
| 0.8212 | MODEL V2 external-reference validation |
| 0.8218 | Experimental M2; not MODEL V3 |
| 0.8266 | LZH Prior-H; not WYH-owned |
| 0.8828 / 0.8786 | Diagnostic oracle coverage only |

No verified official leaderboard score is attributable specifically to
MODEL V1 or MODEL V2 in this personal repository. Do not invent
captain-submission provenance.
