---
layout: default
title: Ingestion Source Error Handling
nav_order: 40
---

# Ingestion Source Error Handling

OpenSearch provides flexible error handling strategies for ingestion sources to manage errors that may occur during polling or processing of records. This document explains the available error handling strategies and how to configure them dynamically.

## Error Handling Strategies

There are two main error handling strategies available:

1. **DROP**: This strategy drops failures and proceeds with remaining updates in the ingestion source.
2. **BLOCK**: This strategy blocks on failures, preventing processing of remaining updates in the ingestion source.

### DROP Strategy

The DROP strategy is implemented by the `DropIngestionErrorStrategy` class. When an error occurs:

- The error is logged with details about the ingestion source.
- Failed update stats are recorded and metrics are emitted (to be implemented).
- Processing continues with the next update.

### BLOCK Strategy

The BLOCK strategy is implemented by the `BlockIngestionErrorStrategy` class. When an error occurs:

- The error is logged with details about the ingestion source.
- The blocking update is recorded and metrics are emitted (to be implemented).
- Processing stops for the current batch of updates.

## Configuring Error Handling Strategies

Error handling strategies can be configured dynamically for each ingestion source. The `IngestionErrorStrategy` interface defines the core functionality:

```java
public interface IngestionErrorStrategy {
    void handleError(Throwable e, ErrorStage stage);
    boolean shouldIgnoreError(Throwable e, ErrorStage stage);
}
```

To create an error handling strategy for an ingestion source:

```java
IngestionErrorStrategy errorStrategy = IngestionErrorStrategy.create(
    ErrorStrategy.parseFromString(errorStrategyString),
    ingestionSourceName
);
```

Where `errorStrategyString` is either "DROP" or "BLOCK" (case-insensitive).

## Error Stages

The `ErrorStage` enum defines different stages where errors can occur:

- `POLLING`: Errors that occur while polling records from the ingestion source.
- `PROCESSING`: Errors that occur while processing the polled records.

This allows for fine-grained control over error handling based on the stage of the ingestion process.

## Dynamic Configuration

Error handling strategies can be dynamically configured per ingestion source. This allows for flexibility in managing different types of ingestion sources and their specific error handling requirements.

To update the error handling strategy for an ingestion source, you would typically use an API endpoint or configuration file to specify the desired strategy. The exact method for updating the configuration may depend on your specific OpenSearch setup and plugins.

## Best Practices

1. Choose the appropriate error handling strategy based on the criticality of the data and the nature of your ingestion process.
2. Monitor and analyze error logs and metrics to identify recurring issues or patterns.
3. Regularly review and update your error handling strategies as your data ingestion requirements evolve.
4. Consider implementing retry mechanisms for transient errors before resorting to dropping or blocking.
5. Ensure that blocked updates are properly addressed to prevent long-term ingestion stalls.

## Conclusion

Proper error handling is crucial for maintaining the reliability and consistency of your data ingestion processes. By leveraging OpenSearch's flexible error handling strategies, you can ensure that your ingestion pipelines are robust and can gracefully handle various error scenarios.