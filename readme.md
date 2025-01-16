# 1-Organization

## Purpose

The purpose of this step is to setup development, non-production, and production environments within the Google Cloud organization that you've created.


## Prerequisites
1. 0-bootstrap executed successfully.

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


## Inputs
| Name                                        | Description                                                                                   | Type                | Default Value                                                                                                         | Required |
|---------------------------------------------|-----------------------------------------------------------------------------------------------|---------------------|-----------------------------------------------------------------------------------------------------------------------|----------|
| [activate_apis](#input_activate_apis)       | List of APIs to enable in the seed project.                                                   | `list(string)`      | `["admin.googleapis.com", "appengine.googleapis.com", "bigquery.googleapis.com", "cloudbilling.googleapis.com", ...]`  | no       |
| [enable_os_login_policy](#enable_os_login_policy)         | Enable OS Login Organization Policy.                                                          | `bool`              | `false`                                                                                                               | no       |
| [audit_logs_table_expiration_days](#audit_logs_table_expiration_days) | Period before tables expire for all audit logs in milliseconds. Default is 30 days.             | `number`            | `30`                                                                                                                  | no       |
| [skip_gcloud_download](#skip_gcloud_download)           | Whether to skip downloading gcloud (assumes gcloud is already available outside the module).  | `bool`              | `true`                                                                                                               | no       |
| [scc_notification_filter](#scc_notification_filter)        | Filter used to create the Security Command Center Notification.                              | `string`            | `"state = \"ACTIVE\""`                                                                                                 | no       |
| [parent_folder](#parent_folder)           | Optional folder ID for placing resources under a specific folder instead of the root organization. | `string`            | `"807812708582"`                                                                                                      | no       |
| [create_access_context_manager_access_policy](#create_access_context_manager_access_policy) | Whether to create access context manager access policy.                                         | `bool`              | `true`                                                                                                               | no       |
| [data_access_logs_enabled](#data_access_logs_enabled)     | Enable Data Access logs for all GCP services. May incur additional charges.                    | `bool`              | `true`                                                                                                               | no       |
| [log_export_storage_force_destroy](#log_export_storage_force_destroy) | Force deletion of all contents when destroying the resource.                                  | `bool`              | `true`                                                                                                               | no       |
| [log_export_storage_versioning](#log_export_storage_versioning) | Enable bucket versioning. Retains non-current object versions when live versions are replaced. | `bool`              | `false`                                                                                                              | no       |
| [audit_logs_table_delete_contents_on_destroy](#audit_logs_table_delete_contents_on_destroy) | Delete all tables in the dataset when destroying the resource.                                  | `bool`              | `true`                                                                                                               | no       |
| [log_export_storage_retention_policy](#log_export_storage_retention_policy) | Data retention policy configuration for the bucket's objects.                                 | `object`            | `null`                                                                                                               | no       |
| [dns_hub_project_alert_spent_percents](#dns_hub_project_alert_spent_percents) | List of percentages for budget alerts on the DNS hub project.                                  | `list(number)`      | `[0.5, 0.75, 0.9, 0.95]`                                                                                               | no       |
| [dns_hub_project_alert_pubsub_topic](#dns_hub_project_alert_pubsub_topic) | The Pub/Sub topic for budget alerts for the DNS hub project.                                    | `string`            | `null`                                                                                                               | no       |
| [dns_hub_project_budget_amount](#dns_hub_project_budget_amount) | Budget amount for the DNS hub project.                                                         | `number`            | `1000`                                                                                                               | no       |
| [base_net_hub_project_alert_spent_percents](#base_net_hub_project_alert_spent_percents) | List of percentages for budget alerts on the base net hub project.                             | `list(number)`      | `[0.5, 0.75, 0.9, 0.95]`                                                                                               | no       |
| [base_net_hub_project_alert_pubsub_topic](#base_net_hub_project_alert_pubsub_topic) | The Pub/Sub topic for budget alerts for the base net hub project.                               | `string`            | `null`                                                                                                               | no       |
| [base_net_hub_project_budget_amount](#base_net_hub_project_budget_amount) | Budget amount for the base net hub project.                                                     | `number`            | `1000`                                                                                                               | no       |
| [restricted_net_hub_project_alert_spent_percents](#restricted_net_hub_project_alert_spent_percents) | List of percentages for budget alerts on the restricted net hub project.                       | `list(number)`      | `[0.5, 0.75, 0.9, 0.95]`                                                                                               | no       |
| [restricted_net_hub_project_alert_pubsub_topic](#restricted_net_hub_project_alert_pubsub_topic) | The Pub/Sub topic for budget alerts for the restricted net hub project.                         | `string`            | `null`                                                                                                               | no       |
| [restricted_net_hub_project_budget_amount](#restricted_net_hub_project_budget_amount) | Budget amount for the restricted net hub project.                                               | `number`            | `1000`                                                                                                               | no       |
| [org_secrets_project_alert_spent_percents](#org_secrets_project_alert_spent_percents) | List of percentages for budget alerts on the org secrets project.                              | `list(number)`      | `[0.5, 0.75, 0.9, 0.95]`                                                                                               | no       |
| [org_secrets_project_alert_pubsub_topic](#org_secrets_project_alert_pubsub_topic) | The Pub/Sub topic for budget alerts for the org secrets project.                                | `string`            | `null`                                                                                                               | no       |
| [org_secrets_project_budget_amount](#org_secrets_project_budget_amount) | Budget amount for the org secrets project.                                                     | `number`            | `1000`                                                                                                               | no       |
| [org_billing_logs_project_alert_spent_percents](#org_billing_logs_project_alert_spent_percents) | List of percentages for budget alerts on the org billing logs project.                          | `list(number)`      | `[0.5, 0.75, 0.9, 0.95]`                                                                                               | no       |
| [org_billing_logs_project_alert_pubsub_topic](#org_billing_logs_project_alert_pubsub_topic) | The Pub/Sub topic for budget alerts for the org billing logs project.                           | `string`            | `null`                                                                                                               | no       |
| [org_billing_logs_project_budget_amount](#org_billing_logs_project_budget_amount) | Budget amount for the org billing logs project.                                                | `number`            | `1000`                                                                                                               | no       |
| [org_audit_logs_project_alert_spent_percents](#org_audit_logs_project_alert_spent_percents) | List of percentages for budget alerts on the org audit logs project.                           | `list(number)`      | `[0.5, 0.75, 0.9, 0.95]`                                                                                               | no       |
| [org_audit_logs_project_alert_pubsub_topic](#org_audit_logs_project_alert_pubsub_topic) | The Pub/Sub topic for budget alerts for the org audit logs project.                             | `string`            | `null`                                                                                                               | no       |
| [org_audit_logs_project_budget_amount](#org_audit_logs_project_budget_amount) | Budget amount for the org audit logs project.                                                  | `number`            | `1000`                                                                                                               | no       |
| [scc_notifications_project_alert_spent_percents](#scc_notifications_project_alert_spent_percents) | List of percentages for budget alerts on the SCC notifications project.                        | `list(number)`      | `[0.5, 0.75, 0.9, 0.95]`                                                                                               | no       |
| [scc_notifications_project_alert_pubsub_topic](#scc_notifications_project_alert_pubsub_topic) | The Pub/Sub topic for budget alerts for the SCC notifications project.                          | `string`            | `null`                                                                                                               | no       |
| [scc_notifications_project_budget_amount](#scc_notifications_project_budget_amount) | Budget amount for the SCC notifications project.                                               | `number`            | `1000`                                                                                                               | no       |
| [gcp_platform_viewer](#gcp_platform_viewer)             | G Suite or Cloud Identity group with the ability to view resource information across the GCP org. | `string`            | `null`                                                                                                               | no       |
| [gcp_security_reviewer](#gcp_security_reviewer)         | G Suite or Cloud Identity group for members of the security team reviewing cloud security.     | `string`            | `null`                                                                                                               | no       |
| [gcp_network_viewer](#gcp_network_viewer)               | G Suite or Cloud Identity group for members of the networking team reviewing network configs.  | `string`            | `null`                                                                                                               | no       |
| [gcp_scc_admin](#gcp_scc_admin)                         | G Suite or Cloud Identity group for admins of Security Command Center.                         | `string`            | `null`                                                                                                               | no       |
| [gcp_audit_viewer](#gcp_audit_viewer)                   | Members part of the audit team with view access to audit logs in the logging project.          | `string`            | `null`                                                                                                               | no       |
| [gcp_global_secrets_admin](#gcp_global_secrets_admin)   | G Suite or Cloud Identity group for admins of Secrets Manager.                                 | `string`            | `null`                                                                                                               | no       |
| [gcp_org_admin_user](#gcp_org_admin_user)               | Identity with organization administrator permissions.                                          | `string`            | `null`                                                                                                               | no       |
| [gcp_billing_creator_user](#gcp_billing_creator_user)   | Identity for creating billing accounts.                                                        | `string`            | `null`                                                                                                               | no       |
| [gcp_billing_admin_user](#gcp_billing_admin_user)       | Identity with billing administrator permissions.                                               | `string`            | `null`                                                                                                               | no       |

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

