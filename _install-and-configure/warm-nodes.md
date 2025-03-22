---
layout: default
title: Warm nodes
nav_order: 12
parent: Creating a cluster
---

# Warm nodes

Warm nodes are a specialized type of data node in OpenSearch designed to hold warm indices. Warm indices are those that are accessed less frequently than hot indices but still need to be searchable. Using warm nodes can help optimize cluster performance and reduce costs.

## Configuring warm nodes

To designate a node as a warm node, you need to set the `node.roles` setting in the `opensearch.yml` file:

```yaml
node.roles: [ data_warm ]
```

This setting assigns the `data_warm` role to the node, indicating that it should be used for storing warm indices.

## Moving indices to warm nodes

By default, newly created indices are allocated to hot nodes. To move an index to a warm node, you can use the index settings API to update the `index.routing.allocation.require` setting:

```json
PUT /my-index/_settings
{
  "index.routing.allocation.require": {
    "data": "warm"
  }
}
```

This setting ensures that the index will only be allocated to nodes with the `data_warm` role.

## Warm node attributes

Warm nodes have the following attributes:

- They can contain data (unlike coordinator nodes)
- They are optimized for less frequent access patterns
- They may use less expensive hardware compared to hot nodes

## Use cases

Warm nodes are particularly useful in scenarios where:

- You have a large amount of historical data that is still occasionally queried
- You want to reduce costs by storing less frequently accessed data on cheaper hardware
- You need to optimize your cluster's performance by separating hot and warm workloads

## Considerations

When implementing warm nodes, keep the following in mind:

1. Ensure you have enough warm nodes to handle your expected warm index volume
2. Monitor the performance of queries on warm indices to ensure they meet your requirements
3. Consider implementing an index lifecycle management policy to automatically move indices from hot to warm nodes based on age or other criteria

## Related settings

The following cluster settings can be useful when working with warm nodes:

- `cluster.routing.allocation.awareness.attributes`: Can be used to ensure proper distribution of shards across different types of hardware
- `cluster.routing.allocation.disk.threshold_enabled`: Controls shard allocation based on disk usage, which can be important for warm nodes that may have different disk capacities than hot nodes

## Example configuration

Here's an example `opensearch.yml` configuration for a warm node:

```yaml
node.name: warm-node-1
node.roles: [ data_warm ]
node.attr.data: warm
cluster.name: my-opensearch-cluster
network.host: 0.0.0.0
discovery.seed_hosts: ["hot-node-1", "hot-node-2", "warm-node-2"]
cluster.initial_cluster_manager_nodes: ["hot-node-1", "hot-node-2"]
```

This configuration sets up a warm node named `warm-node-1` and assigns it the `data_warm` role. It also sets a custom attribute `data: warm` which can be used for more granular shard allocation control.

## Next steps

- Learn about [index lifecycle management]({{site.url}}{{site.baseurl}}/im-plugin/ism/) to automate the process of moving indices between hot and warm nodes
- Explore [cluster formation]({{site.url}}{{site.baseurl}}/opensearch/cluster/) to understand how to set up a cluster with different node types
- Review [shard allocation awareness]({{site.url}}{{site.baseurl}}/opensearch/cluster/#advanced-step-6-configure-shard-allocation-awareness-or-forced-awareness) to optimize shard distribution in clusters with heterogeneous nodes