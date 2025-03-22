---
layout: default
title: Handling pipeline failures
nav_order: 15
redirect_from:
  - /api-reference/ingest-apis/pipeline-failures/
---
# Handling pipeline failures

OpenSearch provides two strategies for handling errors encountered during ingestion pipeline processing:

- **Drop**: Drops the document and continues processing subsequent documents (default behavior)
- **Block**: Stops processing the current batch of documents when an error is encountered

## Ingest pipeline metrics

OpenSearch tracks metrics for ingest pipelines to help monitor their performance and error rates. Some key metrics include:

- Number of documents processed 
- Number of documents failed
- Processing time
- Failure rate

You can view these metrics using the [Nodes Stats API]({{site.url}}{{site.baseurl}}/api-reference/nodes-apis/nodes-stats/).

## Dynamic error handling strategy

Starting in OpenSearch 2.9, you can configure a dynamic error handling strategy for ingestion sources. This allows you to specify whether to drop or block processing when errors occur during either the polling or processing stages.

To configure the error handling strategy, use the `error_strategy` parameter when creating an ingestion source:

```json
{
  "error_strategy": "BLOCK"
}
```

Valid values are:

- `DROP` - Continue processing and drop documents that encounter errors (default)
- `BLOCK` - Stop processing the current batch when an error is encountered

You can configure different strategies for the polling and processing stages:

```json
{
  "error_strategy": {
    "polling": "BLOCK",
    "processing": "DROP"
  }
}
```

The error handling strategy can be updated dynamically without restarting the ingestion source.

When using the `BLOCK` strategy, OpenSearch will stop processing the current batch of documents if an error occurs. You can then investigate the error, address any issues, and resume processing.

The `DROP` strategy allows processing to continue by skipping problematic documents, which can be useful for maintaining ingestion throughput when occasional errors are expected and acceptable.

Choose the appropriate strategy based on your data quality requirements and operational needs. Use metrics and monitoring to track error rates and adjust the strategy as needed.
