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

The update settings API allows you to change index-level settings dynamically.

## Example request

```json
PUT /sample-index1/_settings
{
  "index" : {
    "number_of_replicas" : 2
  }
}
```

## Path and HTTP methods

```
PUT /<index>/_settings
PUT /_settings
```

## URL parameters

All URL parameters are optional.

Parameter | Type | Description
:--- | :--- | :---
allow_no_indices | Boolean | Whether to ignore if a wildcard expression retrieves no concrete indices. Default is true.
expand_wildcards | String | Expands wildcard expressions to concrete indices. Supports comma-separated values. Valid values are all, open, closed, hidden, none. Default is open.
flat_settings | Boolean | Returns settings in flat format. Default is false.
ignore_unavailable | Boolean | Whether to ignore unavailable indexes. Default is false.
preserve_existing | Boolean | Whether to preserve existing settings. Default is false.
cluster_manager_timeout | Time | The amount of time to wait for a connection to the cluster manager node. Default is 30 seconds.

## Request body

You can specify the following options in the request body.

Option | Type | Description
:--- | :--- | :---
&lt;setting&gt; | String | Index setting to update.

## Response

```json
{
  "acknowledged" : true
}
```

## Update dynamic index settings

You can update dynamic index settings while an index is open. For example, you can update the `index.ingestion_source.error_strategy` setting dynamically:

```json
PUT /sample-index1/_settings
{
  "index": {
    "ingestion_source": {
      "error_strategy": "RETRY"
    }
  }
}
```

This updates the error handling strategy for ingestion sources associated with the `sample-index1` index.

## Update static index settings

Some index settings are static and can only be set at index creation or on a closed index. To update static settings:

1. Close the index:

   ```json
   POST /sample-index1/_close
   ```

2. Update the setting:

   ```json
   PUT /sample-index1/_settings
   {
    "index" : {
      "number_of_shards" : 2
    }
   }
   ```

3. Open the index:

   ```json
   POST /sample-index1/_open
   ```

## Common options

The following options can be applied to all update settings API requests:

Option | Type | Description
:--- | :--- | :---
timeout | Time | The period of time to wait for a response. If no response is received before the timeout expires, the request fails and returns an error. Default is 30 seconds.
cluster_manager_timeout | Time | The period of time to wait for a connection to the cluster manager node. Default is 30 seconds.
