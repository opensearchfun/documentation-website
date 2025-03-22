---
layout: default
title: Search node role
parent: Managing indexes
nav_order: 37
---

# Search node role

The search node role is a specialized node type in OpenSearch designed to host search replicas and handle search-specific workloads. This role helps optimize cluster performance by dedicating resources to search operations.

## Overview

The search node role (`DiscoveryNodeRole.SEARCH_ROLE`) has the following characteristics:

- It is dedicated to hosting search replicas.
- It cannot be combined with any other role on a node.
- Nodes with this role can contain data (search replicas).

## Configuring search nodes

To configure a node as a search node, add the following setting to the `opensearch.yml` file:

```yaml
node.roles: [ search ]
```

## Use cases

Search nodes are useful in scenarios where you want to:

- Separate search workloads from indexing and other cluster operations.
- Scale search capacity independently from other cluster functions.
- Optimize hardware resources for search-intensive workloads.

## Limitations

When using search nodes, keep in mind the following limitations:

1. A search node cannot have any other roles assigned to it.
2. Primary shards cannot be allocated to search nodes.

## Cluster behavior with search nodes

When search nodes are present in a cluster:

1. Only search replicas will be allocated to these nodes.
2. Primary shards and write replicas will be allocated to other data nodes in the cluster.
3. Search requests will be preferentially routed to search nodes when possible.

## Example cluster configuration

Here's an example of how you might configure a cluster with search nodes:

```yaml
# Cluster manager node
node.roles: [ cluster_manager ]

# Data node
node.roles: [ data ]

# Search node
node.roles: [ search ]
```

This configuration creates a cluster with dedicated nodes for cluster management, data operations, and search operations.

## Performance considerations

While search nodes can improve overall cluster performance by dedicating resources to search operations, it's important to monitor and tune your cluster based on your specific workload. Consider the following:

1. Balancing the number of search nodes with other node types in your cluster.
2. Adjusting the number of search replicas based on your search workload and availability requirements.
3. Monitoring search node performance and scaling as needed.

## Related settings

The following cluster settings can be used to fine-tune search node behavior:

- `cluster.routing.allocation.allow_rebalance`: Controls when OpenSearch is allowed to rebalance shards between nodes.
- `cluster.routing.allocation.cluster_concurrent_rebalance`: Sets the number of concurrent shard rebalances allowed cluster-wide.

## API usage

When using the Cluster API, you can check if a node has the search role:

```json
GET /_nodes

Response:
{
  "nodes": {
    "node_id": {
      "roles": ["search"]
    }
  }
}
```

## Next steps

- Learn more about [node types and roles](../../install-and-configure/install-opensearch/index/#important-settings) in OpenSearch.
- Explore [index management](../index/) strategies to optimize your search workloads.
- Understand how to [tune your cluster](../../tuning-your-cluster/index/) for better performance.