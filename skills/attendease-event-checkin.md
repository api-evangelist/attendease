---
name: attendease-event-checkin
description: Check attendees in at an Attendease event and reconcile session attendance using the Organization API.
api: Attendease Organization API
operations:
- GET /api/v2/events.json
- GET /api/v2/events/<event_id>/attendees.json
- POST /api/v2/attendees/<attendee_id>/checkins.json
- GET /api/v2/session_attendance.json
---

# Attendease Event Check-in

Operating instructions for checking attendees in at an event and reading session
attendance, grounded in the documented Attendease Organization API (`/api/v2/`).

## Prerequisites

- An organization Access Key ID + Secret Access Token.
- Sign every request with HMAC-SHA1: build the canonical string
  `content-type,content-MD5,request-URI,timestamp`, sign it, and send
  `Authorization: APIAuth <access_key>:<signature>` with a `Date` header in
  RFC 1123 format within 15 minutes of server time. See
  `authentication/attendease-authentication.yml`.
- All requests use HTTPS and JSON (`Content-Type: application/json`).

## Steps

1. **Find the event.** `GET /api/v2/events.json`. Page through results using
   `page` and `records_per_page` (1-100, default 50); read the `pagination`
   envelope. Capture the target `event_id`.
2. **List the event's attendees.** `GET /api/v2/events/<event_id>/attendees.json`.
   Match your registrant to an `attendee_id`.
3. **Check the attendee in.** `POST /api/v2/attendees/<attendee_id>/checkins.json`.
4. **Reconcile session attendance.** `GET /api/v2/session_attendance.json` to
   confirm attendance was recorded.

## Error handling

Errors return `{ "error": "message" }`. Handle 401 (re-sign; check the Date
skew), 403 (insufficient permissions), 404 (bad id), 422 (fix the payload from
the field-keyed `errors` object), and 429 (back off). See
`errors/attendease-problem-types.yml`.
