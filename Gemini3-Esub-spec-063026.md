# 📑 TECHNICAL SPECIFICATION FOR AGENTICDOC PRO INTELLIGENCE PLATFORM (V3.0)

## 🌟 Specialization: Taiwan TFDA Medical Device e-Submission System (第一等級查驗登記電子化送件系統)

---

## 1. Executive Summary

### 1.1 Scope and Objective

AgenticDoc Pro v3.0 is a production-grade, domain-specific iteration of the Document Intelligence Platform. It is engineered to transform raw medical device submission data, scientific whitepapers, and draft text into regulatory artifacts that directly match the **Taiwan Food and Drug Administration (TFDA) Medical Device Electronic Submission System (醫療器材查驗登記電子化送件系統)**.

The platform supports local compliance workflows governed by the **"Regulations Governing Issuance of Medical Device Licenses, Registration, and Annual Reporting" (醫療器材許可證核發與登錄及年度申報準則)**. It balances complex processing pipelines with clean, high-density dashboard layouts to reduce the administrative burden on Regulatory Affairs (RA) professionals and TFDA Reviewing Officers.

### 1.2 Architectural Constraints

* **Target Architecture:** Python 3.11+ / Streamlit Enterprise Framework.
* **Deployment Vector:** Hugging Face Spaces (Enterprise Tier) or private, air-gapped VPCs for secure regulatory processing.
* **Primary Intelligence Sandbox:** `gemini-3.1-flash-lite` serves as the default, low-latency, large-context model for structural cross-referencing and interactive form filling.
* **Fallback Framework:** Configured dynamically via `agents.yaml` across secondary models (`gemini-3-flash-preview`, `gpt-4o-mini`, `grok-4-fast-reasoning`).
* **Static Asset Grounding:** Rules-driven validation enforced through an embedded `skill.md` engine containing structural rules derived directly from Taiwan’s Food and Drug Administration operational protocols.

---

## 2. 10 Major Architectural Updates & Feature Extensions

The v3.0 architecture introduces ten major advancements to upgrade the system from a general document tool into a highly specialized TFDA compliance workspace:

```
+---------------------------------------------------------------------------------------------------------+
|                                     AGENTICDOC PRO v3.0 ARCHITECTURE                                    |
+---------------------------------------------------------------------------------------------------------+
|  [Doc Intake Engine] ----> [TFDA Mapping Matrix] ----> [Dynamic Form State] ----> [Regulatory Validator] |
|   (PDF/MD Extraction)       (CFR / Regulations)         (Auto-Populated Fields)     (TFDA Rulebook Engine)  |
+---------------------------------------------------------------------------------------------------------+

```

### Update 1: Dynamic Two-Way State-Synced Interactive Form Engine

Replaces the static markdown editor with an asymmetric, dual-panel presentation layer. The left panel renders a real-time editable digital duplicate of the TFDA Class I Registration Form (第一等級查驗登記申請書/基本資料), while the right panel displays the markdown analytical report. Any modifications made by the user in the form elements trigger an immediate reactive sync to the markdown abstract layer via `st.session_state` mutating hooks, preventing version discrepancies.

### Update 2: Automated Submission Form Hydration Pipeline (Contextual Auto-Fill)

An autonomous agent parses text files or PDFs loaded into the workspace, maps semantic entities to specific TFDA schema keys (e.g., matching text containing standard company identifier formats to the `統一編號` schema field), and populates target inputs instantly. It handles complex conditional definitions, such as parsing structural configurations to automatically mark checkboxes for `國產` (Domestic), `輸入` (Imported), or `陸輸` (Mainland China Imports).

### Update 3: Legal Discrepancy & Conflict Resolution Tracer (Inconsistent Items Agent)

A real-time validation thread that scans structural text across different files to detect conflicting statements. It checks for mismatches in critical definitions, such as an English product title variant in an operational manual not matching the title on an ISO 13485 manufacturing certificate. Conflicts are flagged visually using clear warning indicators, pinning the disputed passages next to each other along with a direct confirmation checklist for the reviewer.

### Update 4: 30-Question Interactive Socratic Form-Completion Wizard

When text is uploaded, the agent checks the file against the information required by the TFDA submission system. It generates 30 targeted, interactive questions to collect missing details (e.g., specific sterilization validations, intended use limits, or classification rules). User answers are injected directly back into the submission draft, converting unstructured technical data into a complete, regulatory-ready application form.

### Update 5: TFDA Regulatory Compliance Auditor Engine

An internal review engine grounded in the **"Regulations Governing Issuance of Medical Device Licenses, Registration, and Annual Reporting" (醫療器材許可證核發與登錄及年度申報準則)**. It verifies submission files against official legal requirements—such as checking for required manufacturer declarations under Article 14 or verifying QMS documentation under Article 15—and generates a detailed, audit-ready compliance report.

### Update 6: Secure, Multiformat Multi-Engine Export System

An industrial-grade document compiler that exports completed forms and summaries into four formats:

* **Markdown (.md):** Clean text structure optimized for version control systems.
* **HTML5:** Pre-styled, independent offline documentation using encapsulated CSS rules.
* **JSON Schema:** Structured data exports designed for seamless API integration with external agency databases.
* **PDF:** Clean layout formatting that embeds administrative signatures and stamps to meet official print submission rules.

### Update 7: Advanced Multi-Tier Lineage Context Manager

Tracks the lineage of all data modifications throughout a session. Reviewers can easily track adjustments back to their source, choosing whether an export or summary should draw from the raw source files, edited drafts, or past agent outputs. This helps prevent context drift and provides a clear audit trail for compliance verification.

### Update 8: Procode Classification and Database Sync Adapter

An intelligent classification tool that monitors inputs in the `分類分級品項` selector. It matches selected classifications against Taiwan's official medical device registration databases (A through P categories, such as J. General Hospital or I. General Surgery). The tool automatically pulls and displays relevant performance standards, testing expectations, and required regulatory filings based on the selected category.

### Update 9: Secure Client-Side Cryptographic API Governance

An updated API key management system built for secure regulatory workflows. When server keys are missing, it enables masked, browser-scoped session storage for user API credentials. It includes strict, isolated processing rules that block text caching on public servers, protect intellectual property, and automatically clear all session data upon logout.

### Update 10: Structural Data Integrity Dashboard

An advanced analytics dashboard tailored for regulatory compliance. It moves away from generic text metrics to track specific submission health indicators, including field completeness rates, cross-referencing accuracy, verification metrics against Taiwan TFDA guidelines, validation errors, and overall data completeness across the ten core submission attachments.

---

## 3. Product Goals & Architectural Non-Goals

### 3.1 Primary Goals

* **Regulatory Alignment:** Align unstructured product data with the exact layout rules of the Taiwan TFDA Class I Registration System.
* **Automated Data Extraction:** Use `gemini-3.1-flash-lite` to automatically parse and extract critical information from raw technical files.
* **Automated Validation:** Provide automated verification based on the *"Regulations Governing Issuance of Medical Device Licenses, Registration, and Annual Reporting"*.
* **Consistent Data Management:** Eliminate manual transcription errors across medical device summaries, application forms, and external attachments.
* **Audit-Ready Security:** Ensure complete data privacy on public clouds through session-isolated, encrypted client-side memory handling.

### 3.2 Non-Goals

* **No Permanent Storage:** The application operates as an active workspace and does not store or host historical user application files.
* **No Direct Government Portal Submission:** The system formats, validates, and exports submission bundles; it does not connect directly to the TFDA web portal via automated network injection scripts.
* **No Local Model Execution:** The software relies on secure, cloud-based API integrations and does not require local GPU-backed hardware to run.

---

## 4. High-Level System Architecture

The software uses a clear, multi-tiered architecture built on top of Python and Streamlit, optimized to handle complex data extraction and compliance checking for TFDA submissions.

```
+---------------------------------------------------------------------------------------------------------+
|                                     HIGH-LEVEL DATA FLOW ARCHITECTURE                                   |
+---------------------------------------------------------------------------------------------------------+
| [User Uploads Source Data]                                                                              |
|           |                                                                                             |
|           v                                                                                             |
| [Document Layer: Extraction] ---> [Intelligence Layer: Gemini 3.1] ---> [Orchestration Layer: State Sync] |
|                                                                                |                        |
|                                                                                v                        |
| [Data Export Bundles] <---------- [Observability Layer] <-------- [TFDA Legal Validation Engine]         |
+---------------------------------------------------------------------------------------------------------+

```

### 4.1 Tier Components

| Layer | Primary Technology | Responsibility |
| --- | --- | --- |
| **Presentation Layer** | Streamlit + Tailwind-Injected Custom HTML5 Targets | Renders dual-column workspaces, interactive TFDA application forms, compliance dashboards, and reactive text inputs. |
| **Document Layer** | PyMuPDF / Python-docx / Native Markdown Parser | Handles multi-format data ingestion, processes page selection ranges, extracts structured tables, and manages multi-document downloads. |
| **Intelligence Layer** | Gemini API Adapter Core (`gemini-3.1-flash-lite`) | Manages form extraction, generates interactive questions, executes discrepancy cross-checks, and drafts technical summaries. |
| **Orchestration Layer** | `agents.yaml` Routing Engine + Streamlit Session State | Coordinates data transformations, ensures form elements stay synced with the markdown view, and enforces fallback routing rules. |
| **Validation Layer** | `skill.md` Rule Processor Engine | Validates inputs against Taiwan's official medical device licensing regulations, flagging missing documents or fields. |
| **Observability Layer** | Metric Registry Rails + Streamlit Live Buffer Logs | Provides real-time operational logs, tracks parameter changes, measures extraction confidence, and displays grounding verification scores. |

### 4.2 Configuration Protocols

#### 4.2.1 `agents.yaml` (Model Orchestration Framework)

```yaml
version: "3.0"
default_provider: "gemini"
default_model: "gemini-3.1-flash-lite"

features:
  form_hydration:
    primary_model: "gemini-3.1-flash-lite"
    temperature: 0.1
    max_tokens: 8000
    system_instruction: |
      You are an expert TFDA regulatory engineer. Extract properties from unstructured data 
      and align them with the Taiwan Electronic Submission System schema definitions. 
      Ensure precise string formatting for identifiers like '統一編號'.
  
  conflict_detector:
    primary_model: "grok-4-fast-reasoning"
    fallback_model: "gemini-3-flash-preview"
    temperature: 0.0
    system_instruction: |
      Compare submission documents side-by-side. Flag discrepancies in technical parameters, 
      product designations, or regulatory classifications. Output clear references with page and line citations.

  regulatory_compliance_auditor:
    primary_model: "gemini-3.1-flash-lite"
    temperature: 0.1
    ruleset_reference: "醫療器材許可證核發與登錄及年度申報準則"

```

#### 4.2.2 `skill.md` (TFDA Compliance Verification Engine Rulebook)

```markdown
# TFDA Class I Compliance Ruleset

## Core Checkpoints
1. Verify presence of Applicant Company Tax Identifier (統一編號) consisting of 8 numeric characters.
2. Ensure Chinese Product Name (中文品名) includes mandatory sterilization descriptors if '滅菌' checkbox is toggled.
3. Validate classification string matches official category parameters (Procodes A through P).
4. Enforce mandatory tracking of structural parameters for designated codes:
   - I.4040 (Medical Apparel / Face Masks)
   - O.3930 (Mobile Wheelchair Lifts)
   - O.5150 (Powered Patient Transport Devices)

## Verification Logic Matrix
- IF Device == "I.4040" AND Product == "醫用口罩":
    Enforce mandatory validation checks on the specifications panel text field.

```

---

## 5. Global User Experience & Interface (UX/UI) Specification

The system layout is built for high-density professional work, providing clear navigation, scannable data layouts, and reactive controls designed for complex regulatory analysis.

### 5.1 Main Layout Panels

```
+---------------------------------------------------------------------------------------------------------+
|                                  AGENTICDOC PRO v3.0 WORKSPACE INTERFACE                                |
+---------------------------------------------------------------------------------------------------------+
| [TOP BAR] Brand Asset Engine | Language: Traditional Chinese | Theme: Studio Minimalist | Auth Token UI |
+---------------------------------------------------------------------------------------------------------+
| [SIDEBAR CONTROLS]       | [LEFT FIELD PANEL: ELECTRONIC FORM]     | [RIGHT WORK PANEL: SUMMARY/AI UI]  |
| - File Drop Ingest Core  | - 查驗登記申請-基本資料 (Editable Form)  | - Markdown Document Intelligence   |
| - Page Range Trimmer     | - Electronic Running Number             | - AI Socratic Completion Wizard     |
| - Classification Query   | - Product Classification Grid          | - Multi-Format Export Workspace    |
|                          | - Manufacturing Architecture Select     | - Verification Error Logs Panel    |
+---------------------------------------------------------------------------------------------------------+
| [BOTTOM ZONE] Dynamic Stream Logs Tracker | Grounding Verifier Metrics Panel | Live Execution Pipeline  |
+---------------------------------------------------------------------------------------------------------+

```

### 5.2 System Layout and Design Controls

* **Themed Visual Options:** Features a clean, low-fatigue workspace layout built for long review sessions, using standard high-contrast elements and accessible text spacing.
* **Language Profiling Controls:** Defaults language rendering to Traditional Chinese (`zh-TW`) optimized with standard Taiwan TFDA legal and medical terms. Supports quick toggling to Regulatory English (`en-US`).
* **Interactive System Logs:** Includes an expandable processing log at the bottom of the workspace that tracks processing steps, model validation outputs, and validation alerts.

---

## 6. Core Functional Modules (Deep Technical Breakdown)

### 6.1 PDF Intake and Structural Page Trimming Module

```
+---------------------------------------------------------------------------------------------------------+
|                                    PDF INTAKE AND TRIMMING FLOW PIPELINE                                |
+---------------------------------------------------------------------------------------------------------+
| [Raw Submission Document PDF] ---> [Extract Layout Metrics] ---> [Page Selection Matrix Filter]          |
|                                                                              |                          |
|                                                                              v                          |
| [Extracted Clean Text Stream] <--- [Incorporate Target Subsets] <--- [Generate Trimmed Download Archive] |
+---------------------------------------------------------------------------------------------------------+

```

* **Multi-Document File Loading:** Ingests high-volume source PDFs (such as technical manuals, ISO certifications, and laboratory test profiles).
* **Page-Range Extraction Engine:** Extracts text cleanly across user-defined page ranges, ensuring tables and structural layouts remain intact.
* **Pre-Flight Structural Validation Rules:**
* Rejects broken files, password-locked PDFs, and invalid page range selections.
* Calculates character count metrics and estimates source token consumption before initiating the AI extraction pipeline.



### 6.2 Submission Form Hydration Module (Contextual Auto-Fill)

The module maps unstructured information from uploaded document profiles directly to specific input fields required by the TFDA application schema.

```
+---------------------------------------------------------------------------------------------------------+
|                                  FORM HYDRATION AND EXTRACTION ENGINE                                   |
+---------------------------------------------------------------------------------------------------------+
| [Raw Contextual Source Buffer] ---> [Run Extraction Regex Framework] ---> [Map Extracted Data Elements] |
|                                                                                     |                   |
|                                                                                     v                   |
| [Render Field Values in Form] <--- [Match Streamlit Input State UI] <--- [Validate Field Schemas]        |
+---------------------------------------------------------------------------------------------------------+

```

#### Field Schema Definitions

```
       [Electronic Tracking ID File Structure] ---> (MDE + YYMMDD + 5-Digit Automated Token Sequence)
       [Tax Registry Identifier Field] ---------> (Strict 8-Digit Numerical Matching Sequence)
       [Source Classification Selector Block] --> (Regulated Dropdown Lists: Codes Category A through P)

```

The data extraction engine uses strict validation protocols to ensure information is captured accurately:

```python
# System validation pattern mapping reference structure
{
    "電子流水號": "MDE" + datetime.now().strftime("%y%m%d") + r"\d{5}",
    "統一編號": r"^\d{8}$",
    "產地屬性": ["國產", "輸入", "陸輸"],
    "產品類別": [f"{chr(i)}." for i in range(65, 81)] # Categories A through P
}

```

* **Dynamic Data Matching:** The automation agent reads the source text, identifies standard information structures, and populates the application fields. For example, text blocks matching standard manufacturing locations are analyzed to automatically select `國產` (Domestic), `輸入` (Imported), or `陸輸` (Mainland China Imports) origins.

### 6.3 Real-Time Legal Discrepancy & Conflict Resolution Tracer

The tracking system runs real-time cross-checks across all text inputs to find and flag technical contradictions before submission.

```
+---------------------------------------------------------------------------------------------------------+
|                                REAL-TIME CONFLICT TRACKING & RESOLUTION                                 |
+---------------------------------------------------------------------------------------------------------+
| [Extracted Application Context] ---> [Run Discrepancy Evaluation Pipeline] ---> [Identify Contradictions]|
|                                                                                      |                  |
|                                                                                      v                  |
| [Reviewer Correction Interventions] <--- [Highlight UI Warning Panel Alerts] <--- [Pin Contradictory Elements] |
+---------------------------------------------------------------------------------------------------------+

```

#### Core Validation Checks

* **Product Name Matching:** Cross-references the application's Chinese and English product names against titles used within accompanying technical attachments and ISO certification files.
* **Sterilization Property Checks:** Confirms that if a device is marked as `滅菌` (Sterile), the associated technical specifications and packaging manuals contain clear, corresponding sterilization verification data.
* **Classification System Cross-Checking:** Verifies that performance claims listed in the user-entered text match the scope and definitions of the selected TFDA classification category (`分類分級品項`).

#### Visual Discrepancy UI Alerts

Contradictions are displayed in an alert panel next to the affected form fields. Each alert includes direct text comparisons, source citations, and a confirmation check to assist the reviewer during updates:

```
+---------------------------------------------------------------------------------------------------------+
| ⚠️ REGULATORY COMPLIANCE ALERT: PRODUCT TITLE VARIANT CONTRADICTION                                      |
+---------------------------------------------------------------------------------------------------------+
| Form Field Label (中文品名): "醫用不織布口罩 (滅菌)"                                                    |
| Document Attachment (Page 14): "一般民用防護口罩 (未滅菌)"                                               |
| ------------------------------------------------------------------------------------------------------- |
| [ ] Confirm Form Field as Authoritative  |  [ ] Override and Update Form Value to Match Attachment       |
+---------------------------------------------------------------------------------------------------------+

```

### 6.4 30-Question Interactive Socratic Form-Completion Wizard

An automated compliance assistant reviews the input data, identifies incomplete fields, and generates 30 targeted questions to guide the user through completing the missing registration details.

```
+---------------------------------------------------------------------------------------------------------+
|                                    SOCRATIC QUESTION GENERATION PIPELINE                                |
+---------------------------------------------------------------------------------------------------------+
| [Scan Current Application Form State] ---> [Identify Blank/Incomplete Fields] ---> [Generate 30 Questions] |
|                                                                                             |           |
|                                                                                             v           |
| [Update Form Elements Instantly] <--- [Inject Captured Data Back to State] <--- [User Inputs Responses]  |
+---------------------------------------------------------------------------------------------------------+

```

#### Structured Question Examples for Submission Optimization

* **Administrative & Identification Profiles:**
1. *"The primary Tax Registry ID field is currently empty. Please provide your company's official 8-digit identifier."*
2. *"Please specify the official document filing number (申請案公文文號) if an introductory official request has been logged."*


* **Device Properties & Structural Variations:**
3. *"The application has the 'Sterile' attribute enabled. Please provide the precise design specifications for the sterile barrier packaging system."*
4. *"Please verify whether the product label uses an established brand prefix, or if the registration name should use the generic descriptive title."*
* **Classification & Scope Mapping:**
5. *"The application lists classification category 'I. General & Plastic Surgery Devices'. Please clarify if this device fits the specific definition of 'I.4040 Medical Apparel'."*
6. *"For this Class I medical apparel filing, please specify if the product is configured as a surgical face mask or a general medical gown."*
* **Performance and Intended Use Contexts:**
7. *"The product use description is empty. Please define the intended patient groups and target clinical environments for this device."*
8. *"Please specify any clear limitations or environments where use of this device is restricted or contraindicated."*
* **Manufacturing Quality Systems Data:**
9. *"Please enter the registration tracking number for your Quality Management System (QMS/QSD) certificate."*
10. *"Is manufacturing performed entirely by a single facility, or are specific production stages outsourced to sub-contracted facilities?"*

### 6.5 TFDA Regulatory Compliance Auditor Engine

An automated auditing engine that checks completed drafts against the official **"Regulations Governing Issuance of Medical Device Licenses, Registration, and Annual Reporting" (醫療器材許可證核發與登錄及年度申報準則)**.

```
+---------------------------------------------------------------------------------------------------------+
|                                    TFDA COMPLIANCE AUDITING PIPELINE                                    |
+---------------------------------------------------------------------------------------------------------+
| [Compile Complete Submission Draft] ---> [Run Regulatory Auditing Engine] ---> [Check Submission Rules] |
|                                                                                        |                |
|                                                                                        v                |
| [Export Compliance Audit Report] <--- [Generate Flagged Violations Logs] <--- [Validate Key Regulations]|
+---------------------------------------------------------------------------------------------------------+

```

#### Core Regulatory Compliance Checkpoints

```
[Article 14 Validation Ruleset] ---> Confirm Presence of Complete Manufacturer Location & Identity Strings
[Article 15 Validation Ruleset] ---> Verify Valid QMS/QSD Certification Reference Numbers are Logged
[Form 3654 Structural Ruleset]  ---> Check Complete Document Attachment Matrix Checklist Status (1 to 10)

```

* **Compliance Evaluation Logic:**
* **Article 14 Checks:** Confirms that application fields contain valid manufacturer names, addresses, and country-of-origin details that match company registration profiles.
* **Article 15 Checks:** Validates that appropriate QMS compliance records are provided for the selected device classification, flagging missing production certifications.
* **Attachment Checklist Verification:** Checks the completeness of the ten core submission attachments (第一等級查登檢送簡表, 品質管理系統證明文件, 原廠產品說明書, etc.), highlighting incomplete or missing files.



### 6.6 3 Additional AI Tool Enhancements

#### Feature A: Intelligent Procode Regulation Dynamic Lookup Core

An automated reference engine that monitors the active `分類分級品項` selection fields. When a category is selected (e.g., *J. General Hospital and Personal Use Devices*), it searches an internal compliance database to find and display relevant regulatory requirements, testing standards, and historical administrative guidance.

```
+---------------------------------------------------------------------------------------------------------+
|                                   PROCODE REGULATION DYNAMIC LOOKUP                                     |
+---------------------------------------------------------------------------------------------------------+
| [User Toggles Classification Select] ---> [Query Database Repository] ---> [Extract Specific Regulations] |
|                                                                                     |                   |
|                                                                                     v                   |
| [Inject Target Performance Guidelines] <--- [Filter Core Testing Rules] <--- [Isolate Relevant Criteria]|
+---------------------------------------------------------------------------------------------------------+

```

#### Feature B: Automated Structure Extraction Engine for Complex Multi-Page Tables

A structural parsing engine designed to extract tabular data from unformatted text and messy technical reports. It cleans, formats, and restructures the information into clear markdown tables that match standard TFDA presentation layouts.

```
+---------------------------------------------------------------------------------------------------------+
|                                    STRUCTURED TABLE PROCESSING FLOW                                     |
+---------------------------------------------------------------------------------------------------------+
| [Messy Contextual Text Stream] ---> [Run Extraction Framework] ---> [Map Extracted Data Elements]        |
|                                                                                    |                    |
|                                                                                    v                    |
| [Render Clean Markdown Tables Matrix] <--- [Verify Cell Values] <--- [Align Column Elements Properly]   |
+---------------------------------------------------------------------------------------------------------+

```

#### Feature C: Automated Regulatory Deficiencies Correspondence Draft Generator

An automated reporting tool that processes validation flags, technical gaps, and missing document entries to generate an official internal summary report or a formal response letter addressed to the submission sponsor.

```
+---------------------------------------------------------------------------------------------------------+
|                                  DEFICIENCIES RECONCILIATION GENERATOR                                  |
+---------------------------------------------------------------------------------------------------------+
| [Read Logged Validation Conflicts] ---> [Process Legal Templates] ---> [Draft Formal Response Report]   |
|                                                                                      |                  |
|                                                                                      v                  |
| [Provide Clear Markdown Document Views] <--- [Incorporate Actionable Remediation Fixes]                |
+---------------------------------------------------------------------------------------------------------+

```

---

## 7. Model Governance & Secure Environments

### 7.1 LLM Execution Matrix

| Operational Feature | Primary Model Target | Target System Directives |
| --- | --- | --- |
| **Interactive Form Filling Core** | `gemini-3.1-flash-lite` | Structural text extraction, high-speed context mapping, and field data validation. |
| **Discrepancy Cross-Checking** | `grok-4-fast-reasoning` | Deep analysis of technical data, structural validation, and cross-document integrity verification. |
| **Regulatory Compliance Auditing** | `gemini-3.1-flash-lite` | Compliance rule checking, legal cross-referencing, and verification against TFDA frameworks. |
| **Socratic Wizard Generator** | `gemini-3.1-flash-lite` | Incomplete data identification, conversation management, and question generation. |
| **Deficiency Document Writing** | `gemini-3-flash-preview` | Professional correspondence drafting, legal terminology formatting, and template alignment. |

### 7.2 Secure API Key and Governance Infrastructure

* **Server-Side Environment Integrity:** Uses secure, read-only system environment variables when deployed inside protected private clouds or enterprise network spaces.
* **Webpage Fallback Logic:** If environment keys are missing, the UI provides masked, secure text input fields. These keys are stored temporarily within browser-scoped memory targets (`st.session_state`) and are never saved permanently or shared across network logs.
* **Data Protection Safeguards:** Processed text files are kept only in temporary system memory during active sessions. The platform avoids persistent disk caching to safeguard intellectual property and ensure compliance with strict security requirements.

---

## 8. Observability & Quality Verification Framework

### 8.1 Real-Time Submission Health Dashboard

The system replaces generic text analysis metrics with an advanced dashboard tailored for regulatory compliance verification:

```
+---------------------------------------------------------------------------------------------------------+
|                                    TFDA SUBMISSION COMPLIANCE DASHBOARD                                 |
+---------------------------------------------------------------------------------------------------------+
| Form Field Completeness Track: [████████████████████░░░░░░] 78% Complete                               |
| Required Attachments Uploaded: 7 / 10 Essential Files Present                                           |
| Detected Data Contradictions: 2 Active Flags | Unresolved Regulatory Errors: 1 Block                     |
| ------------------------------------------------------------------------------------------------------- |
| Model Latency Target Tracker: 1.2s | Extraction Grounding Confidence Score: 94% Valid                   |
+---------------------------------------------------------------------------------------------------------+

```

### 8.2 Operational Trace Logging Core

* **User Log Mode:** Renders clear, friendly descriptions of active background processing steps (e.g., *"Extracting corporate identification numbers from attachment profile..."*).
* **Expert Audit Mode:** Provides deep system trace details, including precise timestamp logs, verification hash values, execution times for API calls, and evaluation metrics from the validation engine.

---

## 9. Comprehensive System Data Flow

```
+---------------------------------------------------------------------------------------------------------+
|                                     DETAILED SYSTEM DATA FLOW MATRIX                                    |
+---------------------------------------------------------------------------------------------------------+
| [Source Technical Documents & Manuals]                                                                  |
|               |                                                                                         |
|               v                                                                                         |
| [PDF Intake & Trimming Processing Panel]                                                                |
|               |                                                                                         |
|               v                                                                                         |
| [Form Hydration Agent Engine] ----> [Populate Form Fields]                                              |
|               |                                                                                         |
|               v                                                                                         |
| [Discrepancy Tracking Engine] ----> [Flag Context Inconsistencies]                                      |
|               |                                                                                         |
|               v                                                                                         |
| [Socratic Questioning Assistant] -> [Collect Missing Registration Information]                         |
|               |                                                                                         |
|               v                                                                                         |
| [TFDA Compliance Auditor] --------> [Validate Application against Legal Criteria]                       |
|               |                                                                                         |
|               v                                                                                         |
| [Multi-Format Document Compiler] -> [Generate Final Clean Export Packages] (MD / HTML / JSON / PDF)    |
+---------------------------------------------------------------------------------------------------------+

```

---

## 10. Robust Defensive Engineering (Error and Bug Mitigation)

To guarantee enterprise-grade stability inside a Streamlit container running within Hugging Face Spaces, the architecture natively integrates custom error-handling and stability protocols:

### 10.1 Streamlit-Specific State Defenses

```
[User Interventions / Form Toggles] ---> (Blocks Unexpected Session Resets and Clears State Failures)
                                                   |
                                                   v
                             [Maintain Continuous State Sync using Explicit Key Bindings]

```

* **Mitigation Strategy:** Prevents random form resets by tying all input fields to explicit, persistent keys in `st.session_state`. Changes are captured using structured callback handlers, ensuring user modifications remain intact during workspace updates.

### 10.2 JSON Formatting and Parsing Defenses

```
[LLM Raw Output Block] ---> [Validate Structural Integrity] ---> (Catches Broken Closures and Missing Commas)
                                                                            |
                                                                            v
                                                  [Repair Outputs via Clean Local Parsing Rails]

```

* **Mitigation Strategy:** Uses regex validation blocks to inspect raw output data. It strips out invalid formatting tags and repairs broken string structures or missing commas, ensuring high-volume extraction runs complete smoothly.

### 10.3 API Failure and Rate-Limit Protections

```
[Network Block Timeout / API Overload Error] ---> [Execute Backoff Protocol] ---> (Exponential Step Delay)
                                                                                             |
                                                                                             v
                                                                 [Graceful Failures without App Crashes]

```

* **Mitigation Strategy:** Integrates an automatic retry loop with exponential backoff delays. If an API provider goes offline or experiences rate limits, the system shifts tasks to designated backup models to keep the active review session running smoothly.

---

## 11. Complete Verification Sign-Off Checklist

The application system bundle is considered ready for target deployment when it satisfies the following operational benchmarks:

* [ ] **Two-Way UI State Sync:** User inputs on the digital application form update the matching markdown abstracts instantly.
* [ ] **Automated Data Population:** The extraction agent reads source files and accurately populates registration fields like `統一編號` (Tax ID) and corporate details.
* [ ] **Conflict Identification:** The discrepancy engine catches conflicting values across different files and highlights them in the warning panel.
* [ ] **30-Question Completion Assistant:** The virtual assistant generates 30 context-aware questions to guide users through filling in incomplete sections.
* [ ] **TFDA Legal Validation:** Completed submission bundles are cross-checked against the *"Regulations Governing Issuance of Medical Device Licenses, Registration, and Annual Reporting"*.
* [ ] **Clean Multi-Format Export:** The export framework outputs complete, properly formatted documents across all four target types (.md, .html, .json, .pdf).
* [ ] **Context Lineage Tracking:** The platform provides clear controls for managing data context, allowing users to trace information back to original, edited, or agent-generated sources.
* [ ] **Secure Token Infrastructure:** API credentials remain masked, browser-scoped, and isolated within local session memory.
* [ ] **Detailed System Logging:** The monitoring log panel tracks system actions cleanly across both basic user and advanced audit viewing modes.
* [ ] **Streamlit App Stability:** The application interface handles rapid user edits and extensive background processing runs without interface crashes or state resets.

---

## 12. 20 Comprehensive Follow-Up Questions

To help guide the next phase of implementation and fine-tune operational workflows, please provide feedback on the following functional preferences:

1. Should the platform's **Automated Form Auto-Fill** use strict formatting and overwrite existing field data completely, or should it append new information as editable suggestions within the user view?
2. When the system detects a conflict between files (e.g., mismatched title strings on certificates), should it block final export generation until the user checks an acknowledgment box, or simply note the variance in the compliance log?
3. For the **30-Question Completion Assistant**, should questions be presented all at once in an interactive grid format, or step-by-step as a guided wizard panel?
4. Should the **TFDA Compliance Auditor** use a fixed, embedded ruleset for Class I medical devices, or allow regulatory teams to upload custom evaluation checklists for specific product types?
5. How should the system format final **PDF Export Documents**? Do you require an exact match to the TFDA's layout designs, or a clean, standardized report layout suitable for digital archival?
6. When pulling data from the **Classification Category Selector**, should the app show only basic regulatory definitions, or include links to technical standards such as ISO or AAMI compliance guidelines?
7. How should the platform manage instances where text extraction returns an ambiguous company name variant? Should it prompt the user with a confirmation dropdown or default to the primary registration details?
8. For secure enterprise installations, should the system provide an automated session-clearing countdown timer that wipes local browser memory after a period of user inactivity?
9. Should the **Discrepancy and Conflict Resolution Engine** run automatically after every document upload, or run only when manually triggered via a validation button?
10. For high-volume multi-page technical manuals, should the extraction agent summarize comprehensive engineering notes into concise bullet points, or retain long-form descriptions for accuracy?
11. Should the data export engine allow users to combine all ten standard TFDA submission attachments into a **single, unified JSON schema bundle**, or generate separate data files for each component?
12. For the **Automated Deficiencies Correspondence Generator**, should the draft response letter use strict, formal regulatory language by default, or provide customizable templates for different communication styles?
13. When users edit the markdown view directly, should the form input engine update the corresponding field fields instantly, or use an intermediate verification confirmation check?
14. Should the system dashboard display real-time estimate logs for API token usage and transaction processing costs during high-volume document reviews?
15. How should the system handle specialized device components that require custom validation rules, such as advanced software metrics or complex chemical sterilization processes?
16. Should the **Interactive Question Assistant** skip questions for fields that already have high-confidence extracted data, or include them anyway to allow for user verification?
17. Do you require the compliance log tool to archive system trace summaries locally as an exportable TXT audit trail file for team compliance reviews?
18. Should the interface design use standard Streamlit sidebar layouts, or deploy custom responsive layouts to make better use of space on wide displays?
19. When processing foreign manufacturing registration files, should the system automatically flag variations in international address terms for manual review?
20. Should the platform include built-in version comparison dashboards to help teams track updates across multiple review sessions?
