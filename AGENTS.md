# AGENTS.md — WYH Modeling Track Developer Contract

This file is the developer/agent contract for the WYH modeling track of the
University of Rochester Biomedical Data Science Hackathon Summer 2026.

It is **not** a public README.

Read this file before modifying the repository. The project is in a closed
archival state. Do not reopen modeling unless the user explicitly authorizes
a new formal experiment or correction.

---

# 1. Canonical final state

| Identity | Public name | Status | Result | Evaluation |
|---|---|---|---|---|
| **MODEL V1** | Hierarchical Signature Specialists | Frozen historical baseline | 3799 / 5000 = **0.7598 (75.98%)** | Conventional frozen 3-fold competition-label OOF (`experiments/folds.csv`) |
| **MODEL V2** | External Reference Transfer (LightGBM) | Frozen **selected WYH personal model** | 4106 / 5000 = **0.8212 (82.12%)** | Honest external-reference validation on the 5000 competition-train cells |
| **V3 program** | — | **Completed.** MODEL V3 was **not created.** | Best experimental standalone: E06M M2 4109 / 5000 = 0.8218 | Not promoted |

Internal frozen identifier for MODEL V2: **`V2-B-REFONLY`**. Preserve that
string in artifacts, scripts, tests, and tags. Do not rename frozen files.

There is **no public modeling stage after V3**.

Do not treat diagnostic oracle coverage as deployable accuracy.

No verified official leaderboard score is attributable specifically to
MODEL V1 or MODEL V2 in this personal repository. Do not invent
captain-submission provenance.

---

# 2. Repository and branch roles

## Personal development source of truth

Repository: `Ywan033/Hackathon-Summer-2026`

Branch: `ywan/ml-pipeline`

This branch is the source of truth for:

- frozen MODEL V1
- frozen MODEL V2
- the completed V3 research program
- validation contracts
- error analysis
- documentation
- version tags `model-v1` and `model-v2`

Do not merge archival documentation into personal `main` as a substitute for
this branch. Personal `main` should remain relatively close to upstream.

## Team collaboration / delivery repository

Repository: `X0X0X00/Hackathon-Summer-2026`

WYH team branch: `wyh` (`team/wyh`)

`team/wyh` is **not** the daily development branch. Only mature, validated
release checkpoints from `ywan/ml-pipeline` were integrated there.

Purpose of `team/wyh`:

- team collaboration
- reproducibility
- ensemble use
- contribution traceability
- mature code delivery
- sharing evaluation / test probability artifacts

Do not create a separate `ywan/ml-pipeline` branch in the team repository.

Never force-push `team/wyh`.

## Team branch mapping

When inspecting teammate work, use remote-tracking branches as read-only
references:

- `team/main` — zzh / team integration line
- `team/revert-1-lzh` — inspect especially `/work`; lzh method line
- `team/yhh` — yhh method line
- `team/wyh` — WYH mature team delivery branch

Files visible on a teammate branch may have been inherited from shared team
history. Do not automatically attribute every visible file, model, or method
on a branch to that teammate. Use branch-specific commits, reports, README
changes, and experiments when making contribution attributions.

## Captain official-submission repository

Repository: `Baiyu-ying05/Hackathon-Summer-2026`

This repository received the team-selected daily candidate for official
leaderboard scoring.

Do not modify the captain repository unless explicitly authorized.

Do not treat a local validation score as an official leaderboard score.

---

# 3. Competition task

Predict `MERFISH_cell_type_annotation` for the official MERFISH test cells.

- 5000 train cells
- 5000 test cells
- 200 competition genes
- 60 cell-type classes
- official metric: overall accuracy

Official files:

- `data/counts_train.csv`
- `data/counts_test.csv`
- `data/meta_train.csv`
- `data/meta_test.csv`

Test targets must remain hidden.

Cell_ID must always remain a lossless string. Never cast the 19-digit Cell_ID
values through floating-point representations.

---

# 4. Frozen MODEL V1

| Item | Value |
|---|---|
| Public name | Hierarchical Signature Specialists |
| Internal origin | Frozen YW-004 full-train per-signature specialists |
| Tag | `model-v1` |
| Documentation | `docs/versions/model_v1.md` |
| Result | **0.7598** (3799 / 5000) |
| Evaluation | Frozen 3-fold competition-label OOF (`experiments/folds.csv`) |
| Status | Frozen historical baseline |
| Official score | No verified official leaderboard score is attributable specifically to MODEL V1 in this personal repository |

Architecture summary: `(Region, E/I, Segment)` signature routing; single-class
signatures are deterministic; ambiguous signatures use per-signature multinomial
L2 logistic regression on log1p official 200-gene counts; fallback is a global
gene-only logistic regression.

Do not modify MODEL V1 architecture, submission candidate, metrics, release
tag, or frozen V1 validation folds unless the user explicitly authorizes a
formal correction.

---

# 5. Frozen MODEL V2

| Item | Value |
|---|---|
| Public name | External Reference Transfer (LightGBM) |
| Internal frozen ID | `V2-B-REFONLY` |
| Tag | `model-v2` |
| Documentation | `docs/versions/model_v2.md` |
| Result | **0.8212** (4106 / 5000) |
| Macro-F1 | approximately 0.7936 |
| Evaluation | Honest external-reference validation on the 5000 competition-train cells |
| Fold file | `experiments/team_folds_5_seed42.csv` (robustness / evaluation partition) |
| Status | Frozen selected WYH personal model |
| Official score | No verified official leaderboard score is attributable specifically to MODEL V2 in this personal repository |

LightGBM is fit **only on the 136,574 cleaned Zenodo reference rows**.
Competition training labels are **not** the boosting target. Do **not** call
0.8212 conventional competition-label OOF.

Fold accuracies: 0.822 / 0.831 / 0.812 / 0.821 / 0.820.

Slice performance: hard metadata-missing bucket 0.7569; neuron 0.9198;
glial / non-neuronal 0.7629.

Protocol-matched comparator: **BRIDGE-YW004-5F = 0.7596**.
Protocol-matched improvement:

`0.8212 − 0.7596 = +0.0616` (**+6.16 percentage points**)

The descriptive **cross-protocol** difference between MODEL V1 3-fold OOF
0.7598 and MODEL V2 external-reference validation 0.8212 is **+0.0614 /
+6.14 percentage points**. That +6.14 pp figure is not the protocol-matched
improvement.

A predeclared V2-C blend (C1, 0.8224) was evaluated and **rejected**. MODEL V2
remains `V2-B-REFONLY`.

## Attribution nuance

The LightGBM specification / defaults used by MODEL V2 are documented as
deriving from the `team/main` `ext_refonly.py` implementation
(`reports/V2-B-REFONLY_methodology.json`: `read_only_source` =
`team/main via git show`).

WYH owns the personal MODEL V2 freeze, external-reference evaluation
contract, reproducibility, leakage-control implementation, artifacts, and
model-selection evidence.

That is distinct from originating every underlying modeling component or
default. Do not claim that WYH originated the reference-LightGBM modeling
idea.

---

# 6. MODEL V2 development history (frozen identifiers)

Preserve these internal IDs exactly.

## BRIDGE-YW004-5F

Evaluate frozen YW-004 under the team-compatible 5-fold partition.
Accuracy: `0.7596`. Bridge benchmark, not a new formal MODEL version.

## V2-A-SPATIAL-LGBM

Competition-only spatial LightGBM (normalized genes, PCA, same-section
spatial neighbors, expression-space neighbors, fold-safe neighbor-label
histograms, metadata, E/I constraint). Best recorded accuracy: `0.7690`.

## V2-B-REFONLY

Approved external-reference route. Accuracy: `0.8212`. Selected MODEL V2.

## V2-C fixed blend audit

Only five predeclared probability blends were evaluated. No arbitrary weight
search. Strongest blend C1 (Reference 0.75 / Spatial 0.25) scored `0.8224`
and was rejected (+6 net cells, mixed folds, folds 3–4 regressed, lower
macro-F1). Do not replace MODEL V2 with that blend without a new formal
experiment.

---

# 7. Completed V3 research program

MODEL V3 was **not created**. The V3 program is **completed**.

Authoritative summary: `reports/v3/v3_research_program_summary.md`

Best standalone experimental candidate: **E06M M2 = 4109 / 5000 = 0.8218**.
Promotion was **not justified** (net +3 vs MODEL V2; McNemar p = 0.8895;
folds 3–4 net −9; bootstrap CI includes 0). MODEL V2 remains selected.

Diagnostic oracle coverage (for example five-expert 4393 / 5000 = 0.8786, or
the earlier three-expert 4414 / 5000 = 0.8828) is a **retrospective coverage
ceiling**. It requires ground-truth knowledge and a perfect selector. It is
not deployable accuracy, not OOF accuracy, and not a MODEL V3 score.

Do not start a public MODEL V4, routing, blending, or additional reference
stage from this contract.

Historical V3 stage IDs `V3-E00T` … `V3-E07D` are frozen experiment
identifiers. Preserve them.

---

# 8. External reference dataset

Approved source: Zenodo record `18039571`

File: `MERFISH_spinal_cord_resolved_0718.h5ad`

Local path: `work/external/MERFISH_spinal_cord_resolved_0718.h5ad`

MD5: `ce06f62c0ec4973581dae17bb76f0cd9`

Raw reference: 146,621 cells × 500 genes. All 200 competition genes are
available and must be aligned in exact official competition order. All 60
competition classes are represented.

## Required exclusion policy

Before reference modeling, remove:

- all 5000 competition train Cell_IDs
- all 5000 competition test Cell_IDs
- all exact aligned 200-gene count-vector duplicates

Historical independently reproduced exact-vector duplicates: `47`

Final usable reference cells: `136,574`

## External-data Git policy

Never commit:

- `*.h5ad`
- raw external datasets
- `work/external/cache/*.npz`
- disposable external preprocessing caches

The following may be committed: provenance metadata, MD5 metadata, exclusion
manifests, alignment audits, and reproducible preparation/modeling code.

Additional V3 source `SNI_merged_0917.h5ad` is documented in
`docs/data_provenance.md` and V3-E04S/E06M reports. It is not a MODEL V2
training source.

---

# 9. Validation protocols

There are **two** frozen protocols. They are not interchangeable.

Full definitions: `docs/validation_protocol.md`

## MODEL V1 — competition-label 3-fold OOF

File: `experiments/folds.csv`

```text
StratifiedKFold(n_splits=3, shuffle=True, random_state=20260819)
```

Competition labels **are** the fitting target. Each train-cell prediction is
genuinely out-of-fold. Result: `0.7598`. Do not overwrite this file.

## MODEL V2 / V3 — team-compatible 5-fold partition

File: `experiments/team_folds_5_seed42.csv`

```text
StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
```

For MODEL V2 this file is the common **evaluation / robustness partition**.
LightGBM is not fit on competition labels. Predictions on the 5000
competition-train cells are honest **external-reference validation**, not
conventional competition-label OOF. Result: `0.8212`.

V3 experimental candidates used the same partition unless a frozen report
says otherwise. Teammate comparators may have native protocols (LZH Prior-H
is native 3-fold).

Do not overwrite either fold file.

---

# 10. Leakage rules

Never use competition test labels.

Held-out competition fold labels must remain invisible from:

- model fitting
- label-derived neighbor features
- candidate masks
- prototypes
- routing logic learned from targets
- class masks learned from targets
- feature selection using targets
- post-processing threshold tuning
- ensemble weight tuning

Reference labels may remain visible when approved external-reference use is
permitted.

Competition test features may be used only where allowed as unlabeled
features. Any transductive use of train + test features must be explicitly
documented.

---

# 11. Overfitting / model-selection / promotion principles

Do not:

- select lucky seeds
- repeatedly change CV splits because a score is higher
- perform broad post-hoc blend-weight search on full evaluation predictions
- optimize arbitrary ensemble weights with scipy, greedy search, or
  Nelder-Mead
- fit a meta-model and report performance on the same evaluation used to
  fit it
- tune specialists on the same held-out cells used for final reporting
- use leaderboard feedback as the primary model-selection signal
- create a formal MODEL number for a statistically unsupported gain

Prefer:

- predeclared experiments
- fixed folds
- nested selection when labels are used for model decisions
- locked holdout / stability partitions
- conservative acceptance criteria
- simpler models when gains are within expected CV noise
- preservation of negative results

MODEL V2 rejected a nominally higher 0.8224 blend. V3 rejected M2 (0.8218)
as a version. Preserve that promotion standard.

Formal MODEL V1 / MODEL V2 are frozen releases. Experiment IDs such as
YW-004, V2-A, V2-B, V2-C, M2, and V3-E00T…V3-E07D are experimental
components, not additional formal versions.

Do not create a new formal MODEL Vn unless the user explicitly authorizes it
and the historical release standard in the frozen documentation is met.

---

# 12. Submission safety

Personal model development must **not** modify:

`prediction/prediction.csv`

That file belongs to the team/captain submission workflow.

Personal candidates belong under `outputs/submissions/`, for example:

- `outputs/submissions/model_v1.csv`
- `outputs/submissions/model_v2_candidate.csv`

A candidate is not an official leaderboard submission.

No verified official leaderboard score is attributable specifically to
MODEL V1 or MODEL V2 in this personal repository unless exact
captain-submission provenance is later established. Do not invent that
provenance.

---

# 13. Attribution boundaries

WYH-owned on `ywan/ml-pipeline`:

- validation contracts and Cell_ID integrity
- MODEL V1 / YW-004 implementation and freeze
- MODEL V2 personal freeze and reproducibility
- external-reference evaluation pipeline
- exclusion / leakage safeguards
- MODEL V2 probability / artifact generation
- V3 E00T–E07D research and analysis
- SNI / M2 experimentation where evidenced
- contribution / reproducibility infrastructure
- personal-branch documentation and team-handoff artifacts where evidenced

Not WYH-owned:

- LZH Prior-H (`depth_masked_prior_h_anchor`)
- YHH models
- `team/main` integration
- team final ensemble
- `prediction/prediction.csv` unless exact provenance later proves otherwise

YHH cell-level OOF was recorded as unavailable during the V3 program. Do not
retroactively rewrite that V3-era statement.

---

# 14. Git safety rules

Before every task:

```text
git branch --show-current
git status --short
```

Do not perform any of the following unless explicitly authorized:

- git commit
- git push
- git merge
- git rebase
- branch checkout/switch
- git reset
- git clean
- tag creation/deletion
- force push

Never run `git push --force` on team branches.

Never directly modify `team/main` during normal WYH work.

Do not modify:

- `prediction/prediction.csv`
- official `data/*.csv`
- organizer-provided README content below the WYH block separator
- frozen MODEL V1 / V2 / V3 numeric artifacts
- tags `model-v1` / `model-v2`
- historical internal experiment IDs in filenames

---

# 15. Working with team branches

Before reading current teammate work:

```text
git fetch team --prune
```

Use remote-tracking branches as read-only methodology references. Prefer:

```text
git show team/main:path/to/file
git show team/yhh:path/to/file
git show team/revert-1-lzh:path/to/file
```

Do not checkout teammate branches merely to inspect their code.

---

# 16. First-run handoff procedure

When an agent first opens this repository on a new machine, read in order:

1. `AGENTS.md`
2. `README.md`
3. `docs/versions/model_v1.md`
4. `docs/versions/model_v2.md`
5. `docs/validation_protocol.md`
6. `reports/v3/v3_research_program_summary.md`
7. `docs/contributions/wyh_contribution_summary.md`
8. recent Git history
9. current Git branch / status

Before modifying any file, summarize:

- repository and branch roles
- frozen MODEL V1
- frozen MODEL V2
- completed V3 program (MODEL V3 not created)
- validation-protocol distinction (3-fold OOF vs external-reference validation)
- external-reference provenance
- leakage rules
- attribution boundaries

Do not modify files during that initial handoff audit.

---

# 17. New-machine environment check

After cloning:

```text
git branch --show-current
git status --short
git tag --list
```

Expected development branch: `ywan/ml-pipeline`

Expected frozen tags: `model-v1`, `model-v2`

Install dependencies from `requirements.txt`. Do not copy another machine’s
`.venv`.

Copy the approved external reference separately to
`work/external/MERFISH_spinal_cord_resolved_0718.h5ad` and verify MD5
`ce06f62c0ec4973581dae17bb76f0cd9` before any reference reproduction.

Run the relevant test suite before changing validation or release contracts.
Do not retrain frozen models unless explicitly authorized.

---

# 18. Archival working style

Follow an evidence-driven workflow. Distinguish:

- research hypothesis
- engineering implementation
- empirical evidence
- inference / speculation

Negative results are part of the frozen record.

Do not optimize only for a higher single validation number.

Prioritize:

- robust performance
- reproducibility
- leakage control
- overfitting control
- team usefulness
- contribution traceability
- protection of frozen artifacts

Public documentation polish of the README WYH block is a separately
authorized archival phase. Do not rewrite `README.md` unless explicitly
asked.
