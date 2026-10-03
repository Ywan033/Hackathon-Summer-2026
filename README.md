# MERFISH Cell-Type Annotation — WYH branch

This repository documents the WYH modeling contribution to 60-class MERFISH
cell-type annotation over a 200-gene panel. The work includes reproducible
validation, hierarchical classification, external-reference transfer,
controlled model selection, and V3 research. MODEL V2 is the selected
personal model. Team integration and official submission provenance are
separate.

The public `main` branch integrates the personal archive, a provenance-locked
snapshot of the team `wyh` branch's `work/` tree, and an index of personal
prediction artifacts. The frozen personal development source remains:
[`ywan/ml-pipeline`](https://github.com/Ywan033/Hackathon-Summer-2026/tree/ywan%2Fml-pipeline).

## Results Summary

| Stage | Method | Evaluation | Result | Decision |
|---|---|---|---:|---|
| MODEL V1 | Hierarchical Signature Specialists | 3-fold competition-label OOF | 75.98% | Historical baseline |
| **MODEL V2** | External Reference Transfer (LightGBM) | External-reference validation | **82.12%** | **Selected personal model** |
| E06M M2 | Source-Balanced Multi-Reference LightGBM | V3 external-validation protocol | 82.18% | Experimental; not promoted |

Evaluation protocols differ across stages and are not one homogeneous
cross-validation protocol.

**Protocol-matched comparison** (same 5-fold partition,
`experiments/team_folds_5_seed42.csv`):

| Comparator | Result |
|---|---:|
| BRIDGE-YW004-5F (V1 architecture on the 5-fold file) | 75.96% |
| MODEL V2 | 82.12% |
| Improvement | **+6.16 percentage points / +308 correct cells** |

The descriptive difference between MODEL V1 75.98% (3-fold OOF) and MODEL V2
82.12% (external-reference validation) is a **cross-protocol** comparison, not
the protocol-matched gain.

No verified official leaderboard score is attributed specifically to MODEL V1
or MODEL V2 in this personal repository.

## Integrated Archive

- **Personal source of truth:** frozen MODEL V1, MODEL V2, V3 research,
  validation contracts, reports, and artifacts inherited from
  `ywan/ml-pipeline` at
  `e3b5f323c5ab0de9aa1cf1e8f65dd530db79059b`.
- **Team delivery snapshot:**
  [`team_archive/wyh/work/`](team_archive/wyh/work/) is the exact tracked
  `work/` tree from `X0X0X00/Hackathon-Summer-2026`
  `wyh@f4ba901105adea195f5474132cf793688f14e2fa`. Scope, exclusions,
  checksums, attribution, and execution caveats are in
  [`team_archive/wyh/PROVENANCE.md`](team_archive/wyh/PROVENANCE.md).
  It is a snapshot of that branch, not every team branch or the complete
  final team solution.
- **Prediction versions:**
  [`prediction_versions/README.md`](prediction_versions/README.md) separates
  formal personal candidates, labeled validation/OOF artifacts, hidden-label
  test inference, V3 experiments, and non-personal files. The machine-readable
  [`manifest.csv`](prediction_versions/manifest.csv) records rows, output
  types, SHA-256, source commits, and metric evidence.
- **Organizer material:** the original challenge description remains below
  the separator in this README and in
  [`Data.Description.md`](Data.Description.md). It is retained for
  traceability.

## MODEL V2 — Selected Personal Model

Public method: **External Reference Transfer (LightGBM)**. Internal frozen
identifier: `V2-B-REFONLY`.

Classifier training uses the approved annotated external MERFISH reference
(Zenodo record 18039571). After documented exclusions, **136,574** cleaned
reference cells remain. Features and labels are aligned to the official
200-gene / 60-class competition space. File-level provenance, checksums, and
exclusion counts are in [`docs/data_provenance.md`](docs/data_provenance.md).

Competition-train labels are used only for external validation and are not
used to fit the LightGBM classifier. MODEL V2 reached **4106 / 5000 = 82.12%**.
Later candidates, including
a rejected predeclared V2-C blend (0.8224) and experimental E06M M2 (0.8218),
did not provide sufficient robust evidence for promotion. MODEL V2 remains
the selected WYH personal model.

MODEL V2 uses the reference-LightGBM family documented in `team/main`; the
WYH pipeline contributes the frozen personal evaluation, leakage controls,
reproducibility artifacts, and model-selection evidence. Detailed ownership
evidence is in [`docs/contributions/wyh_contribution_summary.md`](docs/contributions/wyh_contribution_summary.md).

- Model card: [`docs/versions/model_v2.md`](docs/versions/model_v2.md)
- Validation contracts: [`docs/validation_protocol.md`](docs/validation_protocol.md)
- Data provenance: [`docs/data_provenance.md`](docs/data_provenance.md)
- Submission candidate (not an official captain file):
  [`outputs/submissions/model_v2_candidate.csv`](outputs/submissions/model_v2_candidate.csv)

## Research Progression

```text
MODEL V1
Hierarchical Signature Specialists
75.98%  (3-fold competition-label OOF)
        ↓
MODEL V2
External Reference Transfer (LightGBM)
82.12%  (external-reference validation)
        ↓
V3 Research Program
Best standalone experimental candidate: E06M M2  82.18%
No MODEL V3 promotion
```

V3 investigated multi-reference transfer and complementary-expert strategies.
E06M M2 reached 82.18%, only three additional correct cells over MODEL V2.
The improvement did not satisfy the predefined robustness criteria, therefore
MODEL V3 was not created. Diagnostic oracle coverage values from that program
are retrospective ceilings, not deployable accuracy.

- Program summary: [`reports/v3/v3_research_program_summary.md`](reports/v3/v3_research_program_summary.md)
- V3 contribution record: [`docs/contributions/wyh_v3_contribution.md`](docs/contributions/wyh_v3_contribution.md)
- MODEL V1 card: [`docs/versions/model_v1.md`](docs/versions/model_v1.md)

## Reproducibility

Install pinned dependencies from `requirements.txt` (Python 3.9.x) and run the
official-data and freeze-contract tests. That is an integrity check, not a
one-command full model reproduction.

```sh
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
.venv/bin/python scripts/10_official_manifest.py --verify
.venv/bin/python scripts/00_validate_data.py
.venv/bin/python -m pytest tests/test_official_contract.py tests/test_folds.py \
  tests/test_model_v1.py tests/test_model_v2_release.py \
  tests/test_v3_research_program_closure.py -q
```

Full MODEL V1 / MODEL V2 retraining, external-reference placement, and
checksum steps are documented in [`docs/reproducibility.md`](docs/reproducibility.md).
Clean-clone archive integrity was verified at commit `89f9ef2`; see the
[`Phase D1 report`](reports/finalization/phase_d1_clean_clone_verification.md).
That historical integrity check was not a full from-scratch model
reproduction. The relocated team snapshot has separate path and external-data
requirements; copying it successfully does not establish that it runs from
its archive location.

## Repository Structure

| Path | Role |
|---|---|
| [`src/merfish60/`](src/merfish60/) | Library: data contracts, Cell_ID loaders, MODEL V1, reference exclusion, V2 helpers |
| [`scripts/`](scripts/) | Reproduction entry points for validation, MODEL V1, MODEL V2, and submission checks |
| [`experiments/`](experiments/) | Frozen fold files, experiment registries, V3 scripts; see [`experiments/README.md`](experiments/README.md) |
| [`outputs/`](outputs/) | Metrics, evaluation predictions, probabilities, submission candidates; see [`outputs/README.md`](outputs/README.md) |
| [`reports/`](reports/) | Sprint comparisons, V2 methodology JSON, V3 reports, finalization audits |
| [`docs/`](docs/) | Model cards, provenance, contribution records, documentation index |
| [`tests/`](tests/) | Data-contract, freeze, and V3-closure tests |
| [`data/`](data/) | Organizer-provided official challenge CSVs |
| [`prediction_versions/`](prediction_versions/) | Verified index of personal validation and test prediction artifacts |
| [`team_archive/wyh/`](team_archive/wyh/) | Provenance-locked team `wyh` `work/` snapshot; not the personal source tree |
| [`prediction/prediction.csv`](prediction/prediction.csv) | Organizer example submission; **not** a WYH model |

Documentation index: [`docs/README.md`](docs/README.md).
Final archival report: [`docs/final_project_report.md`](docs/final_project_report.md).

## Contribution Scope and Attribution

WYH contributions documented in this repository include validation and
data-contract infrastructure, MODEL V1, the MODEL V2 personal freeze, V3
research and analysis, leakage controls, and model-selection evidence.

Full attribution record: [`docs/contributions/wyh_contribution_summary.md`](docs/contributions/wyh_contribution_summary.md).
Files in the team snapshot retain upstream history and must not be interpreted
as proof that WYH authored every visible method or file.

## Limitations

Remaining errors concentrate among closely related glial subtypes, especially
when Region / E/I / Segment metadata are missing. MODEL V2 depends on a
matched same-study external reference and can be sensitive to domain shift.
V3 gains were too small and insufficiently stable to justify a new version.
Internal validation scores on the 5000 competition-train cells are distinct
from an official hidden-test or leaderboard score. Full MODEL V1 / MODEL V2
retraining and bit-for-bit MODEL V2 probability regeneration remain
unverified, and external `.h5ad` inputs must be obtained separately.

---

# University of Rochester Biomedical Data Science Hackathon Summer 2026
Welcome to the landing page for the hackathon. The hackathon will commence 8/18. It will be a prediction challenge. All predictions should be submitted through GitHub using the captain's handle. Scoring will also happen in GitHub. All details regarding the hackathon will be posted here.  

 Register for the hackathon [here](https://forms.gle/TEW1BHqezsKgTTKL9). Please make sure each individual competing on your team is fully registered. Each team needs a captain with a github handle. To receive a prize, you must supply your University of Rochester e-mail address. All teams scoring better than random will receive a participation prize. 1st and 2nd place winning teams in each division will get a cash prize (see below).
 **All team members must submit their own registration form to participate.**  

# Overview
This is a prediction challenge with spatial transcriptomics data. The objective of the hackathon is to correctly predict cell type labels in MERFISH_cell_type_annotation. Group performance will be measured by the confusion matrix overall accuracy: number of correct predictions / total number of predictions.

# Challenge description
The challenge is to classify cell types in a mouse neuronal tissue dataset collected using MERFISH, an imaging-based spatial transcriptomics technique that measures gene expression while preserving each cell's exact location in the tissue. The dataset covers ~10,000 cells and 200 genes, with sparse transcript counts paired with spatial coordinates and cell metadata (like cell volume, region, gender, and mouse ID). Participants must predict the cell type label for each cell in the test set. A full description of the challenge and dataset is here: [Data.Description.md](Data.Description.md).

# Logistics

0.   Each team must have a github handle associated with it in order to participate.  Make sure you edit your registration or email the organizers to provide this, if you haven't yet. Your team will not be scored if you do not provide a handle.
1.   You may add team members up
to noon EDT on 8/18 by editing your response to the google form or emailing the organizers.
2.  Teams of entirely undergraduates will be in the undergraduate
division, else they will be in the open division.
3. Further instructions for submitting predictions will be posted here as they become available
4.  Competition runs through 2:59 PM EDT 22-August-2026.  The predictions each team has committed to their repository at that time will be used to determine their final score. Captains must submit their own predictions. Any use of predictions from other teams is disqualifying. Winning teams must submit their code to organizers to claim their prize.

Scores will be posted shortly after 3pm EDT each day here [Leaderboard.Hackathon.2026.md](Leaderboard.Hackathon.2026.md). 

# Prizes
   
1.  First place in each division: $300 + $75 x (team size)
2.  Second place in each division: 0 + $50 x (team size)
  
