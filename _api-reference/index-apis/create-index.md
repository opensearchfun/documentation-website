---
layout: default
title: Create index
parent: Index APIs
nav_order: 25
redirect_from:
  - /opensearch/rest-api/index-apis/create-index/
  - /opensearch/rest-api/create-index/
---

# Create index
Introduced 1.0
{: .label .label-purple }

Use the create index API operation to add a new index to an OpenSearch cluster.

## Endpoints

```json
PUT /<index>
```

## Index naming restrictions

Index names must meet the following criteria:

- Lowercase only
- Cannot include `\`, `/`, `*`, `?`, `"`, `<`, `>`, `|`, ` ` (space), `,`, `#`
- Indices prior to 7.0.0 could contain `:`, but that's no longer allowed
- Cannot start with `-`, `_`, `+`
- Cannot be `.` or `..`
- Cannot be longer than 255 bytes (note that it is bytes, so multi-byte characters will count towards the 255 limit faster)

## Path parameters

Parameter | Data type | Description
:--- | :--- | :---
index | String | Name of the index to create. Required.

## Query parameters

Parameter | Data type | Description | Default value
:--- | :--- | :--- | :---
wait_for_active_shards | String | The number of shard copies that must be active before the index creation returns. | 1 (just the primary shard)
master_timeout | Time | How long to wait for a connection to the primary node. | 30 seconds
timeout | Time | How long to wait for the index to be created. | 30 seconds

## Request body

```json
{
  "settings": {
    "index": {
      "number_of_shards": 3,
      "number_of_replicas": 2,
      "plugins": {
        "index_state_management": {
          "rollover_skip": true
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "field1": {
        "type": "keyword"
      },
      "field2": {
        "type": "text"
      }
    }
  },
  "aliases": {
    "my-alias": {}
  },
  "context": {
    "name": "context1",
    "schema": {
      "properties": {
        "field1": {
          "type": "keyword"
        }
      }
    }
  }
}
```

You can specify the following options in the request body:

Field | Description
:--- | :---
settings | Configure the index, such as number of shards and replicas, refresh interval, etc.
mappings | Define how documents and their fields are stored and indexed.
aliases | Specify index aliases.
context | Define a context for the index to be used with the Index Context feature.

## Example request

```json
PUT /sample-index
{
  "settings": {
    "index": {
      "number_of_shards": 3,
      "number_of_replicas": 2,
      "plugins": {
        "index_state_management": {
          "rollover_skip": true
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "field1": {
        "type": "keyword"
      },
      "field2": {
        "type": "text"
      }
    }
  },
  "aliases": {
    "my-alias": {}
  },
  "context": {
    "name": "context1",
    "schema": {
      "properties": {
        "field1": {
          "type": "keyword"
        }
      }
    }
  }
}
```

{% include copy-curl.html %}

## Example response

```json
{
  "acknowledged": true,
  "shards_acknowledged": true,
  "index": "sample-index"
}
```

## Response body fields

Field | Description
:--- | :---
acknowledged | Indicates whether the index was successfully created.
shards_acknowledged | Indicates whether the configured number of shards were started before timing out.
index | The name of the index that was created.

If the index already exists, OpenSearch returns an error:

```json
{
  "error": {
    "root_cause": [
      {
        "type": "resource_already_exists_exception",
        "reason": "index [sample-index/1234567890] already exists",
        "index_uuid": "1234567890",
        "index": "sample-index"
      }
    ],
    "type": "resource_already_exists_exception",
    "reason": "index [sample-index/1234567890] already exists",
    "index_uuid": "1234567890",
    "index": "sample-index"
  },
  "status": 400
}
```

## New options

### Error prevention validation

Starting in OpenSearch 2.9, the create index API supports error prevention validation for the rollover action. When creating an index, you can specify the `index.plugins.index_state_management.rollover_skip` setting to control whether the index should be skipped during rollover operations:

```json
{
  "settings": {
    "index": {
      "plugins": {
        "index_state_management": {
          "rollover_skip": true
        }
      }
    }
  }
}
```

This setting helps prevent errors in index management workflows by allowing you to explicitly define which indices should be excluded from rollover actions.

### Index context

Starting in OpenSearch 2.11, you can define a context for the index using the `context` field in the request body:

```json
{
  "context": {
    "name": "context1",
    "schema": {
      "properties": {
        "field1": {
          "type": "keyword"
        }
      }
    }
  }
}
```

The `context` field allows you to specify a named context and its schema, which can be used with the Index Context feature for more flexible index management and querying.

For more information about index contexts, see [Index context]({{site.url}}{{site.baseurl}}/opensearch/index-context/).
