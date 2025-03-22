---
layout: default
title: Node roles
nav_order: 15
parent: Configuring OpenSearch

---

# Node roles

OpenSearch allows you to configure different roles for nodes in your cluster to optimize performance and resource utilization. By assigning specific roles to nodes, you can create a cluster architecture that best fits your use case.

## Available node roles

OpenSearch supports the following node roles:

- `cluster_manager`: Handles cluster-wide actions such as creating or deleting an index, tracking which nodes are part of the cluster, and deciding which shards to allocate to which nodes. 
- `data`: Holds data and performs data related operations like CRUD, search, and aggregations.
- `ingest`: Pre-processes documents before indexing.
- `ml`: Runs machine learning jobs.
- `remote_cluster_client`: Can act as a client to connect to remote clusters.
- `search`: Handles search requests and all queries.
- `transform`: Handles transform operations.
- `warm`: Holds warm data that is accessed less frequently than hot data.

## Configuring node roles

You can configure node roles in the `opensearch.yml` file using the `node.roles` setting:

```yaml
node.roles: [ data, search ]
```

This example configures the node to be both a data node and a search node.

## Default roles

If no roles are explicitly configured, a node will be assigned all roles by default except for `cluster_manager`.

## Special considerations

- At least one node in the cluster must have the `cluster_manager` role.
- The `warm` role can only be used in conjunction with the `data` role.
- The `search` role requires either the `data` role or `remote_cluster_client` role to be useful.

## Example configurations

Here are some common node role configurations:

1. Dedicated cluster manager node:
   ```yaml
   node.roles: [ cluster_manager ]
   ```

2. Data node with search capabilities:
   ```yaml
   node.roles: [ data, search ]
   ```

3. Dedicated ingest node:
   ```yaml
   node.roles: [ ingest ]
   ```

4. Warm data node:
   ```yaml
   node.roles: [ data, warm ]
   ```

## Impact on cluster behavior

The way you configure node roles can significantly impact your cluster's behavior and performance:

- Having dedicated cluster manager nodes can improve cluster stability, especially in large clusters.
- Separating data and search roles can help balance the load between indexing and search operations.
- Using warm nodes can help optimize storage costs for less frequently accessed data.

## Verifying node roles

You can verify the roles assigned to nodes in your cluster using the Nodes Info API:

```
GET /_nodes/_all/roles
```

This will return information about all nodes in the cluster, including their assigned roles.

Remember to restart your OpenSearch nodes after making changes to the `opensearch.yml` file for the new role configurations to take effect.