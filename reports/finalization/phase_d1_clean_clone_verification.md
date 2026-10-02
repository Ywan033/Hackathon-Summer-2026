# Phase D1 clean-clone archive verification

Date: 2026-08-22
Verification path: `/Users/yyl/Documents/Hackathon-Summer-2026-phase-d-89f9ef2`
Source repository: `Ywan033/Hackathon-Summer-2026`
Source local path: `/Users/yyl/Documents/Hackathon-Summer-2026`
Source branch: `ywan/ml-pipeline`
Source HEAD: `89f9ef26d56c0febfef4c66f7ac3fea1d0d23f17` (`89f9ef2`)
Mode: isolated disposable-clone verification. Source repository not modified.
Verification-time publication state: this report had not yet been committed or
pushed. No MODEL V1 / MODEL V2 retraining was performed.

**D1 STATUS: PASS**

---

## 0. Source-repository gate

Checked in `/Users/yyl/Documents/Hackathon-Summer-2026` before any clone work:

| Check | Result |
|---|---|
| `pwd` | `/Users/yyl/Documents/Hackathon-Summer-2026` |
| `git branch --show-current` | `ywan/ml-pipeline` |
| `git rev-parse HEAD` | `89f9ef26d56c0febfef4c66f7ac3fea1d0d23f17` |
| `git status --short` | empty (clean) |

Gate passed. Preferred verification path did not already exist.

---

## 1. Isolated disposable clone

Command:

```text
git clone --no-local \
  /Users/yyl/Documents/Hackathon-Summer-2026 \
  /Users/yyl/Documents/Hackathon-Summer-2026-phase-d-89f9ef2
```

Cloned from the local source repository, not GitHub/origin.

| Check | Result |
|---|---|
| Verification path | `/Users/yyl/Documents/Hackathon-Summer-2026-phase-d-89f9ef2` |
| `git checkout ywan/ml-pipeline` | Already on `ywan/ml-pipeline` after clone |
| `git rev-parse HEAD` | `89f9ef26d56c0febfef4c66f7ac3fea1d0d23f17` |
| `git branch --show-current` | `ywan/ml-pipeline` |
| `git status --short` | empty (clean) |
| `git remote -v` | `origin` → `/Users/yyl/Documents/Hackathon-Summer-2026` (fetch and push) |

The disposable clone’s `origin` pointing at the local source path is acceptable
for this verification.

---

## 2. Repository content

All expected public/archive files are present:

- `README.md`
- `CHANGELOG.md`
- `AGENTS.md`
- `requirements.txt`
- `docs/README.md`
- `docs/final_project_report.md`
- `docs/reproducibility.md`
- `docs/data_provenance.md`
- `docs/validation_protocol.md`
- `docs/contributions/wyh_contribution_summary.md`
- `docs/versions/model_v1.md`
- `docs/versions/model_v2.md`
- `experiments/README.md`
- `outputs/README.md`
- `reports/finalization/repository_finalization_audit.md`
- `reports/finalization/phase_b_consistency_review.md`
- `reports/finalization/phase_c_public_presentation_review.md`

| Check | Result |
|---|---|
| `docs/versions/model_v3.md` | absent (expected) |
| tag `model-v1` | present |
| tag `model-v2` | present |
| tag `model-v3` | absent (expected) |

---

## 3. Python interpreter

Inspected without installing system software:

| Interpreter | Result |
|---|---|
| `python3 --version` | Python 3.9.6 (`/usr/bin/python3`) |
| `python3.9 --version` | command not found |
| source `.venv/bin/python --version` | Python 3.9.6 (inspected only; not copied) |

Documented target is Python 3.9.x. `/usr/bin/python3` is macOS system Python
3.9.6 and was used to create the clone venv. No Homebrew or system-interpreter
install was performed. Source `.venv` was not copied.

OS/platform:

- Darwin 24.6.0, arm64 (Apple T6020)
- macOS 15.7.3 (Build 24G419)
- hostname: Ywan-MacBook-Pro.local

---

## 4. Dependencies

Inside the disposable clone only:

```text
/usr/bin/python3 -m venv .venv
.venv/bin/python -m pip install --upgrade pip
.venv/bin/pip install -r requirements.txt
```

`requirements.txt` was not modified.

| Item | Result |
|---|---|
| `.venv/bin/python --version` | Python 3.9.6 |
| pip upgrade | pip 21.2.4 → 26.0.1 |
| `pip install -r requirements.txt` | success (exit 0) |

`.venv/bin/pip freeze`:

```text
anndata==0.10.9
array_api_compat==1.11.2
exceptiongroup==1.3.1
h5py==3.14.0
iniconfig==2.1.0
joblib==1.5.3
lightgbm==4.6.0
natsort==8.4.0
numpy==2.0.2
packaging==26.3
pandas==2.3.3
pluggy==1.6.0
pyarrow==19.0.1
Pygments==2.21.0
pytest==8.4.2
python-dateutil==2.9.0.post0
pytz==2026.3.post1
scikit-learn==1.6.1
scipy==1.13.1
six==1.17.0
threadpoolctl==3.6.0
tomli==2.4.1
typing_extensions==4.16.0
tzdata==2026.3
```

Pinned packages from `requirements.txt` all installed at the requested
versions. Unpinned transitive versions are as resolved by pip on this
machine.

---

## 5. Quick archive integrity check

All commands run from the disposable clone with `.venv/bin/python`.

| Command | Result |
|---|---|
| `scripts/10_official_manifest.py --verify` | `OFFICIAL MANIFEST OK` (exit 0) |
| `scripts/00_validate_data.py` | `DATA CONTRACT OK` (exit 0) |
| `scripts/90_validate_submission.py outputs/submissions/model_v1.csv` | `SUBMISSION CONTRACT OK` (exit 0) |
| `scripts/90_validate_submission.py outputs/submissions/model_v2_candidate.csv` | `SUBMISSION CONTRACT OK` (exit 0) |

Official files verified:

- `data/counts_train.csv` sha256=`1e14658be651060e97fa925daefccdc205b7e7fb9a989b2d3c02c244a5e035a9`
- `data/counts_test.csv` sha256=`a0d94c4f1b502aa9bef91eab198d042c39d79eef2019b744aecdb31b37c4f501`
- `data/meta_train.csv` sha256=`8a49896b4afe38aa7521b5712f6b38c34497be073820a01755adde67755c436a`
- `data/meta_test.csv` sha256=`ffd02a7e3cb412ee2d1fe62e3e9f64c3fbf3eba9f49e5ff42633b2e53757f545`

Data contract: 5000×200 counts, 5000×12 meta, 200 identical gene columns, 60
train classes, hidden test target, 19-digit string Cell_IDs, train/test
Cell_IDs disjoint.

Both personal candidates: UTF-8 CSV, header
`['Cell_ID', 'MERFISH_cell_type_annotation.y']`, 5000 rows in `meta_test`
order, no train Cell_IDs, all labels in the 60-class train set.

`prediction/prediction.csv` was not validated as a WYH model.

---

## 6. Test suite

Command:

```text
.venv/bin/python -m pytest -q \
  tests/test_data_contract.py \
  tests/test_official_contract.py \
  tests/test_folds.py \
  tests/test_submission_contract.py \
  tests/test_model_v1.py \
  tests/test_model_v2_release.py \
  tests/test_v3_research_program_closure.py
```

Result: **59 passed, 0 failed, 0 skipped** in 8.80s (exit 0).

No test was excluded. No platform-specific runtime error occurred.
`tests/test_model_v1.py` in-memory specialist fitting ran in this clone
environment; frozen source artifacts were not overwritten.

---

## 7. Markdown / public-layer verification

### Relative links

Scoped files (14):

- `README.md`
- `CHANGELOG.md`
- `experiments/README.md`
- `outputs/README.md`
- all `docs/**/*.md` (10 files)

Method: extract inline Markdown links and reference definitions; skip
`http(s):`, `mailto:`, `ftp:`, `tel:`, and fragment-only targets; resolve
remaining targets relative to the source file; require the file or
directory to exist. External HTTP URLs were not fetched.

| Item | Count |
|---:|---:|
| Relative links checked | 74 |
| Broken relative links | 0 |
| External links skipped (not fetched) | 1 |
| Fragment-only targets skipped | 0 |

`CHANGELOG.md` and several model/provenance cards contain no Markdown
hyperlinks. Organizer-body relative targets `Data.Description.md` and
`Leaderboard.Hackathon.2026.md` exist at the repository root.

### Organizer README SHA-256

Extraction semantics match Phase C: from the heading

`# University of Rochester Biomedical Data Science Hackathon Summer 2026`

through EOF of `README.md` (exact bytes; heading included).

| Item | Value |
|---|---|
| Heading byte offset | 6929 |
| Section length | 3025 bytes |
| SHA-256 | `56b3bc149f08c91b55e4c57aa36b4e1008592eb5ee8cd18ed67badd89caa0be4` |
| Expected | `56b3bc149f08c91b55e4c57aa36b4e1008592eb5ee8cd18ed67badd89caa0be4` |
| Match | yes |

---

## 8. Archive cleanliness after checks

Recorded after integrity checks, pytest, Markdown-link check, and organizer
hash, and **before** writing this report:

| Command | Result |
|---|---|
| `git status --short` | empty |
| `git diff --check` | empty |
| `git diff --stat` | empty |
| `git rev-parse HEAD` | `89f9ef26d56c0febfef4c66f7ac3fea1d0d23f17` |
| `git branch --show-current` | `ywan/ml-pipeline` |

Ignored `.venv/` is present and gitignored (`.gitignore:1:.venv/`). No
tracked file changed during verification.

Source repository after D1 (read-only confirm): still `89f9ef2` on
`ywan/ml-pipeline`, working tree clean.

This report is untracked inside the disposable clone only. It is not
committed.

---

## 9. Pass criteria

| # | Criterion | Result |
|---|---|---|
| 1 | Exact HEAD `89f9ef2` reproduced in isolated clone | PASS |
| 2 | Dependency environment created using documented Python 3.9.x | PASS (3.9.6) |
| 3 | Official manifest verification passes | PASS |
| 4 | Data validation passes | PASS |
| 5 | MODEL V1 candidate validates | PASS |
| 6 | MODEL V2 candidate validates | PASS |
| 7 | Complete relevant test set passes | PASS (59 passed / 0 failed / 0 skipped) |
| 8 | No broken relative Markdown links | PASS (74 / 74) |
| 9 | Organizer README hash matches | PASS |
| 10 | No tracked file changed during verification | PASS |

---

## 10. Warnings / blockers

Warnings (non-blocking):

- The `python3.9` executable name is not on `PATH`. The usable 3.9.x
  interpreter is `/usr/bin/python3` (Python 3.9.6).
- Disposable-clone `origin` is the local source path, not GitHub. That is
  expected because commit `89f9ef2` had not yet been pushed when this
  verification was run.

Blockers: none.

Not done in D1 (by instruction):

- no source-repository edit
- no commit / push / pull / merge / rebase / reset / clean
- no tag or release creation
- no MODEL V1 or MODEL V2 retraining
- no validation of `prediction/prediction.csv` as a WYH model
- this report is not committed

---

## 11. Verdict

**D1 PASS**
