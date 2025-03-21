```markdown
---
layout: default
title: Error handling for ingestion
nav_order: 40
has_children: false
parent: Ingest pipelines
---

# Error handling for ingestion

OpenSearch provides configurable error handling strategies for ingestion pipelines to manage failures during document processing. This page explains the available error handling options and how to configure them.

## Error handling strategies

There are two main error handling strategies available:

1. Block ingestion error strategy
2. Drop ingestion error strategy

### Block ingestion error strategy

The block ingestion error strategy prevents processing of remaining updates in the ingestion source when an error occurs. This strategy is useful when you want to ensure all documents are successfully processed or none at all.

Key characteristics:
- Blocks on failures
- Prevents processing of remaining updates
- Logs errors but does not ignore them

### Drop ingestion error strategy

The drop ingestion error strategy allows processing to continue by dropping failed updates. This strategy is useful when you want to ingest as much data as possible, even if some documents fail.

Key characteristics:
- Drops failures
- Continues processing remaining updates
- Logs errors but ignores them for continued processing

## Configuring error handling

To configure the error handling strategy for an ingestion pipeline, you can specify it when creating or updating the pipeline. Here's an example of how to set the error handling strategy:

```json
PUT _ingest/pipeline/my-pipeline
{
  "description": "My ingestion pipeline",
  "processors": [ ... ],
  "error_handling": {
    "strategy": "block"
  }
}
```

The `error_handling.strategy` field can be set to either `"block"` or `"drop"`.

## Error handling behavior

When an error occurs during document processing:

1. The error is logged with details about the failed update and the ingestion source.
2. Depending on the configured strategy:
   - Block strategy: Processing stops for all remaining updates.
   - Drop strategy: The failed update is skipped, and processing continues with the next update.
3. Metrics about failed updates are emitted for monitoring purposes.

## Monitoring and troubleshooting

To monitor and troubleshoot ingestion errors:

1. Check the OpenSearch logs for error messages related to ingestion failures.
2. Use the [Ingest APIs]({{site.url}}{{site.baseurl}}/api-reference/ingest-apis/index/) to inspect pipeline configurations and statistics.
3. Monitor ingestion-related metrics to track error rates and performance.

## Best practices

- Choose the appropriate error handling strategy based on your data quality requirements and ingestion workflow.
- Implement proper monitoring and alerting for ingestion errors to quickly identify and address issues.
- Regularly review and optimize your ingestion pipelines to minimize errors and improve performance.

## Next steps

- Learn how to [create an ingest pipeline]({{site.url}}{{site.baseurl}}/ingest-pipelines/create-ingest/).
- Explore [ingest processors]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/index-processors/) to customize your data processing.
- Understand how to [simulate pipelines]({{site.url}}{{site.baseurl}}/ingest-pipelines/simulate-ingest/) for testing and debugging.
```