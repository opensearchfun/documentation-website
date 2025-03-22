---
layout: default
title: Search node role
parent: Cluster management
nav_order: 90
---

# Search node role

The search node role is a specialized node type in OpenSearch that is dedicated to hosting search replicas. Search nodes provide enhanced query performance by offloading search operations from data nodes.

## Enabling search nodes

To enable a node as a search node, add the following configuration to `opensearch.yml`:

```yaml
node.roles: [ search ]
```

You can also combine the search role with other roles:

```yaml 
node.roles: [ data, search ]
```

## Key features

Search nodes offer the following benefits:

- Improved query performance by dedicating resources to search operations
- Ability to scale search capacity independently from data ingestion
- Reduced load on data nodes by offloading search queries

## Limitations 

Search nodes have some important limitations to be aware of:

- They can only host search replicas, not primary shards
- They cannot ingest new data or perform indexing operations
- They cannot be combined with the cluster manager role

## Best practices

Consider the following best practices when using search nodes:

- Use search nodes in clusters with high query volume to improve overall performance
- Balance the number of search nodes with data nodes based on your workload
- Monitor search node resource utilization to ensure optimal performance

## Configuring search replicas

To take advantage of search nodes, you need to configure search replicas for your indexes. You can do this when creating an index:

```json
PUT /my-index
{
  "settings": {
    "index": {
      "number_of_shards": 3,
      "number_of_replicas": 1,
      "number_of_search_replicas": 2
    }
  }
}
```

Or update an existing index:

```json
PUT /my-index/_settings
{
  "index": {
    "number_of_search_replicas": 2
  }
}
```

## Monitoring search nodes

You can monitor search node status and performance using the Cluster API:

```
GET /_cat/nodes?v&h=name,node.role,search_load_1m,search_load_5m,search_load_15m
```

This will return information about search load on each node in your cluster.

## Related topics

- [Node roles]({{site.url}}{{site.baseurl}}/opensearch/cluster/)
- [Index settings]({{site.url}}{{site.baseurl}}/im-plugin/index-settings/)
- [Cluster API]({{site.url}}{{site.baseurl}}/api-reference/cluster-api/)