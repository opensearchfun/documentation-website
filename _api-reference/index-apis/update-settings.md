---
layout: default
title: Update settings
parent: Index APIs
nav_order: 75
redirect_from:
  - /opensearch/rest-api/index-apis/update-settings/
---

# Update settings
Introduced 1.0
{: .label .label-purple }

Use the update settings API operation to dynamically change or add index settings.

## Endpoints

```json
PUT /<index>/_settings
```

```json
PUT /_settings
```

## Path parameters

Parameter | Data type | Description
:--- | :--- | :---
index | String | (Optional) Comma-separated list of index names or index aliases used to limit the request. Supports wildcard expressions. If not specified, the operation applies to all indexes.

## Query parameters

Parameter | Data type | Description
:--- | :--- | :---
allow_no_indices | Boolean | Whether to ignore wildcards that don't match any indexes. Default is true.
expand_wildcards | String | Expands wildcard expressions to different indexes. Combine multiple values with commas. Valid values are: `all`, `open`, `closed`, `hidden`, `none`. Default is `open`.
flat_settings | Boolean | Whether to return settings in flat format. Default is false.
ignore_unavailable | Boolean | Whether to ignore unavailable indexes. Default is false.
preserve_existing | Boolean | Whether to preserve existing settings. Default is false.
cluster_manager_timeout | Time | How long to wait for a connection to the cluster manager node. Default is 30 seconds.
timeout | Time | How long to wait for the operation to return a response. Default is 30 seconds.

## Request body

You can specify the settings you want to update in the request body. For a list of available settings, see [Index settings]({{site.url}}{{site.baseurl}}/im-plugin/index-settings/).

## Example request

The following request updates the number of replicas for the `movies` index:

```json
PUT /movies/_settings
{
  "index" : {
    "number_of_replicas" : 2
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

## Dynamically updating the ingestion error strategy

As of version 2.9.0, you can dynamically update the ingestion error strategy for an index. This allows you to change how OpenSearch handles errors during data ingestion without having to recreate the index.

To update the ingestion error strategy, use the following request:

```json
PUT /my-index/_settings
{
  "index.ingestion_source.error_strategy": "DROP"
}
```
{% include copy-curl.html %}

The `index.ingestion_source.error_strategy` setting can be set to either `DROP` or `FAIL`. 

- `DROP`: Ignores the erroneous document and continues ingestion with the next document.
- `FAIL`: Stops the ingestion process when an error is encountered.

This dynamic update capability provides more flexibility in managing data ingestion processes, allowing you to adjust error handling strategies based on your specific needs without downtime.
