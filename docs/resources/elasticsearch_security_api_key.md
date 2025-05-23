---
subcategory: "Security"
layout: ""
page_title: "Elasticstack: elasticstack_elasticsearch_security_api_key Resource"
description: |-
  Creates an API key for access without requiring basic authentication. See, https://www.elastic.co/guide/en/elasticsearch/reference/current/security-api-create-api-key.html
---

# elasticstack_elasticsearch_security_api_key (Resource)

Creates an API key for access without requiring basic authentication. See, https://www.elastic.co/guide/en/elasticsearch/reference/current/security-api-create-api-key.html

## Example Usage

```terraform
resource "elasticstack_elasticsearch_security_api_key" "api_key" {
  # Set the name
  name = "My API key"

  # Set the role descriptors
  role_descriptors = jsonencode({
    role-a = {
      cluster = ["all"],
      indices = [
        {
          names      = ["index-a*"],
          privileges = ["read"]
        }
      ]
    }
  })

  # Set the expiration for the API key
  expiration = "1d"

  # Set the custom metadata for this user
  metadata = jsonencode({
    "env"    = "testing"
    "open"   = false
    "number" = 49
  })
}

# restriction on a role descriptor for an API key is supported since Elastic 8.9
resource "elasticstack_elasticsearch_security_api_key" "api_key_with_restriction" {
  # Set the name
  name = "My API key"
  # Set the role descriptors
  role_descriptors = jsonencode({
    role-a = {
      cluster = ["all"],
      indices = [
        {
          names      = ["index-a*"],
          privileges = ["read"]
        }
      ],
      restriction = {
        workflows = ["search_application_query"]
      }
    }
  })

  # Set the expiration for the API key
  expiration = "1d"

  # Set the custom metadata for this user
  metadata = jsonencode({
    "env"    = "testing"
    "open"   = false
    "number" = 49
  })
}

output "api_key" {
  value     = elasticstack_elasticsearch_security_api_key.api_key
  sensitive = true
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `name` (String) Specifies the name for this API key.

### Optional

- `elasticsearch_connection` (Block List, Deprecated) Elasticsearch connection configuration block. (see [below for nested schema](#nestedblock--elasticsearch_connection))
- `expiration` (String) Expiration time for the API key. By default, API keys never expire.
- `metadata` (String) Arbitrary metadata that you want to associate with the API key.
- `role_descriptors` (String) Role descriptors for this API key.

### Read-Only

- `api_key` (String, Sensitive) Generated API Key.
- `encoded` (String, Sensitive) API key credentials which is the Base64-encoding of the UTF-8 representation of the id and api_key joined by a colon (:).
- `expiration_timestamp` (Number) Expiration time in milliseconds for the API key. By default, API keys never expire.
- `id` (String) Internal identifier of the resource.
- `key_id` (String) Unique id for this API key.

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

Import is not supported due to the generated API key only being visible on create.
