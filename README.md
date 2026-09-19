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


How It Works

1. Commercial Invoice Analysis
A Gemini Gem receives the Commercial Invoice and extracts shipment information that is clearly supported by the document.
The Gem is instructed not to invent missing information.
If required information is missing, ambiguous or conflicting, it asks the user before continuing.

2. Structured JSON
Once the required information has been validated, the Gem generates a structured JSON payload.
The JSON includes variable information such as:
exporter data;
consignee data;
destination;
cargo insurance amount;
commodity information;
quantity;
unit of measure;
shipping weight;
export classification;
export authorization;
commercial value;
SLI date.
Some compliance-sensitive information remains user-controlled instead of being autonomously inferred.

3. Google Sheet Control Layer
The validated JSON is transferred to a Google Sheet called:
SLI Control
Each row represents a shipment.
The control sheet stores:
Shipment ID
Gem JSON Payload
Automation Status
Google Doc
DOCX
PDF
Generated At

4. Google Apps Script Validation
A Google Apps Script reads the JSON and validates the data before generating the SLI.
The process stops if:
required information is missing;
unresolved conflicts remain;
the JSON schema is incorrect;
deprecated fields are present;
mandatory commodity information is incomplete.
This prevents the automation from silently continuing with incomplete information.

5. Master Template
Fields that remain constant across shipments are configured directly in the Google Docs master template.
Variable fields use controlled placeholders that are populated by Apps Script.
This separation reduces unnecessary automation logic and helps protect permanent shipment instructions from accidental modification.

6. Document Generation
When validation passes, Apps Script:
Creates a copy of the SLI master template.
Replaces variable placeholders.
Populates commodity information.
Checks that no unresolved placeholders remain.
Creates the completed Google Doc.
Exports the document as DOCX.
Exports the document as PDF.
Writes the document links back to the control Sheet.

An Example of the Iteration Process
One of the first versions generated:
Box 22: 101 Piece
Box 23: N/A
But the workflow required:
Box 22: 101
Box 23: Piece
The JSON structure and Apps Script logic therefore had to be modified.
The corrected data structure became:
{
  "box_22_quantity": 101,
  "box_23_unit": "Piece"
}
This was representative of the development process:
test → identify the error → determine whether the issue came from the prompt, JSON schema or code → modify → test again.

Validation Philosophy
One of the main principles of this project is:
If required information is missing, the automation should stop instead of guessing.
The workflow intentionally separates:
AI-assisted extraction;
human confirmation;
structured data;
deterministic validation;
document generation.
This was especially important because the final output is used as part of an international logistics workflow where data accuracy matters.

Tech Stack
Gemini
Prompt Engineering
Context Engineering
JSON
Google Sheets
Google Apps Script
Google Docs
Google Drive
JavaScript
Process Automation
International Logistics

Results
The first shipment was used as the main testing environment while I refined the workflow and corrected different behaviors.
After that initial development, I tested the automation with 6 additional shipments containing different information.
The workflow has now been tested across:
7 different shipments
The result is a more controlled and repeatable process for preparing the SLI.

What I Learned
I am not a software developer.
My background is in procurement and international logistics.
This project reinforced something important for me while learning AI and Data Analytics:
Domain knowledge is extremely valuable when building automation.
I knew:
which information was required;
which fields changed between shipments;
which information remained constant;
which errors were operationally important;
what the final document needed to look like.
AI helped me build and troubleshoot the technical components, while I focused on defining requirements, testing outputs and validating the process.
This project started with a simple question:
What repetitive task in my daily work could I improve with what I am learning?
For me, the answer was the SLI.
