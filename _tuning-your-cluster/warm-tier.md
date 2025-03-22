---
layout: default
title: Warm tier
nav_order: 25
parent: Tuning your cluster
---

# Warm tier

The warm tier functionality in OpenSearch allows you to create indexes with partial data locality, providing a balance between performance and storage costs. Warm indexes are designed for data that is accessed less frequently but still needs to be searchable.

## Overview

Warm indexes use a combination of local storage and remote storage:

- Frequently accessed segments are kept in local storage for fast access
- Less frequently accessed segments are stored remotely for cost savings
- Segments are automatically moved between local and remote storage based on access patterns

This tiered approach allows you to optimize storage costs while maintaining acceptable query performance for warm data.

## Enabling warm indexes

To create a warm index, set the following setting when creating the index:

```json
PUT /my-warm-index
{
  "settings": {
    "index.store.locality.type": "partial"
  }
}
```

You can also enable warm indexes at the cluster level by setting:

```yaml
index.store.locality.type: partial
```

in `opensearch.yml`.

## Configuration options

The following settings can be used to tune warm index behavior:

- `index.store.locality.cache.max_cache_size`: Maximum size of the local cache for warm indexes. Default is 10% of JVM heap.
- `index.store.locality.cache.expiration`: How long segments stay in the local cache before being eligible for eviction. Default is 48 hours.
- `index.store.locality.cache.write_buffer_size`: Size of the write buffer for uploading segments to remote storage. Default is 16MB.

## Monitoring

You can monitor warm index stats using the [Cluster Stats API]({{site.url}}{{site.baseurl}}/api-reference/cluster-api/cluster-stats/):

```
GET /_cluster/stats?human
```

Look for the `warm_indexes` section in the response for metrics on warm index usage.

## Limitations

- Warm indexes are currently experimental and not recommended for production use
- Not all index operations are supported on warm indexes
- Performance may be degraded compared to fully local indexes, especially for less frequently accessed data

## Next steps

- [Learn more about index settings]({{site.url}}{{site.baseurl}}/opensearch/index-settings/)
- [Explore tiered storage options]({{site.url}}{{site.baseurl}}/opensearch/tiered-storage/)