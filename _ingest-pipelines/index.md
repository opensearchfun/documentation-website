---
layout: default
title: Ingest pipelines
nav_order: 5
nav_exclude: true
has_toc: true
permalink: /ingest-pipelines/
redirect_from:
  - /api-reference/ingest-apis/ingest-pipelines/
  - /ingest-pipelines/index/
---

# Ingest pipelines

An _ingest pipeline_ is a sequence of _processors_ that are applied to documents as they are ingested into an index. Each [processor]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/index-processors/) in a pipeline performs a specific task, such as filtering, transforming, or enriching data. 

Processors are customizable tasks that run in a sequential order as they appear in the request body. This order is important, as each processor depends on the output of the previous processor. The modified documents appear in your index after the processors are applied.

## OpenSearch ingest pipelines compared to Data Prepper

OpenSeach ingest pipelines run within the OpenSearch cluster, whereas [Data Prepper]({{site.url}}{{site.baseurl}}/data-prepper/) is an external component that runs on the OpenSearch cluster. 

OpenSearch ingest pipelines perform actions on indexes and are preferred for use cases involving pre-processing simple datasets, [machine learning (ML) processors]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/sparse-encoding/), and [vector embedding processors]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/text-image-embedding/). OpenSearch ingest pipelines are recommended for simple data pre-processing and small datasets. 

Data Prepper is recommended for any data processing tasks it supports, particularly when dealing with large datasets and complex data pre-processing requirements. It streamlines the process of transferring and fetching large datasets while providing robust capabilities for intricate data preparation and transformation operations. Refer to the [Data Prepper]({{site.url}}{{site.baseurl}}/data-prepper/) documentation for more information.      

OpenSearch ingest pipelines can only be managed using [Ingest API operations]({{site.url}}{{site.baseurl}}/api-reference/ingest-apis/index/).
{: .note}

## Prerequisites 

The following are prerequisites for using OpenSearch ingest pipelines:

- When using ingestion in a production environment, your cluster should contain at least one node with the node roles permission set to `ingest`. For information about setting up node roles within a cluster, see [Cluster Formation]({{site.url}}{{site.baseurl}}/opensearch/cluster/).
- If the OpenSearch Security plugin is enabled, you must have the `cluster_manage_pipelines` permission to manage ingest pipelines.

## Define a pipeline

A _pipeline definition_ describes the sequence of an ingest pipeline and can be written in JSON format. An ingest pipeline consists of the following:

```json
{
    "description" : "..."
    "processors" : [...]
}
```

#### Request body fields

Field | Required | Type | Description
:--- | :--- | :--- | :---
`processors` | Required | Array of processor objects | A component that performs a specific data processing task as the data is being ingested into OpenSearch.
`description` | Optional | String | A description of the ingest pipeline. 

## Kafka ingestion

OpenSearch now supports ingesting data from Kafka topics using ingest pipelines. This allows you to easily stream data from Kafka into OpenSearch for indexing and analysis.

To configure Kafka ingestion:

1. Create an ingest pipeline that specifies Kafka as the source:

```json
PUT _ingest/pipeline/kafka-pipeline
{
  "description": "Ingest pipeline for Kafka data",
  "processors": [
    {
      "kafka": {
        "topic": "my-topic",
        "bootstrap_servers": ["localhost:9092"],
        "group_id": "my-consumer-group"
      }
    }
  ]
}
```

2. Apply the pipeline to an index:

```json
PUT my-index
{
  "settings": {
    "index.default_pipeline": "kafka-pipeline"
  }
}
```

3. Start ingesting data by sending messages to the specified Kafka topic. The pipeline will consume messages and index them into OpenSearch.

You can configure additional options like security settings, message format, etc. in the Kafka processor configuration. See the [Kafka processor documentation]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/kafka/) for more details.

## Next steps

Learn how to:

- [Create a pipeline]({{site.url}}{{site.baseurl}}/ingest-pipelines/create-ingest/).
- [Test a pipeline]({{site.url}}{{site.baseurl}}/ingest-pipelines/simulate-ingest/).
- [Retrieve information about a pipeline]({{site.url}}{{site.baseurl}}/ingest-pipelines/get-ingest/).
- [Delete a pipeline]({{site.url}}{{site.baseurl}}/ingest-pipelines/delete-ingest/). 
- [Use ingest processors in OpenSearch]({{site.url}}{{site.baseurl}}/ingest-pipelines/processors/index-processors/)
