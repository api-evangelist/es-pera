---
name: es-pera-public-data
description: Consult published ES·pera data about Spanish healthcare waiting lists and hospitals. Use for release-aware, source-preserving queries through the public read-only API.
---

# ES·pera Public Data

Use this skill to retrieve public data from `https://es-pera.org/api/v1`.

## Workflow

1. Request `/releases/current` and retain the returned release identifiers with the result.
2. For hospital data, discover identifiers with `/entities`, then request `/entities/{entityId}`.
3. For autonomous waiting-list data, inspect `/waiting/communities`, then use `/waiting/facets?community={slug}` before requesting `/waiting/observations`.
4. Follow `pagination.hasMore` and `pagination.nextOffset` until the required result set is complete.
5. Request only the pages needed for the task. If the API returns `429`, wait for the number of seconds in `Retry-After` before retrying; do not run parallel retries.
6. Preserve period, unit, perimeter, source and provenance fields when presenting or storing observations.

## Data rules

- Treat `null` as unavailable, never as zero.
- Do not compare similarly named metrics across autonomous communities unless their comparability is explicitly established.
- Keep hospitals and hospital complexes as distinct reporting perimeters.
- Treat entity identifiers as opaque and reuse only those returned by the API; do not infer identity from similar names.
- Internal source-unit identifiers and identity-resolution evidence are intentionally absent from the public contract. Do not attempt to reconstruct or correlate them.
- The API is read-only. Do not attempt write operations.
- Respect the anonymous limits: 120 requests per minute overall and 20 per minute for `/waiting/facets` and `/waiting/observations`.

The complete machine-readable contract is available at `https://es-pera.org/openapi.json`. Human documentation and source caveats are at `https://es-pera.org/metodologia/api/`.
