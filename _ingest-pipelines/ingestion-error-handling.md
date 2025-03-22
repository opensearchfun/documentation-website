---
layout: default
title: Ingestion error handling
nav_order: 14
parent: Ingest pipelines
---

# Ingestion error handling

OpenSearch provides error handling strategies for ingest pipelines to manage errors that may occur during data ingestion. This feature allows you to configure how OpenSearch should respond to errors encountered during the polling of records from an ingestion source or during the processing of polled records.

## Error handling strategies

OpenSearch supports two error handling strategies:

1. **DROP**: This strategy ignores the error and continues processing subsequent records. It is the default strategy if not specified.
2. **BLOCK**: This strategy stops the ingestion process when an error is encountered.

## Configuring error handling

You can configure the error handling strategy when creating or updating an ingest pipeline. The configuration is done using the `error_strategy` parameter in the pipeline definition.

### Example configuration

```json
{
  "description": "Ingest pipeline with error handling",
  "processors": [...],
  "error_strategy": "BLOCK"
}
```

## Error stages

OpenSearch distinguishes between two stages where errors can occur:

1. **POLLING**: Errors that occur while polling records from the ingestion source.
2. **PROCESSING**: Errors that occur while processing the polled records.

## IngestionErrorStrategy interface

The `IngestionErrorStrategy` interface defines the contract for error handling strategies. It includes two main methods:

1. `handleError(Throwable e, ErrorStage stage)`: This method processes and records the error.
2. `shouldIgnoreError(Throwable e, ErrorStage stage)`: This method determines if the error should be ignored.

### Implementation

OpenSearch provides two implementations of the `IngestionErrorStrategy` interface:

1. `BlockIngestionErrorStrategy`: Implements the BLOCK strategy.
2. `DropIngestionErrorStrategy`: Implements the DROP strategy.

The appropriate strategy is created based on the `error_strategy` configuration using the `create` method:

```java
static IngestionErrorStrategy create(ErrorStrategy errorStrategy, String ingestionSource) {
    switch (errorStrategy) {
        case BLOCK:
            return new BlockIngestionErrorStrategy(ingestionSource);
        case DROP:
        default:
            return new DropIngestionErrorStrategy(ingestionSource);
    }
}
```

## Configuring error strategy

You can configure the error strategy when setting up an ingest pipeline. Here's an example of how to set the error strategy to BLOCK:

```json
PUT _ingest/pipeline/my-pipeline
{
  "description": "My ingest pipeline with BLOCK error strategy",
  "processors": [...],
  "error_strategy": "BLOCK"
}
```

If not specified, the default error strategy is DROP.

## Error logging and monitoring

Regardless of the chosen strategy, all errors are logged for monitoring and troubleshooting purposes. It's recommended to set up appropriate logging and monitoring to track and analyze ingestion errors.

## Best practices

1. Choose the appropriate error strategy based on your data quality requirements and operational needs.
2. Use the BLOCK strategy for critical data where accuracy is paramount.
3. Use the DROP strategy for high-volume data ingestion where occasional errors can be tolerated.
4. Implement proper error logging and monitoring to track and analyze ingestion errors.
5. Regularly review error logs to identify and address recurring issues in your data or ingestion process.

By effectively using ingestion error handling, you can create more robust and reliable data pipelines in OpenSearch, ensuring that your data ingestion processes are resilient to errors and align with your data quality requirements.