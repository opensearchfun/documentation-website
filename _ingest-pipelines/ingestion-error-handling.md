---
layout: default
title: Ingestion error handling
parent: Ingest pipelines
nav_order: 20
---

# Ingestion error handling

OpenSearch provides dynamic error handling strategies for ingestion sources to manage errors that occur during data ingestion. This feature allows you to configure how OpenSearch handles errors encountered during the ingestion process, providing flexibility and robustness to your data pipelines.

## Configuring error handling strategies

You can configure the error handling strategy for an ingestion source using the `INGESTION_SOURCE_ERROR_STRATEGY_SETTING` in the index settings. This setting accepts the following values:

- `DROP`: Drops the erroneous records and continues processing. This is the default behavior.
- `BLOCK`: Blocks the ingestion process when an error is encountered.

### Example configuration

To set the error handling strategy to `BLOCK` for an index:

```json
PUT /my-index
{
  "settings": {
    "index": {
      "ingestion_source": {
        "error_strategy": "BLOCK"
      }
    }
  }
}
```

## Dynamic updates

The error handling strategy can be updated dynamically without restarting the cluster. This allows you to adjust the error handling behavior based on your current needs or in response to specific error patterns.

To update the error handling strategy for an existing index:

```json
PUT /my-index/_settings
{
  "index": {
    "ingestion_source": {
      "error_strategy": "DROP"
    }
  }
}
```

## Implementing custom error handling

For more advanced error handling, you can implement custom logic in your ingestion pipeline. This can be done by using the `IngestionErrorStrategy` interface and creating a custom implementation.

Here's an example of how to create a custom error handling strategy:

```java
public class CustomErrorStrategy implements IngestionErrorStrategy {
    @Override
    public void handleError(Throwable e, ErrorStage stage) {
        // Custom error handling logic
    }

    @Override
    public boolean shouldIgnoreError(Throwable e, ErrorStage stage) {
        // Custom logic to determine if an error should be ignored
    }
}
```

To use your custom error strategy, you need to register it with OpenSearch and configure it for your ingestion source.

## Monitoring error handling

To monitor how errors are being handled during ingestion, you can use the OpenSearch monitoring APIs. These APIs provide insights into error rates, types of errors encountered, and the actions taken based on your configured error handling strategy.

## Best practices

1. Start with the `DROP` strategy for non-critical data to ensure continuous ingestion.
2. Use the `BLOCK` strategy for critical data where data integrity is paramount.
3. Implement custom error handling for complex scenarios that require specific logic.
4. Regularly monitor error rates and adjust your strategy as needed.
5. Use dynamic updates to fine-tune error handling in production environments.

By properly configuring and utilizing ingestion error handling strategies, you can build more resilient and reliable data pipelines in OpenSearch.