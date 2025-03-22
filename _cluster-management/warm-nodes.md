---
layout: default
title: Warm nodes
nav_order: 120
parent: Cluster management
---

# Warm nodes

Warm nodes are a specialized type of data node in OpenSearch designed to store and manage indices that are accessed less frequently but still need to be searchable. They provide a cost-effective way to store large amounts of older data while maintaining query performance for more recent, frequently accessed data on hot nodes.

## Configuration

To configure a node as a warm node, add the following settings to the `opensearch.yml` file:

```yaml
node.roles: [ data, warm ]
```

This configuration assigns both the `data` and `warm` roles to the node.

## File cache

Warm nodes utilize a file cache to improve query performance on less frequently accessed data. The file cache size can be configured using the `node.search.cache.size` setting:

```yaml
node.search.cache.size: 80%
```

By default, the file cache is set to 80% of the available disk space on warm nodes.

## Index allocation

To allocate an index to warm nodes, you can use index-level settings:

```json
PUT /my-index-000001/_settings
{
  "index.routing.allocation.require.box_type": "warm"
}
```

This setting ensures that the index shards are allocated only to nodes with the `warm` role.

## Shard filtering

Warm nodes use shard filtering to optimize storage and query performance. This feature allows warm nodes to store only a subset of an index's shards, reducing storage requirements while maintaining searchability.

To enable shard filtering on an index:

```json
PUT /my-index-000001/_settings
{
  "index.routing.allocation.include.box_type": "warm",
  "index.routing.allocation.total_shards_per_node": 2
}
```

This configuration limits the number of shards per warm node to 2 for the specified index.

## Monitoring

You can monitor warm node status and performance using the Cluster API:

```
GET /_nodes/stats
```

Look for nodes with the `warm` role in the response to identify warm nodes and their associated statistics.

## Best practices

1. Use warm nodes for indices that are accessed less frequently but still need to be searchable.
2. Configure appropriate file cache sizes based on your cluster's resources and query patterns.
3. Implement index lifecycle management policies to automatically move indices from hot to warm nodes based on age or access patterns.
4. Monitor warm node performance and adjust configurations as needed to optimize storage and query efficiency.

By leveraging warm nodes effectively, you can create a tiered storage architecture in your OpenSearch cluster, balancing performance and cost-effectiveness for your data storage and retrieval needs.