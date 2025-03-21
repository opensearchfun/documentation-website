---
layout: default
title: Java client
nav_order: 30
---

# Ingestion error handling

The OpenSearch Java client provides options for handling errors that may occur during data ingestion, particularly when using the `DefaultStreamPoller` class. This document outlines the available error handling strategies and how to configure them.

## Error handling strategies

OpenSearch supports two main error handling strategies:

1. DROP: Ignore the error and continue processing subsequent records.
2. BLOCK: Stop processing and pause the poller when an error is encountered.

These strategies are defined in the `IngestionErrorStrategy.ErrorStrategy` enum.

## Configuring error handling

You can configure the error handling strategy when initializing the `DefaultStreamPoller`:

```java
IngestionErrorStrategy errorStrategy = IngestionErrorStrategy.create(
    IngestionErrorStrategy.ErrorStrategy.DROP, 
    "my-ingestion-source"
);

DefaultStreamPoller poller = new DefaultStreamPoller(
    startPointer,
    persistedPointers,
    consumer,
    ingestionEngine,
    resetState,
    resetValue,
    errorStrategy
);
```

## Handling errors during polling

The `DefaultStreamPoller` handles errors differently depending on the stage where they occur:

### Polling errors

When an error occurs during the polling stage:

1. The error is logged: `logger.error("Error in polling the shard {}: {}", consumer.getShardId(), e);`
2. The error strategy's `handleError` method is called: `errorStrategy.handleError(e, IngestionErrorStrategy.ErrorStage.POLLING);`
3. If the error strategy indicates the error should be ignored:
   - The batch start pointer is advanced to the next record.
4. If the error should not be ignored:
   - The poller is paused to stop processing remaining updates.

### Processing errors

Errors that occur during the processing stage are handled by the `MessageProcessorRunnable` class, which uses the same error strategy configured for the poller.

## Updating the error strategy

You can update the error handling strategy at runtime using the `updateErrorStrategy` method:

```java
IngestionErrorStrategy newStrategy = IngestionErrorStrategy.create(
    IngestionErrorStrategy.ErrorStrategy.BLOCK, 
    "my-ingestion-source"
);
poller.updateErrorStrategy(newStrategy);
```

This method updates the strategy for both the poller and the message processor.

## Custom error strategies

You can create custom error handling strategies by implementing the `IngestionErrorStrategy` interface. This allows you to define more complex error handling logic if needed.

## Best practices

1. Choose an appropriate error strategy based on your data integrity requirements and operational needs.
2. Monitor error logs and adjust your strategy if you notice recurring issues.
3. Consider implementing custom error strategies for more fine-grained control over error handling.
4. Regularly review and update your error handling configuration as your ingestion processes evolve.

By properly configuring error handling for your Java client, you can ensure more robust and reliable data ingestion processes in OpenSearch.
