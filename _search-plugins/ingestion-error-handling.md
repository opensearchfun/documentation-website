```markdown
---
layout: default
title: Ingestion Error Handling
nav_order: 99
---

# Ingestion Error Handling

The ingestion error handling feature in OpenSearch provides strategies for dealing with errors encountered during data ingestion, either while polling records from an ingestion source or during the processing of polled records.

## Error Handling Strategies

OpenSearch supports two main error handling strategies:

1. **Drop**: Drops failures and proceeds with remaining updates in the ingestion source.
2. **Block**: Blocks on failures, preventing processing of remaining updates in the ingestion source.

These strategies are implemented through the `IngestionErrorStrategy` interface.

## Configuring Error Handling

To configure the error handling strategy, use the `ErrorStrategy` enum in your ingestion configuration:

```java
ErrorStrategy errorStrategy = ErrorStrategy.parseFromString(errorStrategyString);
IngestionErrorStrategy strategy = IngestionErrorStrategy.create(errorStrategy, ingestionSource);
```

Where `errorStrategyString` is either "DROP" or "BLOCK", and `ingestionSource` is a string identifier for the ingestion source.

## Error Stages

Errors can occur in two stages of the ingestion process:

1. **POLLING**: Errors that occur while polling records from the ingestion source.
2. **PROCESSING**: Errors that occur while processing the polled records.

These stages are represented by the `ErrorStage` enum.

## Implementing Custom Error Handling

To implement custom error handling logic, you can create a class that implements the `IngestionErrorStrategy` interface:

```java
public class CustomIngestionErrorStrategy implements IngestionErrorStrategy {
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

## Built-in Error Handling Implementations

OpenSearch provides two built-in implementations of `IngestionErrorStrategy`:

### DropIngestionErrorStrategy

This strategy logs errors but allows ingestion to continue:

```java
public class DropIngestionErrorStrategy implements IngestionErrorStrategy {
    @Override
    public void handleError(Throwable e, ErrorStage stage) {
        logger.error("Error processing update from {}: {}", ingestionSource, e);
        // Additional error recording and metric emission can be implemented here
    }

    @Override
    public boolean shouldIgnoreError(Throwable e, ErrorStage stage) {
        return true;
    }
}
```

### BlockIngestionErrorStrategy

This strategy logs errors and prevents further ingestion:

```java
public class BlockIngestionErrorStrategy implements IngestionErrorStrategy {
    @Override
    public void handleError(Throwable e, ErrorStage stage) {
        logger.error("Error processing update from {}: {}", ingestionSource, e);
        // Additional error recording and metric emission can be implemented here
    }

    @Override
    public boolean shouldIgnoreError(Throwable e, ErrorStage stage) {
        return false;
    }
}
```

## Best Practices

When implementing error handling for ingestion:

1. Always log errors for observability and debugging.
2. Consider implementing metrics to track error rates and types.
3. For critical ingestion processes, use the BLOCK strategy to ensure data integrity.
4. For less critical processes where some data loss is acceptable, use the DROP strategy to maintain ingestion throughput.
5. Implement custom error handling strategies for complex scenarios that require more nuanced behavior.

By properly configuring and implementing ingestion error handling, you can ensure that your OpenSearch cluster remains stable and continues to ingest data effectively, even in the face of errors or inconsistencies in the ingestion process.
```