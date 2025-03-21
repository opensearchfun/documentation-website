---
layout: default
title: Managing indexes
nav_order: 1
has_children: false
nav_exclude: true
permalink: /im-plugin/
redirect_from:
  - /opensearch/index-data/
  - /opensearch/rest-api/index-apis/index/
  - /im-plugin/index/
---

# Managing indexes

You index data using the OpenSearch REST API. Two APIs exist: the index API and the `_bulk` API.

For situations in which new data arrives incrementally (for example, customer orders from a small business), you might use the index API to add documents individually as they arrive. For situations in which the flow of data is less frequent (for example, weekly updates to a marketing website), you might prefer to generate a file and send it to the `_bulk` API. For large numbers of documents, lumping requests together and using the `_bulk` API offers superior performance. If your documents are enormous, however, you might need to index them individually.

When indexing documents, the document `_id` must be 512 bytes or less in size.

## Introduction to indexing

Before you can search data, you must *index* it. Indexing is the method by which search engines organize data for fast retrieval. The resulting structure is called, fittingly, an index.

In OpenSearch, the basic unit of data is a JSON *document*. Within an index, OpenSearch identifies each document using a unique ID.

A request to the index API looks like this:

```json
PUT <index>/_doc/<id>
{ "A JSON": "document" }
```

A request to the `_bulk` API looks a little different, because you specify the index and ID in the bulk data:

```json
POST _bulk
{ "index": { "_index": "<index>", "_id": "<id>" } }
{ "A JSON": "document" }
```

Bulk data must conform to a specific format, which requires a newline character (`\n`) at the end of every line, including the last line. This is the basic format:

```
Action and metadata\n
Optional document\n
Action and metadata\n
Optional document\n
```

The document is optional, because `delete` actions don't require a document. The other actions (`index`, `create`, and `update`) all require a document. If you specifically want the action to fail if the document already exists, use the `create` action instead of the `index` action.
{: .note }

To index bulk data using the `curl` command, navigate to the folder where you have your file saved and run the following command:

```json
curl -H "Content-Type: application/x-ndjson" -POST https://localhost:9200/data/_bulk -u 'admin:admin' --insecure --data-binary "@data.json"
```

If any one of the actions in the `_bulk` API fail, OpenSearch continues to execute the other actions. Examine the `items` array in the response to figure out what went wrong. The entries in the `items` array are in the same order as the actions specified in the request.

## Error handling strategies

OpenSearch provides two error handling strategies for ingestion: block and drop. These strategies determine how OpenSearch handles errors during the ingestion process.

### Block ingestion error strategy

The block ingestion error strategy prevents processing of remaining updates in the ingestion source when an error occurs. This strategy is useful when you want to ensure that all data is processed successfully before continuing.

Example usage:

```java
BlockIngestionErrorStrategy strategy = new BlockIngestionErrorStrategy("myIngestionSource");
strategy.handleError(new Exception("Sample error"), ErrorStage.SOME_STAGE);
```

The `BlockIngestionErrorStrategy` logs the error and blocks further processing. Future implementations may include additional actions such as recording blocking updates and emitting metrics.

### Drop ingestion error strategy

The drop ingestion error strategy logs the error but allows processing to continue with the remaining updates in the ingestion source. This strategy is useful when you want to process as much data as possible, even if some records fail.

Example usage:

```java
DropIngestionErrorStrategy strategy = new DropIngestionErrorStrategy("myIngestionSource");
strategy.handleError(new Exception("Sample error"), ErrorStage.SOME_STAGE);
```

The `DropIngestionErrorStrategy` logs the error and continues processing. Future implementations may include additional actions such as recording failed update statistics and emitting metrics.

## Choosing an error handling strategy

When deciding which error handling strategy to use, consider the following:

1. Data integrity requirements: If it's critical that all data be processed successfully, use the block strategy.
2. Processing speed: If you need to process data quickly and can tolerate some failures, use the drop strategy.
3. Error investigation: The block strategy may be more helpful for investigating errors as it stops processing at the point of failure.
4. Resource utilization: The drop strategy may be more efficient in terms of resource utilization as it continues processing despite errors.

You can configure the error handling strategy at the index or cluster level, depending on your specific requirements.
