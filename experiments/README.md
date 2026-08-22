# Experiments

Navigation for persisted folds, registries, and experiment scripts on
`ywan/ml-pipeline`. This is not a dump of every generated table.

**Formal frozen models** are MODEL V1 and MODEL V2. Everything else listed
below is a baseline, bridge, family, blend audit, or V3 experimental
candidate. MODEL V3 was not created.

Reports live under `reports/` (sprints), `reports/v3/` (V3), and
`docs/versions/` (model cards). Machine-readable outputs live under
`outputs/`.

## Canonical folds and registries

| File | Role |
|---|---|
| [`folds.csv`](folds.csv) | Frozen MODEL V1 3-fold assignments. `StratifiedKFold(n_splits=3, shuffle=True, random_state=20260819)`. Do not overwrite. |
| [`team_folds_5_seed42.csv`](team_folds_5_seed42.csv) | Frozen MODEL V2 / V3 evaluation partition. `StratifiedKFold(n_splits=5, shuffle=True, random_state=42)`. Do not overwrite. |
| [`official_data_manifest.json`](official_data_manifest.json) | SHA256 hashes of organizer `data/*.csv` |
| [`registry.csv`](registry.csv) | MODEL V1 line: YW-000 … YW-007 |
| [`registry_v2.csv`](registry_v2.csv) | V2 family: BRIDGE-YW004-5F, V2-A-SPATIAL-LGBM, V2-B-REFONLY |
| [`V2-B-REFONLY_exclusion_manifest.json`](V2-B-REFONLY_exclusion_manifest.json) | Reference ID / exact-vector exclusion counts |

Validation semantics: [`docs/validation_protocol.md`](../docs/validation_protocol.md).

## MODEL V1 experiments

Formal frozen model: **MODEL V1** (YW-004 Hierarchical Signature Specialists).

| ID | Role | Formal model? |
|---|---|---|
| YW-000 | Per-fold majority baseline | No |
| YW-001 | Gene-only L2 logistic baseline | No |
| YW-002 | Genes + metadata one-hot | No |
| YW-003 | Genes + candidate masking | No |
| **YW-004** | Per-signature specialists | **Yes — MODEL V1** |
| YW-005 | Exploratory non-nested blend | No (excluded) |
| YW-006 | Hard-bucket inner-CV ablation | No |
| YW-007 | Hybrid duplicate of YW-004 | No |

Scripts: `scripts/01_baseline.py`, `scripts/02_sprint2_experiments.py`,
`scripts/04_sprint3_experiments.py`, `scripts/06_model_v1.py`.
Card: [`docs/versions/model_v1.md`](../docs/versions/model_v1.md).

## MODEL V2 experiments

Formal frozen model: **MODEL V2** = internal ID **`V2-B-REFONLY`**.

| ID | Role | Formal model? |
|---|---|---|
| BRIDGE-YW004-5F | V1 architecture on the 5-fold file (0.7596) | No (bridge) |
| V2-A-SPATIAL-LGBM | Competition-only spatial LightGBM (0.7690) | No (family; not selected) |
| **V2-B-REFONLY** | External Reference Transfer (0.8212) | **Yes — MODEL V2** |
| V2-C C0–C4 | Five predeclared probability blends | No (C1 0.8224 rejected) |

Scripts: `scripts/07_bridge_yw004_5f.py`, `scripts/08_v2a_spatial_lgbm.py`,
`scripts/09_v2b_refonly.py`, `scripts/11_v2c_select.py`.
Card: [`docs/versions/model_v2.md`](../docs/versions/model_v2.md).

## V3 experiment program

Completed research program. **MODEL V3 was not created.**

Scripts: [`v3/`](v3/). Reports: [`reports/v3/`](../reports/v3/).
Summary: [`reports/v3/v3_research_program_summary.md`](../reports/v3/v3_research_program_summary.md).

| ID | Script | Formal model? |
|---|---|---|
| V3-E00T | `v3/v3_e00t_team_expert_audit.py` | No |
| V3-E02D | `v3/v3_e02d_privileged_gene_distillation.py` | No |
| V3-E03A | `v3/v3_e03a_rescue_audit.py` | No |
| V3-E04S | `v3/v3_e04s_sni_source_expert.py` | No |
| V3-E05A | `v3/v3_e05a_directional_complementarity_audit.py` | No |
| V3-E06M | `v3/v3_e06m_source_balanced_multireference.py` | No (best standalone M2 = 0.8218; not promoted) |
| V3-E07D | `v3/v3_e07d_final_deployable_decision_audit.py` | No (promotion rejected) |

Preserve these historical identifiers in filenames. Do not rename them.

## Historical / internal identifiers

Preserve exactly in artifacts, scripts, tests, and tags:

`YW-004`, `BRIDGE-YW004-5F`, `V2-A-SPATIAL-LGBM`, `V2-B-REFONLY`, `V2-C`,
`V3-E00T` … `V3-E07D`, `M2`, tags `model-v1` / `model-v2`.

Public MODEL V2 name is External Reference Transfer (LightGBM). The internal
frozen ID remains `V2-B-REFONLY`.
