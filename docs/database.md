# Database

`4demon_pilot.sqlite` is the **only** file this site depends on. It is built by
the `multised-engine` package (`create_db("pilot", "4demon")`), not in this
repository, and is published as an asset on this repository's latest GitHub
release.

The name changed from `pilot_4demon.sqlite` at site v0.1.7, matching the
engine's `<source>_pilot.sqlite` convention.

## Schema

Six tables, the smallest of the five pilot databases: four reference tables plus
`sample` and `sediment`. All access is client-side, through stratum-sqlite.

| Table | PK | Rows | Notes |
|---|---|--:|---|
| `sediment` | `survey_seq_no` | 23,542 | the fact table: value, corrected_value, unit, and five flags (value_flag, det_limit_flag, range_check_flag, outlier_extreme_flag, outlier_stdev_flag) |
| `sample` | `sample_id` | 1,796 | station_id, project_id, sample_ref_code, sample_timestamp, gear_code, depth_range, replicate_number |
| `project` | `project_id` | 222 | project, campaign_code, year, month, season |
| `method` | `method_id` | 176 | method_code, matrix_code, fraction_range_um |
| `station` | `station_id` | 166 | station_code, station_group, latitude, longitude, dist_to_coast, est_country, country_code, municipality, sea_name |
| `parameter` | `parameter` | 17 | parameter_name, parameter_type |

Site coordinates are rounded to 3 decimal places. The nearest-country column is
`est_country`, as on ICES-DOME and MUDAB.

## The geo columns

`station.dist_to_coast`, `est_country`, `country_code`, `municipality` and
`sea_name` are computed by the external
[seastamp](https://github.com/AIQC-Hub/seastamp) CLI (GSHHG full resolution,
Natural Earth 1:10m, GISCO LAU 2021, IHO Sea Areas v3), run over the distinct
station positions in an LAEA projection derived from the points themselves. They
are **not** in the raw 4Demon export.

They were recomputed at site v0.1.8: before that they came from an `sf` /
`rnaturalearth` / `giscoR` implementation, under which every station read
`North Atlantic Ocean` rather than `North Sea`. `distance-to-coast.qmd` and
`location-names.qmd` document the method and the measured change.

Two things make this source distinctive downstream. `sediment` is the only pilot
fact table carrying **source-native quality flags**, which the slim generation
folds into its `src_flag`. And 4Demon reports **no grain size and no organic
carbon**, which is why it contributes only four elements to the later
generations and skips the grain-size steps entirely.

`db-schema.qmd` renders the ER diagram and the full column definitions;
`data-preparation.qmd` documents how the raw 4Demon export was transformed
before import.

## Querying it from a page

Every page that reads the database includes `_db-setup.qmd`:

```qmd
{{< include _db-setup.qmd >}}
```

`header.html` sets `window._stratumSQLite`, `window._dbPath` and
`window._sqljsBase` at page load; `_db-setup.qmd` opens the file and exposes it
as `db`, which is then available to every OJS block on that page.

**One database per page.** Opening a second one on the same page fails.

## The cache key

stratum-sqlite caches the database in the browser, keyed by the `cacheKey` in
`_db-setup.qmd` (`4demon-pilot@vX.Y.Z`). Whenever the database **content**
changes, bump that key, or returning browsers keep serving the stale cached copy
and queries fail with "no such column".

This is the one version string that still has to be edited by hand; the download
URLs resolve to the latest release on their own.

## Scope

This site documents the **pilot** generation only. The slim, clean, merged and
refined generations have their own sites, and nothing here should link to their
schemas or ship their database files.
