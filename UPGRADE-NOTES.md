# NIDAN incremental upgrade notes

Implemented without removing the existing intake, Bhashini adapter, Gemini draft, help queue, and platform OAuth seams:

- Local patient registration/login with scrypt password hashing, HTTP-only session cookies, 30-day expiry, and database-backed sessions.
- Explicit patient, staff, doctor, and admin roles. Staff/doctor provisioning is admin-only; clinician report review/edit is doctor/admin-only.
- Patient history is scoped to the authenticated patient account. Staff/doctor lists are protected by role middleware.
- Additive database migration for accounts, sessions, uploads, reports, and account ownership.
- Server-only environment placeholders for Bhashini and Gemini. No secret is exposed through Vite or public runtime config.
- Upload persistence through the existing storage helper; OCR is represented honestly as pending/unsupported until a configured Bhashini OCR pipeline returns success.
- Original patient responses, translated drafts, provenance, consent, and clinician-review-required flags are stored separately.
- Existing Gemini behavior remains consent-gated, draft-only, and clinician-reviewed.
- All 22 scheduled-language codes are accepted by the server and catalogue; the support matrix records what is application-tested versus provider-unverified.
- A generated `client/src/data/locales.json` catalog covers 21 core user-facing strings for all 22 scheduled languages, including welcome, account, consent, upload, validation, safety, and fallback copy. It is explicitly machine-drafted and requires native-speaker/clinical review.
- ASR now defaults to the catalog-listed `bhashini/ai4bharat/conformer-multilingual-asr` service, and OCR defaults to the catalog-listed `bhashini/bodhan/indic-doc/ocr` service. The intake exposes audio upload → ASR transcription and document upload → OCR extraction routes; originals remain preserved and provider failures remain visible.

Remaining limitations requiring real deployment configuration and clinician/product review:

1. Provider support must be verified per language, task, account, and service ID using the integration test console; this ZIP does not invent support.
2. The generated catalog covers the core intake/account strings, but some existing portal/dashboard labels remain hard-coded English until native-speaker review is completed. English fallback is explicitly disclosed rather than claimed as translation.
3. Upload OCR now invokes the configured Bhashini OCR task when a source language is supplied; provider-specific success remains unverified until credentials and service IDs are available to the runtime.
4. Production requires a managed database, secret manager, HTTPS, storage access policy, audit retention, backup/restore, rate limiting, CSRF protection for non-SameSite deployments, and operational monitoring.
5. The application is not a medical device and does not diagnose, prescribe, or autonomously triage.

The service IDs and language lists were checked against the [Bhashini available-models catalog](https://dibd-bhashini.gitbook.io/bhashini-apis/available-models-for-usage). Authentication and request construction follow the [Bhashini API overview](https://bhashini-developer-portal-dev.bhashini.co.in/docs/api/overview) and [pipeline compute request schema](https://bhashini-developer-portal-dev.bhashini.co.in/docs/api/pipeline-compute-call/request-payload).
