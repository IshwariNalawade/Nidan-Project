# NIDAN: AI-Assisted Multilingual Patient Case-Taking & Triage System

[![Project Category](https://img.shields.io/badge/Category-Engineering%20%26%20Technology-blue)](https://github.com)
[![Tech Stack](https://img.shields.io/badge/Stack-React%20%7C%20TypeScript%20%7C%20Express%20%7C%20tRPC%20%7C%20Drizzle-green)](https://github.com)
[![Compliance](https://img.shields.io/badge/Compliance-DPDP%20Act%202023%20%7C%20ABDM%20FHIR%20R4-orange)](https://github.com)

**NIDAN** is an advanced, AI-assisted multilingual patient intake and triage platform designed for outpatient (OPD) departments in India. It addresses the critical bottleneck of rushed clinical consultations (averaging $\sim$2 minutes in India) by capturing structured medical histories via voice and touch in local Indian languages, digitizing prior records, integrating traditional AYUSH assessments, and outputting an editable HL7 FHIR R4 summary ready for the Ayushman Bharat Digital Mission (ABDM).

---

##  Key Features

1. **Multilingual Speech & Touch Intake:** Built-in integration with **Bhashini / AI4Bharat Indic models** (IndicASR, IndicWhisper) and SOCRATES clinical history framework for natural patient interaction in regional languages.
2. **Document Intelligence & OCR:** Instant digitization of prior prescriptions and lab reports using OCR engines with automatic error-flagging.
3. **Red-Flag Safety Triage:** Automated symptom severity scoring that prioritizes urgent cases and escalates them ahead of the standard queue.
4. **AYUSH Integration:** Seamless incorporation of classical **Dashavidha Pariksha** (tenfold examination) into routine clinical intake.
5. **Interoperability & Privacy:** Generates standardized **HL7 FHIR R4** summaries for ABDM compliance, backed by strict automated data consent and auto-purge workflows in compliance with the **DPDP Act, 2023**.
6. **Physician-in-the-Loop Dashboard:** Doctors receive structured AI draft summaries that can be reviewed, edited, and confirmed with a single click.

---

##  Technology Stack

* **Frontend:** React, TypeScript, Tailwind CSS, Vite, Radix UI, Lucide Icons.
* **Backend:** Node.js, Express, tRPC, Drizzle ORM, MySQL / PostgreSQL.
* **AI & NLP:** Bhashini (MeitY) / AI4Bharat Indic ASR/TTS, LLM Clinical Summarizer, Tesseract / PaddleOCR.
* **Interoperability & Standards:** HL7 FHIR R4, ABDM Standards, DPDP Act 2023.

---

##  Project Structure

```text
nidan/
├── client/                 # React frontend application (SPA)
│   ├── src/components/     # UI components & dashboard layouts
│   ├── src/pages/          # Main application views & intake flows
│   └── src/data/           # Localization dictionaries & language support
├── server/                 # Node.js backend & API routers
│   ├── _core/              # Platform core services (auth, storage, AI integrations)
│   ├── bhashini.ts         # Bhashini Indic ASR/TTS & translation adapter
│   └── routers.ts          # tRPC API endpoints & intake workflow logic
├── drizzle/                # Database schema and SQL migrations
└── docs/                   # Documentation and language support matrices
