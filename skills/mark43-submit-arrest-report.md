---
name: Submit an arrest report to Mark43
description: Create a single arrest report (or a batch) in an agency's Mark43 RMS via the Partnerships API.
api: openapi/mark43-partnerships-openapi.yml
operations: [postReportsArrest, postReportsArrestBulk, getReportsArrests]
---

# Submit an arrest report to Mark43

Use this to push an arrest report from a partner system into a Mark43 agency's records management system (RMS).

## Prerequisites
- Base URL is the agency tenant: `https://{department}.mark43.com/partnerships/api`.
- Authenticate every request with HTTP Basic — put the agency-issued secure API token in the `Authorization` header. Tokens come from the agency's Mark43 Technical Services Representative. See `authentication/mark43-authentication.yml`.
- All bodies and responses are JSON (UTF-8).

## Steps
1. Build the arrest report payload per the agency's configured schema.
2. Submit one report with `postReportsArrest` (`POST /partnerships/api/external/reports/arrest`). For many at once, use `postReportsArrestBulk` (`POST /partnerships/api/external/reports/arrest/bulk`).
3. Read the response envelope: check `success` is `true`; the created object(s) are under `data`. If `success` is `false`, read `error` (see `errors/mark43-problem-types.yml`).
4. To verify, list arrests with `getReportsArrests` (`GET /partnerships/api/external/reports/arrests`), paging with `size`/`from` and filtering with `updatedAfter`.

## Rules
- No idempotency key is documented; do not blindly retry a `POST` on a network error — reconcile with `getReportsArrests` first to avoid duplicates.
- Handle `429` (rate limiting) with backoff; `401/403` mean the token is missing/invalid or lacks access.
