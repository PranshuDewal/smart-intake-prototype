# smart-intake-prototype
# AI-Powered Insurance Case Intake — Analyst-in-the-Loop Prototype

> **Live demo:** [pranshudewal.github.io/smart-intake-prototype](https://pranshudewal.github.io/smart-intake-prototype/)

A 6-screen interactive prototype demonstrating how an AI-assisted intake pipeline can replace manual data entry in insurance and claims operations — while keeping analysts firmly in control of every decision.

---

## Problem Addressed

Insurance and claims operations teams receive high volumes of unstructured intake documents — PDFs, scanned forms, and emails. Manual data extraction is slow, error-prone, and creates first-mile bottlenecks. Analysts spend disproportionate time re-keying data that could be extracted automatically, leaving less capacity for the judgment-intensive decisions only humans should make.

**The core design question this prototype answers:** How should a human-AI intake workflow actually behave — screen by screen — to be trustworthy, auditable, and adoptable in a regulated environment?

---

## Intended Users

| User | Role in Prototype |
|---|---|
| **Claims / intake analyst** | Reviews AI-extracted fields, corrects errors, approves or escalates cases |
| **Operations manager** | Views work queue, monitors backlog health, tracks AI confidence trends |
| **IT / transformation team** | Evaluates AI-assisted intake as a target-state design before committing to a production build |

---

## Workflow Being Redesigned

The prototype models the transition from a fully manual intake process to an AI-assisted, analyst-validated pipeline.

| Current State | Target State (this prototype) |
|---|---|
| Analyst opens each document and reads it | AI agent extracts all structured fields automatically |
| Manual re-keying into legacy case management system | Analyst reviews, corrects, and approves pre-filled data |
| No visibility into extraction quality | Per-field AI confidence scores surface every ambiguous extraction |
| All cases handled in FIFO order | Priority and confidence scoring drives work queue sequencing |
| No structured exception path | Flagged cases route to explicit resolution actions before proceeding |
| No audit metadata | Approval captures analyst identity, AI confidence, and processing time |

---

## What I Designed and Built

This is a solo-built, static HTML/CSS/JavaScript prototype:

### Screen 1 — Analyst Work Queue (Dashboard)
A prioritized case list with status badges, AI confidence score column (colour-coded green / amber / red), and filter controls for All / Ready to Review / Needs Attention / Blocked states. The design question: what does an analyst need to see to triage their queue efficiently without opening every case?

### Screen 2 — Case Detail: Split-Screen View
The core "analyst-in-the-loop" interface. Left panel: AI-extracted fields with a confidence percentage displayed per field. Right panel: the source document for side-by-side verification. The design question: how do you let an analyst verify AI output without re-reading the whole document?

### Screen 3 — Review & Validate (Happy Path)
All required fields extracted at acceptable confidence. Analyst sees a field-by-field validation summary, can edit any field inline, and proceeds to approve. The design question: what is the minimum friction path for a clean case?

### Screen 4 — Edge Case: Incomplete / Low-Confidence Data
Three fields are missing (shown as `[MISSING]` in red) and one field has a low-confidence extraction (45% — handwritten date). The **Approve button is disabled**. Analyst must choose a resolution path: Request Missing Info, Manual Entry, or Block & Notify Client. The design question: how does the system prevent a bad case from proceeding without blocking the analyst from choosing their own resolution?

### Screen 5 — Approval Confirmation
A pre-approval summary showing all case data plus validation metadata (AI confidence aggregate, analyst name, processing time). One final confirmation step before the case is pushed to the legacy system. The design question: what does an analyst need to see to feel accountable for their approval?

### Screen 6 — Success & Tracking
Confirmation with original case ID mapped to the generated legacy system ID, client notification status, and a time-saved metric (18 minutes per case versus baseline). The design question: what closes the loop for the analyst and makes the efficiency gain visible?

---

## Human-in-the-Loop Decision Points

The prototype is built around a single principle: **AI handles extraction; humans retain decision authority.**

1. **Field-level confidence review** — every extracted field shows its confidence %; the analyst decides whether to accept or correct it
2. **Approval gate** — no case advances without explicit analyst action; there is no auto-approval path
3. **Ambiguity disables approval** — missing or low-confidence fields disable the Approve button; the system cannot be bypassed
4. **Explicit exception paths** — the analyst chooses how to resolve each flagged field (request info, enter manually, or escalate)
5. **Block & Notify** — the analyst can halt a case entirely and escalate to the client before it enters the system
6. **Audit metadata on every approval** — analyst name, AI confidence summary, and processing time are captured on the success screen

---

## Responsible AI and Security Considerations

These governance principles are embedded in the prototype design, not bolted on afterwards:

| Principle | How it is applied |
|---|---|
| **Explainability** | Per-field confidence scores are always visible; the system makes no opaque decisions |
| **Human override** | Every AI-extracted field is editable; extraction is advisory, not deterministic |
| **Fail-safe on ambiguity** | The Approve action is disabled until all flagged fields are resolved |
| **Data minimization** | No real PII is used; all data in the prototype is synthetic |
| **Audit trail** | Every approved case captures case ID, analyst identity, and processing metadata |
| **No auto-routing** | Every case requires a human decision; the system cannot route a case without analyst action |

**In a production environment, additional controls would include:** role-based access control (RBAC) and SSO integration; encryption at rest and in transit; model drift monitoring and retraining triggers; bias auditing on extraction confidence by document type; formal GRC sign-off and model risk assessment; data retention and privacy policy enforcement.

---

## Current Prototype Limitations

This is an intentional validation artifact — a static UI prototype designed to test workflow logic and UX before committing to a production build. It does not misrepresent backend readiness.

| Limitation | Notes |
|---|---|
| **Static HTML only** | No backend; AI extraction is simulated with hardcoded representative values |
| **No document parsing** | Source document panel is illustrative — no OCR or NLP pipeline is wired in |
| **Single user session** | No authentication, no roles, no multi-analyst queue management |
| **No integration layer** | The "push to legacy system" action is UI-only — no actual API call is made |
| **No persistence** | No database; state resets on page reload |
| **Single case type** | Only personal injury / general liability intake is modelled |

---

## What Productionization Would Require

A production version of this workflow would require the following layers:

**AI/ML layer**
- Document ingestion pipeline (PDF parser, OCR for scanned documents)
- NLP/LLM extraction model (e.g., fine-tuned model or structured extraction via Azure Document Intelligence / AWS Textract)
- Per-field confidence calibration, threshold governance, and human review trigger rules
- Model monitoring pipeline, drift alerting, and retraining cadence

**Application layer**
- Backend API (Python/FastAPI or Node.js) connecting the UI to extraction and case management services
- Authentication and role-based access control (IAM/SSO integration)
- Case state machine with full audit logging
- Integration with legacy case management system via REST/SOAP API

**Governance layer**
- Formal model risk assessment and regulatory sign-off
- Explainability documentation for compliance and audit purposes
- Data privacy assessment (PII handling, retention policy, cross-border data rules)
- Organizational change management plan for analyst adoption

**Infrastructure**
- Cloud hosting (Azure or AWS) with appropriate security controls and network segmentation
- CI/CD pipeline with automated testing for extraction quality and UI regression
- Monitoring and alerting for extraction accuracy, throughput, and system health

---

## Live Demo

🔗 [pranshudewal.github.io/smart-intake-prototype](https://pranshudewal.github.io/smart-intake-prototype/)

---

## Prototype Walkthrough

### Screen 1: Analyst Work Queue
The dashboard shows a prioritized list of incoming cases. AI confidence is colour-coded (green ≥ 90%, amber 70–89%, red < 70%). Analysts can filter by status and take quick actions without opening each case.

### Screen 2: Case Detail — Split-Screen View
The core working view. Left panel: 8 extracted fields (Case ID, Client Name, Case Type, Jurisdiction, Incident Date, Claim Amount, Policy Number, Loss Description) each with its confidence %. Right panel: the source document. Analyst can verify and correct side-by-side.

### Screen 3: Review & Validate (Happy Path)
All fields extracted successfully. Field-level checkmarks, individual confidence scores, and edit capability. The Approve button is enabled.

### Screen 4: Edge Case — Incomplete / Low-Confidence Data
Three fields show `[MISSING]` in red. One field shows 45% confidence with a warning indicator. The Approve button is **disabled**. Analyst must choose: Request Missing Info (triggers automated email), Manual Entry, or Block & Notify Client. This screen is the governance core of the design.

### Screen 5: Approval Confirmation
Pre-approval summary with full case details, AI confidence aggregate, analyst name, and processing time. Final commit step before the case enters the legacy system.

### Screen 6: Success & Tracking
Confirmation with original Case ID → Legacy System ID mapping, client notification status, and the time-saved metric (18 minutes per case versus manual baseline).

---

*Built by [Pranshu Dewal](https://pranshudewal.github.io/portfolio/) as a portfolio prototype demonstrating AI workflow design, human-in-the-loop governance, and enterprise intake modernization thinking. The prototype is a deliberate validation artifact — it tests the logic, UX, and governance structure before a production commitment, not a claim of production-ready AI infrastructure.*
