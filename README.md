# GEODES Extension Specification

- **Title:** GEODES
- **Identifier:** <https://cnes.github.io/geodes-stac-extension/v0.4.0/schema.json>
- **Field Name Prefix:** geodes
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @emmanuelmathot

This document explains the GEODES Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.

[GEODES](https://geodes.cnes.fr/) is the Earth observation data catalog of CNES. This extension defines the few fields
that the GEODES catalog needs and that no other STAC extension covers. It is a last resort: GEODES uses official
STAC extensions first, and mission-specific metadata goes in a mission extension
(for example the [SPOT Extension](https://github.com/CNES/spot-stac-extension)).

- Examples:
  - [Item example](examples/item.json): A THEIA SPOT World Heritage Level-1C item with visibility and quality statistics
  - [Collection example](examples/collection.json): Summary of the visibility in a STAC Collection
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Fields

The fields in the table below can be used in these parts of STAC documents:

- [ ] Catalogs
- [x] Collections (summaries only)
- [x] Item Properties (incl. Summaries in Collections)
- [ ] Assets (for both Collections and Items, incl. Item Asset Definitions in Collections and Asset Templates)
- [ ] Links
- [ ] Bands

| Field Name        | Type   | Description                                               |
| ----------------- | ------ | --------------------------------------------------------- |
| geodes:visibility | string | Access tier of the item: `public`, `private` or `expert`. |

An Item that declares this extension must have `geodes:visibility` or `statistics`.

### Additional Field Information

#### geodes:visibility

The data provider sets the access tier of each item. The GEODES access policies use it to allow or refuse the read and the download:

- `public`: anyone can access the item.
- `private`: only authenticated users of the authorised groups (usually the group of the collection) can access the item.
- `expert`: only the expert audience of the collection can access the item.

Example: `"geodes:visibility": "public"`.

## Statistics keys

GEODES does not define its own statistics field. The quality statistics of an Item go in the `statistics` field of the
Item properties: the [Statistics Object](https://github.com/radiantearth/stac-spec/blob/v1.1.0/commons/common-metadata.md#statistics-object)
of STAC common metadata. The STAC specification allows this object in the Item properties, and allows keys other than
`minimum`, `maximum`, `mean`, `stddev`, `count` and `valid_percent`, for example coverage percentages.
The [Sentinel-2 Extension](https://github.com/stac-extensions/sentinel-2) also moved its `*_percentage` fields to this object.

This extension defines the GEODES keys of the Statistics Object and validates them when an Item declares the extension.
All values are percentages of the pixels of the Item, from 0 to 100, except `interpolated_columns`, which is a count.
All keys are optional, and other keys of the
Statistics Object stay allowed.

| Key                         | Type    | Description                                                                                    |
| --------------------------- | ------- | ---------------------------------------------------------------------------------------------- |
| water                       | number  | Percentage of water pixels.                                                                    |
| nodata\_pixel               | number  | Percentage of no-data pixels (= 100 − `valid_percent`).                                        |
| saturated\_defective\_pixel | number  | Percentage of saturated or defective pixels.                                                   |
| ground\_useful\_pixel       | number  | Percentage of useful ground pixels (not cloud, not no-data).                                   |
| min\_useful\_pixel          | number  | Minimum useful pixel percentage, as defined by the data provider.                              |
| interpolated\_columns       | integer | Number of image columns restored by interpolation (dead detectors). A count, not a percentage. |

The key names follow the rule of the Sentinel-2 Extension: remove the prefix and the `_percentage` suffix
of the mission field name (`nodata_pixel_percentage` becomes `nodata_pixel`).

Example:

```json
"statistics": {
  "water": 0.0,
  "nodata_pixel": 0.0,
  "saturated_defective_pixel": 6.807989,
  "ground_useful_pixel": 75.490373,
  "min_useful_pixel": 10.410544,
  "interpolated_columns": 0
}
```

Each value has one stable property path, for example `statistics.nodata_pixel`, so a
[CQL2](https://docs.ogc.org/is/21-065r2/21-065r2.html) filter can use it like any other number:

```json
{
  "filter-lang": "cql2-json",
  "filter": {
    "op": "<",
    "args": [{ "property": "statistics.nodata_pixel" }, 10]
  }
}
```

A Collection should also describe the keys that its Items use with a JSON Schema in `summaries.statistics`,
as the STAC specification recommends.

## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PRs are part of the repository and can be run locally to verify that changes are valid.
To run tests locally, you'll need `npm`, which is a standard part of any [node.js installation](https://nodejs.org/en/download/).

First you'll need to install everything with npm once. Just navigate to the root of this repository and on
your command line run:

```bash
npm install
```

Then to check markdown formatting and test the examples against the JSON schema, you can run:

```bash
npm test
```

This will spit out the same texts that you see online, and you can then go and fix your markdown or examples.

If the tests reveal formatting problems with the examples, you can fix them with:

```bash
npm run format-examples
```
