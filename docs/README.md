# Documentation index

Canonical public documentation for the WYH personal modeling contribution on
`ywan/ml-pipeline`. Detailed experiment reports stay in `reports/`; this index
does not duplicate them.

Developer operating rules (not a public landing page): [`AGENTS.md`](../AGENTS.md).

## Project overview

- [`README.md`](../README.md) — public landing page (WYH section above the organizer text)
- [`final_project_report.md`](final_project_report.md) — archival research report
- [`CHANGELOG.md`](../CHANGELOG.md) — verified milestone history

## Model versions

| Identity | Document | Status |
|---|---|---|
| MODEL V1 | [`versions/model_v1.md`](versions/model_v1.md) | Frozen historical baseline (75.98% 3-fold OOF) |
| MODEL V2 | [`versions/model_v2.md`](versions/model_v2.md) | Frozen selected personal model (82.12% external-reference validation) |
| MODEL V3 | — | **Not created.** See the V3 program summary |

There is no public modeling stage after V3.

## Validation and reproducibility

- [`validation_protocol.md`](validation_protocol.md) — two frozen protocols (V1 3-fold OOF vs V2/V3 5-fold external-reference validation)
- [`reproducibility.md`](reproducibility.md) — environment, tests, and reproduction entry points
- [`experiments/README.md`](../experiments/README.md) — folds, registries, experiment navigation
- [`outputs/README.md`](../outputs/README.md) — artifact semantics (frozen vs experimental vs candidate)

## Data provenance

- [`data_provenance.md`](data_provenance.md) — official CSVs, Zenodo 18039571, SNI (V3 only)
- [`data_audit.md`](data_audit.md) — official-file shapes, Cell_ID contract, class counts

## Contribution records

- [`contributions/wyh_contribution_summary.md`](contributions/wyh_contribution_summary.md) — V1 / V2 / V3 ownership map
- [`contributions/wyh_v3_contribution.md`](contributions/wyh_v3_contribution.md) — V3-only evidence record

## V3 research

- [`reports/v3/v3_research_program_summary.md`](../reports/v3/v3_research_program_summary.md) — authoritative program closure
- Per-stage reports: [`reports/v3/`](../reports/v3/)
- Per-stage scripts: [`experiments/v3/`](../experiments/v3/)

Diagnostic oracle coverage in those reports is a retrospective ceiling, not
deployable accuracy.

## Finalization / audit records

- [`reports/finalization/repository_finalization_audit.md`](../reports/finalization/repository_finalization_audit.md) — Phase A
- [`reports/finalization/phase_b_consistency_review.md`](../reports/finalization/phase_b_consistency_review.md) — Phase B
- [`reports/finalization/phase_c_public_presentation_review.md`](../reports/finalization/phase_c_public_presentation_review.md) — Phase C
- [`reports/finalization/phase_d1_clean_clone_verification.md`](../reports/finalization/phase_d1_clean_clone_verification.md) — Phase D1 clean-clone archive-integrity verification
