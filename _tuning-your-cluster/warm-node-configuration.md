---
layout: default
title: Warm node configuration
parent: Creating a cluster
nav_order: 7
---

# Warm node configuration

Warm nodes are a specialized type of data node in OpenSearch designed to store and manage older, less frequently accessed data. This configuration allows for more cost-effective storage of historical data while maintaining searchability.

## Overview

Warm nodes are part of a hot-warm architecture, where:

- Hot nodes store the most recent, frequently accessed data
- Warm nodes store older, less frequently accessed data

This setup helps optimize cluster performance and reduce costs by using different hardware configurations for different data access patterns.

## Configuring a warm node

To configure a node as a warm node:

1. Set the node role to `warm` in the `opensearch.yml` file:

```yaml
node.roles: [ warm ]
```

2. Configure the node attributes to identify it as a warm node:

```yaml
node.attr.temp: warm
```

## Warm node settings

The following settings are specific to warm nodes:

### `node.roles`

Set this to `warm` to designate the node as a warm node.

### `node.attr.temp`

Set this to `warm` to identify the node as a warm node for index allocation purposes.

### `node.search.cache.size`

This setting controls the amount of memory allocated for the search cache on warm nodes. By default, it is set to 80% of the available heap memory on dedicated warm nodes.

Example:

```yaml
node.search.cache.size: 80%
```

You can adjust this value based on your specific requirements and available resources.

## Index allocation for warm nodes

To allocate an index to warm nodes, use the following index setting:

```json
PUT oldindex
{
  "settings": {
    "index.routing.allocation.require.temp": "warm"
  }
}
```

This ensures that the index `oldindex` is allocated only to nodes with the `temp` attribute set to `warm`.

## Best practices

1. Use appropriate hardware: Warm nodes typically require less powerful hardware than hot nodes, as they handle less frequent queries.

2. Balance storage and performance: Configure warm nodes with a higher ratio of storage to CPU and RAM compared to hot nodes.

3. Use Index Lifecycle Management (ISM): Implement ISM policies to automatically move indices from hot to warm nodes based on age or other criteria.

4. Monitor and adjust: Regularly review the performance and storage utilization of your warm nodes, adjusting configurations as needed.

5. Consider using larger shards: Since warm data is accessed less frequently, you may benefit from using larger shard sizes to reduce the total number of shards in the cluster.

## Related topics

- [Creating a cluster](./index.md)
- [Hot-warm architecture](../im-plugin/index-rollups/index.md#example)
- [Index Lifecycle Management](../im-plugin/ism/index.md)

By properly configuring and utilizing warm nodes, you can optimize your OpenSearch cluster for both performance and cost-effectiveness when dealing with large volumes of historical data.