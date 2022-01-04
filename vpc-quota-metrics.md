---

copyright:
  years:  2021
lastupdated: "2021-12-20"

keywords: IBM Cloud monitoring, platform metrics, metrics, vpc metrics, vpc monitoring metrics

subcollection: cloud-infrastructure

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}

## Quota metrics available by resource type
{: metrics-by-plan}

Resources that offer quota metrics, detailed in Table 1.

| Resource type | Resource quota name | Region | Secondary resource ID |
|-----------|-----------|-----------|-----------|
| `instance` | `instance-vcpu` | {region} | {account id} |
| `instance` | `instance-memory` | {region} | {account id} |
| `vpc` | `vpc` | {region} | {account id} |
| `security-group` | `security-group` | {region} | {vpc id} |
| `security-group` | `security-group-rule` | {region} | {security group id} |
| `subnet` | `subnet` | {region} | {account id} |
| `floating-ip` | `floating-ip` | {region} | {account id} |
| `network-acl` | `network-acl` | {region} | {vpc id} |
| `network-acl` | `network-acl-rule` | {region} | {network acl id} |
| `share` | `share` | {region} | {account id} |

{: caption="Table 1: Services offering quota metrics" caption-side="top"}

<!---| `load-balancer` | `load-balancer` | {region} | {account id} |
| `volume` | `volume` | {region} | {account id} |--->

### Resource quota consumption
{: #ibm_is_resource_quota_consumption}

The amount of a resource that is consumed for a given resource type, detailed in Table 2.

| Description | Metadata |
|----------|-------------|
| `Metric Name` | `ibm_is_resource_quota_consumption`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `IBM IS Generation, Name of resource type for quota, Associated Resource` |

{: caption="Table 2: Resource quota consumption metric metadata" caption-side="top"}

### Resource quota limit
{: #ibm_is_resource_quota_limit}

The resource quota limit for a given resource type, detailed in Table 3.

| Description | Metadata |
|----------|-------------|
| `Metric Name` | `ibm_is_resource_quota_limit`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `IBM IS Generation, Name of resource type for quota, Associated Resource` |

{: caption="Table 3: Resource quota limit metric metadata" caption-side="top"}

## Attributes for segmentation
{: attributes}

### Global attributes
{: global-attributes}

The following attributes are available for segmenting all previously mentioned metrics.

| Attribute name | Attribute | Attribute description |
|-----------|----------------|-----------------------|
| `Cloud Type` | `ibm_ctype` | The cloud type is a value of public, dedicated or local |
| `Location` | `ibm_location` | The location of the monitored resource - this may be a region, data center or global |
| `Resource` | `ibm_resource` | The resource being measured by the service - typically a indentifying name or GUID |
| `Resource Type` | `ibm_resource_type` | The type of the resource being measured by the service |
| `Resource group` | `ibm_resource_group_name` | The resource group where the service instance was created |
| `Scope` | `ibm_scope` | The scope is the account, organization or space GUID associated with this metric |
| `Service name` | `ibm_service_name` | Name of the service generating this metric |

{: caption="Table 4: Global attributes" caption-side="top"}

### Additional attributes
{: additional-attributes}

The following attributes are available for segmenting one or more attributes as described in the Global attributes reference. See the individual metrics for segmentation options.

| Attribute name | Attribute | Attribute description |
|-----------|----------------|-----------------------|
| `Associated Resource` | `ibm_secondary_resource_id` | The ID of a linked or associated resource |
| `IBM IS Generation` | `ibm_is_generation` | IBM IS Generation |
| `Name of resource type for quota` | `ibm_resource_quota_name` | The name of the resource type being monitored for quota usage/limit |

{: caption="Table 5: Additional attributes" caption-side="top"}

## Next steps
{: next-steps}

- [Quotas and service limits](/docs/vpc?topic=vpc-quotas)
