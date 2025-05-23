---
subcategory: "Security"
layout: ""
page_title: "Elasticstack: elasticstack_elasticsearch_security_role Data Source"
description: |-
  Retrieves roles in the native realm.
---

# Data Source: elasticstack_elasticsearch_security_role

Use this data source to get information about an existing Elasticsearch role. See, https://www.elastic.co/guide/en/elasticsearch/reference/current/security-api-get-role.html

## Example Usage

```terraform
provider "elasticstack" {
  elasticsearch {}
}

data "elasticstack_elasticsearch_security_role" "role" {
  name = "testrole"
}

output "role" {
  value = data.elasticstack_elasticsearch_security_role.role.name
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `name` (String) The name of the role.

### Optional

- `elasticsearch_connection` (Block List, Max: 1, Deprecated) Elasticsearch connection configuration block. This property will be removed in a future provider version. Configure the Elasticsearch connection via the provider configuration instead. (see [below for nested schema](#nestedblock--elasticsearch_connection))
- `run_as` (Set of String) A list of users that the owners of this role can impersonate.

### Read-Only

- `applications` (Set of Object) A list of application privilege entries. (see [below for nested schema](#nestedatt--applications))
- `cluster` (Set of String) A list of cluster privileges. These privileges define the cluster level actions that users with this role are able to execute.
- `description` (String) The description of the role.
- `global` (String) An object defining global privileges.
- `id` (String) Internal identifier of the resource
- `indices` (Set of Object) A list of indices permissions entries. (see [below for nested schema](#nestedatt--indices))
- `metadata` (String) Optional meta-data.
- `remote_indices` (Set of Object) A list of remote indices permissions entries. Remote indices are effective for remote clusters configured with the API key based model. They have no effect for remote clusters configured with the certificate based model. (see [below for nested schema](#nestedatt--remote_indices))

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


<a id="nestedatt--applications"></a>
### Nested Schema for `applications`

Read-Only:

- `application` (String)
- `privileges` (Set of String)
- `resources` (Set of String)


<a id="nestedatt--indices"></a>
### Nested Schema for `indices`

Read-Only:

- `allow_restricted_indices` (Boolean)
- `field_security` (List of Object) (see [below for nested schema](#nestedobjatt--indices--field_security))
- `names` (Set of String)
- `privileges` (Set of String)
- `query` (String)

<a id="nestedobjatt--indices--field_security"></a>
### Nested Schema for `indices.field_security`

Read-Only:

- `except` (Set of String)
- `grant` (Set of String)



<a id="nestedatt--remote_indices"></a>
### Nested Schema for `remote_indices`

Read-Only:

- `clusters` (Set of String)
- `field_security` (List of Object) (see [below for nested schema](#nestedobjatt--remote_indices--field_security))
- `names` (Set of String)
- `privileges` (Set of String)
- `query` (String)

<a id="nestedobjatt--remote_indices--field_security"></a>
### Nested Schema for `remote_indices.field_security`

Read-Only:

- `except` (Set of String)
- `grant` (Set of String)
