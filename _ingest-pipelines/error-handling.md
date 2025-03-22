---
layout: default
title: Error handling in ingest pipelines
parent: Ingest pipelines
nav_order: 16
---

# Error handling in ingest pipelines

Ingest pipelines in OpenSearch allow you to preprocess documents before indexing. However, errors can occur during pipeline execution. This page explains how to handle pipeline failures and configure error behavior.

## Pipeline execution behavior

By default, an ingest pipeline stops processing if one of its processors fails. There are two options for handling failures:

1. Fail the entire pipeline: The document is not indexed if any processor fails.
2. Continue execution: The pipeline continues to the next processor even if the current one fails.

## Configuring error handling

You can configure error handling behavior at the processor level using the `ignore_failure` parameter:

```json
PUT _ingest/pipeline/my-pipeline
{
  "description": "My ingest pipeline",
  "processors": [
    {
      "rename": {
        "field": "provider",
        "target_field": "cloud.provider",
        "ignore_failure": true
      }
    }
  ]
}
```

Setting `ignore_failure` to `true` allows the pipeline to continue even if that specific processor fails.

## Using on_failure processors

For more granular error handling, you can specify `on_failure` processors that execute when a processor fails:

```json
PUT _ingest/pipeline/my-pipeline
{
  "description": "My ingest pipeline",
  "processors": [
    {
      "rename": {
        "field": "provider",
        "target_field": "cloud.provider",
        "on_failure": [
          {
            "set": {
              "field": "_index",
              "value": "failed-{{ _index }}"
            }
          }
        ]
      }
    }
  ]
}
```

In this example, if the rename processor fails, the document will be indexed to a different index prefixed with "failed-".

## Pipeline-level error handling

You can also specify `on_failure` processors at the pipeline level to handle any errors that occur during pipeline execution:

```json
PUT _ingest/pipeline/my-pipeline
{
  "description": "My ingest pipeline",
  "processors": [...],
  "on_failure": [
    {
      "set": {
        "field": "error.message",
        "value": "{{ _ingest.on_failure_message }}"
      }
    }
  ]
}
```

This adds an error message field to documents that fail processing in the pipeline.

## Monitoring pipeline errors

You can monitor ingest pipeline errors using the [Nodes Stats API]({{site.url}}{{site.baseurl}}/api-reference/nodes-apis/nodes-stats/):

```
GET /_nodes/stats/ingest?filter_path=nodes.*.ingest
```

This returns statistics for all ingest pipelines, including error counts.

## Best practices

1. Use `ignore_failure` for non-critical processors where pipeline continuation is more important than data accuracy.
2. Implement `on_failure` processors for critical steps to handle errors gracefully.
3. Consider using pipeline-level error handling for consistent error management across all processors.
4. Regularly monitor pipeline statistics to identify and address recurring errors.

By properly configuring error handling in your ingest pipelines, you can ensure more robust and reliable data processing in OpenSearch.