# NIDAN implementation plan

## Scope

NIDAN is a multilingual patient-intake workspace for a reception kiosk, tablet, patient phone, staff assistance, and doctor review. The first usable slice will be a working patient intake shell, a staff-facing help queue, a doctor review view, persistent demo records, and an administrator-facing Bhashini readiness console. It must remain usable when Bhashini is not configured: typing, file upload, and browser speech are fallbacks, never silently presented as Bhashini.

The Bhashini integration will be server-side only. The adapter will use the documented Udyat inference key as a raw `Authorization` header for the documented inference endpoint when configured. It will not invent a pipeline ID, service ID, language mapping, request body, or capability. The app will make configuration gaps explicit and expose safe test results without displaying secret values.

## Architecture

- `client/src/pages/Home.tsx`: role-aware product shell with patient intake, integration readiness, staff queue, and doctor review views.
- `client/src/index.css`: visual system and responsive layout.
- `server/routers.ts`: tRPC procedures for intake records, help requests, integration status, and Bhashini capability tests.
- `server/bhashini.ts`: isolated Bhashini/Udyat adapter with explicit configuration checks, safe errors, and capability-specific request builders.
- `drizzle/schema.ts` and `server/db.ts`: persistent patients, intake sessions, help requests, integration test history, and uploads metadata. The app uses safe in-memory fallback only when the managed database is unavailable during local preview; the UI labels that state.
- `public/manus-routes.json`: route declaration for the single-page workspace.
- `.env.example`: placeholder variable names only; no credentials.

## Product design

### Candidate styles

1. Clinical command center — probability 0.07.
2. Government service kiosk — probability 0.06.
3. Editorial wellness journal — probability 0.08.

### Chosen direction: Clinical command center

**Design movement:** quiet clinical modernism: Swiss information design, softened for a public healthcare kiosk.

**Core principles:** calm hierarchy, high legibility, immediate next actions, and visible provenance for every language-processing result.

**Color philosophy:** white and warm mist form the safe canvas; deep ink-blue carries trust and navigation; an electric teal is the NIDAN signature for verified system states; amber is reserved for review and human attention, never used as a colour-only warning.

**Layout paradigm:** a left rail for mode and trust status, a wide working canvas for intake, and a right-side “signal” column for progress, provenance, and human help. On small screens the rail collapses into a compact top bar and the signal column follows the main action.

**Signature elements:** a blue “care line” running through progress steps, small provenance chips (“Manual”, “Browser fallback”, “Bhashini”), and a rounded signal panel for human attention.

**Interaction philosophy:** one primary action per panel, generous touch targets, editable speech transcripts, confirmation before medically important answers, and always-visible staff help.

**Animation:** short 160–220ms ease-out transitions for panel changes, a slow pulse only on active microphone or pending help, and no auto-speaking or looping motion.

**Typography:** Inter for interface text and a large, high-contrast display style for bilingual headings; Devanagari and Latin must share the same readable rhythm.

**Brand essence:** NIDAN helps every patient tell their health story in the language and mode that works for them. Personality: calm, inclusive, accountable.

**Brand voice:** direct, reassuring, and transparent. Example lines: “Your health story, in your language.” and “Nothing is final until you review it.”

**Wordmark & logo:** a compact “N” built from two offset care-line strokes, paired with the NIDAN wordmark and the bilingual welcome lockup.

**Signature brand color:** NIDAN teal `#11A6A0`, used only for positive provenance and completion states.

## Delivery shape

The first build should be a polished, responsive SPA with functional local interactions and backend persistence hooks. Bhashini capability testing will require the user to provide only the credential/configuration values through the secure environment/secret flow when ready. Until then, the app must report ASR, OCR, TTS, translation, and other capabilities as unconfigured rather than simulated.
