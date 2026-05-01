# Spot (spot)
Spot by Flexera provides cloud infrastructure automation and optimization solutions. The platform includes Elastigroup for compute workload management across spot, reserved, and on-demand instances, Ocean for Kubernetes and container infrastructure automation, and Eco for cloud commitment management. The Spot API enables programmatic control over all platform capabilities including administration, compute groups, Kubernetes clusters, and cost optimization.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/spot/refs/heads/main/apis.yml)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Scope

- **Type:** Index 
- **Position:** Consuming 
- **Access:** 3rd-Party 

## Tags:

 - Autoscaling, Cloud Infrastructure, Containers, Cost Optimization, FinOps, Kubernetes, Spot Instances

## Timestamps

- **Created:** 2026-01-02 
- **Modified:** 2026-04-28 

## APIs

### Spot Administration API
The Spot Administration API provides endpoints for managing organizations, accounts, users, access policies, cloud credentials, subscriptions, and event notifications within the Spot by Flexera platform. It enables programmatic control over user permissions, account setup, and cloud provider credential linking for AWS, Azure, and GCP.

**Human URL:** [https://docs.spot.io/api/](https://docs.spot.io/api/)

#### Tags:

 - Access Control, Accounts, Administration, Cloud Credentials, Organizations, Users

#### Properties

- [Documentation](https://docs.spot.io/api/)
- [OpenAPI](openapi/spot-administration-api-openapi.yml)
- [JSONSchema](json-schema/organization.json)
- [JSONSchema](json-schema/account.json)
- [JSONSchema](json-schema/user.json)
- [JSONSchema](json-schema/access-policy.json)
- [JSONSchema](json-schema/subscription.json)
- [JSONLD](json-ld/spot-context.jsonld)
- [JSONLD](json-ld/spot-administration-context.jsonld)

### Spot Elastigroup API
The Spot Elastigroup API enables programmatic management of Elastigroup compute groups across AWS, Azure, and GCP. Elastigroup simplifies and automates cloud infrastructure for scale-out applications, continuously analyzing resource usage and optimizing compute resources to ensure availability while leveraging the lowest-cost compute options including spot instances, reserved instances, and on-demand capacity.

**Human URL:** [https://docs.spot.io/api/](https://docs.spot.io/api/)

#### Tags:

 - Autoscaling, AWS, Azure, Compute, Elastigroup, EMR, GCP, Spot Instances

#### Properties

- [Documentation](https://docs.spot.io/api/)
- [OpenAPI](openapi/spot-elastigroup-api-openapi.yml)
- [JSONSchema](json-schema/elastigroup.json)
- [JSONLD](json-ld/spot-context.jsonld)
- [JSONLD](json-ld/spot-elastigroup-context.jsonld)

### Spot Ocean API
The Spot Ocean API provides programmatic management of Ocean Kubernetes clusters across AWS EKS, Azure AKS, GCP GKE, and Amazon ECS. Ocean is a serverless Kubernetes infrastructure engine that automatically manages and optimizes cloud infrastructure for containers, handling node provisioning, scaling, and cost optimization with intelligent use of spot instances, reserved capacity, and on-demand resources.

**Human URL:** [https://docs.spot.io/api/](https://docs.spot.io/api/)

#### Tags:

 - AKS, Apache Spark, Autoscaling, Containers, ECS, EKS, GKE, Kubernetes, Ocean

#### Properties

- [Documentation](https://docs.spot.io/api/)
- [OpenAPI](openapi/spot-ocean-api-openapi.yml)
- [JSONSchema](json-schema/ocean-cluster.json)
- [JSONSchema](json-schema/virtual-node-group.json)
- [JSONLD](json-ld/spot-context.jsonld)
- [JSONLD](json-ld/spot-ocean-context.jsonld)

### Spot Eco API
The Spot Eco API provides programmatic access to cloud commitment management and optimization across AWS, Azure, and GCP. Eco automates the purchase, management, and optimization of reserved instances, savings plans, and committed use discounts to maximize cloud cost savings while maintaining flexibility.

**Human URL:** [https://docs.spot.io/api/](https://docs.spot.io/api/)

#### Tags:

 - AWS, Azure, Commitments, Cost Optimization, FinOps, GCP, Reserved Instances, Savings Plans

#### Properties

- [Documentation](https://docs.spot.io/api/)
- [OpenAPI](openapi/spot-eco-api-openapi.yml)
- [JSONLD](json-ld/spot-context.jsonld)
- [JSONLD](json-ld/spot-eco-context.jsonld)

### Spot Billing Engine API
The Spot Billing Engine API provides programmatic access to cloud billing management, cost allocation, and invoicing capabilities. Billing Engine streamlines multi-cloud invoicing with intelligent cost allocation, chargeback and showback reporting, and comprehensive billing analytics across AWS, Azure, and GCP accounts.

**Human URL:** [https://docs.spot.io/api/](https://docs.spot.io/api/)

#### Tags:

 - Billing, Chargeback, Cost Allocation, Cost Intelligence, FinOps, Invoicing

#### Properties

- [Documentation](https://docs.spot.io/api/)
- [OpenAPI](openapi/spot-billing-engine-api-openapi.yml)
- [JSONLD](json-ld/spot-context.jsonld)
- [JSONLD](json-ld/spot-billing-engine-context.jsonld)

## Common Properties

- [GitHubOrganization](https://github.com/spotinst)
- [Documentation](https://docs.spot.io/)
- [APIReference](https://docs.spot.io/api/)
- [OpenAPI](https://github.com/spotinst/openapi)
- [Authentication](https://docs.spot.io/administration/api/create-api-token)
- [Blog](https://spot.io/blog/)
- [TermsOfService](https://spot.io/terms-of-use/)

## Features

| Name | Description |
|------|-------------|
| Elastigroup Compute Management | Automated management of compute workloads across spot, reserved, and on-demand instances for optimal cost and availability. |
| Ocean Kubernetes Automation | Serverless container infrastructure that automatically right-sizes and scales Kubernetes node pools using the lowest-cost compute. |
| Eco Commitment Optimization | Automated purchase and management of reserved instances, savings plans, and committed use discounts across clouds. |
| Billing Engine | Multi-cloud billing management with cost allocation, chargeback, showback, and invoicing analytics. |
| Multi-Cloud Support | Unified management across AWS, Azure, and GCP with consistent APIs and optimization strategies. |
| Intelligent Autoscaling | Predictive and reactive autoscaling that analyzes workload patterns to optimize resource provisioning. |
| Stateful Workload Management | Support for stateful applications with persistent storage, ENI, and IP preservation during instance replacements. |

## Use Cases

| Name | Description |
|------|-------------|
| Cloud Cost Optimization | Reduce cloud compute costs by up to 90% by leveraging spot instances with automated fallback to on-demand capacity. |
| Kubernetes Cost Management | Optimize Kubernetes infrastructure costs with bin-packing, right-sizing, and intelligent node pool management. |
| FinOps Reporting | Centralized cloud cost visibility with chargeback, showback, and commitment utilization reporting across teams. |
| Big Data Cost Reduction | Run Apache Spark and EMR workloads on spot instances with automatic scaling and cost optimization. |
| Reserved Instance Management | Automate the lifecycle of cloud commitments including purchasing, exchanging, and selling unused reservations. |
| Multi-Cloud Governance | Unified access control, audit logging, and policy management across AWS, Azure, and GCP accounts. |

## Integrations

| Name | Description |
|------|-------------|
| AWS | Deep integration with EC2, EKS, ECS, EMR, Auto Scaling Groups, and Savings Plans for AWS workload optimization. |
| Azure | Integration with Azure VMs, AKS, Virtual Machine Scale Sets, and Azure Reserved VM Instances. |
| Google Cloud | Support for GCP Compute Engine, GKE, and Committed Use Discounts for Google Cloud optimization. |
| Kubernetes | Native integration with EKS, AKS, GKE, and self-managed Kubernetes clusters through the Ocean controller. |
| Terraform | Official Terraform provider for managing Elastigroup, Ocean, and Eco resources as infrastructure as code. |
| Jenkins | Jenkins plugin for scaling CI/CD build agents dynamically using Elastigroup spot instances. |
| Ansible | Ansible modules for automating Spot resource provisioning and configuration management. |

## Artifacts

Machine-readable API specifications organized by format.

### OpenAPI

- [Spot Administration API](openapi/spot-administration-api-openapi.yml)
- [Spot Elastigroup API](openapi/spot-elastigroup-api-openapi.yml)
- [Spot Ocean API](openapi/spot-ocean-api-openapi.yml)
- [Spot Eco API](openapi/spot-eco-api-openapi.yml)
- [Spot Billing Engine API](openapi/spot-billing-engine-api-openapi.yml)

### JSON-LD

- [Spot Context](json-ld/spot-context.jsonld)
- [Spot Administration Context](json-ld/spot-administration-context.jsonld)
- [Spot Elastigroup Context](json-ld/spot-elastigroup-context.jsonld)
- [Spot Ocean Context](json-ld/spot-ocean-context.jsonld)
- [Spot Eco Context](json-ld/spot-eco-context.jsonld)
- [Spot Billing Engine Context](json-ld/spot-billing-engine-context.jsonld)

## Capabilities

Naftiko capabilities organized as shared per-API definitions composed into customer-facing workflows.

### Shared Per-API Definitions

- [Spot Administration](capabilities/shared/administration.yaml) -- 26 operations for organization, account, and user management
- [Spot Elastigroup](capabilities/shared/elastigroup.yaml) -- 32 operations for compute group management across AWS, Azure, and GCP
- [Spot Ocean](capabilities/shared/ocean.yaml) -- 39 operations for Kubernetes cluster management across cloud providers
- [Spot Eco](capabilities/shared/eco.yaml) -- 11 operations for cloud commitment optimization
- [Spot Billing Engine](capabilities/shared/billing-engine.yaml) -- 11 operations for billing and cost analysis

### Workflow Capabilities

| Workflow | APIs Combined | Tools | Persona |
|----------|--------------|-------|---------|
| [Compute Optimization](capabilities/compute-optimization.yaml) | Elastigroup + Ocean | 16 | DevOps Engineer / Cloud Infrastructure Team |
| [FinOps](capabilities/finops.yaml) | Eco + Billing Engine + Administration | 15 | FinOps Analyst / Cloud Finance Team |

## Vocabulary

- [Spot Vocabulary](vocabulary/spot-vocabulary.yaml) -- Unified taxonomy for Spot API operations and schemas

## Rules

- [Spot Spectral Rules](rules/spot-spectral-rules.yml) -- Spectral rules enforcing Spot API conventions

## Maintainers

**FN:** Kin Lane

**Email:** info@apievangelist.com
