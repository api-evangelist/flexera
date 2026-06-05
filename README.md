# Spot (spot)

Spot by Flexera provides cloud infrastructure automation and optimization solutions. The platform includes Elastigroup for compute workload management across spot, reserved, and on-demand instances, Ocean for Kubernetes and container infrastructure automation, and Eco for cloud commitment management. The Spot API enables programmatic control over all platform capabilities including administration, compute groups, Kubernetes clusters, and cost optimization.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/spot/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/spot/refs/heads/main/apis.yml)

## Scope

- **Type:** Index
- **Position:** Consumer
- **Access:** 3rd-Party

## Tags

- Autoscaling
- Cloud Infrastructure
- Containers
- Cost Optimization
- FinOps
- Kubernetes
- Spot Instances

## Timestamps

- **Created:** 2026-01-02
- **Modified:** 2026-05-19

## APIs

### Spot Administration API

The Spot Administration API provides endpoints for managing organizations, accounts, users, access policies, cloud credentials, subscriptions, and event notifications within the Spot by Flexera platform. It enables programmatic control over user permissions, account setup, and cloud provider credential linking for AWS, Azure, and GCP.

- **Human URL:** [https://docs.spot.io/api/](https://docs.spot.io/api/)
- **Base URL:** `https://api.spotinst.io`

#### Tags

- Access Control
- Accounts
- Administration
- Cloud Credentials
- Organizations
- Users

#### Properties

- [Documentation](https://docs.spot.io/api/)
- [OpenAPI](openapi/spot-administration-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/spot-administration-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/spot-administration-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [JSON Schema](json-schema/organization.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/account.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/user.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/access-policy.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/subscription.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON-LD](json-ld/spot-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)
- [JSON-LD](json-ld/spot-administration-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)

### Spot Elastigroup API

The Spot Elastigroup API enables programmatic management of Elastigroup compute groups across AWS, Azure, and GCP. Elastigroup simplifies and automates cloud infrastructure for scale-out applications, continuously analyzing resource usage and optimizing compute resources to ensure availability while leveraging the lowest-cost compute options including spot instances, reserved instances, and on-demand capacity.

- **Human URL:** [https://docs.spot.io/api/](https://docs.spot.io/api/)
- **Base URL:** `https://api.spotinst.io`

#### Tags

- Autoscaling
- AWS
- Azure
- Compute
- Elastigroup
- EMR
- GCP
- Spot Instances

#### Properties

- [Documentation](https://docs.spot.io/api/)
- [OpenAPI](openapi/spot-elastigroup-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/spot-elastigroup-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/spot-elastigroup-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [JSON Schema](json-schema/elastigroup.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON-LD](json-ld/spot-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)
- [JSON-LD](json-ld/spot-elastigroup-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)

### Spot Ocean API

The Spot Ocean API provides programmatic management of Ocean Kubernetes clusters across AWS EKS, Azure AKS, GCP GKE, and Amazon ECS. Ocean is a serverless Kubernetes infrastructure engine that automatically manages and optimizes cloud infrastructure for containers, handling node provisioning, scaling, and cost optimization with intelligent use of spot instances, reserved capacity, and on-demand resources.

- **Human URL:** [https://docs.spot.io/api/](https://docs.spot.io/api/)
- **Base URL:** `https://api.spotinst.io`

#### Tags

- AKS
- Apache Spark
- Autoscaling
- Containers
- ECS
- EKS
- GKE
- Kubernetes
- Ocean

#### Properties

- [Documentation](https://docs.spot.io/api/)
- [OpenAPI](openapi/spot-ocean-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/spot-ocean-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/spot-ocean-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [JSON Schema](json-schema/ocean-cluster.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/virtual-node-group.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON-LD](json-ld/spot-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)
- [JSON-LD](json-ld/spot-ocean-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)

### Spot Eco API

The Spot Eco API provides programmatic access to cloud commitment management and optimization across AWS, Azure, and GCP. Eco automates the purchase, management, and optimization of reserved instances, savings plans, and committed use discounts to maximize cloud cost savings while maintaining flexibility.

- **Human URL:** [https://docs.spot.io/api/](https://docs.spot.io/api/)
- **Base URL:** `https://api.spotinst.io`

#### Tags

- AWS
- Azure
- Commitments
- Cost Optimization
- FinOps
- GCP
- Reserved Instances
- Savings Plans

#### Properties

- [Documentation](https://docs.spot.io/api/)
- [OpenAPI](openapi/spot-eco-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/spot-eco-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/spot-eco-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [JSON-LD](json-ld/spot-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)
- [JSON-LD](json-ld/spot-eco-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)

### Spot Billing Engine API

The Spot Billing Engine API provides programmatic access to cloud billing management, cost allocation, and invoicing capabilities. Billing Engine streamlines multi-cloud invoicing with intelligent cost allocation, chargeback and showback reporting, and comprehensive billing analytics across AWS, Azure, and GCP accounts.

- **Human URL:** [https://docs.spot.io/api/](https://docs.spot.io/api/)
- **Base URL:** `https://api.spotinst.io`

#### Tags

- Billing
- Chargeback
- Cost Allocation
- Cost Intelligence
- FinOps
- Invoicing

#### Properties

- [Documentation](https://docs.spot.io/api/)
- [OpenAPI](openapi/spot-billing-engine-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/spot-billing-engine-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/spot-billing-engine-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [JSON-LD](json-ld/spot-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)
- [JSON-LD](json-ld/spot-billing-engine-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)

## Common Properties

- [LinkedIn](https://www.linkedin.com/company/spothq)
- [GitHub Organization](https://github.com/spotinst)
- [Documentation](https://docs.spot.io/)
- [API Reference](https://docs.spot.io/api/)
- [OpenAPI](https://github.com/spotinst/openapi) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Authentication](https://docs.spot.io/administration/api/create-api-token)
- [Blog](https://spot.io/blog/)
- [Terms of Service](https://spot.io/terms-of-use/)
- [Features](undefined)
- [Use Cases](undefined)
- [Integrations](undefined)

## Maintainers

**FN:** Kin Lane
**Email:** info@apievangelist.com
