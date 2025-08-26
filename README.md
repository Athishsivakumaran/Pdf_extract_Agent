# Pdf_extract_Agent
## Executive Summary
Pixie Settlement File Creator Agent automates the extraction of key settlement data from user-uploaded digital PDFs and generates a structured Excel file with predefined columns. The agent is designed to streamline and standardize the process of transferring information from diverse PDF statements into a uniform settlement spreadsheet, reducing manual effort and error risk.
 
## Goals & Objectives
- Automate data extraction from varied-format PDFs representing settlement information.
- Standardize output as an Excel file with specified columns, ready for further processing or analysis.
- Ensure accuracy, completeness, and readiness of data for downstream business use.
 
## Scope (In/Out)
**In-Scope:**
- Manual upload of digital PDFs (one at a time) via Pixie interface.
- Extraction of the following fields from PDF:
    - Property Name
    - Prd Date
    - Prod (Oil/Gas)
    - Volume
    - Share
    - Property Amount (for Gas/Oil Sales)
    - Your Share (for Gas/Oil Sales)
    - Copas Administrative Overhead (if available)
    - Total Operating Costs (if available)
- Processing logic for row deduplication and aggregation, as outlined below.
- Output as a newly generated Excel file with standardized columns.
- Summary report of processing results to the user.
 
**Out of Scope:**
- Extraction from scanned image PDFs (OCR), non-digital formats, or batch uploads.
- Integration with authentication or access control systems.
- Appending to existing Excel files or applying advanced Excel formatting/logic.
- Delivery outside Pixie (e.g., email, cloud storage integration).
 
## Inputs
- **File Type:** Digital PDF (no OCR required)
- **Upload Method:** Manual upload via Pixie UI
- **Fields to Extract:**
    - Property Name
    - Prd Date
    - Prod (Oil/Gas)
    - Volume
    - Share
    - Property Amount (Gas/Oil Sales)
    - Your Share (Gas/Oil Sales)
    - Copas Administrative Overhead (if available)
    - Total Operating Costs (if available)
- **Source Format Variability:** PDF formats may vary, but key fields are named or inferable; one file per operation
 
## Processing & Logic
- **Field Extraction:** Extract only the specified fields from the input PDF.
- Property Name should be divided into Name and Lease Id. Lease Id is a number in bracket
- Volume/Share should be divided into two columns Volume and Share.
- Prod should only extract Gas or Oil
- **Row Aggregation:**
    - If multiple rows have the same Property Name, Prod, and Volume, combine into a single row and sum the Share values; "Your Share" and "Property Amount" should be similarly summed if the context dictates
    - If Property Name and Prod are the same but Volume differs, each row should be retained distinctly
    - Prd Date and Prod should be combined (Prd Date Prod) example - (5/25 Oil) 
- **Missing/Optional Fields:**
    - If Copas Administrative Overhead or Total Operating Costs are absent for a property, leave columns blank
    - If any other specified field is missing, leave the cell blank in Excel
    - If the Prod is Oil, it should populate Property Amount(Oil) and Your Share (Oil) and keep Property Amount(Gas) and Your Share (Gas) empty.
    - If the Prod is Gas, it should populate Property Amount(Gas) and Your Share (Gas) and keep Property Amount(Oil) and Your Share (Oil) empty.
- **Error Handling:**
    - Malformed data is left blank; no processing halts
    - Summary to include count of processed, aggregated, and skipped items
 
## Outputs
- **Output Type:** Plain Excel file (.xlsx)
- **Format:** Columns as follows:
    1. Property Name
    2. (Prd Date Prod)
    3. Prod (Oil/Gas)
    4. Volume
    5. Share
    6. Property Amount(Oil)
    7. Your Share(Oil)
    8. Property Amount(Gas)
    9. Your Share(Gas) 
    10. Copas Administrative Overhead
    11. Total Operating Costs
- **File Management:** Always create a new file per upload; no appending
- **Data Structure:** Each line represents a unique (Property Name, Prd Date, Prod, Volume) tuple post-aggregation
- **If a value is not available from the PDF, the corresponding cell remains empty**
 
## User Interaction
- **Interaction Channel:** Pixie interface (UI)
- **Workflow:** User uploads 1 digital PDF, triggers "create settlement file" action, receives Excel file for download
- **Data Preview:** Not required; process is upload → output
- **Summaries:** Display a simple summary upon completion (e.g., records processed, aggregated, skipped)
- **No authentication/access control required**
 
## Performance & Constraints
- **Performance:** No strict SLAs, batch or real-time not required; intended for interactive manual use
- **Constraints:** No compliance, security, cost, or stack limitations specified
- **Assumption:** Agent will not process scanned documents or images; only text-based PDFs
 
## Delivery & Notifications
- **Delivery:** User downloads Excel file directly from Pixie interface
- **Notifications:** Simple completion/success summary in UI; no additional notification channels required
 
## Future Extensions
- **Potential Extensions:**
   - Batch processing of multiple files
   - Enhanced data preview allowing user confirmation or corrections before generating Excel
   - Support for OCR to enable extraction from scanned documents
   - Integration with automated downstream systems (e.g., cloud uploads, database integration)
- **Design is modular to facilitate these enhancements as needed**
