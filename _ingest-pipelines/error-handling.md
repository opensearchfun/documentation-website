---
layout: default
title: Ingestion error handling
nav_order: 15
---

# Ingestion Error Handling

The OpenSearch ingestion framework provides configurable error handling strategies to manage errors encountered during polling or processing of records from ingestion sources.

## Error Handling Strategies

There are two main error handling strategies available:

1. `DROP` - This strategy drops records that encounter errors and continues processing remaining records.

2. `BLOCK` - This strategy blocks further processing when an error is encountered.

## Configuring Error Handling

The error handling strategy can be configured when setting up an ingestion pipeline:

```java
IngestionErrorStrategy errorStrategy = IngestionErrorStrategy.create(
    ErrorStrategy.DROP, 
    "my-ingestion-source"
);
```

The `ErrorStrategy` enum defines the available strategies:

```java
public enum ErrorStrategy {
    DROP,
    BLOCK
}
```

## Error Stages

The error handling framework distinguishes between two stages where errors can occur:

1. `POLLING` - Errors that occur while polling records from the ingestion source.

2. `PROCESSING` - Errors that occur while processing polled records.

This allows for more granular error handling if needed.

## Implementing Custom Error Handling

To implement custom error handling logic, you can extend the `IngestionErrorStrategy` interface:

```java
public interface IngestionErrorStrategy {

    void handleError(Throwable e, ErrorStage stage);
    
    boolean shouldIgnoreError(Throwable e, ErrorStage stage);

}
```

The `handleError` method allows you to define custom error handling behavior, while `shouldIgnoreError` determines whether a particular error should be ignored.

## Built-in Implementations

OpenSearch provides two built-in implementations of `IngestionErrorStrategy`:

1. `DropIngestionErrorStrategy` - Implements the DROP strategy
2. `BlockIngestionErrorStrategy` - Implements the BLOCK strategy

These can be used as-is or extended for more customized error handling.

## Best Practices

- Choose an appropriate error strategy based on your use case and data integrity requirements.
- For critical ingestion pipelines, consider using the BLOCK strategy to prevent data loss.
- Implement custom error handling for more granular control over specific error scenarios.
- Use logging and metrics to monitor error rates and types for troubleshooting.

By leveraging these error handling capabilities, you can build more robust and reliable ingestion pipelines in OpenSearch.