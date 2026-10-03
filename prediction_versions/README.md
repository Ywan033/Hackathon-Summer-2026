# Personal prediction artifact index

This index identifies prediction artifacts that already existed in the
personal history at archive commit
`e3b5f323c5ab0de9aa1cf1e8f65dd530db79059b`. No prediction was retrained,
derived, or relabeled for this integration.

The complete per-file inventory is
[`manifest.csv`](manifest.csv). It records 49 artifacts with model or
experiment identity, release status, path, labeled-validation versus
hidden-label-test scope, row and column counts, output type, SHA-256, last
artifact commit, evidence path, and accuracy only where labels are available.

## Reading the index safely

- Accuracy on a labeled competition-train validation artifact is not an
  accuracy for its companion hidden-label test prediction.
- Files under `outputs/submissions/` are personal candidates, not verified
  captain submissions or official leaderboard files.
- `outputs/oof/` is a historical directory name. MODEL V1 files are
  conventional competition-label OOF; MODEL V2 files are external-reference
  validation predictions.
- V3 artifacts are research outputs. MODEL V3 was not created.
- Diagnostic oracle coverage is not prediction accuracy and is not indexed as
  a deployable prediction version.

## Formal personal models

### MODEL V1 — frozen historical baseline

Status: formal frozen historical personal model.

Validated result: **3799 / 5000 = 0.7598 (75.98%)**, conventional 3-fold
competition-label OOF. Evidence:
[`model_v1_metrics.json`](../outputs/metrics/model_v1_metrics.json) and
[`model_v1.md`](../docs/versions/model_v1.md).

- Test labels:
  [`outputs/submissions/model_v1.csv`](../outputs/submissions/model_v1.csv),
  5000 hidden-label test cells, SHA-256
  `c82acc6a444206eb48b5f4e9dec04ab3657892b7e6d4c1ab573d1dbd611553fd`,
  release commit `a3722c06f1844bce825b38a05c4be85838de491a`.
- Test probabilities:
  [`model_v1_test_probabilities.csv.gz`](../outputs/probabilities/model_v1_test_probabilities.csv.gz),
  5000 × 60 probabilities plus Cell_ID, SHA-256
  `850a0a934ff864eb15880855382909c4e0d2eda4cdff303a49c5f787f571e3b8`.
- Labeled OOF predictions:
  [`YW-004_oof.csv`](../outputs/oof/YW-004_oof.csv), 5000 rows, SHA-256
  `ebb09b7459f5d66bd3d88da3c41a6b9a7a9a329264ab8c17ff8be64ee44db97b`.
- OOF probabilities:
  [`YW-004_oof_probabilities.csv.gz`](../outputs/probabilities/YW-004_oof_probabilities.csv.gz),
  5000 × 60 probabilities plus Cell_ID, SHA-256
  `795d8b4a9bddb7fb000ebde276ed33faffcc56bf977e885cce59577b651b1794`.

The 75.98% result belongs to the labeled OOF artifacts. Test labels remain
unknown.

### MODEL V2 — frozen selected personal model

Public method: External Reference Transfer (LightGBM). Internal frozen ID:
`V2-B-REFONLY`.

Validated result: **4106 / 5000 = 0.8212 (82.12%)**, honest
external-reference validation. The LightGBM classifier was fit on the cleaned
external reference, not on competition-train labels. Evidence:
[`model_v2_metrics.json`](../outputs/metrics/model_v2_metrics.json) and
[`model_v2.md`](../docs/versions/model_v2.md).

- Test labels:
  [`outputs/submissions/model_v2_candidate.csv`](../outputs/submissions/model_v2_candidate.csv),
  5000 hidden-label test cells, SHA-256
  `8a110aec5d2b957e603cc3e3cbdc1e0402949790a0208dff0f285c14adc7c7ac`,
  release commit `2a25ee2ddeda46b1219cdd96bffc84d17e115c37`.
- Selected test probabilities:
  [`V2-B-REFONLY_test_probabilities_seg.csv.gz`](../outputs/probabilities/V2-B-REFONLY_test_probabilities_seg.csv.gz),
  5000 × 60 probabilities plus Cell_ID, SHA-256
  `6c4382e2ed3ee91accd7d7602ce22472df112dcfe3d36d275529fe56d693c700`.
- Labeled external-reference validation predictions:
  [`V2-B-REFONLY_oof.csv`](../outputs/oof/V2-B-REFONLY_oof.csv), 5000 rows,
  SHA-256
  `2ddb7ac653ba4bd262b8f412034bd059ee637ac596f4c7996a275e098475525e`.
- Selected validation probabilities:
  [`V2-B-REFONLY_oof_probabilities_seg.csv.gz`](../outputs/probabilities/V2-B-REFONLY_oof_probabilities_seg.csv.gz),
  5000 × 60 probabilities plus Cell_ID, SHA-256
  `172fa1e258385d767b182e7c894eb79a62a093d742442d27da172c5dbb21f612`.

`MODEL-V2_oof.csv` is a byte-identical historical alias of
`V2-B-REFONLY_oof.csv`. Raw, E/I-constrained, and Segment-constrained
probability variants are retained in place and enumerated in the manifest.
The 82.12% result belongs to labeled external-reference validation, not to
the hidden-label test candidate.

## V3 research prediction outputs

All entries below are experiments, not formal versions.

- **V3-E02D S0 / S1 / S2:** labeled 5000-cell validation predictions exist
  for all three students. Their recorded external-reference validation
  accuracies are 0.6302 / 0.6294 / 0.6292. Only S2 has committed validation
  and hidden-label test probability matrices. Evidence:
  [`v3_e02d_student_comparison.csv`](../outputs/v3/v3_e02d_student_comparison.csv).
- **V3-E03A S0 inference:** a 5000-cell competition-train probability matrix
  exists at
  [`s0_inference_probabilities.csv.gz`](../outputs/v3/v3_e03a_tables/s0_inference_probabilities.csv.gz).
  It is deterministic inference from the frozen S0 checkpoint recorded by
  E03A, not a test prediction and not a new model version.
- **V3-E04S SNI:** labeled validation predictions, validation probabilities,
  and hidden-label test probabilities exist. Standalone
  external-reference validation accuracy is 0.5682. This was a
  source-diversity experiment, not MODEL V3.
- **V3-E06M M1:** labeled validation predictions/probabilities and
  hidden-label test probabilities exist. External-reference validation
  accuracy is 0.8172; the naive pool regressed.
- **V3-E06M M2:** labeled validation predictions/probabilities and
  hidden-label test probabilities exist. The test probability file is
  [`v3_e06m_m2_test_probabilities.csv.gz`](../outputs/v3/v3_e06m_m2_test_probabilities.csv.gz),
  with 5000 × 60 probabilities plus Cell_ID and SHA-256
  `80729ac6de2d165d6f817d7e5a9934f326013b322d66039d145b72cb0a2e70a1`.
  Its labeled external-reference validation result is
  **4109 / 5000 = 0.8218**, but it was not promoted. MODEL V3 was not
  created. Evidence:
  [`v3_e06m_metrics.json`](../outputs/v3/v3_e06m_metrics.json) and the
  [`V3 program summary`](../reports/v3/v3_research_program_summary.md).

The manifest contains the exact path, size shape, SHA-256, source commit, and
evidence pointer for every retained V3 prediction artifact.

## Other retained personal experiments

The manifest also covers YW-000 through YW-007, BRIDGE-YW004-5F,
V2-A-SPATIAL-LGBM, and MODEL V2 raw/post-processed probability variants.
These stay at their historical paths so scripts and reports are not broken.
They are baselines, comparators, or unselected experiments, not additional
formal model versions.

## Team and organizer files

The team `wyh` snapshot contains byte-identical delivery copies of the
personal MODEL V1 and MODEL V2 candidate CSVs under
`team_archive/wyh/work/experiments/evidence/submissions/`. Their provenance is
documented separately in
[`team_archive/wyh/PROVENANCE.md`](../team_archive/wyh/PROVENANCE.md).

The root [`prediction/prediction.csv`](../prediction/prediction.csv) in this
personal repository is the organizer example, SHA-256
`411195ef53f17236a88bd1bcc9375bcb308f2fe6a8d86df0965aaf00b089739d`.
It is not a personal model. The different team-root prediction at the source
`wyh` commit was deliberately not imported and must not be attributed to
MODEL V1 or MODEL V2.

## Missing or intentionally unavailable outputs

- Official hidden test labels and per-model official leaderboard scores are
  unavailable.
- Exact captain-submission provenance for personal MODEL V1 or MODEL V2 is
  not established.
- No formal MODEL V3 submission exists because MODEL V3 was not created.
- S0 and S1 have no committed hidden-label test probability artifact; S2
  does.
- V3 hidden-label test outputs are probabilities only where listed; this
  integration did not manufacture label CSVs from them.
- YHH cell-level OOF was unavailable during the historical V3 audit.
- External `.h5ad` inputs and model checkpoints are local-only and are not
  prediction artifacts in this Git repository.
