# Team `wyh` branch snapshot provenance

This directory is an archival snapshot of the `work/` tree from the team
collaboration repository. It is not a claim that the `wyh` branch contains
every team branch, every teammate's work, the captain repository, or the
complete final team solution.

## Source

- Repository: <https://github.com/X0X0X00/Hackathon-Summer-2026>
- Branch: `wyh`
- Full source commit:
  `f4ba901105adea195f5474132cf793688f14e2fa`
- Source path: `work/`
- Archived path: `team_archive/wyh/work/`
- Personal archive base:
  `e3b5f323c5ab0de9aa1cf1e8f65dd530db79059b`

The import preserves the 210 tracked blobs from the source `work/` tree
without editing their contents. Their total source size is 2,706,526 bytes.
[`SNAPSHOT_MANIFEST.csv`](SNAPSHOT_MANIFEST.csv) records every source path,
archive path, byte size, source Git blob SHA-1, and content SHA-256.

## Scope and exclusions

No tracked file under source `work/` was filtered during this import. The
source tree contains no tracked `.h5ad`, external-data directory, environment,
cache, `*.npz`, or similar raw-data artifact.

Paths outside source `work/` were intentionally out of scope, including:

- the team repository's root `data/`;
- the team repository's root `prediction/prediction.csv`;
- the team repository's root README and challenge files; and
- material that exists only on other team branches or in the captain
  repository.

The source `work/README.md` states that large probability arrays, OOF dumps,
and parquet registries had already been removed from the team delivery tree
before this snapshot. They are therefore absent from the source, not silently
discarded by this import.

The team root prediction was not copied. At the source commit its SHA-256 is
`9e17512492aad325a3c3c06bef5c45d57c6b330883c4b396cddb7ad4fcae3e22`.
It differs from this personal repository's organizer example at
`prediction/prediction.csv` and is not evidence that MODEL V1 or MODEL V2 was
the official team submission.

## Relationship to the personal archive

Comparing each source `work/<relative-path>` with
`<relative-path>` at the personal archive base:

- 32 files are byte-identical;
- 53 paths exist in both places with different content, generally because the
  team delivery copy uses team-root-relative paths and cleanup adaptations;
- 125 files exist only in the team `work/` snapshot.

The two retained team candidate copies are byte-identical to the personal
candidate files:

- `work/experiments/evidence/submissions/model_v1.csv`:
  SHA-256 `c82acc6a444206eb48b5f4e9dec04ab3657892b7e6d4c1ab573d1dbd611553fd`;
- `work/experiments/evidence/submissions/model_v2_candidate.csv`:
  SHA-256 `8a110aec5d2b957e603cc3e3cbdc1e0402949790a0208dff0f285c14adc7c7ac`.

Those copies remain team-delivery evidence. The canonical personal candidates
remain under `outputs/submissions/`.

## Attribution and licensing

Files visible on `team/wyh` may be inherited from shared team history. Their
presence here does not by itself establish individual authorship. In
particular, the underlying reference-LightGBM method and defaults are
attributed in the personal documentation to the `team/main`
`ext_refonly.py` family; LZH and YHH model lines are not WYH-owned.

No `LICENSE`, `COPYING`, `NOTICE`, or `AUTHORS` file was present anywhere in
the source repository tree at the recorded commit. This snapshot preserves
all source notices that did exist, but it does not add a license or grant new
redistribution rights. Commit history and the upstream repository remain the
authority for attribution.

## Running archived code

This is a relocated archival copy, not an independently verified executable
package. Source commands assume the original team layout, where `work/` is at
repository root and organizer `data/` and `prediction/` are sibling
directories. They also require the dependencies in
`work/requirements.txt`; reference workflows additionally require local,
gitignored external `.h5ad` files with the documented checksums.

For faithful execution, use the recorded upstream commit in a disposable
checkout. Running from `team_archive/wyh/work/` requires path adaptation and
must not overwrite personal frozen artifacts or
`prediction/prediction.csv`.
