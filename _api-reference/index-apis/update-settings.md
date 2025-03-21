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

The request body must include all of the index settings that you want to update.

```json
{
  "index.plugins.index_state_management.rollover_skip": true,
  "index": {
    "number_of_replicas": 4
  },
  "ingestion_source": {
    "error_strategy": "DROP"
  }
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
  "ingestion_source": {
    "error_strategy": "DROP"
  }
}
```
{% include copy-curl.html %}

## Example response

```json
{
    "acknowledged": true
}
```

## Updating ingestion_source.error_strategy

The `ingestion_source.error_strategy` setting can be dynamically updated to control how errors are handled during ingestion. Valid values are:

- `DROP`: Drop records that cause errors during ingestion (default).
- `HALT`: Stop ingestion when an error occurs.
- `BYPASS`: Ingest records that cause errors without processing.

For example, to update the error strategy to HALT:

```json
PUT /sample-index1/_settings
{
  "ingestion_source": {
    "error_strategy": "HALT"
  }
}
```

This allows you to change the error handling behavior without restarting the cluster or recreating the index.
