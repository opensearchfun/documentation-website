---
layout: default
title: Warm node role
nav_order: 75
parent: Configuring OpenSearch

---

# Warm node role

The warm node role allows you to designate certain nodes in your OpenSearch cluster to handle "warm" data - data that is accessed less frequently than "hot" data but still needs to be searchable. Warm nodes typically use less expensive hardware than hot nodes.

## Enabling the warm node role 

To designate a node as a warm node, add the following setting to its `opensearch.yml` file:

```yaml
node.roles: [data, warm]
```

This configures the node to be both a data node and a warm node. You can also use the `node.attr.data` setting:

```yaml
node.attr.data: warm
```

## Configuring warm nodes

Warm nodes support the following settings:

- `node.search.cache.size`: Sets the size of the search cache on warm nodes. Default is 80% of the JVM heap. For example:

  ```yaml
  node.search.cache.size: 60%  
  ```

- `cluster.routing.allocation.awareness.attributes`: Use to ensure warm shards are allocated across different racks/availability zones. For example:

  ```yaml
  cluster.routing.allocation.awareness.attributes: zone
  node.attr.zone: zone1
  ```

## Index allocation 

To allocate an index to warm nodes, use the index setting:

```json
PUT my-index
{
  "settings": {
    "index.routing.allocation.require.data": "warm"
  }
}
```

You can also move existing indices to warm nodes:

```json
PUT /my-index/_settings
{
  "index.routing.allocation.require.data": "warm"
}
```

## Search performance considerations

Warm nodes are optimized for search performance on less frequently accessed data. Some considerations:

- Warm nodes use a larger search cache to optimize for repeated queries on mostly static data
- The search cache is persistent across node restarts
- Consider using force merge on indices allocated to warm nodes to optimize for read performance

## Monitoring

You can monitor warm node metrics using the [Nodes Stats API]({{site.url}}{{site.baseurl}}/api-reference/nodes-apis/nodes-stats/). The `node.roles` field will show `warm` for warm nodes.

## Next steps

- Learn about [hot-warm architecture]({{site.url}}{{site.baseurl}}/tuning-your-cluster/index/#advanced-step-7-set-up-a-hot-warm-architecture) in OpenSearch
- Explore [index lifecycle management]({{site.url}}{{site.baseurl}}/im-plugin/ism/) to automatically move indices between hot and warm nodes