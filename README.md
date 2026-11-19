# Cotral maintenance: the dataset

The inputs of [cotral-maintenance](https://github.com/ohMaruf/cotral-maintenance), the exact bytes a run
reads. That repository includes this one as a git submodule at `inputs/`, pinned to a commit, so every
commit of the pipeline names the data it runs on.

[`dataset.toml`](dataset.toml) is the manifest: every file, its role, where it came from (URL, date of the
data, licence) and its sha256. `cotral inputs check` verifies the files against it; `cotral inputs fetch`
gets a missing file from its URL and accepts it only if the sha256 matches.

| Role | Path | What |
|---|---|---|
| `gtfs` | `GTFS_COTRAL.zip` | the Cotral GTFS feed of 2026-10-28 ([source](http://travel.mob.cotralspa.it:7777/GTFS/GTFS_COTRAL.zip)) |
| `osm` | `osm/centro-latest.osm.pbf`, `osm/sud-latest.osm.pbf` | Geofabrik's Central and Southern Italy extracts of 2026-11-06, with Geofabrik's `.md5`; in **Git LFS** |
| `sign`, `lookalike` | `GROUND_TRUTH/stop_sign_reference.png`, `GROUND_TRUTH/lookalikes/*.png` | the explorer's reference sign and look-alikes |
| `reference` | `GROUND_TRUTH/*.xlsx` | verified stop positions, for experiments |

Cloning needs [Git LFS](https://git-lfs.com) for the extracts (`git lfs install` once; then a clone or
`git lfs pull` fetches them).

## Licences

- OpenStreetMap data: © OpenStreetMap contributors, [ODbL 1.0](https://opendatacommons.org/licenses/odbl/);
  extracts by [Geofabrik GmbH](https://download.geofabrik.de/).
- The GTFS feed: published by Cotral S.p.A.; check the operator's terms before redistributing.

## Updating

Replace a file (for OSM, `uv run cotral osm download` in cotral-maintenance fetches the newest extracts in
place and prints their sha256), update its `sha256` and dates in `dataset.toml`, commit and push. Then in
cotral-maintenance, `git -C inputs pull origin main` and commit the new `inputs` pointer. A run is tied to
the data it started with: start new runs on new data.
