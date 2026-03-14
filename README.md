# Prior Auth Auto-Assembler

An agentic AI prototype that automates the assembly of prior authorization (PA) packets for home oxygen (DMEPOS) — the most common and administratively burdensome PA category in US healthcare.

Drop in clinical documents → AI extracts the relevant evidence → rules engine checks payer-specific criteria → submission-ready packet with gap analysis.

Runs entirely in the browser. No backend, no data upload.

---

## The problem it solves

Prior authorization for home oxygen requires assembling multiple documents — ABG/pulse-ox results, face-to-face visit notes, MD notes, CMN forms, 6MWT results — and checking them against payer-specific coverage criteria (Medicare LCD, MA plan rules). Errors or missing items result in denials that delay patient care.

This tool automates the extraction, validation, and assembly steps that are currently done manually by clinical staff.

---

## What it does

**Step 1 — Upload documents**
Drop PDFs, images, CCDAs, clinical notes, or faxes. The AI extracts structured clinical data from each document and classifies it by type (ABG, PulseOx, 6MWT, MDNote, FaceToFace, Order, CMN, POD, Authorization).

**Step 2 — Evidence extraction**
Extracted values are mapped to the clinical evidence model:
- SpO₂ at rest / during exercise / during sleep
- PaO₂ values
- 6-Minute Walk Test nadir
- Face-to-face visit date
- Diagnosis, comorbidities (oedema, pulmonary HTN, OSA)
- Order fields (flows at rest/exertion, hours/day, modality)
- Chronic stable state confirmation

**Step 3 — Payer rules check**
The rules engine evaluates the extracted evidence against payer-specific criteria. Currently supports:
- **Medicare (DMEPOS Oxygen)** — Group I/II rest criteria, exercise pathway, sleep pathway (with and without OSA), CMN/SWO requirements
- **Plan X (MA — stricter)** — all Medicare requirements plus 6MWT with continuous SpO₂ trace, recent progress note, supplier attestation

**Step 4 — Readiness score & gap analysis**
A completeness score (0–100%) is computed based on qualifying criteria met. Outstanding items are categorised as:
- **Critical** — required for submission
- **Important** — improves approval chance
- **Optional** — recommended for audit trail

**Step 5 — Packet assembly & sharing**
Builds a payer-specific PDF bundle or ePA payload with attached qualifying evidence. Generates a unique tracking ID and sharing link for real-time status updates via email or SMS.

---

## Coverage criteria implemented

### Medicare Group I (rest)
- SpO₂ ≤ 88% or PaO₂ ≤ 55 mmHg on room air
- SpO₂ 89% with secondary diagnosis (oedema, pulmonary HTN, haematocrit > 56%)

### Medicare exercise pathway
- Non-qualifying at rest (SpO₂ ≥ 90%) + SpO₂ ≤ 88% during 6MWT + improvement with O₂

### Medicare sleep pathway
- ≥ 5 min ≤ 88% within ≥ 120 min recording (no OSA)
- With OSA: facility-titrated PSG, PAP optimised, AHI/RDI ≤ 10

### Additional checks
- Face-to-face visit ≤ 30 days prior to initial certification
- Chronic stable state at time of qualifying test
- Blood gas study by qualified provider
- Order fields complete (flows at rest/exertion + hours/day)
- Portable oxygen: patient mobile in home

---

## Tech stack

- **AI extraction**: LLM-powered document parsing (agentic pipeline)
- **Rules engine**: deterministic JavaScript — payer criteria coded as explicit check functions
- **Frontend**: HTML/CSS/JS — single-file, runs locally, no dependencies
- **State**: in-memory with full audit trail

---

## Running locally

Open the HTML file directly in any modern browser. No installation required.

---

## Project structure

```
priorauthorization/
└── 29th Oct_agentic_ai_solution_prior_auth_REFINED_COMPLETE.html   # Full prototype (single file)
```

---

## Status

Functional prototype. Medicare and one MA plan ruleset implemented. Additional payer rulesets and document types in progress.

---

## Author

Silvia Adinda — personal project, October 2025
