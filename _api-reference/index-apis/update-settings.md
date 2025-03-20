---
layout: default
title: Update settings
parent: Index APIs
nav_order: 75
redirect_from:
  - /opensearch/rest-api/index-apis/update-settings/
---

# Update settings
**Introduced 1.0**
{: .label .label-purple }

You can use the update settings API operation to update index-level settings. You can change dynamic index settings at any time, but static settings cannot be changed after index creation. For more information about static and dynamic index settings, see [Configuring OpenSearch]({{site.url}}{{site.baseurl}}/install-and-configure/configuring-opensearch/index/).

Aside from the static and dynamic index settings, you can also update individual plugins' settings. To get the full list of updatable settings, run `GET <target-index>/_settings?include_defaults=true`.

## Endpoints

```json
PUT /<index>/_settings
```

## Path parameters

Parameter | Type | Description
:--- | :--- | :---
&lt;index&gt; | String | The index to update. Can be a comma-separated list of multiple index names. Use `_all` or `*` to specify all indexes.

## Query parameters

All update settings parameters are optional.

Parameter | Data type | Description
:--- | :--- | :---
allow_no_indices | Boolean | Whether to ignore wildcards that don't match any indexes. Default is `true`.
expand_wildcards | String | Expands wildcard expressions to different indexes. Combine multiple values with commas. Available values are `all` (match all indexes), `open` (match open indexes), `closed` (match closed indexes), `hidden` (match hidden indexes), and `none` (do not accept wildcard expressions), which must be used with `open`, `closed`, or both. Default is `open`.
cluster_manager_timeout | Time | How long to wait for a connection to the cluster manager node. Default is `30s`.
preserve_existing | Boolean | Whether to preserve existing index settings. Default is `false`.
timeout | Time | How long to wait for a connection to return. Default is `30s`.

## Request body

The request body must contain all of the index settings that you want to update.

```json
{
  "index.plugins.index_state_management.rollover_skip": true,
  "index": {
    "number_of_replicas": 4
  },
  "index.ingestion_source.error_strategy": "SKIP"
}
```

## Example request

```json
PUT /sample-index1/_settings
{
  "index.plugins.index_state_management.rollover_skip": true,
  "index": {
    "number_of_replicas": 4
  },
  "index.ingestion_source.error_strategy": "SKIP"
}
```
{% include copy-curl.html %}

## Example response

```json
{
    "acknowledged": true
}
```

## New setting: index.ingestion_source.error_strategy

The `index.ingestion_source.error_strategy` setting is a new dynamic setting that can be updated using this API. It controls how OpenSearch handles errors during ingestion from external sources. 

Possible values are:

- `SKIP`: Skip the erroneous document and continue ingestion (default)
- `FAIL`: Fail the entire ingestion process on any error

To update this setting:

```json
PUT /sample-index/_settings
{
  "index.ingestion_source.error_strategy": "SKIP"
}
```

This setting allows users to configure more granular error handling for ingestion processes, improving reliability and control over data imports.
