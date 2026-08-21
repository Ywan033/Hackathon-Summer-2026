# WYH Modeling Track

## Released models

| Version | Method | Validation | OOF | Official score |
|---|---|---|---:|---|
| MODEL V1 | Hierarchical Signature Specialists | personal/frozen 3-fold | 75.98% | Not submitted |
| **MODEL V2** | **Reference-only LightGBM + approved Zenodo reference** | **team-compatible 5-fold** | **82.12%** | **Not submitted** |

## Current Released Candidate — MODEL V2

MODEL V2 is the current frozen WYH candidate. It is a reference-only LightGBM fit on 136,574 cleaned cells from the approved Zenodo MERFISH spinal-cord deposit (record 18039571; MD5 `ce06f62c0ec4973581dae17bb76f0cd9`). Local 5-fold OOF accuracy is **82.12%** (4106 / 5000), **+6.14 pp** versus MODEL V1 (75.98%). There is no official leaderboard score.

- Candidate: [`outputs/submissions/model_v2_candidate.csv`](outputs/submissions/model_v2_candidate.csv)
- Full write-up: [`docs/versions/model_v2.md`](docs/versions/model_v2.md)
- Test probabilities: `outputs/probabilities/V2-B-REFONLY_test_probabilities_seg.csv.gz`

A predeclared V2-C blend (C1, 0.8224) was evaluated and rejected: +6 net cells, mixed folds, folds 3–4 down, lower macro-F1. MODEL V2 therefore keeps the simpler reference-only architecture.

MODEL V1 remains frozen at 75.98% 3-fold OOF. Details: [`docs/versions/model_v1.md`](docs/versions/model_v1.md).

## V3 Research Program

The V3 research program is **completed**. MODEL V3 was **not created**. MODEL V2 remains the frozen personal deployable model.

- Program summary: [`reports/v3/v3_research_program_summary.md`](reports/v3/v3_research_program_summary.md)
- Contribution record: [`docs/contributions/wyh_v3_contribution.md`](docs/contributions/wyh_v3_contribution.md)

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
  
