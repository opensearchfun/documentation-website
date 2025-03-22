---
layout: default
title: Node roles
parent: Configuring OpenSearch
nav_order: 25
---

# Node roles

Node roles define the capabilities and responsibilities of nodes in an OpenSearch cluster. By assigning specific roles to nodes, you can optimize your cluster for different workloads and improve performance and stability.

## Available node roles

OpenSearch supports the following node roles:

- `cluster_manager`: Manages cluster-wide actions like creating or deleting an index, tracking which nodes are part of the cluster, and allocating shards to nodes. 
- `data`: Stores and searches data.
- `ingest`: Pre-processes documents before indexing.
- `remote_cluster_client`: Enables the node to act as a client for remote clusters.
- `search`: Handles search and aggregation requests.
- `ml`: Handles machine learning job coordination and processing.

## Configuring node roles

You can configure node roles in the `opensearch.yml` file using the `node.roles` setting:

```yaml
node.roles: [ data, ingest ]
```

If `node.roles` is not set, the node will be assigned all roles by default.

## Node role combinations

Nodes can have multiple roles. Some common combinations include:

- Dedicated cluster manager node: `[ cluster_manager ]`
- Data node: `[ data ]`
- Coordinating node: `[]` (empty list)
- Ingest node: `[ ingest ]`
- Machine learning node: `[ ml ]`

## Special considerations

- At least one node in the cluster must have the `cluster_manager` role.
- For production environments, it's recommended to have at least three dedicated cluster manager nodes for high availability.
- The `search` role is typically combined with the `data` role, but can be separated for specific use cases.
- Nodes with no roles specified (`node.roles: []`) act as coordinating nodes, which can help distribute load for search-heavy workloads.

## Example configurations

1. Dedicated cluster manager node:

```yaml
node.roles: [ cluster_manager ]
```

2. Data and ingest node:

```yaml
node.roles: [ data, ingest ]
```

3. Coordinating node:

```yaml
node.roles: []
```

4. Machine learning node:

```yaml
node.roles: [ ml ]
```

## Changing node roles

If you need to change a node's roles, update the `node.roles` setting in `opensearch.yml` and restart the node. Be aware that changing roles may impact cluster stability and data distribution, so plan changes carefully, especially in production environments.

## Related settings

- `cluster.remote.node`: Set to `true` to allow the node to act as a client node for remote clusters, even if it doesn't have the `remote_cluster_client` role explicitly set.
- `node.master`: Deprecated in favor of `cluster_manager`. If set to `true`, adds the `cluster_manager` role to the node.
- `node.data`: If set to `true`, adds the `data` role to the node.
- `node.ingest`: If set to `true`, adds the `ingest` role to the node.

## Monitoring node roles

You can use the Cluster API to check the roles of nodes in your cluster:

```
GET /_cat/nodes?v&h=ip,roles
```

This will return a list of nodes with their IP addresses and assigned roles.