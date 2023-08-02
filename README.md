# Stereo Imagery Extension Specification

- **Title:** Stereo Imagery
- **Identifier:** <https://stac-extensions.github.io/stereo-imagery/v0.0.1/schema.json>
- **Field Name Prefix:** stereo-img
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @m-mohr

This document explains the Stereo Imagery Extension to the
[SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.
This extension helps to describe stereo imagery, tri-stereo imagery, etc.
This can be any group of 2+ captures with varying viewing angles that allows derive a 3-dimensional view from it.
The overlap of the captures should be significantly high (usually 75 % or more) and
the time difference between the captures should be minimal (usually a couple of minutes or less).

- Example:
  - [Collection](examples/collection.json): A Collection pointing to a pair of stereo imagery as Items.
  - [Item 1](examples/item1.json): The first Item of the pair.
  - [Item 2](examples/item2.json): The second Item of the pair.
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Fields

The extension allows to provide the captures as multiple Items.

The field in the table below can be used in these parts of STAC documents:
- [x] Catalogs
- [x] Collections
- [x] Item Properties (incl. Summaries in Collections)

| Field Name           | Type    | Description |
| -------------------- | ------- | ----------- |
| stereo-img:count     | integer | **REQUIRED**. The total number of captures in the group. 2 for stereo imagery (minimum), 3 for tri-stereo imagery, etc. |
| stereo-img:group     | string  | A unique identifier that for the group of captures. Helps to search for all images of a group. |

The field in the table below can be used in these parts of STAC documents:
- [x] Item Properties (incl. Summaries in Collections)
- [x] Links (in Items)

| Field Name           | Type    | Description |
| -------------------- | ------- | ----------- |
| stereo-img:number    | integer | RECOMMENDED. The capture number in the group, must be > 0 and <= the total count. |

The order of the captures that is reflected in `stereo-img:number` can usually be derived
from the acquisition time (`datetime`) unless there's another specific order for the captures.

It is recommended to provide exact viewing angles, geometries and timestamps for each capture in the Item Properties.

## Relation types

The following types should be used as applicable `rel` types in the
[Link Object](https://github.com/radiantearth/stac-spec/tree/master/item-spec/item-spec.md#link-object).

| Type    | Description |
| ------- | ----------- |
| related | Link to the other captures in the group. |

If the `related` relation type is used, it is **REQUIRED** to provide the `stereo-img:number` and `type` fields in the Link Object.
This allows clients to distinguish them from other "related" links.

## Search Examples

If you'd want to search for distinct stereo imagery groups you could use the
API [Filter Extension](https://github.com/stac-api-extensions/filter) as follows:

- Find one image per pair for stereo-imagery: `stereo-img:count = 2 and stereo-img:number = 1`
  (and then navigate from the first to the other images)
- Only find tri-stereo imagery: `stereo-img:count = 3`

All examples are expressed in CQL Text.

## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PR's are part of the repository and can be run locally to verify that changes are valid. 
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
