---
subcategory: "Logstash"
layout: ""
page_title: "Elasticstack: elasticstack_elasticsearch_logstash_pipeline Resource"
description: |-
  Creates or updates centrally managed logstash pipelines.
---

# Resource: elasticstack_elasticsearch_logstash_pipeline

Creates or updates centrally managed logstash pipelines. See: https://www.elastic.co/guide/en/elasticsearch/reference/current/logstash-apis.html

## Example Usage

```terraform
provider "elasticstack" {
  elasticsearch {}
}

resource "elasticstack_elasticsearch_logstash_pipeline" "example" {
  pipeline_id = "test_pipeline"
  description = "This is an example pipeline"

  pipeline = <<-EOF
  input{}
  filter{}
  output{}
EOF

  pipeline_metadata = jsonencode({
    "type"    = "logstash_pipeline"
    "version" = 1
  })
  pipeline_batch_delay         = 50
  pipeline_batch_size          = 125
  pipeline_ecs_compatibility   = "disabled"
  pipeline_ordered             = "auto"
  pipeline_plugin_classloaders = false
  pipeline_unsafe_shutdown     = false
  pipeline_workers             = 1
  queue_checkpoint_acks        = 1024
  queue_checkpoint_retry       = true
  queue_checkpoint_writes      = 1024
  queue_drain                  = false
  queue_max_bytes              = "1gb"
  queue_max_events             = 0
  queue_page_capacity          = "64mb"
  queue_type                   = "persisted"
}

output "pipeline" {
  value = elasticstack_elasticsearch_logstash_pipeline.example.pipeline_id
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `pipeline` (String) Configuration for the pipeline.
- `pipeline_id` (String) Identifier for the pipeline.

### Optional

- `description` (String) Description of the pipeline.
- `elasticsearch_connection` (Block List, Max: 1, Deprecated) Elasticsearch connection configuration block. This property will be removed in a future provider version. Configure the Elasticsearch connection via the provider configuration instead. (see [below for nested schema](#nestedblock--elasticsearch_connection))
- `pipeline_batch_delay` (Number) Time in milliseconds to wait for each event before sending an undersized batch to pipeline workers.
- `pipeline_batch_size` (Number) The maximum number of events an individual worker thread collects before executing filters and outputs.
- `pipeline_ecs_compatibility` (String) Sets the pipeline default value for ecs_compatibility, a setting that is available to plugins that implement an ECS compatibility mode for use with the Elastic Common Schema.
- `pipeline_metadata` (String) Optional JSON metadata about the pipeline.
- `pipeline_ordered` (String) Set the pipeline event ordering.
- `pipeline_plugin_classloaders` (Boolean) (Beta) Load Java plugins in independent classloaders to isolate their dependencies.
- `pipeline_unsafe_shutdown` (Boolean) Forces Logstash to exit during shutdown even if there are still inflight events in memory.
- `pipeline_workers` (Number) The number of parallel workers used to run the filter and output stages of the pipeline.
- `queue_checkpoint_acks` (Number) The maximum number of ACKed events before forcing a checkpoint when persistent queues are enabled.
- `queue_checkpoint_retry` (Boolean) When enabled, Logstash will retry four times per attempted checkpoint write for any checkpoint writes that fail. Any subsequent errors are not retried.
- `queue_checkpoint_writes` (Number) The maximum number of written events before forcing a checkpoint when persistent queues are enabled.
- `queue_drain` (Boolean) When enabled, Logstash waits until the persistent queue is drained before shutting down.
- `queue_max_bytes` (String) Units for the total capacity of the queue when persistent queues are enabled.
- `queue_max_events` (Number) The maximum number of unread events in the queue when persistent queues are enabled.
- `queue_page_capacity` (String) The size of the page data files used when persistent queues are enabled. The queue data consists of append-only data files separated into pages.
- `queue_type` (String) The internal queueing model for event buffering. Options are memory for in-memory queueing, or persisted for disk-based acknowledged queueing.
- `username` (String) User who last updated the pipeline.

### Read-Only

- `id` (String) Internal identifier of the resource
- `last_modified` (String) Date the pipeline was last updated.

<a id="nestedblock--elasticsearch_connection"></a>
### Nested Schema for `elasticsearch_connection`

Optional:

- `api_key` (String, Sensitive) API Key to use for authentication to Elasticsearch
- `bearer_token` (String, Sensitive) Bearer Token to use for authentication to Elasticsearch
- `ca_data` (String) PEM-encoded custom Certificate Authority certificate
- `ca_file` (String) Path to a custom Certificate Authority certificate
- `cert_data` (String) PEM encoded certificate for client auth
- `cert_file` (String) Path to a file containing the PEM encoded certificate for client auth
- `endpoints` (List of String, Sensitive) A list of endpoints where the terraform provider will point to, this must include the http(s) schema and port number.
- `es_client_authentication` (String, Sensitive) ES Client Authentication field to be used with the JWT token
- `headers` (Map of String, Sensitive) A list of headers to be sent with each request to Elasticsearch.
- `insecure` (Boolean) Disable TLS certificate validation
- `key_data` (String, Sensitive) PEM encoded private key for client auth
- `key_file` (String) Path to a file containing the PEM encoded private key for client auth
- `password` (String, Sensitive) Password to use for API authentication to Elasticsearch.
- `username` (String) Username to use for API authentication to Elasticsearch.

## Import

Import is supported using the following syntax:

```shell
terraform import elasticstack_elasticsearch_logstash_pipeline.example <cluster_uuid>/<pipeline ID>
```
