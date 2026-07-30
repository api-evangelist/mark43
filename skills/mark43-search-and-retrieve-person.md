---
name: Search and retrieve a person in Mark43
description: Look up person records in a Mark43 agency RMS and fetch a specific master person by id.
api: openapi/mark43-partnerships-openapi.yml
operations: [postPersonSearch, getPerson, postPersonMaster]
---

# Search and retrieve a person in Mark43

Use this to find existing person records and read a specific one before creating or linking records.

## Prerequisites
- Base URL: `https://{department}.mark43.com/partnerships/api`.
- HTTP Basic auth with the agency's secure API token on every request (`authentication/mark43-authentication.yml`).

## Steps
1. Search with `postPersonSearch` (`POST /partnerships/api/external/person/search`), passing your search criteria in the JSON body. Page results with `size`/`from`.
2. From the `data` array, pick the target person id.
3. List/retrieve person records with `getPerson` (`GET /partnerships/api/external/person/`), paging with `size`/`from`, to confirm the match.
4. If no match exists and you need one, create a master person with `postPersonMaster` (`POST /partnerships/api/external/person/master`).

## Rules
- Always search before creating to avoid duplicate master person records; use merge endpoints (`POST /partnerships/api/external/person/merge`) only when the agency's policy allows.
- Read `success`/`error` on every envelope; treat `403` as "token cannot see this person's agency scope."
