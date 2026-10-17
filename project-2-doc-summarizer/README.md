# Automated Knowledge Extraction & Multi-Format Summarizer

## Business Context & Problem
Enterprise organizations generate vast quantities of unstructured textual data (incident debriefs, meeting transcripts, client feedback). Critical quantitative metrics and action items get buried, requiring manual synthesis and delaying operational resolution.

This automation ingests unstructured text payloads, extracts structured operational attributes using LLM reasoning, routes the metrics into permanent storage, and dispatches high-level summaries—all while utilizing automated failover logic to prevent pipeline crashes.

---

## Technical Architecture & Workflow

[Postman / Webhook Trigger]
│
▼
[Custom Webhook Listener (Make.com)]
│
▼
[LLM Multi-Attribute Extractor] ──(On Error)──► [Resume Directive: Default Fallback]
│
▼
[JSON Parsing Module]
│
▼
[Google Sheets / Database Archival]
│
▼
[Executive Notification Dispatch (Email / Slack)]
---

## Implementation Details
* **Multi-Attribute Schema Extraction:** Engineered system prompt templates requiring the model to extract and isolate:
  * `key_metrics`: Array of raw statistics, times, and percentages.
  * `primary_bottleneck`: Normalized single-sentence pain point.
  * `action_items`: Array of objects containing owners, tasks, and deadlines.
  * `sentiment_status`: Categorical label (Critical, Concerned, Neutral, Positive).
* **Fault-Tolerant Error Handling:** Configured a native Make.com `Resume` error handler directly on the LLM module. If the API returns a 429 rate limit or unexpected timeout, the pipeline injects an automated fallback payload to allow downstream notification and logging rather than terminating abruptly.
* **Array Joining & Formatting:** Parsed structured JSON arrays into human-readable notification strings using Make's native array manipulation functions (`join()`).

---

## Repository Artifacts
* [Make.com Scenario Blueprint](./doc-summarizer-blueprint.json)
* [Postman Collection](./doc-summarizer-postman-collection.json)

---

## Blueprints And Architecture Diagrams
* [Make.com Scenario Blueprint diagram](./Multi-format-summarier-blueprint-diagram)
* [Postman Collection diagram](./Multi-format-summarier-postman-architecture-diagram)
