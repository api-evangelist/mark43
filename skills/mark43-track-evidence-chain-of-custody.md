---
name: Track evidence chain of custody in Mark43
description: Create evidence items and record their chain-of-custody events in a Mark43 agency's evidence system.
api: openapi/mark43-partnerships-openapi.yml
operations: [postEvidenceItems, getEvidenceItems, postEvidenceChainsOfCustody, postEvidenceChainEvents]
---

# Track evidence chain of custody in Mark43

Use this to register evidence/property items and log custody transfers from a partner evidence-management system.

## Prerequisites
- Base URL: `https://{department}.mark43.com/partnerships/api`.
- HTTP Basic auth with the agency's secure API token (`authentication/mark43-authentication.yml`).

## Steps
1. Create or list evidence items with `postEvidenceItems` (`POST /partnerships/api/external/evidence/items`) / `getEvidenceItems` (`GET /partnerships/api/external/evidence/items`).
2. Open a chain of custody for an item with `postEvidenceChainsOfCustody` (`POST /partnerships/api/external/evidence/chains_of_custody`).
3. Append each custody transfer as a chain event with `postEvidenceChainEvents` (`POST /partnerships/api/external/evidence/chain_events`).
4. Confirm each write via the `success` flag; the created objects are under `data` (`errors/mark43-problem-types.yml`).

## Rules
- Preserve ordering: post chain events in the real-world sequence of custody transfers; the chain is an append-only audit trail.
- No idempotency key is documented — reconcile against `getEvidenceItems` before re-posting after a failed request.
