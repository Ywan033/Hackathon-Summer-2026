# Outputs

Semantics for committed artifacts under `outputs/`. Filenames are frozen
historical identifiers. Do not rename them to match later public wording.

`prediction/prediction.csv` is **not** in this directory and is **not**
WYH-owned. It is the organizer example submission.

## Categories

| Category | Directory | Meaning |
|---|---|---|
| Metrics | [`metrics/`](metrics/) | JSON/CSV scores, audits, selection records |
| Evaluation predictions | [`oof/`](oof/) | Per-cell labels on the 5000 competition-train cells |
| Probabilities | [`probabilities/`](probabilities/) | Class-probability matrices (train-eval and/or test) |
| Submission candidates | [`submissions/`](submissions/) | Personal test-set CSVs in official column format |
| V3 diagnostics | [`v3/`](v3/) | Experimental and diagnostic V3 artifacts |

The directory name `oof/` is historical. For MODEL V1 the tables **are**
conventional competition-label OOF. For MODEL V2 / V3 external-reference
candidates they are honest **external-reference validation** labels. The
filename `V2-B-REFONLY_oof.csv` keeps the frozen identifier.

## How to read an artifact

| Label | Meaning |
|---|---|
| **Canonical / frozen** | Release identity. Do not modify. MODEL V1, MODEL V2, fold files, and V3 stage outputs used as closure evidence. |
| **Experimental** | A scored candidate that is not a formal MODEL version (YW-00x non-selected runs, V2-A, V2-C, E06M M2). |
| **Diagnostic** | Requires ground truth and is not a deployable score (oracle coverage, unique recoveries, rescue/harm nets). |
| **Submission candidate** | Personal test predictions under `outputs/submissions/`. Not an official leaderboard file. |
| **Official submission** | Captain / team workflow. Not represented by a WYH-owned file in this personal repository. |

Diagnostic oracle values (for example 0.8828 or 0.8786) are coverage
ceilings. They are not model scores.

## Canonical / frozen (do not modify)

| Artifact | Identity |
|---|---|
| `metrics/model_v1_metrics.json` | MODEL V1 freeze (0.7598) |
| `submissions/model_v1.csv` | MODEL V1 submission candidate |
| `probabilities/model_v1_test_probabilities.csv.gz` | MODEL V1 test probabilities |
| `metrics/model_v2_metrics.json` | MODEL V2 freeze (0.8212, `V2-B-REFONLY`) |
| `metrics/model_v2_selection.json` | MODEL V2 selection record |
| `oof/V2-B-REFONLY_oof.csv` | MODEL V2 external-reference validation labels |
| `probabilities/V2-B-REFONLY_test_probabilities_seg.csv.gz` | Selected MODEL V2 test probabilities |
| `submissions/model_v2_candidate.csv` | MODEL V2 submission candidate |
| `v3/v3_research_program_manifest.json` | V3 closure: MODEL V3 not created |
| `v3/v3_e07d_decision.json` | Promotion rejection |

Supporting frozen V2 audits use the `V2-B-REFONLY_*` prefix
(`metrics/V2-B-REFONLY_metrics.json`, leakage / exclusion / provenance JSON).

## Experimental

Examples: `metrics/YW-000_metrics.json` … `YW-007_metrics.json`;
`metrics/V2-A-SPATIAL-LGBM_metrics.json`; `metrics/V2-C-scoreboard.json`;
`v3/v3_e06m_metrics.json` (M2 = 0.8218, not MODEL V3).

## Diagnostic

Examples: `metrics/V2-C-complementarity.json`;
`v3/v3_e00t_pairwise_oracle.csv`; `v3/v3_e06m_complementarity.json`;
three-expert / five-expert oracle fields inside frozen JSON. These ask
whether at least one expert was already correct. They are not OOF, test, or
leaderboard accuracy.

## Submission candidate vs official submission

| File | Kind |
|---|---|
| `outputs/submissions/model_v1.csv` | Personal MODEL V1 candidate |
| `outputs/submissions/model_v2_candidate.csv` | Personal MODEL V2 candidate |
| `prediction/prediction.csv` | Organizer example; **not** a WYH model |

A candidate is not an official leaderboard submission. No verified official
score is attributable specifically to MODEL V1 or MODEL V2 in this personal
repository.

## V3 experimental diagnostics

`outputs/v3/` holds per-stage metrics, complementarity tables, and the
closure package. Treat M2 scores as experimental. Treat oracle fields as
diagnostic. See [`reports/v3/v3_research_program_summary.md`](../reports/v3/v3_research_program_summary.md).

Complete prediction-artifact paths, dataset scope, shapes, SHA-256 values,
source commits, and metric applicability are indexed in
[`prediction_versions/`](../prediction_versions/README.md).
