---
layout: default
title: Error Handling for Ingestion Sources
nav_order: 10
---

# Error Handling for Ingestion Sources

When ingesting data from external sources into OpenSearch, errors may occur during polling or processing of records. The OpenSearch ingestion framework provides configurable error handling strategies to manage these scenarios.

## Error Handling Strategies

There are two main error handling strategies available:

1. **Block** - Stops processing remaining updates when an error is encountered.
2. **Drop** - Ignores errors and continues processing remaining updates.

The error handling strategy is defined by implementing the `IngestionErrorStrategy` interface:

```java
public interface IngestionErrorStrategy {

    void handleError(Throwable e, ErrorStage stage);
    
    boolean shouldIgnoreError(Throwable e, ErrorStage stage);

}
```

## Block Strategy

The block strategy is implemented by the `BlockIngestionErrorStrategy` class. When an error occurs:

- The error is logged
- Processing of remaining updates is halted
- The error is not ignored

This is useful when you want to ensure all data is ingested correctly and stop on any errors.

## Drop Strategy 

The drop strategy is implemented by the `DropIngestionErrorStrategy` class. When an error occurs:

- The error is logged
- Processing continues with the next update
- The error is ignored

This allows ingestion to continue even if some records have errors.

## Configuring the Error Strategy

The error handling strategy can be configured when setting up ingestion:

```java
IngestionErrorStrategy errorStrategy = 
  IngestionErrorStrategy.create(ErrorStrategy.BLOCK, "myIngestionSource");
```

You can choose between `ErrorStrategy.BLOCK` or `ErrorStrategy.DROP`.

## Error Stages

The error handling logic differentiates between two stages where errors can occur:

- `POLLING` - Errors that happen while polling records from the source
- `PROCESSING` - Errors that occur while processing polled records

This allows for potentially different handling based on the error stage.

## Implementing Custom Strategies

You can implement custom error handling strategies by creating a class that implements the `IngestionErrorStrategy` interface. This allows for more advanced error handling logic if needed.

## Best Practices

- Use the block strategy for critical data where all records must be ingested correctly
- Use the drop strategy for high-volume streams where some data loss is acceptable
- Implement custom strategies for more granular control over error handling
- Monitor error logs to identify and resolve persistent ingestion issues

By leveraging these error handling capabilities, you can build more robust and resilient data ingestion pipelines in OpenSearch.