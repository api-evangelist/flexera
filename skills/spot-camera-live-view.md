---
name: Browse the camera estate and open a live view
description: Discover an organization's locations and cameras, then get a live stream or an embeddable live feed URL for a chosen camera.
api: openapi/spot-devices-openapi.json
operations: [GetLocations, GetCameras, GetCameraById, GetLiveUrl, GenerateLiveEmbedUrl]
---

# Browse the camera estate and open a live view

Use the Spot AI Devices API (base `https://dev-api.spot.ai/`) to find a camera and open its live feed.

## Auth
Send the org API key as an HTTP bearer token: `Authorization: Bearer <key>`. Keys are created by an org admin in the dashboard. `GenerateLiveEmbedUrl` additionally requires the `camera_share_create` permission on the camera.

## Steps
1. `GetLocations` — list locations (id, name). Cursor-paginated: pass `limit` (default 50, max 100) and follow the `next` cursor until it is null.
2. `GetCameras` — list cameras for the org (cursor-paginated). Optionally `GetCameraById` for a single camera's details.
3. `GetLiveUrl` (POST `/v1/cameras/live`) — get a live stream URL for the selected camera(s).
4. For an embeddable player, `GenerateLiveEmbedUrl` (POST `/v1/embeds/live`) — returns an iframe-able URL (requires `camera_share_create`).

## Notes
- Errors use conventional HTTP codes; 401 = bad/missing key, 403 = missing permission, 429 = rate limited. See errors/spot-problem-types.yml.
- No idempotency key is supported; these reads/link-generations are safe to retry.
