# Spot (spot)
Spot by Flexera provides cloud infrastructure automation and optimization solutions. The platform includes Elastigroup for compute workload management across spot, reserved, and on-demand instances, Ocean for Kubernetes and container infrastructure automation, and Eco for cloud commitment management. The Spot API enables programmatic control over all platform capabilities including administration, compute groups, Kubernetes clusters, and cost optimization.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/spot/refs/heads/main/apis.yml)

## Scope

- **Type:** Index 
- **Position:** Consuming 
- **Access:** 3rd-Party 

## Tags:

 - Cloud Infrastructure, Kubernetes, Spot Instances, Cost Optimization, FinOps, Autoscaling, Containers

## Timestamps

- **Created:** 2026-01-02 
- **Modified:** 2026-03-14 

## APIs

### Spot Administration API
The Spot Administration API provides endpoints for managing organizations, accounts, users, access policies, cloud credentials, subscriptions, and event notifications within the Spot by Flexera platform. It enables programmatic control over user permissions, account setup, and cloud provider credential linking for AWS, Azure, and GCP.

**Human URL:** [https://docs.spot.io/api/](https://docs.spot.io/api/)


#### Tags:

 - Administration, Accounts, Users, Organizations, Access Control, Cloud Credentials

#### Properties

- [Documentation](https://docs.spot.io/api/)
- [OpenAPI](openapi/spot-administration-api-openapi.yml)
- [JSONSchema](json-schema/organization.json)
- [JSONSchema](json-schema/account.json)
- [JSONSchema](json-schema/user.json)
- [JSONSchema](json-schema/access-policy.json)
- [JSONSchema](json-schema/subscription.json)
- [JSONLD](json-ld/spot-context.jsonld)

### Spot Elastigroup API
The Spot Elastigroup API enables programmatic management of Elastigroup compute groups across AWS, Azure, and GCP. Elastigroup simplifies and automates cloud infrastructure for scale-out applications, continuously analyzing resource usage and optimizing compute resources to ensure availability while leveraging the lowest-cost compute options including spot instances, reserved instances, and on-demand capacity.

**Human URL:** [https://docs.spot.io/api/](https://docs.spot.io/api/)


#### Tags:

 - Elastigroup, Compute, Autoscaling, Spot Instances, AWS, Azure, GCP, EMR

#### Properties

- [Documentation](https://docs.spot.io/api/)
- [OpenAPI](openapi/spot-elastigroup-api-openapi.yml)
- [JSONSchema](json-schema/elastigroup.json)
- [JSONLD](json-ld/spot-context.jsonld)

### Spot Ocean API
The Spot Ocean API provides programmatic management of Ocean Kubernetes clusters across AWS EKS, Azure AKS, GCP GKE, and Amazon ECS. Ocean is a serverless Kubernetes infrastructure engine that automatically manages and optimizes cloud infrastructure for containers, handling node provisioning, scaling, and cost optimization with intelligent use of spot instances, reserved capacity, and on-demand resources.

**Human URL:** [https://docs.spot.io/api/](https://docs.spot.io/api/)


#### Tags:

 - Kubernetes, Containers, Ocean, EKS, AKS, GKE, ECS, Autoscaling, Apache Spark

#### Properties

- [Documentation](https://docs.spot.io/api/)
- [OpenAPI](openapi/spot-ocean-api-openapi.yml)
- [JSONSchema](json-schema/ocean-cluster.json)
- [JSONSchema](json-schema/virtual-node-group.json)
- [JSONLD](json-ld/spot-context.jsonld)

### Spot Eco API
The Spot Eco API provides programmatic access to cloud commitment management and optimization across AWS, Azure, and GCP. Eco automates the purchase, management, and optimization of reserved instances, savings plans, and committed use discounts to maximize cloud cost savings while maintaining flexibility.

**Human URL:** [https://docs.spot.io/api/](https://docs.spot.io/api/)


#### Tags:

 - FinOps, Cost Optimization, Reserved Instances, Savings Plans, Commitments, AWS, Azure, GCP

#### Properties

- [Documentation](https://docs.spot.io/api/)
- [OpenAPI](openapi/spot-eco-api-openapi.yml)
- [JSONLD](json-ld/spot-context.jsonld)

### Spot Billing Engine API
The Spot Billing Engine API provides programmatic access to cloud billing management, cost allocation, and invoicing capabilities. Billing Engine streamlines multi-cloud invoicing with intelligent cost allocation, chargeback and showback reporting, and comprehensive billing analytics across AWS, Azure, and GCP accounts.

**Human URL:** [https://docs.spot.io/api/](https://docs.spot.io/api/)


#### Tags:

 - FinOps, Billing, Cost Allocation, Invoicing, Cost Intelligence, Chargeback

#### Properties

- [Documentation](https://docs.spot.io/api/)
- [OpenAPI](openapi/spot-billing-engine-api-openapi.yml)
- [JSONLD](json-ld/spot-context.jsonld)

## Common Properties

- [GitHubOrganization](https://github.com/spotinst)
- [Documentation](https://docs.spot.io/)
- [APIReference](https://docs.spot.io/api/)
- [OpenAPISpecification](https://github.com/spotinst/openapi)
- [Authentication](https://docs.spot.io/administration/api/create-api-token)
- [Blog](https://spot.io/blog/)
- [TermsOfService](https://spot.io/terms-of-use/)

## Maintainers

**FN:** Kin Lane

**Email:** info@apievangelist.com
