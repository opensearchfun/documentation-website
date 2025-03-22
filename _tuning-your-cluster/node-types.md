---
layout: default
title: Node types
parent: Creating a cluster
nav_order: 10
---

# Node types in OpenSearch

OpenSearch clusters can contain different types of nodes, each serving a specific purpose. Understanding the various node types and their roles is crucial for designing and maintaining an efficient OpenSearch cluster.

## Node types overview

OpenSearch supports the following node types:

- Cluster manager nodes
- Data nodes 
- Ingest nodes
- Coordinating nodes
- Machine learning nodes

Each node type plays a unique role in the cluster's operation and can be configured to handle specific tasks.

## Cluster manager nodes

Cluster manager nodes are responsible for lightweight cluster-wide actions such as creating or deleting an index, tracking which nodes are part of the cluster, and deciding which shards to allocate to which nodes. It's important for cluster stability to have a stable cluster manager node.

To create a dedicated cluster manager node, set the following in `opensearch.yml`:

```yaml
node.roles: [ cluster_manager ]
```

For production clusters, we recommend having three dedicated cluster manager nodes to ensure high availability and prevent split-brain scenarios.

## Data nodes 

Data nodes hold the shards that contain indexed documents. They handle data related operations like CRUD, search, and aggregations. To define a node as a data node, use:

```yaml
node.roles: [ data ]
```

Data nodes require more disk I/O, CPU, and memory than other node types. As your cluster grows, you'll likely need to add more data nodes to handle the increased data volume and query load.

## Ingest nodes

Ingest nodes can apply an ingest pipeline to a document before indexing. This can be useful for pre-processing documents, such as timestamp conversion, geoip lookup, or field rename. To create an ingest node:

```yaml
node.roles: [ ingest ]
```

For heavy ingest workloads, you may want to have dedicated ingest nodes to offload this work from your data nodes.

## Coordinating nodes 

Coordinating nodes act as smart load balancers. They route requests, collate results, and can help offload the coordinating role from data and cluster manager nodes. A pure coordinating node is defined by not having any explicit roles:

```yaml
node.roles: [ ]
```

Coordinating nodes can be beneficial in large clusters to handle search requests and reduce load on data nodes.

## Machine learning nodes

Machine learning nodes are dedicated to running machine learning jobs and handling ML-related tasks. To set up a machine learning node:

```yaml
node.roles: [ ml ]
```

These nodes are useful when you're extensively using OpenSearch's machine learning features and want to isolate this workload from other node types.

## Multi-role nodes

In smaller clusters or development environments, it's common to have nodes that perform multiple roles. For example, a node can be both a data node and an ingest node:

```yaml
node.roles: [ data, ingest ]
```

While multi-role nodes can be convenient, separating roles becomes more important as your cluster grows to ensure optimal performance and stability.

## Best practices

1. Use dedicated cluster manager nodes in production environments.
2. Separate data and coordinating roles for large clusters with high query loads.
3. Consider using dedicated ingest nodes if you have heavy ingest workloads.
4. Isolate machine learning workloads to dedicated ML nodes if you're extensively using ML features.
5. Monitor node performance and adjust your cluster architecture as your workload grows or changes.

By understanding and properly utilizing different node types, you can create an OpenSearch cluster that is efficient, scalable, and tailored to your specific use case.