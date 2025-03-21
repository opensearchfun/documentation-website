---
layout: default
title: Ingestion Error Handling
nav_order: 31
has_children: false
parent: Index Management
---

# Ingestion Error Handling

OpenSearch 2.9 introduces new error handling strategies for ingestion pipelines to provide more control over how errors are handled during data ingestion. This document explains the available strategies and how to configure them.

## Error Handling Strategies

There are two error handling strategies available:

1. DROP - Drops records that encounter errors during ingestion (default behavior)
2. BLOCK - Blocks ingestion when errors are encountered

The strategy can be configured at the index level.

## Configuring Error Handling

To configure the error handling strategy, use the `index.ingestion_source.error_strategy` setting when creating or updating an index:

```json
PUT my-index
{
  "settings": {
    "index.ingestion_source.error_strategy": "BLOCK"
  }
}
```

Valid values are:

- `DROP` - Drop records with errors (default)
- `BLOCK` - Block ingestion on errors

## Error Handling Behavior

### DROP Strategy

With the DROP strategy:

- Records that encounter errors during polling or processing are dropped
- Ingestion continues for other records
- Errors are logged but do not halt the ingestion process

### BLOCK Strategy  

With the BLOCK strategy:

- Ingestion is halted when an error is encountered during polling or processing
- The error is thrown and must be resolved before ingestion can continue
- Provides stronger guarantees that all data is ingested without errors

## Monitoring and Logging

Errors encountered during ingestion are logged regardless of the chosen strategy. You can monitor these logs to identify and troubleshoot ingestion issues.

## Choosing a Strategy

Consider the following when choosing an error handling strategy:

- Use DROP if occasional data loss is acceptable and you want to ensure continuous ingestion
- Use BLOCK if data integrity is critical and you need to ensure all records are processed successfully

The appropriate strategy depends on your specific use case and data requirements.

## API Reference

The error handling strategy can be configured using the Index Create and Index Update APIs. Refer to the [Index API documentation]({{site.url}}{{site.baseurl}}/api-reference/index-apis/create-index/) for more details on these APIs.