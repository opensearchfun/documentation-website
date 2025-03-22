---
layout: default
title: Ingest processors
nav_order: 30
has_children: true
has_toc: false
redirect_from:
  - /api-reference/ingest-apis/ingest-processors/
---
# Ingest processors

Ingest processors are used to pre-process documents before indexing. A processor transforms and enriches the document by manipulating or adding fields.

Processors are configured as part of an [ingest pipeline]({{site.url}}{{site.baseurl}}/api-reference/ingest-apis/create-update-ingest/). Each processor runs in sequence as defined in the pipeline, with changes made by one processor visible to subsequent processors.

## Supported processors

OpenSearch supports the following processors:

- [append]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/append/)
- [bytes]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/bytes/)
- [convert]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/convert/)
- [csv]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/csv/)
- [date]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/date/)
- [dissect]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/dissect/)
- [dot expander]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/dot-expander/)
- [drop]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/drop/)
- [fail]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/fail/)
- [foreach]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/foreach/)
- [grok]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/grok/)
- [gsub]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/gsub/)
- [html strip]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/html-strip/)
- [json]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/json/)
- [kv]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/kv/)
- [lowercase]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/lowercase/)
- [ml inference]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/ml-inference/)
- [pipeline]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/pipeline/)
- [remove]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/remove/)
- [rename]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/rename/)
- [script]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/script/)
- [set]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/set/)
- [sort]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/sort/)
- [split]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/split/)
- [trim]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/trim/)
- [uppercase]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/uppercase/)
- [urldecode]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/urldecode/)
- [user_agent]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/user-agent/)

## Processor limit settings

You can limit the number of processors that can be used in a single ingest pipeline by configuring the `processor_limit` setting:

```json
PUT /_cluster/settings
{
  "persistent": {
    "ingest.processor_limit": 100
  }
}
```

The default limit is 50 processors per pipeline.

## Batch-enabled processors

Some processors support batch processing, which can significantly improve performance when processing large volumes of documents. These processors implement the `BatchProcessor` interface:

- `append`
- `convert`
- `date`
- `drop`
- `foreach`
- `json`
- `kv`
- `lowercase`
- `remove`
- `rename`
- `script`
- `set`
- `split`
- `trim`
- `uppercase`

Batch-enabled processors can process multiple documents in a single execution, reducing overhead and improving throughput.

## Selectively enabling processors

You can selectively enable or disable ingest processors using the `ingest.processor.<processor_type>.enabled` setting. For example, to disable the `kv` processor:

```json
PUT /_cluster/settings
{
  "persistent": {
    "ingest.processor.kv.enabled": false
  }
}
```

This setting can be useful for security purposes or to reduce resource usage by disabling unused processors.

## New dynamic error handling strategy for ingestion sources

OpenSearch now supports a dynamic error handling strategy for ingestion sources. This feature allows you to configure how OpenSearch handles errors during the ingestion process, providing more flexibility and control over error management.

To configure the error handling strategy, use the following cluster setting:

```json
PUT /_cluster/settings
{
  "persistent": {
    "ingestion.error_strategy": "DROP"
  }
}
```

The available options for `ingestion.error_strategy` are:

- `DROP`: Drops the document that caused the error and continues processing the rest of the batch.
- `HALT`: Stops processing the entire batch when an error is encountered.
- `RETRY`: Attempts to retry processing the document that caused the error.

You can also configure the error handling strategy at the index level:

```json
PUT /my-index
{
  "settings": {
    "index.ingestion.error_strategy": "RETRY"
  }
}
```

This allows you to have different error handling strategies for different indexes, providing fine-grained control over error management during ingestion.
