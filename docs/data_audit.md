# MERFISH-60 data audit (Sprint 0, verified)

Source files: `data/counts_train.csv`, `data/counts_test.csv`, `data/meta_train.csv`, `data/meta_test.csv`, plus `README.md` and `Data.Description.md`. Statistics were computed from those files. This note records the contract used by the pipeline; it does not add modeling results.

## Task and metric

Predict `MERFISH_cell_type_annotation` for test cells. Official metric: overall accuracy (correct / total). Evaluation description: `Data.Description.md`.

## Shapes and identifiers

| File | Data rows | Content |
|---|---:|---|
| `data/counts_train.csv` | 5000 | unnamed Cell_ID + 200 gene count columns |
| `data/counts_test.csv` | 5000 | same gene columns, same order |
| `data/meta_train.csv` | 5000 | metadata + labels |
| `data/meta_test.csv` | 5000 | metadata; labels hidden |

- Train counts and train meta Cell_IDs match in **exact row order**.
- Test counts and test meta Cell_IDs match in **exact row order**.
- Train and test Cell_ID sets are **disjoint** (overlap 0).
- No duplicate Cell_IDs in any of the four official tables.
- Cell_ID is a 19-digit decimal string. Values fit in signed int64, but **float64 round-trip is lossy** for 4994/5000 train IDs and 4989/5000 test IDs. Loaders must read Cell_ID as string and must not cast through float.

## Genes and counts

- 200 gene features, identical names and order in train and test; no duplicate gene names; no all-zero gene or all-zero cell.
- Counts are nonnegative integers (train max 195, test max 217).
- Train sparsity (zeros) = 0.925706; test = 0.925507.
- Train per-cell transcript count: min 1, median 21, mean 31.3168, max 550.
- Test per-cell transcript count: min 1, median 21, mean 31.1896, max 598.

## Target

- Column: `MERFISH_cell_type_annotation`.
- Train: 0 missing labels, **60 classes**.
- Test: 5000/5000 missing (`NA` in the official CSV).
- Min class count: 3 (`VH_in_Chat`). Max class count: 703 (`oligodendrocyte_progenitor_2`, 14.06% of train).
- Classes with n ≤ 10: 7. Majority-class rate on train: 0.1406. Prior-matching chance (`sum p_i^2`): 0.05587.

## Metadata

Train missingness: `Region` 3142/5000 (62.84%), `Excitatory_vs_Inhibitory` 3142/5000 (62.84%), `Segment` 2958/5000 (59.16%). Other listed meta fields have 0 missing in train. Test labels are fully hidden; other test missingness is similar (`Region`/`Excitatory_vs_Inhibitory` 3177/5000, `Segment` 3004/5000).

`Region` and `Excitatory_vs_Inhibitory` are missing on the same rows. In the current training set, `Excitatory_vs_Inhibitory` is **strongly target-correlated metadata, potentially annotation-derived**: all 5000 training cells agree with the `_ex_` / `_in_` token in the class name (or neither token when E/I is missing). That agreement is not proof of how the column was created.

## `(Region, Excitatory_vs_Inhibitory, Segment)` signatures (train)

- 28 unique signatures.
- 15 signatures map to exactly one class **in the current training set** (observed-deterministic, not a guaranteed identity). They cover 842/5000 training cells (16.84%). This coverage is **not** a 16.84 percentage-point accuracy gain.
- 13 signatures are ambiguous. The largest is `(NA, NA, NA)`: 2958 cells and 16 types.
- Only `DH_in_Cdh3` occupies more than one signature in train.
- Majority vote within these signatures on the full training set scores 2308/5000 = 0.4616. That number is a **reference baseline**, not a metadata-only ceiling.
- Any signature-to-label mapping used in modeling must be learned from the training fold only.

## Train vs test split

Train and test share all 6 `Datasets`, all 10 `Mouse_ID`s, and all **108 `Section_ID`s**. Continuous marginals are close (library-size two-sample KS = 0.0168; per-gene mean Pearson = 0.9985). This is a **within-section mixed split with closely matched marginals**. It is not described as IID.

Same-section nearest train neighbor for test cells: median distance 71.20; 3507/5000 test cells have a train neighbor within 100 coordinate units.

## Example submission file

`prediction/prediction.csv` is the organizer example. It was inspected, not modified. It has 5000 data rows, columns `Cell_ID` and `MERFISH_cell_type_annotation.y`, test Cell_ID order, no missing values, no duplicate IDs, and 59 of 60 training labels (`VH_in_Chat` unused). It is not a trained model.
