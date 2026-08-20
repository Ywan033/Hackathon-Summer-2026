# WYH — MODEL V1: Hierarchical Signature Specialists

MODEL V1 is the first frozen release of the WYH modeling pipeline for the 60-class MERFISH cell-type classification task.

## Validation result

Frozen 3-fold protocol: `StratifiedKFold(n_splits=3, shuffle=True, random_state=20260819)`.

| Model | Method | 3-fold OOF accuracy | Macro-F1 |
|---|---|---:|---:|
| YW-001 | Gene-only Logistic Regression | 55.00% | 0.3894 |
| YW-002 | Gene + Metadata | 75.68% | 0.6799 |
| YW-003 | Candidate Masking | 75.30% | 0.7130 |
| **MODEL V1 / YW-004** | **Hierarchical Signature Specialists** | **75.98%** | **0.7053** |

MODEL V1 correctly classifies 3,799 / 5,000 cells under the frozen 3-fold OOF protocol. These are local out-of-fold validation results, not an official leaderboard score. MODEL V1 was not submitted for official scoring.

## MODEL V1 method

MODEL V1 uses `(Region, E/I, Segment)` metadata signatures to route cells between deterministic and learned specialists.

- Single-class signatures use deterministic routing.
- Ambiguous signatures use per-signature multinomial L2 logistic-regression specialists.
- Specialists use the 200 log1p-transformed gene-count features.
- Unseen or failed signatures use a global gene-only logistic-regression fallback.
- Signature construction and routing are fold-safe during OOF evaluation.

Full-train routing: **28** signatures (**15** deterministic, **13** ambiguous specialists, **0** failed specialists). Test routing: **797** deterministic, **4,203** specialist, **0** fallback.

## Why MODEL V1 was selected

Gene-only logistic regression established a 55.00% OOF baseline. Adding anatomical metadata increased OOF accuracy to 75.68%. Candidate masking reached 75.30%. Per-signature specialists achieved the strongest validated checkpoint at 75.98%.

Sprint 3 did not replace that architecture: exploratory ensemble diagnostics were not nested and did not beat YW-004; nested hard-bucket specialist search retained the original log1p logistic regression in every outer fold; the hybrid replacement changed no predictions. MODEL V1 therefore freezes YW-004.

## Error analysis

Remaining errors concentrate in the large metadata-missing / glial-non-neuronal regime. That fully missing `(Region, E/I, Segment)` bucket contains 2,958 / 5,000 cells, with YW-004 OOF accuracy 68.15%. Of 1,201 remaining OOF errors, 942 fall in that bucket.

Major residual confusion families:

- `oligodendrocyte_1` ↔ `oligodendrocyte_progenitor_2`
- `oligodendrocyte_2` ↔ `oligodendrocyte_progenitor_2`
- `astrocyte_2` → `astrocyte_1`

This error concentration motivates the spatial and reference-augmented MODEL V2 track.

## Reproduction

```bash
.venv/bin/python scripts/06_model_v1.py --overwrite
.venv/bin/pytest -q tests/
.venv/bin/python scripts/90_validate_submission.py outputs/submissions/model_v1.csv
.venv/bin/python scripts/10_official_manifest.py --verify
```

The candidate is `outputs/submissions/model_v1.csv`. Do not overwrite frozen MODEL V1 artifacts unless `--overwrite` is intended.

## Documentation and release artifacts

Full write-up: [docs/versions/model_v1.md](docs/versions/model_v1.md).

| Artifact | Path |
|---|---|
| Release tag | `model-v1` |
| Submission candidate | `outputs/submissions/model_v1.csv` |
| Test probabilities | `outputs/probabilities/model_v1_test_probabilities.csv.gz` |
| Run metrics | `outputs/metrics/model_v1_metrics.json` |

MODEL V1 is a frozen submission-ready candidate. It has not been selected or pushed to the captain repository as an official team submission.

---

# Original Hackathon Information

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
  
