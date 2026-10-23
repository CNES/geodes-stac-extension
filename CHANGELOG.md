# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [v0.5.0]

### Added

- Optional `geodes:provider_id` (when provider id is overridden) and
  `geodes:provider_dissemination_datetime` for traceability and provider publication datetime.

## [v0.4.0]

First version in this repository, moved from the GEODES data model repository (`schemas/extensions/geodes/`).
The identifier changes from `https://geodes.cnes.fr/stac/extensions/geodes/vX.Y.Z/schema.json`
to `https://cnes.github.io/geodes-stac-extension/vX.Y.Z/schema.json`.

### Added

- Validation of the GEODES keys of the common metadata Statistics Object (`statistics` in Item properties):
  `water`, `nodata_pixel`, `saturated_defective_pixel`, `ground_useful_pixel`, `min_useful_pixel` (percentages)
  and `interpolated_columns` (count of columns restored by interpolation).
- Collection summaries for `geodes:visibility`.

### Removed

- `geodes:statistics`. The common metadata `statistics` field covers the need: STAC allows it in Item properties
  with custom keys. The v0.3.0 keys `nodata`, `saturated_defective`, `ground_useful` and `min_useful` become
  `nodata_pixel`, `saturated_defective_pixel`, `ground_useful_pixel` and `min_useful_pixel` in `statistics`.

## [v0.3.0]

Not published.

### Added

- `geodes:statistics`: item-level quality statistics (`water`, `nodata`, `saturated_defective`, `ground_useful`, `min_useful`). Removed in v0.4.0.

## [v0.2.0]

Published at `https://geodes.cnes.fr/stac/extensions/geodes/v0.2.0/schema.json`. Used by the FLATSIM collections.

### Added

- `geodes:visibility` (`public`, `private`, `expert`).

[Unreleased]: <https://github.com/CNES/geodes-stac-extension/compare/v0.5.0...HEAD>

[v0.5.0]: <https://github.com/CNES/geodes-stac-extension/compare/v0.4.0...v0.5.0>

[v0.4.0]: <https://github.com/CNES/geodes-stac-extension/tree/v0.4.0>

[v0.3.0]: <https://gitlab.cnes.fr/geodes/configuration/geodes-stac-data-model/-/tree/main/schemas/extensions/geodes/v0.3.0>

[v0.2.0]: <https://gitlab.cnes.fr/geodes/configuration/geodes-stac-data-model/-/tree/main/schemas/extensions/geodes/v0.2.0>
