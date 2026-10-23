# NIDAN implementation outcomes

## Delivered in this incremental upgrade

- [x] Preserve the existing React/Express/tRPC intake, help queue, Bhashini adapter, Gemini draft, and platform OAuth seams.
- [x] Add patient self-registration/login with scrypt password hashing, HTTP-only session cookies, expiry, logout, and persistent session rows when `DATABASE_URL` is configured.
- [x] Add explicit patient, staff, doctor, and admin roles. Staff and doctor provisioning is restricted to authenticated administrators; clinician report create/review/edit is doctor/admin-only.
- [x] Scope patient history, uploads, and reports to authenticated account/role boundaries; retain a compatibility staff-code path for existing deployments and tests.
- [x] Add additive Drizzle migration for account ownership, sessions, uploads, and reports without removing existing tables.
- [x] Persist uploads through the existing storage helper and invoke the configured Bhashini OCR capability when a source language is supplied; retain `pending`, `success`, `unsupported`, and `failed` states.
- [x] Preserve original responses separately from translated responses and provenance. Gemini remains consent-gated, draft-only, clinician-review-required, and non-diagnostic.
- [x] Accept all 22 scheduled Indian-language codes, preserve Unicode input, and apply RTL direction for Urdu, Kashmiri, and Sindhi.
- [x] Provide a 22-language matrix, Windows CMD setup guide, migration, environment template, upgrade notes, deterministic language validation script, and test/build evidence.

## Remaining open outcomes

- [ ] Fill and test real Bhashini account credentials/service IDs for each available ASR, TTS, NMT, transliteration, and OCR pipeline; mark matrix rows only from actual provider responses.
- [ ] Complete native-speaker and clinical review of all UI, intake, validation, consent, report, error, and help strings for every language. The current app explicitly discloses English fallback rather than silently claiming translation.
- [ ] Add production audit log retention, rate limiting, CSRF controls for deployment topologies that need them, managed secret injection, backups, restore drills, and clinician-approved operational policies.
