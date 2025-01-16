# 3-networks


This repo is part of a multi-part guide that shows how to configure and deploy
the example.com reference architecture described in
[Google Cloud security foundations guide](https://services.google.com/fh/files/misc/google-cloud-security-foundations-guide.pdf)
(PDF). The following table lists the parts of the guide.

<table>
<tbody>
<tr>
<td><a href="../0-bootstrap">0-bootstrap</a></td>
<td>Bootstraps a Google Cloud organization, creating all the required resources
and permissions to start using the Cloud Foundation Toolkit (CFT). This
step also configures a CI/CD pipeline for foundations code in subsequent
stages.</td>
</tr>
<tr>
<td><a href="../1-org">1-org</a></td>
<td>Sets up top level shared folders, monitoring and networking projects, and
organization-level logging, and sets baseline security settings through
organizational policy.</td>
</tr>
<tr>
<td><a href="../2-environments"><span style="white-space: nowrap;">2-environments</span></a></td>
<td>Sets up development, staging, and production environments within the
Google Cloud organization that you've created.</td>
</tr>
<tr>
<td>3-networks (this file)</td>
<td>Sets up base and restricted shared VPCs with default DNS, NAT (optional),
Private Service networking, VPC service controls, Dedicated or Partner
Interconnect, and baseline firewall rules for each environment. It also sets
up the global DNS hub.</td>
</tr>
<tr>
<td><a href="../4-projects">4-projects</a></td>
<td>Sets up a folder structure, projects, and application infrastructure pipeline for applications,
 which are connected as service projects to the shared VPC created in the previous stage.</td>
</tr>
<tr>
<td><a href="../5-app-infra">5-app-infra</a></td>
<td>Deploy a simple <a href="https://cloud.google.com/compute/">Compute Engine</a> instance in one of the business unit projects using the infra pipeline set up in 4-projects.</td>
</tr>
</tbody>
</table>

For an overview of the architecture and the parts, see the
[terraform-example-foundation README](https://github.com/terraform-google-modules/terraform-example-foundation).

## Purpose

The purpose of this step is to:

- Set up the global [DNS Hub](https://cloud.google.com/blog/products/networking/cloud-forwarding-peering-and-zones).
- Set up base and restricted shared VPCs with default DNS, NAT (optional), Private Service networking, VPC service controls, on-premises Dedicated or Partner Interconnect, and baseline firewall rules for each environment.

## Prerequisites

1. 0-bootstrap executed successfully.
1. 1-org executed successfully.
1. 2-environments executed successfully.
1. Obtain the value for the access_context_manager_policy_id variable. Can be obtained by running

   ```bash
   gcloud access-context-manager policies list --organization YOUR_ORGANIZATION_ID --format="value(name)"
   ```

1. For the manual step described in this document, you need [Terraform](https://www.terraform.io/downloads.html) version 0.13.7 to be installed.

   **Note:** Make sure that you use the same version of Terraform throughout this series. Otherwise, you might experience Terraform state snapshot lock errors.

### Troubleshooting

Please refer to [troubleshooting](../docs/TROUBLESHOOTING.md) if you run into issues during this step.

## Usage

**Note:** If you are using MacOS, replace `cp -RT` with `cp -R` in the relevant
commands. The `-T` flag is needed for Linux, but causes problems for MacOS.

### Networking Architecture

You need to set variables `enable_hub_and_spoke` and `enable_hub_and_spoke_transitivity` to `true` to be able to use the **Hub-and-Spoke** architecture detailed in the **Networking** section of the [Google cloud security foundations guide](https://services.google.com/fh/files/misc/google-cloud-security-foundations-guide.pdf).

### Using Dedicated Interconnect

If you provisioned the prerequisites listed in the [Dedicated Interconnect README](./modules/dedicated_interconnect/README.md), follow these steps to enable Dedicated Interconnect to access on-premises resources.

1. Rename `interconnect.tf.example` to `interconnect.tf` in each environment folder in `3-networks/envs/<ENV>`.
1. Update the file `interconnect.tf` with values that are valid for your environment for the interconnects, locations, candidate subnetworks, vlan_tag8021q and peer info.
1. The candidate subnetworks and vlan_tag8021q variables can be set to `null` to allow the interconnect module to auto generate these values.

### Using Partner Interconnect

If you provisioned the prerequisites listed in the [Partner Interconnect README](./modules/partner_interconnect/README.md) follow this steps to enable Partner Interconnect to access on-premises resources.

1. Rename `partner_interconnect.tf.example` to `partner_interconnect.tf` and `interconnect.auto.tfvars.example` to `interconnect.auto.tfvars` in the environment folder in `3-networks/envs/<environment>` .
1. Update the file `partner_interconnect.tf` with values that are valid for your environment for the VLAN attachments, locations, and candidate subnetworks.
1. The candidate subnetworks variable can be set to `null` to allow the interconnect module to auto generate this value.

### OPTIONAL - Using High Availability VPN

If you are not able to use Dedicated or Partner Interconnect, you can also use an HA Cloud VPN to access on-premises resources.

1. Rename `vpn.tf.example` to `vpn.tf` in each environment folder in `3-networks/envs/<ENV>`.
1. Create secret for VPN private preshared key.
   ```
   echo '<YOUR-PRESHARED-KEY-SECRET>' | gcloud secrets create <VPN_PRIVATE_PSK_SECRET_NAME> --project <ENV_SECRETS_PROJECT> --replication-policy=automatic --data-file=-
   ```
1. Create secret for VPN restricted preshared key.
   ```
   echo '<YOUR-PRESHARED-KEY-SECRET>' | gcloud secrets create <VPN_RESTRICTED_PSK_SECRET_NAME> --project <ENV_SECRETS_PROJECT> --replication-policy=automatic --data-file=-
   ```
1. In the file `vpn.tf`, update the values for `environment`, `vpn_psk_secret_name`, `on_prem_router_ip_address1`, `on_prem_router_ip_address2` and `bgp_peer_asn`.
1. Verify other default values are valid for your environment.

### Deploying with Cloud Build

1. Clone repo.
   ```
   gcloud source repos clone gcp-networks --project=YOUR_CLOUD_BUILD_PROJECT_ID
   ```
1. Change to the freshly cloned repo and change to non-main branch.
   ```
   git checkout -b plan
   ```
1. Copy contents of foundation to new repo.
   ```
   cp -RT ../terraform-example-foundation/3-networks/ .
   ```
1. Copy Cloud Build configuration files for Terraform.
   ```
   cp ../terraform-example-foundation/build/cloudbuild-tf-* .
   ```
1. Copy Terraform wrapper script to the root of your new repository.
   ```
   cp ../terraform-example-foundation/build/tf-wrapper.sh .
   ```
1. Ensure wrapper script can be executed.
   ```
   chmod 755 ./tf-wrapper.sh
   ```
1. Rename `common.auto.example.tfvars` to `common.auto.tfvars` and update the file with values from your environment and bootstrap. See any of the envs folder [README.md](./envs/production/README.md) files for additional information on the values in the `common.auto.tfvars` file.
1. Rename `shared.auto.example.tfvars` to `shared.auto.tfvars` and update the file with the `target_name_server_addresses` (the list of target name servers for the DNS forwarding zone in the DNS Hub).
1. Rename `access_context.auto.example.tfvars` to `access_context.auto.tfvars` and update the file with the `access_context_manager_policy_id`.
1. Commit changes
   ```
   git add .
   git commit -m 'Your message'
   ```
1. You must manually plan and apply the `shared` environment (only once) since the `development`, `staging` and `production` environments depend on it.
    1. Run `cd ./envs/shared/`.
    1. Update `backend.tf` with your bucket name from the bootstrap step.
    1. Run `terraform init`.
    1. Run `terraform plan` and review output.
    1. Run `terraform apply`.
    1. If you would like the bucket to be replaced by Cloud Build at run time, change the bucket name back to `UPDATE_ME`.
1. Push your plan branch to trigger a plan for all environments. Because the
   _plan_ branch is not a [named environment branch](./docs/FAQ.md), pushing your _plan_
   branch triggers _terraform plan_ but not _terraform apply_.
   ```
   git push --set-upstream origin plan
   ```
1. Review the plan output in your Cloud Build project https://console.cloud.google.com/cloud-build/builds?project=YOUR_CLOUD_BUILD_PROJECT_ID
1. Merge changes to production. Because this is a [named environment branch](./docs/FAQ.md#what-is-a-named-branch),
   pushing to this branch triggers both _terraform plan_ and _terraform apply_.
   ```
   git checkout -b production
   git push origin production
   ```
1. Review the apply output in your Cloud Build project https://console.cloud.google.com/cloud-build/builds?project=YOUR_CLOUD_BUILD_PROJECT_ID
1. After production has been applied, apply development.
1. Merge changes to development. Because this is a [named environment branch](./docs/FAQ.md#what-is-a-named-branch),
   pushing to this branch triggers both _terraform plan_ and _terraform apply_.
   ```
   git checkout -b development
   git push origin development
   ```
1. Review the apply output in your Cloud Build project https://console.cloud.google.com/cloud-build/builds?project=YOUR_CLOUD_BUILD_PROJECT_ID
1. After development has been applied, apply staging.
1. Merge changes to staging. Because this is a [named environment branch](./docs/FAQ.md#what-is-a-named-branch),
   pushing to this branch triggers both _terraform plan_ and _terraform apply_.
   ```
   git checkout -b staging
   git push origin staging
   ```
1. Review the apply output in your Cloud Build project https://console.cloud.google.com/cloud-build/builds?project=YOUR_CLOUD_BUILD_PROJECT_ID
1. You can now move to the instructions in the step [4-projects](../4-projects/README.md).

### Deploying with Jenkins

1. Clone the repo you created manually in 0-bootstrap.
   ```
   git clone <YOUR_NEW_REPO-3-networks>
   ```
1. Navigate into the repo and change to a staging branch. All subsequent
   steps assume you are running them from the gcp-environments directory. If
   you run them from another directory, adjust your copy paths accordingly.
   ```
   cd YOUR_NEW_REPO_CLONE-3-networks
   git checkout -b plan
   ```
1. Copy contents of foundation to new repo.
   ```
   cp -RT ../terraform-example-foundation/3-networks/ .
   ```
1. Copy the Jenkinsfile script to the root of your new repository.
   ```
   cp ../terraform-example-foundation/build/Jenkinsfile .
   ```
1. Update the variables located in the `environment {}` section of the `Jenkinsfile` with values from your environment:
    ```
    _TF_SA_EMAIL
    _STATE_BUCKET_NAME
    _PROJECT_ID (the cicd project id)
    ```
1. Copy Terraform wrapper script to the root of your new repository.
   ```
   cp ../terraform-example-foundation/build/tf-wrapper.sh .
   ```
1. Ensure wrapper script can be executed.
   ```
   chmod 755 ./tf-wrapper.sh
   ```
1. Rename `common.auto.example.tfvars` to `common.auto.tfvars` and update the file with values from your environment and bootstrap. See any of the envs folder [README.md](./envs/production/README.md) files for additional information on the values in the `common.auto.tfvars` file.
1. Rename `shared.auto.example.tfvars` to `shared.auto.tfvars` and update the file with the `target_name_server_addresses`.
1. Rename `access_context.auto.example.tfvars` to `access_context.auto.tfvars` and update the file with the `access_context_manager_policy_id`.
1. Commit changes.
   ```
   git add .
   git commit -m 'Your message'
   ```
1. You must manually plan and apply the `shared` environment (only once) since the `development`, `staging` and `production` environments depend on it.
    1. Run `cd ./envs/shared/`.
    1. Update `backend.tf` with your bucket name from the bootstrap step.
    1. Run `terraform init`.
    1. Run `terraform plan` and review output.
    1. Run `terraform apply`.
    1. If you would like the bucket to be replaced by Cloud Build at run time, change the bucket name back to `UPDATE_ME`.
1. Push your plan branch.
   ```
    git push --set-upstream origin plan
    ```
    - Assuming you configured an automatic trigger in your Jenkins Master (see [Jenkins sub-module README](../0-bootstrap/modules/jenkins-agent)), this will trigger a plan. You can also trigger a Jenkins job manually. Given the many options to do this in Jenkins, it is out of the scope of this document see [Jenkins website](http://www.jenkins.io) for more details.
1. Review the plan output in your Master's web UI.
1. Merge changes to production branch.
   ```
   git checkout -b production
   git push origin production
   ```
1. Review the apply output in your Master's web UI (you might want to use the option to "Scan Multibranch Pipeline Now" in your Jenkins Master UI).
1. After production has been applied, apply development and staging.
1. Merge changes to development
   ```
   git checkout -b development
   git push origin development
   ```
1. Review the apply output in your Master's web UI (you might want to use the option to "Scan Multibranch Pipeline Now" in your Jenkins Master UI).
1. Merge changes to staging.
   ```
   git checkout -b staging
   git push origin staging
   ```
1. Review the apply output in your Master's web UI (you might want to use the option to "Scan Multibranch Pipeline Now" in your Jenkins Master UI).

### Run Terraform locally

1. Change into the 3-networks folder.
1. Run `cp ../build/tf-wrapper.sh .`
1. Run `chmod 755 ./tf-wrapper.sh`.
1. Rename `common.auto.example.tfvars` to `common.auto.tfvars` and update the file with values from your environment and bootstrap. See any of the envs folder [README.md](./envs/production/README.md) files for additional information on the values in the `common.auto.tfvars` file.
1. Rename `shared.auto.example.tfvars` to `shared.auto.tfvars` and update the file with the `target_name_server_addresses`.
1. Rename `access_context.auto.example.tfvars` to `access_context.auto.tfvars` and update the file with the `access_context_manager_policy_id`.
1. Update `backend.tf` with your bucket name from the bootstrap step.
   ```
   for i in `find -name 'backend.tf'`; do sed -i 's/UPDATE_ME/<YOUR-BUCKET-NAME>/' $i; done
   ```
   You can run `terraform output gcs_bucket_tfstate` in the 0-bootstrap folder to obtain the bucket name.

We will now deploy each of our environments(development/production/staging) using this script.
When using Cloud Build or Jenkins as your CI/CD tool each environment corresponds to a branch in the repository for 3-networks step
and only the corresponding environment is applied.

To use the `validate` option of the `tf-wrapper.sh` script, please follow the [instructions](https://github.com/GoogleCloudPlatform/terraform-validator/blob/main/docs/install.md) in the **Install Terraform Validator** section and install version `v0.4.0` in your system. You will also need to rename the binary from `terraform-validator-<your-platform>` to `terraform-validator` and the `terraform-validator` binary must be in your `PATH`.

1. Run `./tf-wrapper.sh init shared`.
1. Run `./tf-wrapper.sh plan shared` and review output.
1. Run `./tf-wrapper.sh validate shared $(pwd)/../policy-library <YOUR_CLOUD_BUILD_PROJECT_ID>` and check for violations.
1. Run `./tf-wrapper.sh apply shared`.
1. Run `./tf-wrapper.sh init production`.
1. Run `./tf-wrapper.sh plan production` and review output.
1. Run `./tf-wrapper.sh validate production $(pwd)/../policy-library <YOUR_CLOUD_BUILD_PROJECT_ID>` and check for violations.
1. Run `./tf-wrapper.sh apply production`.
1. Run `./tf-wrapper.sh init staging`.
1. Run `./tf-wrapper.sh plan staging` and review output.
1. Run `./tf-wrapper.sh validate staging $(pwd)/../policy-library <YOUR_CLOUD_BUILD_PROJECT_ID>` and check for violations.
1. Run `./tf-wrapper.sh apply staging`.
1. Run `./tf-wrapper.sh init development`.
1. Run `./tf-wrapper.sh plan development` and review output.
1. Run `./tf-wrapper.sh validate development $(pwd)/../policy-library <YOUR_CLOUD_BUILD_PROJECT_ID>` and check for violations.
1. Run `./tf-wrapper.sh apply development`.

If you received any errors or made any changes to the Terraform config or any `.tfvars`you must re-run `./tf-wrapper.sh plan <env>` before run `./tf-wrapper.sh apply <env>`.


## Requirements
| Name                                          | Version |
|-----------------------------------------------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement_terraform) | >= 1.0.9 |
| <a name="requirement_google"></a> [google](#requirement_google)       | >= 5.0.0 |

## Modules
| Name                                   | Source                         | Version |
|----------------------------------------|--------------------------------|---------|
| <a name="module_base_shared_vpc"></a> `base_shared_vpc` | `"../../../modules/base_shared_vpc"` | n/a     |

## Input
| Name                            | Description                                                        | Type   | Default Value                                                      | Required |
|---------------------------------|--------------------------------------------------------------------|--------|--------------------------------------------------------------------|----------|
| <a name="input_source"></a> `source` | The source of the module.                                          | string | `"../../../modules/base_shared_vpc"`                               | Yes      |
| <a name="input_count"></a> `count` | The number of instances of the resource.                           | number | `1`                                                                | No       |
| <a name="input_custom_vpc_name"></a> `custom_vpc_name` | The name of the VPC.                                              | string | `"bdo-api-vpc-p-shared-base"`                                      | Yes      |
| <a name="input_project_id"></a> `project_id` | The ID of the project where the VPC will be created.               | string | `data.terraform_remote_state.env.outputs.lz_bdo-lz-p-shared-base_project_id` | Yes      |
| <a name="input_private_service_connection"></a> `private_service_connection` | The private service connection ranges.                            | map    | `{"psc-pp-shared-base-1": "172.16.9.0/24", "psc-pp-shared-base-2": "172.16.10.0/24"}` | Yes      |
| <a name="input_org_id"></a> `org_id` | The organization ID.                                               | string | `local.org_id`                                                    | Yes      |
| <a name="input_parent_folder"></a> `parent_folder` | The parent folder ID.                                              | string | `local.parent_folder`                                              | Yes      |
| <a name="input_default_region1"></a> `default_region1` | The default region for the VPC.                                    | string | `local.gcp_region`                                                | Yes      |
| <a name="input_default_region2"></a> `default_region2` | The secondary default region for the VPC.                          | string | `local.default_region2`                                           | Yes      |
| <a name="input_domain"></a> `domain` | The domain associated with the VPC.                                | string | `local.domain`                                                    | Yes      |
| <a name="input_windows_activation_enabled"></a> `windows_activation_enabled` | Whether Windows activation is enabled.                             | bool   | `var.windows_activation_enabled`                                   | Yes      |
| <a name="input_dns_enable_inbound_forwarding"></a> `dns_enable_inbound_forwarding` | Enable inbound DNS forwarding.                                    | bool   | `var.dns_enable_inbound_forwarding`                                | Yes      |
| <a name="input_dns_enable_logging"></a> `dns_enable_logging` | Enable DNS logging.                                               | bool   | `var.dns_enable_logging`                                           | Yes      |
| <a name="input_firewall_enable_logging"></a> `firewall_enable_logging` | Enable firewall logging.                                          | bool   | `var.firewall_enable_logging`                                      | Yes      |
| <a name="input_optional_fw_rules_enabled"></a> `optional_fw_rules_enabled` | Enable optional firewall rules.                                   | bool   | `var.optional_fw_rules_enabled`                                    | Yes      |
| <a name="input_nat_enabled"></a> `nat_enabled` | Enable NAT configuration for the VPC.                              | bool   | `var.nat_enabled`                                                 | Yes      |
| <a name="input_nat_bgp_asn"></a> `nat_bgp_asn` | The BGP ASN for NAT configuration.                                | string | `var.nat_bgp_asn`                                                 | Yes      |
| <a name="input_nat_num_addresses_region1"></a> `nat_num_addresses_region1` | The number of NAT addresses in region 1.                           | number | `var.nat_num_addresses_region1`                                    | Yes      |
| <a name="input_nat_num_addresses_region2"></a> `nat_num_addresses_region2` | The number of NAT addresses in region 2.                           | number | `var.nat_num_addresses_region2`                                    | Yes      |
| <a name="input_nat_num_addresses"></a> `nat_num_addresses` | The total number of NAT addresses.                                | number | `var.nat_num_addresses`                                           | Yes      |
| <a name="input_mode"></a> `mode` | The mode of operation for the VPC.                                 | string | `local.mode`                                                      | Yes      |
| <a name="input_subnets"></a> `subnets` | The list of subnets to create within the VPC.                      | list   | `[ { "subnet_name" : "sb-p-shared-base-asia-southeast1-a", "subnet_ip" : "10.66.32.0/19", "subnet_region" : "asia-southeast1", "subnet_private_access" : "true", "subnet_flow_logs" : "false", "description" : "shared base subnet 1" } ]` | Yes      |
| <a name="input_secondary_ranges"></a> `secondary_ranges` | Secondary IP ranges for the subnets.                               | map    | `{}`                                                               | No       |
| <a name="input_allow_all_ingress_ranges"></a> `allow_all_ingress_ranges` | Allow all ingress ranges (optional).                              | list   | `null`                                                             | No       |
| <a name="input_allow_all_egress_ranges"></a> `allow_all_egress_ranges` | Allow all egress ranges (optional).                               | list   | `null`                                                             | No       |
| <a name="input_peer_networks_projects_vpcs"></a> `peer_networks_projects_vpcs` | The peering relationships between VPCs in different projects.      | map    | `{ "bdo-lz-c-network-hub" : ["vpc-bdo-lz-vpc-p-c-base-hub"] }`      | No       |
| <a name="input_peer_dns_projects_vpcs"></a> `peer_dns_projects_vpcs` | The DNS peering relationships between projects' VPCs.             | map    | `{ "bdo-lz-c-network-hub" : "vpc-bdo-lz-vpc-p-c-base-hub" }`        | No       |

## Output
| Name                                                                                      | Value                                                                                           |
|-------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
| <a name="output_bdo-lz-p-shared-base_bdo-api-vpc-p-shared-base_nw_link"></a> `bdo-lz-p-shared-base_bdo-api-vpc-p-shared-base_nw_link` | `module.vpc_bdo-api-vpc-p-shared-base_base_shared_vpc[0].network_self_link`                      |
| <a name="output_bdo-lz-p-shared-base_bdo-api-vpc-p-shared-base_sb_link"></a> `bdo-lz-p-shared-base_bdo-api-vpc-p-shared-base_sb_link` | `module.vpc_bdo-api-vpc-p-shared-base_base_shared_vpc[0].subnets_self_links`                     |
| <a name="output_bdo-lz-p-shared-base_bdo-api-vpc-p-shared-base_sb_map_link"></a> `bdo-lz-p-shared-base_bdo-api-vpc-p-shared-base_sb_map_link` | `module.vpc_bdo-api-vpc-p-shared-base_base_shared_vpc[0].sub_self_links`                         |
