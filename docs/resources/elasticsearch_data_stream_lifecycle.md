---
subcategory: "Index"
layout: ""
page_title: "Elasticstack: elasticstack_elasticsearch_data_stream_lifecycle Resource"
description: |-
  Manages Lifecycle for Elasticsearch Data Streams
---

# Resource: elasticstack_elasticsearch_data_stream

Configures the data stream lifecycle for the targeted data streams, see: https://www.elastic.co/guide/en/elasticsearch/reference/current/data-stream-apis.html

## Example Usage

```terraform
provider "elasticstack" {
  elasticsearch {}
}

// First we must have a index template created
resource "elasticstack_elasticsearch_index_template" "my_data_stream_template" {
  name = "my_data_stream"

  index_patterns = ["my-stream*"]

  data_stream {}
}

// and now we can create data stream based on the index template
resource "elasticstack_elasticsearch_data_stream" "my_data_stream" {
  name = "my-stream"

  // make sure that template is created before the data stream
  depends_on = [
    elasticstack_elasticsearch_index_template.my_data_stream_template
  ]
}

// finally we can manage lifecycle of data stream
resource "elasticstack_elasticsearch_data_stream_lifecycle" "my_data_stream_lifecycle" {
  name           = "my-stream"
  data_retention = "3d"

  depends_on = [
    elasticstack_elasticsearch_data_stream.my_data_stream,
  ]
}

// or you can use wildcards to manage multiple lifecycles at once
resource "elasticstack_elasticsearch_data_stream_lifecycle" "my_data_stream_lifecycle_multiple" {
  name           = "stream-*"
  data_retention = "3d"
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `name` (String) Name of the data stream. Supports wildcards.

### Optional

- `data_retention` (String) Every document added to this data stream will be stored at least for this time frame. When empty, every document in this data stream will be stored indefinitely
- `downsampling` (Attributes List) Downsampling configuration objects, each defining an after interval representing when the backing index is meant to be downsampled and a fixed_interval representing the downsampling interval. (see [below for nested schema](#nestedatt--downsampling))
- `elasticsearch_connection` (Block List, Deprecated) Elasticsearch connection configuration block. (see [below for nested schema](#nestedblock--elasticsearch_connection))
- `enabled` (Boolean) Data stream lifecycle on/off.
- `expand_wildcards` (String) Determines how wildcard patterns in the `indices` parameter match data streams and indices. Supports comma-separated values, such as `closed,hidden`.

### Read-Only

- `id` (String) Internal identifier of the resource.

<a id="nestedatt--downsampling"></a>
### Nested Schema for `downsampling`

Required:

- `after` (String) Interval representing when the backing index is meant to be downsampled
- `fixed_interval` (String) The interval at which to aggregate the original time series index.


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
terraform import elasticstack_elasticsearch_data_stream_lifecycle.my_data_stream_lifecycle <cluster_uuid>/<data_stream_name>
```
