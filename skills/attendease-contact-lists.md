---
name: attendease-contact-lists
description: Build and maintain Attendease contact lists and their contacts via the Organization API.
api: Attendease Organization API
operations:
- GET /api/v2/contact_lists.json
- POST /api/v2/contact_lists.json
- GET /api/v2/contact_lists/<contact_list_id>/contacts.json
- POST /api/v2/contact_lists/<contact_list_id>/contacts.json
- PUT /api/v2/contact_lists/<contact_list_id>/contacts/<id>.json
- DELETE /api/v2/contact_lists/<contact_list_id>/contacts/<id>.json
---

# Attendease Contact Lists

Operating instructions for managing contact lists and their contacts, grounded in
the documented Attendease Organization API (`/api/v2/`).

## Prerequisites

- Organization Access Key ID + Secret Access Token.
- HMAC-SHA1 sign every request (`Authorization: APIAuth <access_key>:<signature>`,
  RFC 1123 `Date` within 15 minutes). See
  `authentication/attendease-authentication.yml`.
- HTTPS + `Content-Type: application/json`.

## Steps

1. **List existing contact lists.** `GET /api/v2/contact_lists.json`. Page with
   `page` / `records_per_page`; read the `pagination` envelope.
2. **Create a contact list.** `POST /api/v2/contact_lists.json` with the list
   payload. Capture the returned `contact_list_id`.
3. **Add contacts.** `POST /api/v2/contact_lists/<contact_list_id>/contacts.json`.
   Attach custom fields through the `meta` object
   (e.g. `{ "meta": { "segment": "vip" } }`).
4. **Update a contact.** `PUT /api/v2/contact_lists/<contact_list_id>/contacts/<id>.json`.
5. **Remove a contact.** `DELETE /api/v2/contact_lists/<contact_list_id>/contacts/<id>.json`.
   To clear a whole list, `DELETE /api/v2/contact_lists/<contact_list_id>/delete_contacts.json`.

## Conventions and errors

- Filter by custom fields with `?meta[key]=value` (multiple `meta` params AND
  together). See `conventions/attendease-conventions.yml`.
- Errors return `{ "error": "message" }`; validation errors (422) come back as a
  field-keyed `errors` object. See `errors/attendease-problem-types.yml`.
