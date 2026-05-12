# Migration Guide: titiler-pgstac 2.0 to 3.0

This guide covers the breaking changes and new features when upgrading from titiler-pgstac 2.0 to 3.0.

### TiTiler Dependency Update

**Impact:** High - Affects all functionality

titiler-pgstac 3.0 requires titiler `>=2.0,<2.1`, which includes several breaking changes. Please refer to the [TiTiler v2 Migration Guide](https://developmentseed.org/titiler/migrations/v2_migration/) for detailed information.

Key titiler 2.0 changes that affect titiler-pgstac:

- `assets` parameter is mandatory for all endpoints
- `expression` is now defined using simple `b{band-index}` notation (not `{asset}_b{band_index}`)
- tilesize returned by `/tiles/` endpoints is now defined by the TMS specification
- replaced `tile_scale` path-parameter with `tilesize` query-parameter
- tilejson document return tiles endpoint with `tilesize=512` (per specification)

**Action Required:**

- Review the TiTiler v2 migration guide

### Assets With Options 

**Impact:** High - Affects all functionality

`assets=` query parameter can be used to specify which assets to use for a given request, but also to specify options for each asset using the following syntax: `assets={asset_name}|OPTION1=VALUE1|OPTION2=VALUE2`. For example: `assets=visual|bidx=1,2|expression=(b2-b1)/(b2+b1)` will select the band 1 and 2 of within the `visual` asset and apply a normalized difference indexes expression.

ref: 
- https://cogeotiff.github.io/rio-tiler/latest/migrations/v9_migration/#stacreader-asset-options-syntax
- https://developmentseed.org/titiler/migrations/v2_migration/#7-removed-asset_indexes-and-asset_expression-options

**Action Required:**

- update client code for assets/expression definition

### Required `assets` parameter

**Impact:** High - Affects all functionality

All endpoints now require the `assets` query parameter to specify which assets to use for a given request. This change ensures that users explicitly define the assets they want to work with, improving clarity and reducing potential errors.

For `info` and `statistics` endpoints, the `assets` parameter is now mandatory. A special value of `assets=all` can be used to select all assets.

**Action Required:**

- update client code to include `assets` query parameter

### Remove `vrt://` prefix for asset names

**Impact:** Medium - Affects all functionality

`vrt://` connection string is longer allowed.