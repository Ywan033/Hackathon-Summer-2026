# Repository Finalization Audit — V1 / V2 / V3

Repository: `Ywan033/Hackathon-Summer-2026`
Branch: `ywan/ml-pipeline`
HEAD inspected: `b51bcd5` (`docs(v3): close research program without model promotion`)
Audit date: 2026-08-22
Mode: read-only inspection, then this document only.

This audit describes the **canonical closed project history**: frozen MODEL V1,
frozen MODEL V2, and the completed V3 Research Program. MODEL V3 was not
created. There is no further public modeling stage after V3.

No commit, push, tag, README rewrite, `AGENTS.md` rewrite, or model-card rewrite
is authorized by this phase.

---

## 1. Canonical final research state

### MODEL V1

| Item | Value |
|---|---|
| Public method | Hierarchical Signature Specialists |
| Internal origin | Frozen YW-004 full-train per-signature specialists |
| Result | **3799 / 5000 = 0.7598 (75.98%)** |
| Evaluation | Personal frozen 3-fold OOF (`experiments/folds.csv`) |
| Status | Frozen historical baseline |
| Tag | `model-v1` |
| Official score | **Not submitted** (no captain-submission provenance in this personal repo) |

### MODEL V2

| Item | Value |
|---|---|
| Recommended public method name | **External Reference Transfer (LightGBM)** |
| Internal historical identifier | **V2-B-REFONLY** (preserve exactly in artifacts, scripts, and tags) |
| Result | **4106 / 5000 = 0.8212 (82.12%)** |
| Evaluation | Team-compatible 5-fold file `experiments/team_folds_5_seed42.csv` |
| Status | Frozen **selected WYH personal model** |
| Tag | `model-v2` |
| Official score | **Not submitted** unless later exact submission provenance exists |

Current public docs still say “reference-only LightGBM”. That phrase is the
historical internal method label. For the archival README, prefer **External
Reference Transfer (LightGBM)** and keep `V2-B-REFONLY` as the frozen identifier.

A predeclared V2-C blend (C1, 0.8224) was evaluated and **rejected**. MODEL V2
remains the simpler V2-B architecture.

### V3 Research Program

| Item | Value |
|---|---|
| Status | **Completed.** MODEL V3 was **not created.** |
| Best standalone experimental result | E06M M2: **4109 / 5000 = 0.8218 (82.18%)** |
| Promotion | **Not justified** (net +3 vs MODEL V2; McNemar p = 0.8895; folds 3–4 net −9; bootstrap CI includes 0) |
| Selected personal model after V3 | **MODEL V2 remains frozen and selected** |
| Closure sources | `reports/v3/v3_research_program_summary.md`; decision checkpoint `e7f94fd` |

Diagnostic oracle coverage from the V3 program (for example 4393 / 5000 =
0.8786) is a **coverage ceiling**, not deployable accuracy, not OOF accuracy,
and not a MODEL V3 score.

---

## 2. Source-of-truth documents

| Identity | Authoritative documents | Supporting artifacts |
|---|---|---|
| MODEL V1 | `docs/versions/model_v1.md`; tag `model-v1` | `outputs/metrics/model_v1_metrics.json`; `outputs/metrics/YW-004_metrics.json`; `outputs/submissions/model_v1.csv`; `experiments/folds.csv`; `experiments/registry.csv`; `scripts/06_model_v1.py`; `tests/test_model_v1.py` |
| MODEL V2 | `docs/versions/model_v2.md`; tag `model-v2` | `outputs/metrics/model_v2_metrics.json`; `outputs/metrics/V2-B-REFONLY_*.json`; `outputs/metrics/model_v2_selection.json`; `outputs/submissions/model_v2_candidate.csv`; `outputs/probabilities/V2-B-REFONLY_test_probabilities_seg.csv.gz`; `experiments/team_folds_5_seed42.csv`; `experiments/registry_v2.csv`; `scripts/09_v2b_refonly.py`; `tests/test_model_v2_release.py` |
| V3 program | `reports/v3/v3_research_program_summary.md`; `docs/contributions/wyh_v3_contribution.md` | `outputs/v3/v3_research_program_manifest.json`; per-stage `reports/v3/v3_e*.md`, `experiments/v3/`, `outputs/v3/`, `tests/test_v3_*.py`; closure commit `b51bcd5` |
| Official data | `docs/data_audit.md`; `experiments/official_data_manifest.json` | `data/*.csv` (organizer-provided) |
| Developer operating rules | `AGENTS.md` | Currently a live competition runbook; stale relative to V3 closure |
| Public landing page | `README.md` (WYH block above `---`) | Organizer body below the rule must remain unchanged |

---

## 3. Validation-protocol distinctions

Do **not** treat MODEL V1, MODEL V2, and V3 experimental numbers as the same
kind of score. Repository evidence:

### MODEL V1 — conventional competition-label 3-fold OOF

```text
StratifiedKFold(n_splits=3, shuffle=True, random_state=20260819)
```

Persisted in `experiments/folds.csv`. For each training cell, the recorded
prediction comes from a model fit on the other two folds only. Competition
train labels **are** the fitting target. This is standard OOF.

Documented in `docs/versions/model_v1.md` and `docs/validation_protocol.md`.

### MODEL V2 — team 5-fold external-reference evaluation

```text
StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
```

Persisted in `experiments/team_folds_5_seed42.csv`.

Important semantics from `docs/versions/model_v2.md`:

- LightGBM is fit **only on 136,574 cleaned Zenodo reference rows**.
- Competition training labels are **never the boosting target**.
- When scoring competition fold *f*, fold-*f* competition labels are hidden
  from fitting, neighbor-label histograms, masks, and post-process selection.
- Reference labels remain visible as permitted external labeled data.
- Hidden test labels are never read.

The 0.8212 figure is therefore an honest **external-validation** score on the
5000 competition-train cells under a fold-safe histogram contract. Docs often
call it “OOF”. That is convenient but not identical to MODEL V1 OOF.

### Protocol-matched V1 comparison on the 5-fold file

BRIDGE-YW004-5F evaluates the frozen V1 architecture on the team 5-fold file:
**0.7596**. MODEL V2’s protocol-matched delta versus that bridge is **+0.0616**.
The README currently reports +6.14 pp versus MODEL V1’s **3-fold** 0.7598.
Both deltas are real; they are not the same comparison. The archival README
should distinguish them.

### V3 experimental numbers

Canonical **analysis partition** for V3 comparisons is the same 5-fold file as
MODEL V2. Additional caveats from `reports/v3/v3_research_program_summary.md`:

- M2 / S0 / SNI are honest external-validation predictions unless a frozen
  report says otherwise. Competition labels do not enter those boosting or
  student-training objectives.
- Canonical folds 3–4 were viewed across V3 stages and are a **retrospective
  stability partition**, not a pristine holdout.
- LZH Prior-H, used as a frozen comparator, has a **native 3-fold** protocol;
  5-fold slices of that bundle are a common evaluation partition, not LZH’s
  original folds.

### Diagnostic oracle coverage

Values such as V2 three-expert 4414 / 5000 = 0.8828 and V3 five-expert
4393 / 5000 = 0.8786 ask whether **at least one** saved expert was already
correct. They require true labels and a perfect selector. They are **not**
model accuracy.

---

## 4. Stale or inconsistent documentation

Classification:

- **A** correct historical/internal usage — preserve
- **B** stale final-state wording — update later
- **C** ambiguous public terminology — polish later
- **D** unsupported claim — correct later
- **E** final competition-result dependency — defer
- **F** frozen artifact/internal identifier — preserve exactly

### README.md

| Text | Class | Later action |
|---|---|---|
| Results table: V1 75.98% / V2 82.12%, official score Not submitted | A, E | Keep numbers; official score deferred |
| “Reference-only LightGBM + approved Zenodo reference” | C, F | Public name → External Reference Transfer (LightGBM); keep V2-B-REFONLY internally |
| “Current Released Candidate — MODEL V2” | B, C | “Selected personal model” |
| “current frozen WYH candidate” | B, C | Selected frozen personal model; not a captain-repo claim |
| “+6.14 pp versus MODEL V1 (75.98%)” | C | State that V1 is 3-fold and V2 is 5-fold; cite BRIDGE 0.7596 for matched protocol |
| “There is no official leaderboard score” | A, E | Keep until exact provenance exists |
| V3 completed / MODEL V3 not created | A | Keep |
| Organizer body below `---` | A | Do not modify |

### AGENTS.md

| Text | Class | Later action |
|---|---|---|
| Frozen V1/V2 numbers, leakage rules, Zenodo MD5, fold protocols | A, F | Keep |
| “MODEL V3+” as an open development target (§1) | B | Close: V3 completed, not created |
| §13 “MODEL V3 — Current Research State” / “NOT defined or frozen yet” | B | Replace with V3 completed / not created |
| “pairwise oracle accuracy” | A, C | Keep diagnostic meaning; prefer “diagnostic oracle coverage” |
| §15 reliability-gated routing as an active hypothesis | B | Historical V3 motivation only; not an open public stage |
| First-run handoff “current MODEL V3 research question” | B | Point at V3 program summary and this audit |
| “Do NOT treat a local OOF score as an official leaderboard score” | A | Keep |

### docs/versions/model_v2.md

| Text | Class | Later action |
|---|---|---|
| 4106 / 5000, C1 rejection, provenance, leakage | A, F | Keep |
| Title “Reference-Augmented…” / “reference-only LightGBM” | C, F | Public method name polish; preserve V2-B-REFONLY |
| “Frozen MODEL V2 candidate” / “release candidate” | C | Selected frozen personal model |
| Official score “Not submitted” | A, E | Keep until known |
| §9 / §13 “future … MODEL V3” / “next step” | B | V3 happened and was not promoted |

### docs/versions/model_v1.md

| Text | Class | Later action |
|---|---|---|
| 3799 / 5000, YW-004, 3-fold protocol | A, F | Keep |
| “MODEL V1 candidate” | C | Frozen historical baseline |
| Official score “Not submitted” | A, E | Keep |

### docs/contributions/wyh_v3_contribution.md

| Text | Class | Later action |
|---|---|---|
| V3 E00T–E07D evidence and non-attribution of LZH | A | Keep |
| “reference-only LightGBM” for MODEL V2 | C, F | Align public name later |
| “current team expert complementarity” in research questions | A | Historical V3 question |
| YHH cell-level OOF unavailable | A | True of the **V3** program; do not silently rewrite V3 history |

### docs/validation_protocol.md

| Text | Class | Later action |
|---|---|---|
| Frozen 3-fold description | A | Keep as V1 protocol |
| “Frozen for all experiments … unless a later sprint replaces this file” | B | Incomplete: V2/V3 use a second frozen 5-fold file |
| “Future runs load this file” | A | Means later executions of the V1 protocol |

### reports/v3/

| Text | Class | Later action |
|---|---|---|
| Frozen E-stage numbers and “MODEL V3 not created” | A | Keep |
| Per-stage “next action” / “MODEL V3 is not frozen” | A | Historical within-program language |
| “oracle accuracy” already glossed as diagnostic | A, C | Optional wording polish only |
| “current frozen personal deployable model” | A | Still true |

### Frozen identifiers (do not rename artifacts)

`V2-B-REFONLY`, `YW-004`, `BRIDGE-YW004-5F`, `V2-A-SPATIAL-LGBM`, `V2-C`,
`V3-E00T` … `V3-E07D`, `M2`, tags `model-v1` / `model-v2`: **F**.

No **D** (unsupported numeric claim) was found in the frozen V1/V2/V3 sources
inspected, provided oracle figures remain labeled as diagnostic coverage.

---

## 5. Attribution / contribution-boundary issues

WYH-owned on `ywan/ml-pipeline`:

- validation contracts, Cell_ID integrity, fold files, submission validators
- MODEL V1 (YW-004 hierarchical specialists)
- MODEL V2 (V2-B-REFONLY External Reference Transfer)
- Zenodo exclusion / provenance reproduction (usable 136,574; MD5
  `ce06f62c0ec4973581dae17bb76f0cd9`)
- V3 program E00T–E07D, including SNI source work and source-balanced M2

Not WYH-owned, and not to be re-attributed:

- LZH Prior-H (`depth_masked_prior_h_anchor`), used in V3 as a frozen comparator
- YHH models (V3 recorded cell-level OOF as unavailable; do not invent YHH
  authorship or scores in V3 documents)
- `team/main` integration / captain `prediction/prediction.csv`

`prediction/prediction.csv` is **not** a personal WYH release artifact and must
remain unmodified.

---

## 6. Recommended final README structure

Do **not** rewrite `README.md` in this phase. Intended later public structure:

```text
# MERFISH Cell-Type Annotation — WYH Modeling Contribution

Project overview

## Results Summary

## MODEL V2 — Selected Personal Model

## Research Progression

## Reproducibility

## Repository Structure

## Contribution Scope and Attribution

## Limitations

---

# University of Rochester Biomedical Data Science Hackathon Summer 2026

[organizer-provided content continues unchanged]
```

Later README rules:

- A horizontal rule alone separates WYH material from organizer content.
- Results table: MODEL V1 and MODEL V2 only. No MODEL V3 row.
- V3 appears under Research Progression as a completed program with no
  promotion.
- Public V2 method name: External Reference Transfer (LightGBM).
- Validation column must show 3-fold vs 5-fold explicitly.
- Official score remains Not submitted / unknown until exact provenance exists
  (**E**).
- Avoid “Current Released Candidate”.
- Do not present diagnostic oracle coverage as a result-table accuracy.

---

## 7. Documents that should be created (later phases)

| Path | Purpose | Depends on official score? |
|---|---|---|
| `docs/README.md` | Index of model cards, V3 reports, provenance | No |
| `docs/final_project_report.md` | Durable V1 → V2 → V3 narrative | Partial |
| `docs/reproducibility.md` | Install, tests, regenerate V1/V2 | No for local scores |
| `docs/data_provenance.md` | Official CSVs + Zenodo (+ SNI as V3 evidence) | No |
| `docs/contributions/wyh_contribution_summary.md` | Single attribution map for V1, V2, V3 | No, except team-final credit |
| `experiments/README.md` | Registries, two fold files, `v3/` | No |
| `outputs/README.md` | Frozen vs experimental outputs | No |
| `CHANGELOG.md` | Tag-oriented history | Optional official-score line |

Do not create empty boilerplate in this phase. This audit file is the only
new document.

---

## 8. Documents that should be updated (later phases)

| Path | Exact reason |
|---|---|
| `README.md` | Replace “Current Released Candidate”; public V2 method name; protocol-aware V1 vs V2 comparison; keep organizer body |
| `AGENTS.md` | Close V3 as completed/not created; remove “current research state” / open V3+ mandate; keep leakage and git-safety rules |
| `docs/versions/model_v2.md` | Public method name; “selected personal model”; retire “next step … MODEL V3” |
| `docs/versions/model_v1.md` | Optional: “candidate” → frozen historical baseline |
| `docs/contributions/wyh_v3_contribution.md` | Align V2 public name; keep V3-era YHH-unavailable statement as historical |
| `docs/validation_protocol.md` | Document **two** frozen protocols (V1 3-fold and V2/V3 5-fold) |

Do not rewrite frozen numeric results.

---

## 9. Frozen files / artifacts that must remain untouched

- `prediction/prediction.csv`
- Official data: `data/counts_*.csv`, `data/meta_*.csv`
- Organizer README body below `---`; `Data.Description.md`; `Leaderboard.Hackathon.2026.md`
- Tags `model-v1`, `model-v2` and their annotated messages
- V1/V2 fold files: `experiments/folds.csv`, `experiments/team_folds_5_seed42.csv`
- V1/V2 metrics, OOF, probabilities, and candidates under `outputs/`
- Internal identifiers in filenames (`V2-B-REFONLY`, `YW-004`, V3-E IDs)
- Historical V3 stage reports, scripts, tests, and `outputs/v3/`
- `work/external/*.h5ad` gitignore policy; do not commit raw `.h5ad`
- Team remotes and team branches

---

## 10. Items that must wait for final competition results

- Official unseen-test / leaderboard score
- Whether MODEL V2 (or any other file) was the captain’s scored submission
- Exact commit/path provenance of any official score
- Any change to README “Not submitted”
- Team-final model credit beyond this personal branch

Do not invent those facts. A local OOF or experimental score is not an
official score.

---

## 11. Proposed sequence for final repository archival work

1. **This audit (Phase A)** — complete with this file; stop for review.
2. **Protocol and naming pass** — update `docs/validation_protocol.md` and
   model-card status language; introduce public V2 name without renaming
   frozen artifacts.
3. **AGENTS.md archival rewrite** — convert from live V3 operating manual to
   integrity/reproducibility guide consistent with V3 closure.
4. **Contribution index** — `docs/contributions/wyh_contribution_summary.md`
   covering V1, V2, and V3 only.
5. **Reproducibility and provenance docs** — `docs/reproducibility.md`,
   `docs/data_provenance.md`, `experiments/README.md`, `outputs/README.md`.
6. **README rewrite** — WYH block only, structure in §6; organizer body
   unchanged.
7. **Optional `docs/final_project_report.md` and `CHANGELOG.md`.**
8. **Competition-result insert** — only if exact official provenance exists.
9. **Commit / tag / GitHub presentation** — only after explicit authorization.

Each step should remain separately reviewable. Do not combine a README rewrite
with a commit in an unreviewed batch.

---

## Inspection snapshot

| Item | Value |
|---|---|
| `pwd` | `/Users/yyl/Documents/Hackathon-Summer-2026` |
| Branch | `ywan/ml-pipeline` |
| Working tree at inspection | clean |
| Remotes | `origin` = `Ywan033/Hackathon-Summer-2026`; `team` = `X0X0X00/Hackathon-Summer-2026` |
| Tags | `model-v1`, `model-v2` |
| V3 closure commit | `b51bcd5` |
| V3 decision experiment | `e7f94fd` |
