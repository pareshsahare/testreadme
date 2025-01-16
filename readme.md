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


