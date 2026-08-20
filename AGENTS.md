# AGENTS.md — Hackathon Summer 2026 ML Development Guide

This file defines the persistent development, validation, release, collaboration,
and Git rules for the WYH modeling track of the University of Rochester
Biomedical Data Science Hackathon Summer 2026.

Codex must read this file before making changes.

---

# 1. Repository and Branch Roles

## Personal development source of truth

Repository:

`Ywan033/Hackathon-Summer-2026`

Development branch:

`ywan/ml-pipeline`

This is the primary development source of truth for:

- MODEL V1
- MODEL V2
- MODEL V3+
- experiments
- validation
- error analysis
- documentation
- version releases
- release tags

All new MODEL V3+ research and development should happen here unless explicitly
instructed otherwise.

Do NOT merge routine modeling development into personal `main` during the
competition.

Personal `main` should remain relatively close to upstream.

---

## Team collaboration / delivery repository

Repository:

`X0X0X00/Hackathon-Summer-2026`

WYH team branch:

`wyh`

The team `wyh` branch is NOT the daily development branch.

Only mature, validated release checkpoints from `ywan/ml-pipeline` should be
integrated into `team/wyh`.

The purpose of `team/wyh` is:

- team collaboration
- reproducibility
- ensemble use
- contribution traceability
- mature code delivery
- sharing OOF/test probability artifacts

Do NOT create a separate `ywan/ml-pipeline` branch in the team repository.

Never force-push `team/wyh`.

---

## Team branch mapping

When analyzing teammate work, use the following mapping:

- `team/main`
  - zzh's main work / current team integration line

- `team/revert-1-lzh`
  - inspect especially `/work`
  - lzh's work / method line

- `team/yhh`
  - yhh's work / method line

- `team/wyh`
  - WYH mature team delivery branch

Important:

Files visible on a teammate branch may have been inherited from shared team
history.

Do NOT automatically attribute every visible file, model, or method on a branch
to that teammate.

Use branch-specific commits, reports, README changes, and experiments when
making contribution attributions.

---

## Captain official-submission repository

Repository:

`Baiyu-ying05/Hackathon-Summer-2026`

This repository receives the team-selected daily candidate for official
leaderboard scoring.

Do NOT modify the captain repository unless explicitly authorized.

Do NOT treat a local OOF score as an official leaderboard score.

---

# 2. Competition Task

Task:

Predict:

`MERFISH_cell_type_annotation`

for the official MERFISH test cells.

Competition data:

- 5000 train cells
- 5000 test cells
- 200 competition genes
- 60 cell-type classes

Competition metric:

overall accuracy

Official competition files:

- `data/counts_train.csv`
- `data/counts_test.csv`
- `data/meta_train.csv`
- `data/meta_test.csv`

Test targets must remain hidden.

Cell_ID must always remain a lossless string.

Never cast the 19-digit Cell_ID values through floating-point representations.

---

# 3. Frozen MODEL V1

Release tag:

`model-v1`

Documentation:

`docs/versions/model_v1.md`

Selected architecture:

YW-004 Hierarchical Signature Specialists

Validation:

- frozen 3-fold protocol
- OOF accuracy: `0.7598`
- correct: `3799 / 5000`

Architecture:

`(Region, E/I, Segment)` signature routing

Single-class signatures:

→ deterministic prediction

Ambiguous signatures:

→ per-signature multinomial L2 Logistic Regression specialist

Gene features:

→ log1p-transformed official 200-gene counts

Fallback:

→ global gene-only Logistic Regression

MODEL V1 is frozen.

Do NOT modify:

- MODEL V1 architecture
- MODEL V1 submission candidate
- MODEL V1 metrics
- MODEL V1 release tag
- frozen V1 validation folds

unless the user explicitly authorizes a new formal correction or version.

---

# 4. Frozen MODEL V2

Release tag:

`model-v2`

Documentation:

`docs/versions/model_v2.md`

Final selected architecture:

`V2-B-REFONLY`

Model family:

Reference-only LightGBM

Team-compatible 5-fold OOF:

`0.8212`

Correct cells:

`4106 / 5000`

Macro-F1:

approximately `0.7936`

Fold accuracies:

- Fold 0: `0.822`
- Fold 1: `0.831`
- Fold 2: `0.812`
- Fold 3: `0.821`
- Fold 4: `0.820`

Slice performance:

- hard metadata-missing bucket: `0.7569`
- neuron: `0.9198`
- glial / non-neuronal: `0.7629`

Delta:

- vs MODEL V1 `0.7598`: `+0.0614`
- vs BRIDGE `0.7596`: `+0.0616`
- vs V2-A `0.7690`: `+0.0522`

Official leaderboard score:

`Not submitted`

MODEL V2 is frozen.

---

# 5. MODEL V2 Development History

## BRIDGE-YW004-5F

Purpose:

Evaluate frozen YW-004 under the team-compatible 5-fold protocol.

OOF:

`0.7596`

This is a bridge benchmark, not a new formal MODEL version.

---

## V2-A-SPATIAL-LGBM

Competition-only Spatial LightGBM.

Key information:

- normalized gene features
- PCA
- same-section spatial neighbors
- expression-space neighbors
- fold-safe neighbor-label histograms
- metadata
- LightGBM
- E/I constraint

Best OOF:

`0.7690`

V2-A improved over BRIDGE but did not reach reference-model performance.

---

## V2-B-REFONLY

Approved external-reference route.

OOF:

`0.8212`

This became the final MODEL V2.

---

## V2-C fixed blend audit

Only five predeclared probability blends were evaluated.

No arbitrary weight search was allowed.

The strongest blend:

C1

- Reference: `0.75`
- Spatial: `0.25`
- OOF: `0.8224`

It was rejected because:

- only `+6` net correct cells
- folds 3–4 regressed
- fold behavior was mixed
- macro-F1 decreased
- extra complexity was not justified

Therefore:

MODEL V2 remains `V2-B-REFONLY`.

Do NOT replace MODEL V2 with the 0.8224 blend without a new formal experiment.

---

# 6. External Reference Dataset

Approved source:

Zenodo record:

`18039571`

File:

`MERFISH_spinal_cord_resolved_0718.h5ad`

Expected local path:

`work/external/MERFISH_spinal_cord_resolved_0718.h5ad`

MD5:

`ce06f62c0ec4973581dae17bb76f0cd9`

Raw reference:

- 146,621 cells
- 500 genes

Competition genes:

- 200 / 200 available
- must be aligned in exact official competition order

Reference taxonomy:

- all 60 competition classes represented

---

## Required exclusion policy

Before reference modeling, remove:

- all 5000 competition train Cell_IDs
- all 5000 competition test Cell_IDs
- all exact aligned 200-gene count-vector duplicates

Historical independently reproduced exact-vector duplicates:

`47`

Final usable reference cells:

`136,574`

These counts were independently reproduced.

---

## External-data Git policy

Never commit:

- `*.h5ad`
- raw external datasets
- `work/external/cache/*.npz`
- disposable external preprocessing caches

The following may be committed:

- provenance metadata
- MD5 metadata
- exclusion manifests
- alignment audits
- reproducible preparation/modeling code

---

# 7. Validation Protocols

## MODEL V1 frozen protocol

File:

`experiments/folds.csv`

Protocol:

StratifiedKFold(
    n_splits=3,
    shuffle=True,
    random_state=20260819
)

Do not overwrite.

---

## MODEL V2+ team-compatible protocol

File:

`experiments/team_folds_5_seed42.csv`

Protocol:

StratifiedKFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)

Use this protocol for team-compatible MODEL V2+ comparisons unless a new
validation design is explicitly approved.

---

# 8. Leakage Rules

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

Competition test features may be used only where allowed as unlabeled features.

Any transductive use of train + test features must be explicitly documented.

---

# 9. Overfitting / Model-Selection Rules

Do NOT:

- select lucky seeds
- repeatedly change CV splits because a score is higher
- perform broad post-hoc blend-weight search on full OOF
- optimize arbitrary ensemble weights with scipy
- optimize arbitrary ensemble weights with greedy search
- optimize arbitrary ensemble weights with Nelder-Mead
- fit a meta-model and report performance on the same OOF used to fit it
- tune specialists on the same held-out cells used for final reporting
- use leaderboard feedback as the primary model-selection signal

Prefer:

- predeclared experiments
- fixed folds
- nested selection when labels are used for model decisions
- locked holdout validation
- conservative acceptance criteria
- simpler models when gains are within expected CV noise

MODEL V2 intentionally rejected a nominally higher 0.8224 blend because the
gain was not sufficiently robust.

Preserve this model-selection standard.

---

# 10. Submission Safety

Personal model development must NOT modify:

`prediction/prediction.csv`

This belongs to the team/captain submission workflow.

Personal candidates belong under:

`outputs/submissions/`

Examples:

- `outputs/submissions/model_v1.csv`
- `outputs/submissions/model_v2_candidate.csv`

A candidate is not an official leaderboard submission.

Official score must be reported as:

`Not submitted`

unless the exact model was actually selected into the captain repository and
officially scored.

---

# 11. Formal Version Release Standard

Every formal MODEL Vn release must contain:

1. reproducible implementation
2. fixed validation protocol
3. OOF predictions
4. OOF probabilities
5. test probabilities
6. machine-readable metrics
7. fold scores
8. slice metrics
9. error analysis
10. leakage audit
11. overfitting / model-selection audit
12. validated submission candidate
13. version-specific documentation:
   `docs/versions/model_vn.md`
14. root README update
15. implementation / experiment / release commits
16. annotated tag:
   `model-vn`
17. push to:
   `origin/ywan/ml-pipeline`

Only after the version is mature should it be integrated into:

`team/wyh`

Every formal version must have professional README/documentation.

Do not create formal MODEL numbers for every experiment.

Experiment IDs such as:

- YW-004
- V2-A
- V2-B
- V2-C

are experimental components.

MODEL V1 / MODEL V2 / MODEL V3 are formal frozen releases.

---

# 12. Current Team Model Landscape

Use this only as current research context.

Always re-fetch remote team branches before making decisions.

## zzh / team main

Primary role:

strong team anchor / integration line

Historical strategy includes:

- spatial / expression kNN features
- LightGBM
- bagging
- external-reference members
- MLP members
- ensemble integration

Current team main may advance frequently.

Always run:

`git fetch team --prune`

before comparing current methods.

---

## lzh

Inspect:

`team/revert-1-lzh`

especially:

`work/`

Recent work may include:

- biology priors
- graph residuals
- Gene-token representations
- MNN / graph methods
- conservative gated corrections

Do not assume lzh validation uses the exact same fold protocol as WYH.

---

## yhh

Inspect:

`team/yhh`

Recent work includes:

- strong reference ensemble
- hierarchical specialists
- glial / meninges / subtype corrections

Reported high OOF results require validation under common member files / folds
before direct comparison.

---

## wyh

Team delivery branch:

`team/wyh`

Contains mature WYH MODEL V1 and MODEL V2 contributions.

Daily research development remains on:

`origin/ywan/ml-pipeline`

---

# 13. MODEL V3 — Current Research State

MODEL V3 is NOT defined or frozen yet.

Do NOT immediately train another large model.

The first MODEL V3 task should be:

## Canonical cross-model complementarity / headroom audit

Candidate experts:

- zzh / team anchor
- lzh current strong model
- yhh current strong model
- WYH MODEL V2

Before comparing, align exactly:

- Cell_ID
- fold definitions where possible
- official 60-class ordering
- OOF probability format
- evaluation population

Compute:

- individual accuracy
- pairwise disagreements
- pairwise oracle accuracy
- 3-model oracle accuracy
- 4-model oracle accuracy
- exclusive-correct cells
- all-model-wrong cells
- slice-level complementarity
- major confusion families

Oracle accuracy is diagnostic headroom only.

It is NOT a model score.

---

# 14. Existing WYH Headroom Evidence

Internal WYH experts:

- hierarchical expert
- spatial expert
- reference expert

Three-expert diagnostic oracle:

`0.8828`

Correct:

`4414 / 5000`

Reference MODEL V2 correct:

`4106 / 5000`

Oracle headroom:

`308 cells`

Exclusive correct:

- only hierarchical expert: `111`
- only spatial expert: `90`
- only reference expert: `241`

All three wrong:

`586`

This demonstrates meaningful expert complementarity.

However:

static fixed probability blending did NOT convert this oracle headroom into a
robust improvement.

This motivates testing cell-adaptive reliability modeling.

---

# 15. Initial MODEL V3 Research Hypothesis

Potential direction:

## Reliability-Gated / Cell-Adaptive Expert Routing

Research question:

Can test-time-safe reliability signals identify which expert should be trusted
for a given MERFISH cell better than static probability averaging?

Potential expert families:

- reference expert
- spatial expert
- hierarchical expert
- future independently validated team experts

Potential reliability signals:

- expert entropy
- top1 probability
- top1-top2 margin
- expert disagreement
- reference-neighbor consensus
- reference distance / density
- spatial-neighbor agreement
- expression-neighbor agreement
- Region
- E/I
- Segment
- metadata missingness
- hard-bucket flag
- library size
- detected-gene count
- graph degree / graph confidence

Do NOT build the router simply because it sounds advanced.

First prove sufficient cross-model oracle headroom under a canonical evaluation
protocol.

---

# 16. Potential Future External References

Do not add new external datasets blindly.

The current approved same-study Zenodo reference has the smallest domain gap and
must remain the primary reference.

Potential future references may include mouse spinal-cord atlases, but every new
source must pass:

- biological compatibility
- gene coverage
- taxonomy mapping
- source provenance audit
- overlap / duplicate audit
- distribution-shift analysis
- fixed-OOF transfer validation
- complementarity analysis

Do not concatenate multiple external datasets without source-aware auditing.

---

# 17. Git Safety Rules

Before every task:

git branch --show-current
git status --short

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

Never run:

`git push --force`

on team branches.

Never directly modify:

`team/main`

during normal WYH development.

---

# 18. Working with Team Branches

Before reading current teammate work:

git fetch team --prune

Use remote-tracking branches as read-only methodology references.

Prefer:

git show team/main:path/to/file

git show team/yhh:path/to/file

git show team/revert-1-lzh:path/to/file

Do not checkout teammate branches merely to inspect their code.

---

# 19. Codex First-Run Handoff Procedure

When Codex first opens this repository on a new machine:

Read, in order:

1. `AGENTS.md`
2. `README.md`
3. `docs/versions/model_v1.md`
4. `docs/versions/model_v2.md`
5. recent Git history
6. experiment registries
7. current Git branch/status

Before modifying any file, Codex must summarize:

- repository and branch roles
- frozen MODEL V1
- frozen MODEL V2
- validation protocols
- external-reference provenance
- leakage rules
- formal release standard
- team branch mapping
- current MODEL V3 research question

Do not modify files during this initial handoff audit.

---

# 20. New-Machine Environment Check

After cloning the repository, verify:

git branch --show-current
git status --short
git tag --list

Expected development branch:

`ywan/ml-pipeline`

Expected frozen tags:

- `model-v1`
- `model-v2`

Install dependencies from:

`requirements.txt`

Do NOT copy the old machine's `.venv`.

Copy the approved external reference separately to:

`work/external/MERFISH_spinal_cord_resolved_0718.h5ad`

Verify the MD5 before any reference modeling:

`ce06f62c0ec4973581dae17bb76f0cd9`

Run the full test suite before MODEL V3 development.

---

# 21. Research Working Style

Follow an evidence-driven workflow:

Problem / hypothesis
→ controlled experiment
→ fixed validation
→ error analysis
→ decision
→ reproducibility
→ documentation
→ formal release only when mature

Distinguish clearly:

- research hypothesis
- engineering implementation
- empirical evidence
- inference / speculation

Negative results are valuable and should be recorded.

Do not optimize only for a higher single OOF number.

Prioritize:

- robust performance
- reproducibility
- leakage control
- overfitting control
- team usefulness
- contribution traceability
