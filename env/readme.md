# 2-Environment

## Purpose

The purpose of this step is to setup development, non-production, and production environments within the Google Cloud organization that you've created.


## Prerequisites
1. 0-bootstrap executed successfully.
2. 1-org executed successfully.


## Requirements
| Name                                          | Version |
|-----------------------------------------------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement_terraform) | >= 1.0.9 |
| <a name="requirement_google"></a> [google](#requirement_google)       | >= 5.0.0 |


## Modules
| Name                                             | Source                        | Version |
|--------------------------------------------------|-------------------------------|---------|
| <a name="module_lz_projects_1"></a> [lz_projects_1](#module_lz_projects_1) | ../../../modules/lz_projects  | n/a     |
| <a name="module_lz_projects_2"></a> [lz_projects_2](#module_lz_projects_2) | ../../../modules/lz_projects  | n/a     |
| <a name="module_lz_projects_3"></a> [lz_projects_3](#module_lz_projects_3) | ../../../modules/lz_projects  | n/a     |
| <a name="module_lz_projects_4"></a> [lz_projects_4](#module_lz_projects_4) | ../../../modules/lz_projects  | n/a     |

## Input
| Name                     | Description                                                                                  | Type               | Default Value                                                                                                                                                                     | Required |
|--------------------------|----------------------------------------------------------------------------------------------|--------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| <a name="input_source"></a> `source`           | The source path of the module.                                                                | string             | `"../../../modules/lz_projects"`                                                                                                                                                     | Yes      |
| <a name="input_project_name"></a> `project_name`   | The name of the project.                                                                      | string             | `"bdo-lz-d-shared-base"`                                                                                                                                                              | Yes      |
| <a name="input_project_labels"></a> `project_labels` | The labels to be associated with the project.                                                 | map(string)        | `merge(local.custom_labels, { billing_account = lower(local.billing_account), bu_name = "", business_code = "", organisation_id = local.org_id, primary_contact = local.primary_contact, secondary_contact = local.secondary_contact, stage_name = "2-env" }, {})` | Yes      |
| <a name="input_folder_id"></a> `folder_id`         | The folder ID where the project will be located.                                               | string             | `data.terraform_remote_state.folders.outputs.folder_id["api/dev"]`                                                                                                                 | Yes      |
| <a name="input_billing_account"></a> `billing_account` | The billing account associated with the project.                                              | string             | `local.billing_account`                                                                                                                                                             | Yes      |
| <a name="input_apis"></a> `apis`                   | The APIs that should be enabled for the project.                                               | list(string)       | `["billingbudgets.googleapis.com", "container.googleapis.com", "dns.googleapis.com", "iamcredentials.googleapis.com", "logging.googleapis.com", "servicenetworking.googleapis.com"]` | Yes      |
| <a name="input_iam_permissions"></a> `iam_permissions` | The IAM permissions to be assigned to the project.                                            | map(string)        | `{}`                                                                                                                                                                               | No       |
| <a name="input_sa_name"></a> `sa_name`             | The name of the service account to be created.                                                 | string             | `"project-service-account"`                                                                                                                                                          | Yes      |
| <a name="input_default_service_account"></a> `default_service_account` | The default service account setting.                                                          | string             | `"DISABLE"`                                                                                                                                                                        | Yes      |
| <a name="input_budget_amount"></a> `budget_amount`   | The budget amount for the project.                                                            | string             | `"1000"`                                                                                                                                                                           | Yes      |
| <a name="input_log_sink"></a> `log_sink`           | Whether to create a log sink for the project.                                                 | bool               | `false`      


## Output
| Name                                      | Value                                                 |
|-------------------------------------------|-------------------------------------------------------|
| <a name="output_lz_bdo_lz_d_logging_project_id"></a> [lz_bdo-lz-d-logging_project_id](#output_lz_bdo_lz_d_logging_project_id) | `module.lz-projects_bdo-lz-d-logging.id`             |
| <a name="output_lz_bdo_lz_d_logging_project_number"></a> [lz_bdo-lz-d-logging_project_number](#output_lz_bdo_lz_d_logging_project_number) | `module.lz-projects_bdo-lz-d-logging.number`         |
| <a name="output_lz_bdo_lz_d_monitoring_project_id"></a> [lz_bdo-lz-d-monitoring_project_id](#output_lz_bdo_lz_d_monitoring_project_id) | `module.lz-projects_bdo-lz-d-monitoring.id`          |
| <a name="output_lz_bdo_lz_d_monitoring_project_number"></a> [lz_bdo-lz-d-monitoring_project_number](#output_lz_bdo_lz_d_monitoring_project_number) | `module.lz-projects_bdo-lz-d-monitoring.number`      |
| <a name="output_lz_bdo_lz_d_secrets_project_id"></a> [lz_bdo-lz-d-secrets_project_id](#output_lz_bdo_lz_d_secrets_project_id) | `module.lz-projects_bdo-lz-d-secrets.id`             |
| <a name="output_lz_bdo_lz_d_secrets_project_number"></a> [lz_bdo-lz-d-secrets_project_number](#output_lz_bdo_lz_d_secrets_project_number) | `module.lz-projects_bdo-lz-d-secrets.number`         |
| <a name="output_lz_bdo_lz_d_shared_base_project_id"></a> [lz_bdo-lz-d-shared-base_project_id](#output_lz_bdo_lz_d_shared_base_project_id) | `module.lz-projects_bdo-lz-d-shared-base.id`         |
| <a name="output_lz_bdo_lz_d_shared_base_project_number"></a> [lz_bdo-lz-d-shared-base_project_number](#output_lz_bdo_lz_d_shared_base_project_number) | `module.lz-projects_bdo-lz-d-shared-base.number`     |
