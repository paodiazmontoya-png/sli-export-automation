# SLI Export Automation

An AI-assisted workflow that automates the preparation of a **Shipper's Letter of Instructions (SLI)** for international export shipments.

The project combines **Gemini, structured JSON, Google Sheets and Google Apps Script** to transform shipment information into a controlled document-generation workflow.

---

## Why I Built This

As part of my work in procurement and international logistics, I frequently prepare SLI documents for shipments exported from the United States.

The process was repetitive and required manually:

- reviewing Commercial Invoices;
- finding shipment information across different sources;
- entering quantities, classifications, weights and values;
- checking multiple fields;
- validating that required information was not missing;
- reviewing the document again before sending it.

While studying Artificial Intelligence and Data Analytics independently, I wanted to apply what I was learning to a real operational problem.

My first idea was simple:

> Upload the Commercial Invoice to an AI assistant and have it complete the SLI automatically.

The first version did not work as expected.

The AI could understand the document, but reliably editing the original Word template while preserving its structure and generating consistent final documents was not dependable enough for this workflow.

That limitation led me to redesign the solution.

---

## Solution Architecture

Instead of asking the AI to do everything, I separated the workflow into different layers.

```mermaid
flowchart LR
    A[Commercial Invoice] --> B[Gemini Gem]
    B --> C[Validation and Missing Data Check]
    C --> D[Structured JSON]
    D --> E[Google Sheet - SLI Control]
    E --> F[Google Apps Script]
    F --> G[Google Docs Master Template]
    G --> H[Completed Google Doc]
    G --> I[DOCX]
    G --> J[PDF]
