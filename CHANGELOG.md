# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
As this project is still in active development, it does not yet strictly adhere to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.7] - 2026-08-07
### Added
- Pipeline Generations section on the home page (`_generations.qmd`), with links to the other four pilot sites and to the slim, clean, merged and refined generation sites

### Changed
- Database file renamed from `pilot_4demon.sqlite` to `4demon_pilot.sqlite`, matching the engine's `<source>_pilot.sqlite` convention
- Database is downloaded from the latest GitHub release instead of a pinned release tag, so no version string has to be edited when a new database is published
- Database Downloads page lists the single pilot database and links to the latest release
- Data Export menu renamed to EFSA Submission
- CLAUDE.md reduced to the site's invariants, with the detail moved to `docs/database.md` and `docs/site.md`

### Fixed
- Home page said the database has 10 tables; it has six
- Data Preparation link on the home page said the page covers the ICES-DOME dataset

### Removed
- Export to Tabular File page: the pilot generation no longer exports a dataset file
- DB Schema (Slim) page and the slim database download, which belong to the slim generation's own site

## [0.1.6] - 2026-07-24
### Added
- "Database Downloads" page with links to the full and slim SQLite database releases

## [0.1.5] - 2026-07-21
### Changed
- Moved `matrix` and `fraction_range` columns from the Subsample table to the Measurement table in the "DB Schema (Slim)" page, and updated the schema diagram accordingly

## [0.1.4] - 2026-07-17
### Added
- "DB Schema (Slim)" page documenting a common multi-source sediment schema, with table definitions aligned to the 4Demon-to-slim conversion script
### Changed
- Renamed "DB Schema" page to "DB Schema (Full)" and updated nav/home page links accordingly

## [0.1.3] - 2026-07-13
### Added
- EFSA Format v1/v2 and EFSA Submission v1/v2 pages under Data Export, mapping pilot database fields to the EFSA/FHF submission formats
- CLAUDE.md project documentation

## [0.1.2] - 2026-05-07
### Fixed
- average calculation for the interactive map

## [0.1.1] - 2026-05-07
### Changed
- All Quarto pages for the pilot DB

## [0.1.0] - 2026-05-06
### Added
- Initial Quarto pages
