---
layout: default
title: Kafka ingestion
nav_order: 15
parent: Ingest pipelines
---

# Kafka ingestion

The Kafka ingestion plugin allows you to ingest data from Kafka topics directly into OpenSearch. This enables real-time data streaming and indexing from Kafka.

## Configuration

To use Kafka ingestion, you need to configure it in your `opensearch.yml` file:

```yaml
plugins.ingestion.kafka:
  enabled: true
  bootstrap_servers: "localhost:9092"
  topic: "my-topic"
  group_id: "my-group"
  auto_offset_reset: "earliest"
```

The main configuration options are:

- `enabled`: Set to `true` to enable Kafka ingestion
- `bootstrap_servers`: List of Kafka brokers to connect to
- `topic`: The Kafka topic to consume messages from
- `group_id`: The consumer group ID to use
- `auto_offset_reset`: Where to start consuming messages from if no offset is stored

## Usage

Once configured, the Kafka ingestion plugin will automatically start consuming messages from the specified topic and index them into OpenSearch.

You can customize the ingestion behavior by creating an ingest pipeline. For example:

```json
PUT _ingest/pipeline/kafka-pipeline
{
  "description" : "Pipeline for processing Kafka messages",
  "processors" : [
    {
      "json" : {
        "field" : "message"
      }
    },
    {
      "date" : {
        "field" : "timestamp",
        "formats" : ["ISO8601"]
      }
    }
  ]
}
```

Then associate this pipeline with your index:

```json
PUT my-index
{
  "settings" : {
    "index.default_pipeline" : "kafka-pipeline"
  }
}
```

## Monitoring

You can monitor Kafka ingestion metrics using the cluster stats API:

```
GET /_cluster/stats?filter_path=nodes.ingestion.kafka
```

This will return metrics like number of messages consumed, consumer lag, etc.

## Security considerations

- Ensure your Kafka cluster is properly secured
- Use SSL/TLS for connections between OpenSearch and Kafka
- Set up appropriate authentication if required (e.g. SASL)

## Limitations

- Currently supports JSON messages only
- Limited to consuming from a single topic per OpenSearch cluster

## Troubleshooting

Common issues:

- Check Kafka broker connectivity if no messages are being ingested
- Verify topic and consumer group configurations
- Examine OpenSearch logs for any ingestion errors

For more details on troubleshooting, see the [Kafka consumer documentation](https://kafka.apache.org/documentation/#consumerconfigs).