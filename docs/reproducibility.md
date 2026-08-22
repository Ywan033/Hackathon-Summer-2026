# Reproducibility

This guide records commands that exist in the repository. Clean-clone full
reproduction has not yet been verified for the current archive state.

Pinned environment: **Python 3.9.x** and `requirements.txt`.

```text
pandas==2.3.3
numpy==2.0.2
scikit-learn==1.6.1
scipy==1.13.1
pyarrow==19.0.1
lightgbm==4.6.0
anndata==0.10.9
h5py==3.14.0
pytest==8.4.2
```

Scripts prepend `src/` to `sys.path`. There is no separate installable
package, and there is no one-command full reproduction.

Do not copy another machine’s `.venv`.

---

## Quick integrity check

Does **not** retrain models. Uses the official CSVs in `data/` (tracked in
Git).

```sh
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
.venv/bin/python scripts/10_official_manifest.py --verify
.venv/bin/python scripts/00_validate_data.py
.venv/bin/python scripts/90_validate_submission.py outputs/submissions/model_v1.csv
.venv/bin/python scripts/90_validate_submission.py outputs/submissions/model_v2_candidate.csv
.venv/bin/python -m pytest tests/test_data_contract.py tests/test_official_contract.py \
  tests/test_folds.py tests/test_submission_contract.py tests/test_model_v1.py \
  tests/test_model_v2_release.py tests/test_v3_research_program_closure.py -q
```

Expected: official manifest verification, data-contract OK, submission-contract
OK for the two personal candidates, and the listed tests passing.

`scripts/90_validate_submission.py` with no argument checks the organizer
example `prediction/prediction.csv`. That file is not a WYH model.

---

## Reproduction safety

Full retraining should be performed in a disposable clean clone or disposable
working copy, not directly in the archival checkout.

Some reproduction entry points protect frozen tracked artifacts and may
require `--overwrite` for an intentional rerun. Do not pass `--overwrite` in
the archival working tree.

Inspecting committed artifacts under `outputs/` does not require retraining.

---

## Full model reproduction

The commands below are source-supported entry points. They are **not** a
verified clean-clone procedure. Several of them write under `outputs/` and,
for some scripts, `experiments/`.

### Official data

Tracked files:

- `data/counts_train.csv`
- `data/counts_test.csv`
- `data/meta_train.csv`
- `data/meta_test.csv`

Hashes: `experiments/official_data_manifest.json`. Verify with
`scripts/10_official_manifest.py --verify`. Load Cell_ID as a string.

### MODEL V1 (selected-model reproduction)

Does **not** require the Zenodo `.h5ad`. `scripts/06_model_v1.py` is a
selected-model entry point for the frozen YW-004 full-train candidate. It
refuses to replace existing MODEL V1 artifacts unless `--overwrite` is
passed (artifact-protection).

```sh
.venv/bin/python scripts/10_official_manifest.py --verify
.venv/bin/python scripts/00_validate_data.py
.venv/bin/python scripts/02_sprint2_experiments.py
.venv/bin/python scripts/06_model_v1.py
.venv/bin/python scripts/90_validate_submission.py outputs/submissions/model_v1.csv
```

Expected selection number after a faithful OOF rerun: **0.7598**. Fold file
`experiments/folds.csv` is loaded, not regenerated.

Related selection-study scripts (not MODEL V1 itself):
`scripts/01_baseline.py` (YW-000/001),
`scripts/04_sprint3_experiments.py` (YW-005/006/007; not selected).

### Reproduce selected MODEL V2

Requires the approved external reference **locally**:

```text
work/external/MERFISH_spinal_cord_resolved_0718.h5ad
MD5  ce06f62c0ec4973581dae17bb76f0cd9
Zenodo record  18039571
```

The file is gitignored. Place it locally before any reference reproduction.

Checksum without rewriting frozen artifacts:

```sh
PYTHONPATH=src .venv/bin/python -c "from merfish60.reference import verify_reference_md5; print(verify_reference_md5())"
```

Selected-model entry point: `scripts/09_v2b_refonly.py`.

It fits LightGBM only on the 136,574 cleaned reference rows.
Competition-train labels are not the boosting target. Expected committed
result: **4106 / 5000 = 0.8212**. Fold file
`experiments/team_folds_5_seed42.csv` is loaded, not regenerated.

CLI notes from source inspection (no retraining performed here):

- `--overwrite` gates replacement of the `V2-B-REFONLY` row in
  `experiments/registry_v2.csv`.
- Metric, evaluation-label, and probability writers run **before** that
  registry check and are not equivalently gated. A no-flag rerun in the
  archival tree can therefore rewrite tracked V2-B files.
- `--audit-only` audits the reference but **writes** provenance JSON under
  `outputs/metrics/` and `experiments/V2-B-REFONLY_exclusion_manifest.json`.
  Do not use it as a read-only checksum step.

Do not run this script in the archival working tree.

```sh
.venv/bin/python scripts/09_v2b_refonly.py
.venv/bin/python scripts/90_validate_submission.py outputs/submissions/model_v2_candidate.csv
```

The candidate-validation command above inspects the committed file and does
not retrain.

### Reproduce MODEL V2 selection evidence

These scripts are **not** the selected MODEL V2 fit. They reconstruct the
protocol-matched bridge, the unselected spatial family, and the predeclared
V2-C blend audit.

| Script | Role | CLI / write behavior (source inspection) |
|---|---|---|
| `scripts/07_bridge_yw004_5f.py` | Selection-study: BRIDGE-YW004-5F | `--overwrite` gates the registry row only; output writers are not equivalently gated |
| `scripts/08_v2a_spatial_lgbm.py` | Selection-study: V2-A-SPATIAL-LGBM | same `--overwrite` / writer pattern as the bridge script |
| `scripts/11_v2c_select.py` | Selection-study: five predeclared blends from **saved** probabilities; no LightGBM fit | no `--overwrite` flag; writes selection artifacts including `outputs/submissions/model_v2_candidate.csv` |

Do not run these in the archival working tree.

---

## Expected committed outputs

| Kind | Path |
|---|---|
| MODEL V1 metrics | `outputs/metrics/model_v1_metrics.json` |
| MODEL V1 candidate | `outputs/submissions/model_v1.csv` |
| MODEL V2 metrics | `outputs/metrics/model_v2_metrics.json` |
| MODEL V2 evaluation labels | `outputs/oof/V2-B-REFONLY_oof.csv` (filename keeps historical `oof`) |
| MODEL V2 test probabilities | `outputs/probabilities/V2-B-REFONLY_test_probabilities_seg.csv.gz` |
| MODEL V2 candidate | `outputs/submissions/model_v2_candidate.csv` |
| V3 closure | `outputs/v3/v3_research_program_manifest.json` |

---

## Intentionally excluded from Git

Never commit:

- `*.h5ad`
- raw external datasets
- `work/external/` (including cache `*.npz`)
- `work/v3_e02d/`, `work/v3_e04s/`, `work/v3_e06m/`
- `.venv/`

Committed: provenance metadata, MD5 / exclusion manifests, alignment audits,
scripts, tests, and frozen numeric artifacts under `outputs/` that are not
raw counts.

---

## Required Inputs and Dependencies

| Item | In Git? | Required for |
|---|---|---|
| Official `data/*.csv` | Yes | All checks and all models |
| Zenodo `.h5ad` + matching MD5 | No | MODEL V2 / V3 reference work |
| SNI `.h5ad` + matching MD5 | No | V3-E04S / E06M |
| `lightgbm`, `anndata`, `h5py` | via `requirements.txt` | MODEL V2 / V3 reference work |
| `pyarrow` | via `requirements.txt` | V3-E00T parquet registry only |
| `scikit-learn` | via `requirements.txt` | MODEL V1 |

MODEL V1 reproduction needs scikit-learn and the official CSVs only.

V3 experiment scripts live under `experiments/v3/`. See
[`../reports/v3/v3_research_program_summary.md`](../reports/v3/v3_research_program_summary.md).
V3 candidates are experimental; they are not a formal MODEL version.

---

## Reserved for clean-clone verification

Not claimed here:

- a working one-command or disposable-clone command sequence;
- bit-for-bit agreement of a fresh `scripts/09_v2b_refonly.py` rerun with
  committed 0.8212 artifacts;
- runtime behavior of `07` / `08` / `09` when the registry row already
  exists (file writers are known from source; process-level outcome is not
  re-executed in this documentation pass).
