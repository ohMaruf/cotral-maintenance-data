# Cotral maintenance: input data

The inputs of [cotral-maintenance](https://github.com/ohMaruf/cotral-maintenance) that are not code.
That repository includes this one as a git submodule at `inputs/`, pinned to a commit, so every
commit of the pipeline names the data it ran on.

| Path | What | Read by |
|---|---|---|
| `GTFS_COTRAL.zip` | the Cotral GTFS feed ([source](http://travel.mob.cotralspa.it:7777/GTFS/GTFS_COTRAL.zip)), unzipped into `GTFS_COTRAL/` by `scripts/setup_remote.sh` | every run |
| `GROUND_TRUTH/stop_sign_reference.png`, `GROUND_TRUTH/lookalikes/*.png` | the explorer's reference sign and look-alikes | explore |
| `GROUND_TRUTH/bus_stops_new_coordinates.xlsx`, `GROUND_TRUTH/OSM_Stop_Points_Excel.xlsx` | verified stop positions | experiments |

To update the feed: replace `GTFS_COTRAL.zip` here and commit; then in cotral-maintenance,
`git -C inputs pull` and commit the new submodule pointer. A run is tied to the feed it started with:
start new runs on a new feed.
