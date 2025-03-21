---
layout: default
title: Ingest pipelines
nav_order: 5
has_children: false
has_toc: true
parent: OpenSearch
nav_exclude: true
permalink: /ingest-pipelines/
redirect_from:
  - /api-reference/ingest-apis/ingest-pipelines/
  - /ingest-pipelines/index/
---

# Ingest pipelines

Ingest pipelines allow you to pre-process documents before indexing. You can use ingest pipelines to perform tasks such as removing fields, extracting values from fields, and enriching data. 

This page covers how to create and manage ingest pipelines, including new error handling strategies introduced in recent versions.

## Creating an ingest pipeline

To create an ingest pipeline, use the [Create Pipeline API]({{site.url}}{{site.baseurl}}/api-reference/ingest-apis/create-ingest/):

```json
PUT _ingest/pipeline/my-pipeline
{
  "description" : "My ingest pipeline",
  "processors" : [
    {
      "set" : {
        "field": "processed_date",
        "value": "{{_ingest.timestamp}}"
      }
    }
  ]
}
```

## Error handling strategies

As of version X.X, ingest pipelines support configurable error handling strategies. These strategies define how errors are handled during document processing.

You can set the error handling strategy when creating or updating a pipeline:

```json
PUT _ingest/pipeline/my-pipeline
{
  "description": "Pipeline with error strategy",
  "processors": [...],
  "error_strategy": "DROP"
}
```

The available error strategies are:

- `DROP`: Silently drop documents that encounter errors during processing. This is the default behavior.
- `BLOCK`: Block indexing and return an error when a document fails processing.

### Handling errors programmatically

For more advanced error handling, you can implement custom logic using the `IngestionErrorStrategy` interface:

```java
public interface IngestionErrorStrategy {
    void handleError(Throwable e, ErrorStage stage);
    boolean shouldIgnoreError(Throwable e, ErrorStage stage);
}
```

This allows you to define custom behavior for different types of errors or processing stages.

## Managing pipelines

- To retrieve an existing pipeline: `GET _ingest/pipeline/my-pipeline`
- To delete a pipeline: `DELETE _ingest/pipeline/my-pipeline`
- To update a pipeline, simply create a new one with the same name

For more information on ingest pipeline APIs, see the [Ingest APIs documentation]({{site.url}}{{site.baseurl}}/api-reference/ingest-apis/).
