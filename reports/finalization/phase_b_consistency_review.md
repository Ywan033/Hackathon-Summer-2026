# Phase B consistency review

Date: 2026-08-22
Branch: `ywan/ml-pipeline`
Phase A commit: `5fb1b67`
Mode: protocol / naming / attribution / canonical documentation pass
README.md: **not rewritten** (explicit Phase B exclusion)

Search terms: `Reference-only`, `reference-only`, `Reference-Augmented`,
`Current Released Candidate`, `Current Research State`, `release candidate`,
`next step`, `Future`, `Potential`, `MODEL V3`, `MODEL V4`, `pending`,
`not pushed`, `oracle accuracy`, `official score`, `leaderboard`.

Classification:

1. intentional frozen / internal identifier
2. correct historical statement
3. deferred competition-result or later-README dependency
4. unresolved issue requiring correction

---

## 1. Live archival documents updated in this phase

| Location | Term | Class | Note |
|---|---|---|---|
| `docs/versions/model_v2.md` title | External Reference Transfer (LightGBM) | 1 | Public name applied |
| `docs/versions/model_v2.md` | V2-B-REFONLY | 1 | Internal frozen ID retained at first occurrence |
| `docs/versions/model_v2.md` | 0.8212 described as external-reference validation, not conventional OOF | 2 | Required protocol distinction |
| `docs/versions/model_v2.md` `outputs/oof/V2-B-REFONLY_oof.csv` | `oof` in filename | 1 | Frozen artifact name; prose now says evaluation labels |
| `docs/versions/model_v1.md` | 3-fold OOF | 2 | V1 **is** conventional OOF |
| `docs/versions/model_v1.md` | tag `model-v1` exists (`395004e`) | 2 | Replaces “tag had not been pushed” |
| `docs/validation_protocol.md` | “Future runs **load** these files” | 2 | Means later executions; not an open modeling stage |
| `AGENTS.md` | “Do not start a public MODEL V4…” | 2 | Prohibition, not a research mandate |
| `AGENTS.md` / contribution docs | MODEL V3 was not created | 2 | Canonical closed state |
| `AGENTS.md` / model cards | No verified official leaderboard score attributable in this personal repository | 3 | Exact captain provenance still unknown |
| New `docs/contributions/wyh_contribution_summary.md` | LZH / YHH / team/main not WYH-owned | 2 | Required boundary |
| New `docs/data_provenance.md` | Zenodo 18039571; SNI local MD5 only | 2 | No invented license |

No class-4 hits remain in the files this phase was authorized to edit.

Gone from live model cards / `AGENTS.md` / `docs/validation_protocol.md`:

- Reference-Augmented
- Current Released Candidate
- Current Research State
- release candidate
- next step is MODEL V3
- tag not pushed
- “one frozen protocol for all experiments”
- MODEL V3+ development mandate

---

## 2. README.md (intentionally unchanged)

| Text | Class | Later action |
|---|---|---|
| “Current Released Candidate — MODEL V2” | 3 | Phase C README rewrite |
| “Reference-only LightGBM + approved Zenodo reference” | 1+3 | Public name pending README phase; `V2-B-REFONLY` remains the internal ID |
| “Local 5-fold OOF accuracy is 82.12%” | 3 | Should become external-reference validation |
| “+6.14 pp versus MODEL V1 (75.98%)” | 3 | Not the protocol-matched delta; use +6.16 pp vs BRIDGE 0.7596 |
| “There is no official leaderboard score” | 3 | Keep until exact provenance exists |
| “V3 completed / MODEL V3 not created” | 2 | Already correct |

Do not treat these README leftovers as Phase B defects. Rewriting README was
forbidden in this phase.

---

## 3. Frozen historical V3 reports, scripts, and metrics (not rewritten)

These keep contemporaneous wording. Do **not** rename files or internal IDs.

| Pattern | Typical class | Examples |
|---|---|---|
| `reference-only LightGBM` as MODEL V2 method label | 1 | `outputs/metrics/model_v2_metrics.json`; `scripts/09_v2b_refonly.py`; `experiments/registry_v2.csv`; V3-E00T/E04S/E06M reports |
| `reference-only genes` (300 extra Zenodo genes) | 2 | V3-E02D research question |
| “MODEL V3 is not frozen / not defined yet” inside an E-stage report | 2 | Written before E07D closure; historically true at that commit |
| “Recommended next action” / “future E05B” / “future V3 expert pool” | 2 | Within-program next-step language at that experiment |
| “Diagnostic oracle accuracy is not deployable” | 2 | Already glossed; optional later wording polish only |
| `oracle_accuracy` JSON keys | 1 | Frozen machine-readable fields |
| `OOF` in V2 artifact filenames | 1 | `outputs/oof/V2-B-REFONLY_oof.csv` |

`reports/v3/v3_research_program_summary.md` still says “MODEL V2
(reference-only LightGBM…)”. That is a frozen V3 closure document, not in
the Phase B edit list. Optional later alignment only.

---

## 4. Phase A audit file

`reports/finalization/repository_finalization_audit.md` still quotes the
**pre-Phase-B** wording of README / AGENTS / model cards. That is a class-2
snapshot of Phase A. It is not a live contract.

This Phase B review supersedes those “later action” rows for every file
except `README.md` and competition-result inserts.

---

## 5. Closure-test immutability policy

`tests/test_v3_research_program_closure.py` now distinguishes:

- byte-immutable experiment / release artifacts (metrics, OOF tables, fold
  files, submissions, V3 stage reports, `prediction/prediction.csv`);
- archival model cards (`docs/versions/model_v1.md`, `model_v2.md`), which
  may receive factual final-state wording updates.

Canonical card facts are still asserted: V1 3799 / 5000 = 0.7598; V2
4106 / 5000 = 0.8212; internal ID `V2-B-REFONLY`; MODEL V3 not created.

The V2 release-contract test title assertion was updated to the new public
title (`tests/test_model_v2_release.py`). Metrics, candidates, and
probability artifacts were not changed.

Some older per-stage V3 tests still `git diff` the model cards. Those are
outside the lightweight closure suite and were not rewritten in this patch.

---

## 6. Unresolved / deferred (not class 4 in this phase)

| Item | Class |
|---|---|
| README public name, protocol-matched +6.16 pp, “selected personal model” | 3 (later README phase) |
| Official / captain leaderboard score | 3 |
| Whether MODEL V2 was the scored captain file | 3 |
| Team-final credit beyond this personal branch | 3 |
| Optional polish of frozen V3 prose (`oracle accuracy`, `reference-only LightGBM`) | 2; do not rewrite unless separately authorized |
| Older per-stage V3 tests that still `git diff` model cards | 2 relative to those E-stage files; not part of the closure suite |

No remaining class-4 issue was found in the authorized Phase B files after
the edits in this working tree.
