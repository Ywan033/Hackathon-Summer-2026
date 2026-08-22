# Phase C public-presentation review

Date: 2026-08-22
Branch: `ywan/ml-pipeline`
HEAD at start: `3f7ace3` (`docs: finalize model protocols, provenance, and attribution`)
Mode: public-facing documentation only. No model-logic, fold, metric, tag,
or team-branch changes. Not committed. Not pushed.

Classification:

1. intentional frozen / internal identifier
2. correct public statement
3. deferred competition-result or later-archive dependency
4. unresolved issue requiring correction

---

## 1. Files in this phase

Modified:

- `README.md` — WYH section rewritten; organizer body unchanged
- `tests/test_model_v2_release.py` — README title lock updated to the new
  public heading; organizer heading presence asserted; “Current Released
  Candidate” forbidden

Created:

- `docs/README.md`
- `docs/final_project_report.md`
- `docs/reproducibility.md`
- `experiments/README.md`
- `outputs/README.md`
- `CHANGELOG.md`
- `reports/finalization/phase_c_public_presentation_review.md` (this file)

---

## 2. Protocol terminology

| Location | Statement | Class |
|---|---|---|
| README results table | V1 = 3-fold competition-label OOF; V2 = external-reference validation; M2 = V3 external-validation protocol | 2 |
| README below table | Stages are not a homogeneous CV leaderboard | 2 |
| README protocol-matched table | BRIDGE-YW004-5F 75.96% vs MODEL V2 82.12% = +6.16 pp / +308 cells | 2 |
| README | V1 75.98% vs V2 82.12% labeled **cross-protocol**, not the protocol-matched gain | 2 |
| `docs/final_project_report.md` | Same two-protocol table as `docs/validation_protocol.md` | 2 |
| `outputs/README.md` | `oof/` directory name is historical; V2 files are external-reference validation labels | 2 |

No remaining public claim that MODEL V2 0.8212 is conventional
competition-label OOF.

---

## 3. MODEL V2 public name and selected status

| Check | Result | Class |
|---|---|---|
| Public name | External Reference Transfer (LightGBM) | 2 |
| Internal ID | `V2-B-REFONLY` retained | 1 |
| Status language | “Selected personal model”; “Current Released Candidate” removed | 2 |
| Result | 4106 / 5000 = 82.12% | 2 |
| Zenodo 18039571; 136,574 cells; MD5 `ce06f62c0ec4973581dae17bb76f0cd9` | Present | 2 |
| LightGBM defaults vs WYH freeze | Distinguished (`team/main` provenance vs personal freeze) | 2 |

---

## 4. V3 no-promotion status

| Check | Result | Class |
|---|---|---|
| Program completed | Stated | 2 |
| MODEL V3 created? | Explicitly **not created** | 2 |
| Best experimental standalone | E06M M2 82.18%; +3 cells vs V2; not promoted | 2 |
| Public modeling after V3 | Denied in `docs/README.md` and the final report | 2 |
| E06M M2 in the landing-page table | Present as **experimental; not promoted** (not a MODEL V3 row) | 2 |

Landing-page V3 text does not enumerate E00T–E07D. Detail remains in
`reports/v3/v3_research_program_summary.md`.

---

## 5. Official score wording

Public files state that no verified official leaderboard score is
attributable specifically to MODEL V1 or MODEL V2, and that scores remain
**Not submitted** unless exact captain-submission provenance is later
established.

That is class **3** (deferred), not an invented score.

---

## 6. Attribution boundaries

README and `docs/final_project_report.md` list WYH-owned work and explicitly
exclude YHH models, LZH Prior-H, `team/main` integration, the team final
ensemble, and `prediction/prediction.csv`.

Links: `docs/contributions/wyh_contribution_summary.md`.

No class-4 re-attribution found in the new public layer.

---

## 7. Links and paths

Markdown links in the new public files were resolved against the working
tree. After this review file exists, remaining relative links resolve.

Organizer README still links to `Data.Description.md`,
`Leaderboard.Hackathon.2026.md`, and the registration form. Those paths
were already in the unchanged organizer body.

No invented Zenodo URL was kept (record ID only, matching provenance docs).

---

## 8. Duplication

Landing-page numbers, the protocol-matched delta, and the V3 non-promotion
sentence recur in `docs/README.md`, `docs/final_project_report.md`, and
`CHANGELOG.md`. That is index/summary duplication, not a second unpublished
result table. Experiment-level V3 material is linked, not copied.

---

## 9. Stale current/future language

Gone from the live README:

- Current Released Candidate
- WYH Modeling Track (title)
- Reference-only LightGBM (public method label)
- +6.14 pp as the protocol-matched gain

`AGENTS.md` still uses “WYH Modeling Track” in its **internal** title. That
file was not in the Phase C edit list.

Frozen V3 reports and metrics JSON still say “reference-only LightGBM”.
Class **1/2**; not rewritten.

`CHANGELOG.md` records this phase as a working tree, not yet committed.
Class **2** at review time.

---

## 10. Accidental MODEL V3 / later-model / oracle-as-accuracy

| Check | Result | Class |
|---|---|---|
| MODEL V3 score claimed | No | 2 |
| MODEL V4 public stage | No | 2 |
| 0.8786 / 0.8828 as accuracy | Labeled diagnostic coverage only | 2 |
| `docs/versions/model_v3.md` | Still absent | 2 |
| tag `model-v3` | Still absent | 2 |

---

## 11. Organizer README preservation

Separator: one Markdown rule `---` then the organizer heading.

SHA256 of the organizer section (from
`# University of Rochester Biomedical Data Science Hackathon Summer 2026`
through EOF):

`56b3bc149f08c91b55e4c57aa36b4e1008592eb5ee8cd18ed67badd89caa0be4`

Byte-identical to the pre-Phase-C organizer body.

Forbidden wrapper headings (Original README / Organizer README /
Competition README / Original Hackathon Information): **absent**.

---

## 12. Tests and checks

| Check | Result |
|---|---|
| `git diff --check` | clean |
| `scripts/10_official_manifest.py --verify` | OK |
| `scripts/00_validate_data.py` | OK |
| pytest freeze/contract set (45 tests, excluding V1 training) | passed |
| `tests/test_model_v1.py` (14 tests; outside sandbox) | passed |
| Frozen metrics / folds / `prediction/prediction.csv` / official CSVs | unmodified |
| tags `model-v1`, `model-v2` | unchanged |

`tests/test_model_v1.py` bus-errored under the default sandbox (scipy/LBFGS)
and passed with unrestricted process permissions. That is an environment
constraint, not a documentation defect.

---

## 13. Deferred (class 3; not class 4)

- Official unseen-test / leaderboard score
- Whether MODEL V2 (or any other file) was the captain’s scored submission
- Exact commit/path provenance of any official score
- Team-final credit beyond this personal branch
- Clean-clone archival verification (explicitly later)
- GitHub release / public archive completion (not done)
- Optional polish of frozen V3 prose (`reference-only LightGBM`,
  `oracle accuracy` JSON keys)

No class-4 issue was found in the public-facing layer after this pass.
