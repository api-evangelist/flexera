---
name: Monitor license plates with an LPR interest list
description: Create a License Plate Recognition interest list and pull LPR reports for an LPR-enabled camera.
api: openapi/spot-intelligence-openapi.json
operations: [GetInterestLists, CreateInterestList, LprReport]
---

# Monitor license plates with an LPR interest list

Use the Spot AI Intelligence API to manage License Plate Recognition (LPR). Base `https://dev-api.spot.ai/`, bearer API-key auth.

## Steps
1. `GetInterestLists` (GET `/v1/lpi`) — list existing license-plate interest lists.
2. `CreateInterestList` (POST `/v1/lpi`) — create a new interest list of plates to watch for (returns `201`).
3. `LprReport` (GET `/v1/lpr/cameras/{camera_id}/report`) — pull the LPR report for an LPR-enabled camera. The camera must have LPR enabled or the request returns `422`.

## Notes
- Pair with the Intelligence analytics endpoints (`Counting`, `Idle`, `Presence` under `/v1/cameras/{cameraId}/intelligence/{entity}/...`) for people/vehicle activity summaries.
- Errors use conventional HTTP codes with a ValidationError envelope on `422`. See errors/spot-problem-types.yml.
