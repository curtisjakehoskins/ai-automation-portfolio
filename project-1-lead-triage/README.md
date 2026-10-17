# Multi-Agent Inbound Lead Triage & Enrichment Engine

## Business Context & Problem
Inbound business inquiries often arrive through unformatted channels (web forms, emails, support widgets). Manual triage creates latency, delays high-value enterprise follow-up, and wastes sales reps' time manually sorting low-value tickets. 

This automation creates a zero-latency pipeline that ingests raw inbound payloads, runs structured LLM-based intent and urgency classification, and routes qualified leads directly to tracking sheets.

---

## Technical Architecture & Workflow
[Postman / Webhook Trigger]
│
▼
[Custom Webhook Listener (Make.com)]
│
▼
[LLM Intent & Urgency Classifier (Structured JSON Prompt)]
│
▼
[JSON Parsing Module]
│
▼
[Flow Control Router]
 ── (Score >= 4) ──► [Priority Google Sheets / AE Pipeline]
 ── (Score < 4)  ──► [Standard Support Queue]

---

## Implementation Details

* **Webhook Ingestion:** Built a dedicated Make.com endpoint to receive JSON payloads containing lead names, corporate domains, and unstructured text inquiries.
* **API Validation (Postman):** Created and executed an automated Postman test collection verifying endpoint latency, header parsing, and payload schema handling.
* **Prompt Engineering & Schema Enforcement:** Employed system-role prompt constraints to prevent conversational preamble and enforce a strict JSON schema:
  * `category`: Categorizes lead type (Enterprise, SMB, Support, Spam).
  * `urgency_score`: Integer rating from 1 to 5.
  * `summary`: Single-sentence synthesis of client need.
  * `recommended_action`: Deterministic next-step instruction.
* **Dynamic Routing:** Conditioned Make.com router paths using numeric comparison operators against the extracted `urgency_score`.

---

## Edge Case & Failure Handling
* **Non-JSON or Malformed Text:** Enforced strict JSON mode within the LLM completion settings to ensure the downstream JSON Parser module does not fail due to Markdown backticks or prose.
* **Threshold Routing:** Implemented strict numeric operators (`>= 4` vs `< 4`) to prevent ambiguous routing states.

---

## Repository Artifacts
* [Make.com Scenario Blueprint](./lead-triage-blueprint.json)
* [Postman Collection](./lead-triage-postman-collection.json)

---

## Blueprints & Architecture diagrams
* [Make.com Scenario Blueprint](./Lead-triage-blueprint.png)
* [Postman Collection](./Lead-triage-postman-architecture-diagram.png)

---

