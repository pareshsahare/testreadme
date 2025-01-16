# 1-Organization

## Purpose

The purpose of this step is to setup development, non-production, and production environments within the Google Cloud organization that you've created.


## Prerequisites


## Terraform Version

This module requires Terraform **>= 1.0.9**.

## Provider Requirements

| Name          | Source               | Version  |
|--------------|----------------------|----------|
| **google**   | hashicorp/google      | 4.47.0   |
| **google-beta** | hashicorp/google-beta | < 5.0  |

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

## Resources
| Name | Type |
|------|------|
| [google_organization_iam_audit_config.org_config](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/organization_iam_audit_config) | resource |
| [google_folder_iam_audit_config.folder_config](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/folder_iam_audit_config) | resource |
| [google_project_iam_member.security_ops_log_bq_user](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_iam_member) | resource |
| [google_project_iam_member.security_ops_log_bq_data_viewer](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_iam_member) | resource |
| [google_project_iam_member.security_ops_logging_viewer](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_iam_member) | resource |
| [google_project_iam_member.elevated_security_ops_log_bq_user](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_iam_member) | resource |
| [google_project_iam_member.elevated_security_ops_log_bq_data_viewer](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_iam_member) | resource |
| [google_project_iam_member.elevated_security_ops_log_admin](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_iam_member) | resource |
| [google_project_iam_member.operations_log_bq_user](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_iam_member) | resource |
| [google_project_iam_member.operations_log_bq_data_viewer](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_iam_member) | resource |
| [google_project_iam_member.operations_logging_viewer](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_iam_member) | resource |
| [google_project_iam_member.default-audit-logging-iam-permissions](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_iam_member) | resource |
| [google_project_iam_member.default-operations-logging-iam-permissions](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_iam_member) | resource |
| [google_project_iam_member.default-security-logging-iam-permissions](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_iam_member) | resource |
| [google_kms_key_ring.audit_logs_keyring](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/kms_key_ring) | resource |
| [google_kms_crypto_key.audit_logs_key](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/kms_crypto_key) | resource |
| [google_kms_key_ring.org_operations_logs_keyring](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/kms_key_ring) | resource |
| [google_kms_crypto_key.org_operations_logs_key](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/kms_crypto_key) | resource |
| [google_kms_key_ring.storage_destination_security_keyring](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/kms_key_ring) | resource |
| [google_kms_crypto_key.storage_destination_security_key](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/kms_crypto_key) | resource |
| [google_storage_project_service_account.audit-gcs-account](https://registry.terraform.io/providers/hashicorp/google/latest/docs/data-sources/storage_project_service_account) | data source |
| [google_project_iam_member.grant-google-audit-encrypt-decrypt](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_iam_member) | resource |
| [google_storage_project_service_account.ops-gcs-account](https://registry.terraform.io/providers/hashicorp/google/latest/docs/data-sources/storage_project_service_account) | data source |
| [google_project_iam_member.grant-google-ops-encrypt-decrypt](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_iam_member) | resource |
| [google_storage_proje




## Inputs
| Name                                        | Description                                                                                   | Type                | Default Value                                                                                                         | Required |
|---------------------------------------------|-----------------------------------------------------------------------------------------------|---------------------|-----------------------------------------------------------------------------------------------------------------------|----------|
| enable_os_login_policy                      | Enable OS Login Organization Policy.                                                           | bool                | false                                                                                                                 | No       |
| audit_logs_table_expiration_days           | Period before tables expire for all audit logs in milliseconds. Default is 30 days.            | number              | 30                                                                                                                    | No       |
| skip_gcloud_download                       | Whether to skip downloading gcloud (assumes gcloud is already available outside the module. If set to true, you must ensure that Gcloud Alpha module is installed.) | bool                | true                                                                                                                 | No       |
| scc_notification_filter                    | Filter used to create the Security Command Center Notification. More details: [Filter Docs](https://cloud.google.com/security-command-center/docs/how-to-api-filter-notifications#create-filter) | string              | "state = \"ACTIVE\""                                                                                                  | No       |
| parent_folder                              | Optional - for an organization with existing projects or for development/validation. It will place all the example foundation resources under the provided folder instead of the root organization. The value is the numeric folder ID. The folder must already exist. Must be the same value used in previous step. | string              | "807812708582"                                                                                                        | No       |
| create_access_context_manager_access_policy | Whether to create access context manager access policy                                        | bool                | true                                                                                                                 | No       |
| data_access_logs_enabled                   | Enable Data Access logs of types DATA_READ, DATA_WRITE, and ADMIN_READ for all GCP services. Enabling Data Access logs might result in your organization being charged for the additional logs usage. See [Data Access Logs Docs](https://cloud.google.com/logging/docs/audit#data-access) | bool                | true                                                                                                                 | No       |
| log_export_storage_force_destroy           | If set to true, delete all contents when destroying the resource; otherwise, destroying the resource will fail if contents are present. | bool                | true                                                                                                                 | No       |
| log_export_storage_versioning              | Toggles bucket versioning, ability to retain a non-current object version when the live object version gets replaced or deleted. | bool                | false                                                                                                                | No       |
| audit_logs_table_delete_contents_on_destroy| If set to true, delete all the tables in the dataset when destroying the resource; otherwise, destroying the resource will fail if tables are present. | bool                | true                                                                                                                 | No       |
| log_export_storage_retention_policy        | Configuration of the bucket's data retention policy for how long objects in the bucket should be retained. | object              | null                                                                                                                 | No       |
| dns_hub_project_alert_spent_percents       | A list of percentages of the budget to alert on when threshold is exceeded for the DNS hub project. | list(number)        | [0.5, 0.75, 0.9, 0.95]                                                                                               | No       |
| dns_hub_project_alert_pubsub_topic         | The name of the Cloud Pub/Sub topic where budget related messages will be published, in the form of `projects/{project_id}/topics/{topic_id}` for the DNS hub project. | string              | null                                                                                                                 | No       |
| dns_hub_project_budget_amount              | The amount to use as the budget for the DNS hub project.                                         | number              | 1000                                                                                                                 | No       |
| base_net_hub_project_alert_spent_percents  | A list of percentages of the budget to alert on when threshold is exceeded for the base net hub project. | list(number)        | [0.5, 0.75, 0.9, 0.95]                                                                                               | No       |
| base_net_hub_project_alert_pubsub_topic    | The name of the Cloud Pub/Sub topic where budget related messages will be published, in the form of `projects/{project_id}/topics/{topic_id}` for the base net hub project. | string              | null                                                                                                                 | No       |
| base_net_hub_project_budget_amount         | The amount to use as the budget for the base net hub project.                                    | number              | 1000                                                                                                                 | No       |
| restricted_net_hub_project_alert_spent_percents | A list of percentages of the budget to alert on when threshold is exceeded for the restricted net hub project. | list(number)        | [0.5, 0.75, 0.9, 0.95]                                                                                               | No       |
| restricted_net_hub_project_alert_pubsub_topic | The name of the Cloud Pub/Sub topic where budget related messages will be published, in the form of `projects/{project_id}/topics/{topic_id}` for the restricted net hub project. | string              | null                                                                                                                 | No       |
| restricted_net_hub_project_budget_amount   | The amount to use as the budget for the restricted net hub project.                             | number              | 1000                                                                                                                 | No       |
| org_secrets_project_alert_spent_percents   | A list of percentages of the budget to alert on when threshold is exceeded for the org secrets project. | list(number)        | [0.5, 0.75, 0.9, 0.95]                                                                                               | No       |
| org_secrets_project_alert_pubsub_topic     | The name of the Cloud Pub/Sub topic where budget related messages will be published, in the form of `projects/{project_id}/topics/{topic_id}` for the org secrets project. | string              | null                                                                                                                 | No       |
| org_secrets_project_budget_amount          | The amount to use as the budget for the org secrets project.                                     | number              | 1000                                                                                                                 | No       |
| org_billing_logs_project_alert_spent_percents | A list of percentages of the budget to alert on when threshold is exceeded for the org billing logs project. | list(number)        | [0.5, 0.75, 0.9, 0.95]                                                                                               | No       |
| org_billing_logs_project_alert_pubsub_topic | The name of the Cloud Pub/Sub topic where budget related messages will be published, in the form of `projects/{project_id}/topics/{topic_id}` for the org billing logs project. | string              | null                                                                                                                 | No       |
| org_billing_logs_project_budget_amount     | The amount to use as the budget for the org billing logs project.                               | number              | 1000                                                                                                                 | No       |
| org_audit_logs_project_alert_spent_percents | A list of percentages of the budget to alert on when threshold is exceeded for the org audit logs project. | list(number)        | [0.5, 0.75, 0.9, 0.95]                                                                                               | No       |
| org_audit_logs_project_alert_pubsub_topic  | The name of the Cloud Pub/Sub topic where budget related messages will be published, in the form of `projects/{project_id}/topics/{topic_id}` for the org audit logs project. | string              | null                                                                                                                 | No       |
| org_audit_logs_project_budget_amount       | The amount to use as the budget for the org audit logs project.                                 | number              | 1000                                                                                                                 | No       |
| scc_notifications_project_alert_spent_percents | A list of percentages of the budget to alert on when threshold is exceeded for the SCC notifications project. | list(number)        | [0.5, 0.75, 0.9, 0.95]                                                                                               | No       |
| scc_notifications_project_alert_pubsub_topic | The name of the Cloud Pub/Sub topic where budget related messages will be published, in the form of `projects/{project_id}/topics/{topic_id}` for the SCC notifications project. | string              | null                                                                                                                 | No       |
| scc_notifications_project_budget_amount    | The amount to use as the budget for the SCC notifications project.                              | number              | 1000                                                                                                                 | No       |
| gcp_platform_viewer                        | G Suite or Cloud Identity group that have the ability to view resource information across the Google Cloud organization. | string              | null                                                                                                                 | No       |
| gcp_security_reviewer                      | G Suite or Cloud Identity group that members are part of the security team responsible for reviewing cloud security. | string              | null                                                                                                                 | No       |
| gcp_network_viewer                         | G Suite or Cloud Identity group that members are part of the networking team and review network configurations | string              | null                                                                                                                 | No       |
| gcp_scc_admin                              | G Suite or Cloud Identity group that can administer Security Command Center.                   | string              | null                                                                                                                 | No       |
| gcp_audit_viewer                           | Members are part of an audit team and view audit logs in the logging project.                  | string              | null                                                                                                                 | No       |
| gcp_global_secrets_admin                   | G Suite or Cloud Identity group that members are responsible for putting secrets into Secrets Manager. | string              | null                                                                                                                 | No       |
| gcp_org_admin_user                         | Identity that has organization administrator permissions.                                       | string              | null                                                                                                                 | No       |
| gcp_billing_creator_user                   | Identity that can create billing accounts.                                                     | string              | null                                                                                                                 | No       |
| gcp_billing_admin_user                     | Identity that has billing administrator permissions.                                            | string              | null                                                                                                                 | No       |

## Output
| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="output_parent_resource_id"></a> [parent_resource_id](#output_parent_resource_id) | The parent resource id. | `string` | N/A | yes |
| <a name="output_parent_resource_type"></a> [parent_resource_type](#output_parent_resource_type) | The parent resource type. | `string` | N/A | yes |
| <a name="output_logs_export_pubsub_topic_audit"></a> [logs_export_pubsub_topic_audit](#output_logs_export_pubsub_topic_audit) | The Pub/Sub topic for destination of log exports (audit). | `string` | `""` | no |
| <a name="output_logs_export_pubsub_topic_operations"></a> [logs_export_pubsub_topic_operations](#output_logs_export_pubsub_topic_operations) | The Pub/Sub topic for destination of log exports (operations). | `string` | `""` | no |
| <a name="output_logs_export_pubsub_topic_security"></a> [logs_export_pubsub_topic_security](#output_logs_export_pubsub_topic_security) | The Pub/Sub topic for destination of log exports (security). | `string` | `""` | no |
| <a name="output_logs_export_storage_bucket_name_audit"></a> [logs_export_storage_bucket_name_audit](#output_logs_export_storage_bucket_name_audit) | The storage bucket for destination of log exports (audit). | `string` | `""` | no |
| <a name="output_logs_export_storage_bucket_name_operations"></a> [logs_export_storage_bucket_name_operations](#output_logs_export_storage_bucket_name_operations) | The storage bucket for destination of log exports (operations). | `string` | `""` | no |
| <a name="output_logs_export_storage_bucket_name_security"></a> [logs_export_storage_bucket_name_security](#output_logs_export_storage_bucket_name_security) | The storage bucket for destination of log exports (security). | `string` | `""` | no |
| <a name="output_billing_account"></a> [billing_account](#output_billing_account) | The billing account ID. | `string` | N/A | yes |
| <a name="output_folder_id"></a> [folder_id](#output_folder_id) | The folder ID from the `path_id_map`. | `string` | N/A | yes |
| <a name="output_org_audit_logs_project_id"></a> [org_audit_logs_project_id](#output_org_audit_logs_project_id) | The org audit logs project ID. | `string` | N/A | yes |
| <a name="output_org_audit_logs_project_name"></a> [org_audit_logs_project_name](#output_org_audit_logs_project_name) | The org audit logs project name. | `string` | N/A | yes |
| <a name="output_org_billing_logs_project_id"></a> [org_billing_logs_project_id](#output_org_billing_logs_project_id) | The org billing logs project ID. | `string` | N/A | yes |
| <a name="output_base_net_hub_project_id"></a> [base_net_hub_project_id](#output_base_net_hub_project_id) | The Base Network hub project ID. | `string` | `null` | no |
| <a name="output_org_operations_logs_project_id"></a> [org_operations_logs_project_id](#output_org_operations_logs_project_id) | The org operations logs project ID. | `string` | N/A | yes |
| <a name="output_org_operations_logs_project_name"></a> [org_operations_logs_project_name](#output_org_operations_logs_project_name) | The org operations logs project name. | `string` | N/A | yes |
| <a name="output_org_secrets_project_id"></a> [org_secrets_project_id](#output_org_secrets_project_id) | The org secrets project ID. | `string` | N/A | yes |
| <a name="output_org_security_logs_project_id"></a> [org_security_logs_project_id](#output_org_security_logs_project_id) | The org security logs project ID. | `string` | N/A | yes |
| <a name="output_org_security_logs_project_name"></a> [org_security_logs_project_name](#output_org_security_logs_project_name) | The org security logs project name. | `string` | N/A | yes |

