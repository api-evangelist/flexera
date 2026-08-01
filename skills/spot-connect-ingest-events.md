---
name: Stand up a Spot Connect integration and ingest business events
description: Create an integration, define an event type, register a device, and ingest external business events so they are linked to camera footage.
api: openapi/spot-connect-openapi.json
operations: [CreateIntegration, CreateIntegrationEventType, CreateIntegrationDevice, IntegrationEventIngestionWebhook, CreateIntegrationEvents, GetIntegrationEvents]
---

# Stand up a Spot Connect integration and ingest business events

Use Spot Connect (beta) to link external system events (POS, access control, etc.) to video footage. Base `https://dev-api.spot.ai/`, bearer API-key auth.

## Steps
1. `CreateIntegration` (POST `/v1/integrations`) — create the integration (canned type or fully custom). Returns the `integration_id`.
2. `CreateIntegrationEventType` (POST `/v1/integrations/{integration_id}/event-types`) — define the JSON-schema contract every event of this type must satisfy.
3. `CreateIntegrationDevice` (POST `/v1/integrations/{integration_id}/devices`) — register a device and link up to four cameras in the same location. Set an immutable `external_id` if you want automatic device resolution.
4. Ingest events:
   - Single webhook-forwarded event: `IntegrationEventIngestionWebhook` (POST `/v1/integrations/{integration_id}/events`). Device resolution precedence: `integration_device_id` -> `device_external_id` (auto-creates) -> Device ID Template -> 400 if none resolve.
   - Bulk: `CreateIntegrationEvents` (POST `/v1/integrations/{integration_id}/events/import`).
5. `GetIntegrationEvents` — list/filter ingested events to confirm linkage.

## Notes
- Event writes return `202 Accepted` (async). Validation failures return `422` with a ValidationError body. See errors/spot-problem-types.yml and conventions/spot-conventions.yml.
- Providing an invalid `integration_device_id` fails without falling back to `device_external_id`.
