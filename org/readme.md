# 1-Organization

## Purpose

The purpose of this step is to setup development, non-production, and production environments within the Google Cloud organization that you've created.

## Prerequisites
    1. 0-bootstrap executed successfully.

## Requirements
| Name                                          | Version |
|-----------------------------------------------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement_terraform) | >= 1.0.9 |
| <a name="requirement_google"></a> [google](#requirement_google)       | >= 5.0.0 |

## Modules
| Name | Source | Version |
|------|--------|---------|
| <a name="module_foldertree"></a> [foldertree](#module\_foldertree) | ../../modules/foldertree | n/a |
| <a name="module_base_network_hub"></a> [base\_network\_hub](#module\_base\_network\_hub) | ../../modules/base_network_hub | n/a |
| <a name="module_storage_destination"></a> [storage\_destination](#module\_storage\_destination) | ../../modules/storage_destination | n/a |
| <a name="module_bigquery_destination"></a> [bigquery\_destination](#module\_bigquery\_destination) | ../../modules/bigquery_destination | n/a |
| <a name="module_bigquery"></a> [bigquery](#module\_bigquery) | ../../modules/bigquery | n/a |
| <a name="module_storage"></a> [storage](#module\_storage) | ../../modules/storage | n/a |
| <a name="module_pubsub"></a> [pubsub](#module\_pubsub) | ../../modules/pubsub | n/a |
| <a name="module_org_policies"></a> [org\_policies](#module\_org\_policies) | ../../modules/org_policies | n/a |
| <a name="module_domain_restricted_sharing"></a> [domain\_restricted\_sharing](#module\_domain\_restricted\_sharing) | ../../modules/domain_restricted_sharing | n/a |

### foldertree

## resource
| Name                                                              | Type    |
|-------------------------------------------------------------------|---------|
| [google_folder](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/google_folder) | resource |

## Inputs
| Variable Name  | Description                                                                        | Type          | Default |
|----------------|------------------------------------------------------------------------------------|---------------|---------|
| `folder_list`  | List of all the folders paths to be created. Example: [gp/parent/child]             | `set(string)` |         |
| `root_id`      | ID of the Google organization or the top level folder where the first level of folders in the given list should be created. | `string`      |         |

## Outputs
| Output Name   | Description                                         |
|---------------|-----------------------------------------------------|
| `path_id_map` | Map of all the folder's paths to IDs.              |

### base_network_hub

## Inputs
| Variable Name                              | Description                                                                 | Type         | Default |
|--------------------------------------------|-----------------------------------------------------------------------------|--------------|---------|
| random_project_id                          | Adds a suffix of 4 random characters to the `project_id`.                   | bool         | false   |
| org_id                                     | The organization ID.                                                        | string       | N/A     |
| domain                                     | The domain name (optional).                                                  | string       | ""      |
| name                                       | The name for the project.                                                   | string       | N/A     |
| project_id                                 | The ID to give the project. If not provided, the `name` will be used.       | string       | ""      |
| svpc_host_project_id                       | The ID of the host project which hosts the shared VPC.                       | string       | ""      |
| enable_shared_vpc_host_project            | If this project is a shared VPC host project. Default is false.              | bool         | false   |
| billing_account                            | The ID of the billing account to associate this project with.               | string       | N/A     |
| folder_id                                  | The ID of a folder to host this project.                                     | string       | ""      |
| group_name                                 | A group to control the project by being assigned group_role (defaults to project editor). | string       | ""      |
| group_role                                 | The role to give the controlling group (group_name) over the project (defaults to project editor). | string       | "roles/editor" |
| create_project_sa                          | Whether the default service account for the project shall be created.       | bool         | true    |
| project_sa_name                            | Default service account name for the project.                               | string       | "project-service-account" |
| sa_role                                    | A role to give the default Service Account for the project (defaults to none). | string       | ""      |
| activate_apis                              | The list of apis to activate within the project.                             | list(string) | ["compute.googleapis.com"] |
| activate_api_identities                   | The list of service identities to force-create for the project.              | list(object) | []      |
| usage_bucket_name                          | Name of a GCS bucket to store GCE usage reports in (optional).              | string       | ""      |
| usage_bucket_prefix                        | Prefix in the GCS bucket to store GCE usage reports in (optional).          | string       | ""      |
| credentials_path                           | Path to a service account credentials file with rights to run the Project Factory. | string       | ""      |
| impersonate_service_account                | An optional service account to impersonate.                                 | string       | ""      |
| shared_vpc_subnets                         | List of subnets fully qualified subnet IDs.                                 | list(string) | []      |
| labels                                     | Map of labels for project.                                                  | map(string)  | {}      |
| bucket_project                             | A project to create a GCS bucket in, useful for Terraform state (optional). | string       | ""      |
| bucket_name                                | A name for a GCS bucket to create (in the bucket_project project), useful for Terraform state (optional). | string       | ""      |
| bucket_location                            | The location for a GCS bucket to create (optional).                         | string       | "US"    |
| bucket_versioning                          | Enable versioning for a GCS bucket to create (optional).                    | bool         | false   |
| bucket_labels                              | A map of key/value label pairs to assign to the bucket (optional).          | map(any)     | {}      |
| bucket_force_destroy                       | Force the deletion of all objects within the GCS bucket when deleting the bucket (optional). | bool         | false   |
| auto_create_network                        | Create the default network.                                                 | bool         | false   |
| lien                                       | Add a lien on the project to prevent accidental deletion.                   | bool         | false   |
| disable_services_on_destroy                | Whether project services will be disabled when the resources are destroyed. | bool         | true    |
| default_service_account                    | Project default service account setting: can be one of `delete`, `deprivilege`, `disable`, or `keep`. | string       | "disable" |
| disable_dependent_services                 | Whether services that are enabled and which depend on this service should also be disabled when this service is destroyed. | bool         | true    |
| budget_amount                              | The amount to use for a budget alert.                                       | number       | null    |
| budget_alert_pubsub_topic                  | The name of the Cloud Pub/Sub topic where budget related messages will be published. | string       | null    |
| budget_monitoring_notification_channels    | A list of monitoring notification channels.                                 | list(string) | []      |
| budget_alert_spent_percents                | A list of percentages of the budget to alert on when threshold is exceeded. | list(number) | [0.5, 0.7, 1.0] |
| vpc_service_control_attach_enabled         | Whether the project will be attached to a VPC Service Control Perimeter.    | bool         | false   |
| vpc_service_control_perimeter_name         | The name of a VPC Service Control Perimeter to add the created project to.  | string       | null    |
| grant_services_security_admin_role        | Whether to grant Kubernetes Engine Service Agent the Security Admin role on the host project. | bool         | false   |
| consumer_quotas                            | The quotas configuration you want to override for the project.              | list(object) | []      |

## Outputs
| Output Name                         | Description                                                                      |
|-------------------------------------|----------------------------------------------------------------------------------|
| project_name                        | The name of the project                                                          |
| project_id                          | The ID of the project                                                            |
| project_number                      | The project number                                                               |
| domain                              | The organization's domain                                                        |
| group_email                         | The email of the G Suite group with group_name                                    |
| service_account_id                  | The ID of the default service account                                            |
| service_account_display_name        | The display name of the default service account                                   |
| service_account_email               | The email of the default service account                                         |
| service_account_name                | The fully-qualified name of the default service account                          |
| service_account_unique_id           | The unique ID of the default service account                                      |
| project_bucket_self_link            | Project's bucket selfLink                                                        |
| project_bucket_url                  | Project's bucket URL                                                             |
| api_s_account                       | API service account email                                                        |
| api_s_account_fmt                   | API service account email formatted for terraform use                            |
| enabled_apis                        | Enabled APIs in the project                                                      |
| enabled_api_identities              | Enabled API identities in the project                                            |
| budget_name                         | The name of the budget if created   

### storage_destination

## Inputs
| Variable Name            | Description                                                                                                                                                               | Type                      | Default                          |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------|----------------------------------|
| destination_uri          | The self_link URI of the destination resource (This is available as an output coming from one of the destination submodules)                                                 | string                    | N/A                              |
| filter                   | The filter to apply when exporting logs. Only log entries that match the filter are exported. Default is '' which exports all logs.                                       | string                    | ""                               |
| exclusions               | (Optional) A list of sink exclusion filters.                                                                                                                                  | list(object)              | []                               |
| include_children         | Only valid if 'organization' or 'folder' is chosen as var.parent_resource.type. Determines whether or not to include children organizations/folders in the sink export.   | bool                      | false                            |
| log_sink_name            | The name of the log sink to be created.                                                                                                                                      | string                    | N/A                              |
| parent_resource_id       | The ID of the GCP resource in which you create the log sink. If var.parent_resource_type is set to 'project', then this is the Project ID (and etc).                        | string                    | N/A                              |
| parent_resource_type     | The GCP resource in which you create the log sink. The value must not be computed, and must be one of the following: 'project', 'folder', 'billing_account', or 'organization'. | string                    | "project"                        |
| unique_writer_identity   | Whether or not to create a unique identity associated with this sink. If false (the default), then the writer_identity used is serviceAccount:cloud-logs@system.gserviceaccount.com. If true, then a unique service account is created and used for the logging sink. | bool                      | false                            |
| bigquery_options         | (Optional) Options that affect sinks exporting data to BigQuery. use_partitioned_tables - (Required) Whether to use BigQuery's partition tables.                          | object                    | null                             |

## Outputs
| Output Name              | Description                                                                                                 |
|--------------------------|-------------------------------------------------------------------------------------------------------------|
| filter                   | The filter to be applied when exporting logs.                                                               |
| log_sink_resource_id     | The resource ID of the log sink that was created.                                                           |
| log_sink_resource_name   | The resource name of the log sink that was created.                                                         |
| parent_resource_id       | The ID of the GCP resource in which you create the log sink.                                                 |
| writer_identity          | The service account that logging uses to write log entries to the destination.                               |

### bigquery_destination

## Inputs
| Variable Name            | Description                                                                                                  | Type                                  | Default     |
|--------------------------|--------------------------------------------------------------------------------------------------------------|---------------------------------------|-------------|
| destination_uri          | The self_link URI of the destination resource (This is available as an output coming from one of the destination submodules) | `string`                              |             |
| filter                   | The filter to apply when exporting logs. Only log entries that match the filter are exported. Default is '' which exports all logs. | `string`                              | `""`        |
| exclusions               | (Optional) A list of sink exclusion filters.                                                                 | `list(object({name = string, description = string, filter = string, disabled = bool}))` | `[]`        |
| include_children         | Determines whether to include children organizations/folders in the sink export. If true, logs associated with child projects are also exported; otherwise only logs relating to the provided organization/folder are included. | `bool`                                | `false`     |
| log_sink_name            | The name of the log sink to be created.                                                                      | `string`                              |             |
| parent_resource_id       | The ID of the GCP resource in which you create the log sink. If var.parent_resource_type is set to 'project', then this is the Project ID (and etc). | `string`                              |             |
| parent_resource_type     | The GCP resource in which you create the log sink. The value must not be computed, and must be one of the following: 'project', 'folder', 'billing_account', or 'organization'. | `string`                              | `project`   |
| unique_writer_identity   | Whether or not to create a unique identity associated with this sink. If false (the default), then the writer_identity used is serviceAccount:cloud-logs@system.gserviceaccount.com. If true, then a unique service account is created and used for the logging sink. | `bool`                                | `false`     |
| bigquery_options         | (Optional) Options that affect sinks exporting data to BigQuery. use_partitioned_tables - (Required) Whether to use BigQuery's partition tables. | `object({use_partitioned_tables = bool})` | `null`      |

## Outputs
| Output Name              | Description                                                                                      |
|--------------------------|--------------------------------------------------------------------------------------------------|
| filter                   | The filter to be applied when exporting logs.                                                     |
| log_sink_resource_id     | The resource ID of the log sink that was created.                                                |
| log_sink_resource_name   | The resource name of the log sink that was created.                                              |
| parent_resource_id       | The ID of the GCP resource in which you create the log sink.                                      |
| writer_identity          | The service account that logging uses to write log entries to the destination.                    |

### bigquery

## Inputs
| Variable Name               | Description                                                                                         | Type              | Default        |
|-----------------------------|-----------------------------------------------------------------------------------------------------|-------------------|----------------|
| dataset_name                | The name of the bigquery dataset to be created and used for log entries matching the filter.         | string            | None           |
| log_sink_writer_identity    | The service account that logging uses to write log entries to the destination.                       | string            | None           |
| project_id                  | The ID of the project in which the bigquery dataset will be created.                                | string            | None           |
| location                    | The location of the storage bucket.                                                                  | string            | "US"           |
| delete_contents_on_destroy  | (Optional) If set to true, delete all the tables in the dataset when destroying the resource.       | bool              | false          |
| expiration_days             | Table expiration time. If unset logs will never be deleted.                                         | number            | null           |
| description                 | A user-friendly description of the dataset.                                                        | string            | "Log export dataset" |
| labels                      | Dataset labels.                                                                                   | map(string)       | {}             |

## Outputs
| Output Name        | Description                                                        |
|--------------------|--------------------------------------------------------------------|
| console_link       | The console link to the destination bigquery dataset.              |
| project            | The project in which the bigquery dataset was created.             |
| resource_name      | The resource name for the destination bigquery dataset.            |
| resource_id        | The resource id for the destination bigquery dataset.              |
| self_link          | The self_link URI for the destination bigquery dataset.            |
| destination_uri    | The destination URI for the bigquery dataset.                      |

### storage

## Inputs

## Outputs

### pubsub

## Inputs

## Outputs

### domain_restricted_sharing

## Inputs

## Outputs

### org_policies

## Inputs

## Outputs
