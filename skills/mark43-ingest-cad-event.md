---
name: Ingest a CAD event into Mark43
description: Push a computer-aided-dispatch event (call for service) into a Mark43 agency and read active events.
api: openapi/mark43-partnerships-openapi.yml
operations: [postCadEvent, getCadEvents, getCadEventsActive, getCadConfigurationUnits]
---

# Ingest a CAD event into Mark43

Use this to send a computer-aided-dispatch (CAD) event / call for service from a partner dispatch or location system into Mark43.

## Prerequisites
- Base URL: `https://{department}.mark43.com/partnerships/api`.
- HTTP Basic auth with the agency's secure API token (`authentication/mark43-authentication.yml`).

## Steps
1. (Optional) Resolve the agency's units and stations first with `getCadConfigurationUnits` (`GET /partnerships/api/external/cad/configuration/units`) so involved-unit references are valid.
2. Create the event with `postCadEvent` (`POST /partnerships/api/external/cad/event`). Add supplemental detail with `POST /partnerships/api/external/cad/event/additional_info`.
3. List events with `getCadEvents` (`GET /partnerships/api/external/cad/events`) or only in-progress ones with `getCadEventsActive` (`GET /partnerships/api/external/cad/events/active`).

## Rules
- Reference only unit/station ids returned by the CAD configuration endpoints; do not invent identifiers.
- Check `success`/`error` on the envelope; back off on `429`. No idempotency contract is documented, so de-duplicate against `getCadEvents` before re-posting.
