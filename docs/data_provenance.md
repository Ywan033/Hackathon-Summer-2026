# Data provenance

Repository evidence only. This note records where training and evaluation
data live, how they are hashed, and which leakage controls apply. It does
not invent licenses, permissions, or undocumented dataset properties.

Related contracts: `docs/data_audit.md`, `docs/validation_protocol.md`,
`src/merfish60/reference.py`, `experiments/official_data_manifest.json`.

---

# 1. Official hackathon data

Source of the statistics below: organizer files in `data/`, described in
`docs/data_audit.md` and hashed in `experiments/official_data_manifest.json`
(generated_at `2026-08-19T10:30:04Z`).

| File | Rows | Role | Labels | SHA256 (`experiments/official_data_manifest.json`) |
|---|---:|---|---|---|
| `data/counts_train.csv` | 5000 | Official 200-gene counts, train | none in this file | `1e14658be651060e97fa925daefccdc205b7e7fb9a989b2d3c02c244a5e035a9` |
| `data/counts_test.csv` | 5000 | Official 200-gene counts, test | none in this file | `a0d94c4f1b502aa9bef91eab198d042c39d79eef2019b744aecdb31b37c4f501` |
| `data/meta_train.csv` | 5000 | Metadata + target | `MERFISH_cell_type_annotation` present (60 classes) | `8a49896b4afe38aa7521b5712f6b38c34497be073820a01755adde67755c436a` |
| `data/meta_test.csv` | 5000 | Metadata; labels hidden | `MERFISH_cell_type_annotation` is NA for all 5000 rows | `ffd02a7e3cb412ee2d1fe62e3e9f64c3fbf3eba9f49e5ff42633b2e53757f545` |

## Labels available / unavailable

- Train labels: available in `meta_train.csv`. Used as fitting targets for
  MODEL V1 (competition-label OOF) and as the **scoring** target for MODEL V2
  / V3 external-validation numbers. MODEL V2 LightGBM is not fit on these
  labels.
- Test labels: unavailable. Personal modeling must not read them.
- `prediction/prediction.csv` is the organizer example submission file. It
  is not a trained WYH model and must not be modified by personal modeling.

## Git / storage role

Official CSVs are tracked in Git as competition inputs. Verify them with:

```text
.venv/bin/python scripts/10_official_manifest.py --verify
```

Cell_ID is a 19-digit decimal string and must be loaded as a string.
Float64 round-trip is lossy for almost all IDs (`docs/data_audit.md`).

Train and test Cell_ID sets are disjoint. Both splits share all 108
`Section_ID`s (within-section mixed split).

Do not commit replacements of these four files.

---

# 2. MODEL V2 approved external reference

| Item | Repository evidence |
|---|---|
| Record | Zenodo **18039571** |
| File | `MERFISH_spinal_cord_resolved_0718.h5ad` |
| Expected local path | `work/external/MERFISH_spinal_cord_resolved_0718.h5ad` |
| MD5 | `ce06f62c0ec4973581dae17bb76f0cd9` |
| Raw shape | 146,621 cells × 500 genes |
| Label column | `obs["MERFISH cell type annotation"]` |
| Tracked in Git? | **No.** Directory `work/external/` is gitignored (`.gitignore`) |
| Code constants | `src/merfish60/reference.py` (`EXPECTED_MD5`, `ZENOD_RECORD`, `HISTORICAL_USABLE_ROWS = 136574`) |

Same-study identity in frozen docs: Wang … Meltzer MERFISH spinal-cord
deposit. This file is not redistributed from this Git repository.

## Local storage policy

Place the file locally and verify MD5 **before** any reference reproduction.
Never commit:

- `*.h5ad`
- raw external datasets
- `work/external/cache/*.npz`
- disposable external preprocessing caches

Committed: MD5 / exclusion / alignment metadata and reproducible code.

## Role in training

MODEL V2 LightGBM is fit **only** on the cleaned usable reference rows
(136,574). Competition train labels are not the boosting target. Official
200-gene columns must be aligned in exact competition order. All 60
competition classes are represented after space/hyphen → underscore
normalization.

## Competition-ID exclusion safeguards

Independently reproduced on this branch
(`experiments/V2-B-REFONLY_exclusion_manifest.json`):

- remove all 5000 competition train Cell_IDs
- remove all 5000 competition test Cell_IDs
- remove 47 rows whose aligned 200-gene count vector exactly duplicates a
  competition train or test cell

Final usable reference: **136,574**.

## Leakage controls

- Hidden test target never used
- Held-out competition fold labels hidden from fitting, neighbor-label
  histograms, masks, and post-process selection when scoring fold *f*
- Reference labels remain visible as permitted external labeled data
- Cell_ID lossless string joins
- MD5 mismatch aborts reference modeling (`scripts/09_v2b_refonly.py`)

No license text beyond the Zenodo record identifier is asserted here.

---

# 3. Additional V3 references

Documented only where frozen V3 artifacts exist. Not used to train MODEL V2.

## 3.1 SNI source (`SNI_merged_0917.h5ad`)

Evidence: `reports/v3/v3_e04s_sni_source_expert.md`,
`outputs/v3/v3_e04s_dataset_audit.json`, V3-E06M dataset manifest.

| Item | Evidence |
|---|---|
| Local path | `work/external/SNI_merged_0917.h5ad` |
| Tracked in Git? | **No** (`work/external/` gitignored) |
| MD5 | `7e90a801ee57b8fec06cd03c8630f01b` |
| SHA256 | `30fcfed7daa1d5bc690b444a2a4e4fe1801fcad47e1417800ef9117252086226` |
| Raw shape | 55,331 cells × 500 genes |
| Label column used | `obs["voting"]`, mapped with existing `merfish60.reference.norm_label` |
| Official genes | 200 / 200 aligned to MODEL V2 gene order |
| Taxonomy | 60 / 60 competition classes after mapping |
| Usable after exclusion | **55,193** |
| Role | Isolated SNI-only LightGBM expert in V3-E04S; source-balanced rows in V3-E06M M1/M2 |

E04S recorded: 0 Cell_ID overlap with competition train, competition test,
and MERFISH-reference IDs; 116 exact 200-gene vector overlaps versus the
cleaned MERFISH reference (excluded). Condition field in the file:
`{'Sham': 31498, 'SNI': 23833}`.

E04S explicitly did not infer undocumented biological meaning from field
names and did not claim a public license. This provenance note follows that
restriction. Remote origin beyond the local filename is not asserted.

## 3.2 Same Zenodo file in V3 privileged-gene work

V3-E02D reused the MODEL V2 Zenodo file (MD5
`ce06f62c0ec4973581dae17bb76f0cd9`) as a 500-gene teacher / 200-gene student
source. That is the same approved reference, not a new dataset.

---

# 4. What this document does not claim

- Official leaderboard / unseen-test labels
- A license string for Zenodo 18039571 or for the SNI file beyond what
  frozen reports already record
- That raw `.h5ad` files are part of the Git source of truth
- That SNI is a MODEL V2 training source
- That concatenating additional atlases is approved
