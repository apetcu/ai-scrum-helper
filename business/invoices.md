# Invoices - Business Context

> Generated on 2026-02-07 via /module-onboarding

---

## Overview

The Invoices module handles all billing and invoice management within the Finance SaaS platform. It displays a daily-refreshed list of invoices in a tabular format, allows users to preview invoice details via a side panel, and supports creating new invoices by uploading PDF, DOC, or image files through a drag-and-drop modal. The module integrates with Settlements (payment reconciliation), Addons (additional line items), and Storage (file persistence).

## Key User Flows

1. **Browse invoices**: User opens the Invoices page and sees a table with Invoice #, Client, Amount, Status (color-coded), and Date columns
2. **Preview invoice details**: User clicks a row to open a right-side detail panel showing Invoice #, Client, Amount, Status badge, Issue Date, Due Date, and Description
3. **Upload a new invoice**: User clicks "Add Invoice" button (top-right) → modal opens with drag-and-drop zone → selects/drops a file (PDF, DOC, Image) → clicks "Upload" → system validates, stores file via Storage module, extracts metadata, creates a new invoice record with "Pending" status and sequential number (e.g., INV-0011)

## Business Rules & Acceptance Criteria

- Invoice numbers are sequential (INV-0001, INV-0002, etc.)
- New invoices are created with "Pending" status
- Due date is calculated from default payment terms
- Accepted upload formats: PDF, DOC/DOCX, JPG, PNG
- File type validation happens both client-side and server-side
- File size has a configurable maximum limit
- Metadata extraction pre-populates client, amount, description from the uploaded document
- If extraction fails, record is still created with empty fields for manual entry
- No partial or corrupted data on failure — full rollback
- Invoice data refreshes daily with incremental sync
- Status badges: Paid (green), Pending (yellow), Overdue (red)
- Badges appear in both list view and detail panel

## Known Edge Cases & Past Bugs

No resolved bugs found in Jira for this module yet. Potential regression areas based on solution design:
- Unsupported file type upload should show clear error
- File exceeding size limit should inform user of the limit
- Network errors during upload should show retry prompt (not lose the selected file)
- Failed metadata extraction should still create the record (graceful degradation)
- Malware scanning must complete before file is persisted

## Integration Points

- **Settlements**: Invoices trigger settlement flows once payments are received (`invoice.paid` event)
- **Addons**: Active addons generate additional line items on client invoices
- **Storage**: Uploaded invoice files are persisted via Storage module API; Storage returns a unique file reference linked to the invoice record
- **Dashboard**: Settlement data from paid invoices feeds into revenue metrics (Total Revenue)

---

## Confluence Documentation

### Invoices (Module Page)
**Source**: https://onchainro.atlassian.net/wiki/spaces/SNDY/pages/175210517/Invoices

# Invoices

The Invoices module handles all billing and invoice management within the platform.

## Overview

The Invoices page displays a list of all invoices in a tabular format. The invoice data is **refreshed daily** to ensure accuracy. Users can browse invoices, view details, and upload new invoices via PDF.

## UI Layout

### Invoice List (Main View)

The main view presents a table with the following columns:

| Column | Description |
| --- | --- |
| **Invoice #** | Unique invoice identifier (e.g., INV-0001, INV-0002) |
| **Client** | Client name (e.g., Digital Ventures, Global Solutions, Acme Corp, Future Tech, NextGen Systems) |
| **Amount** | Invoice total in USD (e.g., $46,783, $10,415) |
| **Status** | Current invoice status — **Paid** (green), **Pending** (yellow), or **Overdue** (red) |
| **Date** | Invoice date |

### Invoice Detail Panel (Right Side)

Clicking on any invoice row opens a detail panel on the right side of the screen showing:

* **Invoice #** — e.g., INV-0007
* **Invoice Preview** label
* **Client** — Client name
* **Amount** — Invoice total
* **Status** — Color-coded status badge (Paid / Pending / Overdue)
* **Issue Date** — Date the invoice was issued
* **Due Date** — Payment due date
* **Description** — Brief description of the work (e.g., "Web development project")

### Add Invoice (Modal)

An **"Add Invoice"** button is located in the top-right corner of the page. Clicking it opens a modal dialog:

* **Title**: "Add Invoice"
* **Upload Invoice File**: A drag-and-drop zone where users can:
    * Click to browse files, or
    * Drag and drop files directly
    * Accepted formats: **PDF, DOC, or Image files**
* **Cancel** button — Closes the modal without uploading
* **Upload** button — Submits the selected file

The uploaded file is processed and a new invoice record is created from the document contents.

## Key Features

* Daily-refreshed invoice list
* Sortable table with invoice number, client, amount, status, and date
* Inline detail panel for quick invoice preview without leaving the list
* File-based invoice creation via drag-and-drop upload (PDF, DOC, Image)
* Status tracking with color-coded badges: Paid (green), Pending (yellow), Overdue (red)

## Integration Points

* **Settlements**: Invoices trigger settlement flows once payments are received
* **Addons**: Additional line items from active addons are included in invoices
* **Storage**: Uploaded invoice files (PDFs, DOCs, images) are persisted via the Storage module

---

### SD01 - Invoices Upload (Solution Design)
**Source**: https://onchainro.atlassian.net/wiki/spaces/SNDY/pages/175243617/SD01+-+Invoices+Upload

# SD01 - Invoices Upload

**Module**: Invoices | **Status**: Active | **Last Updated**: February 2026

## 1. Introduction

The Invoices Upload feature allows users to create new invoice records by uploading document files directly into the platform. Rather than requiring manual data entry for each invoice field, the system accepts PDF, DOC, and image files through a simple drag-and-drop interface. This approach significantly reduces the time and effort required to onboard invoices into the system, especially for teams that receive invoices from external vendors or clients in document format. The upload mechanism is accessible via the "Add Invoice" button located in the top-right corner of the Invoices list view, ensuring it is always within reach without disrupting the user's current workflow.

## 2. User Experience Flow

When a user clicks the "Add Invoice" button, a modal dialog opens on top of the Invoices list view. The modal presents a clean, focused interface with a single purpose: uploading an invoice file. The central element of the modal is a dashed-border drop zone labeled "Click to upload or drag and drop" with a subtitle indicating the accepted file types — PDF, DOC, or Image files. Users can either click the drop zone to open a native file picker or drag a file directly from their desktop or file manager into the zone. Below the drop zone, two action buttons are provided: "Cancel" to dismiss the modal without taking action, and "Upload" to submit the selected file for processing. This minimal design ensures that users can complete the upload in just two interactions — select a file and click upload.

## 3. Supported File Formats

The upload feature supports three categories of file formats to accommodate the variety of invoice documents teams encounter in practice. PDF files are the most common format, as most accounting and billing systems generate invoices as PDFs. DOC and DOCX files are supported for cases where invoices are created using word processors, which is common for smaller vendors or freelancers. Image files (JPG, PNG) are accepted to handle scenarios where invoices are scanned or photographed from physical documents. By supporting this range of formats, the platform ensures that virtually any invoice a team receives can be uploaded without requiring the user to convert it first, reducing friction and increasing adoption of the digital invoicing workflow.

## 4. File Validation and Processing

Once a user selects a file and clicks "Upload", the system performs a series of validation steps before processing the document. First, the file type is checked against the allowed formats to ensure only PDF, DOC, or image files are accepted. The file size is validated against a configurable maximum limit to prevent excessively large uploads from impacting system performance. If validation passes, the file is uploaded to the Storage module where it is persisted durably. The system then extracts key metadata from the document — such as client name, amount, date, and description — to pre-populate the invoice record. This extraction process reduces the need for manual data entry and ensures consistency between the uploaded document and the invoice data stored in the system.

## 5. Storage Integration

The Invoices Upload feature relies heavily on the Storage module for file persistence. When a file is uploaded, it is sent to the Storage module's API, which handles the actual file storage, versioning, and access control. The Storage module returns a unique file reference that is linked to the newly created invoice record. This separation of concerns means the Invoices module does not need to manage file storage directly — it simply holds a reference to the stored document. If the original file needs to be retrieved later (for example, to display the invoice preview or to download the original document), the Invoices module requests it from Storage using that reference. This architecture keeps the Invoices module focused on business logic while Storage handles all infrastructure concerns related to file management.

## 6. Invoice Record Creation

After the file is uploaded and stored, a new invoice record is created in the system. The record is assigned the next sequential invoice number (e.g., INV-0011 if the last invoice was INV-0010) and is initially set to "Pending" status. The invoice date is set to the current date, and the due date is calculated based on the default payment terms configured for the platform. Key fields — client name, amount, and description — are pre-populated from the document extraction process where possible, and the user can review and edit these fields before the record is finalized. The newly created invoice then appears in the Invoices list view, where it is visible alongside all other invoices and can be clicked to open the detail panel on the right side of the screen.

## 7. Error Handling

The upload process includes comprehensive error handling to ensure a smooth user experience even when things go wrong. If the user attempts to upload an unsupported file type, the modal displays a clear error message indicating which formats are accepted. If the file exceeds the maximum size limit, a message informs the user of the limit and suggests compressing or resizing the file. Network errors during upload are caught and presented as a retry prompt, so the user does not lose their selected file. If the document extraction process fails or returns incomplete data, the invoice record is still created but with empty fields that the user can fill in manually. In all error scenarios, the system ensures that no partial or corrupted data is persisted — either the upload completes fully or it is rolled back cleanly.

## 8. Security Considerations

File uploads are a common attack vector, so the Invoices Upload feature implements several security measures. All uploaded files are scanned for malware before being persisted in Storage. The file type validation is performed on the server side (not just the client) to prevent bypass via modified requests. File names are sanitized to remove potentially dangerous characters, and files are stored with system-generated names rather than user-provided names to prevent path traversal attacks. Access to uploaded invoice files is governed by the same permission model as the invoice records themselves — only users with access to the invoice can view or download the associated file. All file transfers occur over encrypted connections, and files at rest in Storage are encrypted using the platform's standard encryption configuration.

## 9. Performance and Scalability

The upload feature is designed to perform well even as the volume of invoices grows. File uploads are handled asynchronously so the UI remains responsive during the upload process — the modal shows a progress indicator while the file is being transferred. Large files are uploaded in chunks to prevent timeout issues and to allow resumable uploads in case of network interruption. On the backend, the Storage module uses cloud-based object storage that scales horizontally, so there is no practical limit to the number of invoice files that can be stored. The daily refresh mechanism that updates the Invoices list view is optimized to handle growing datasets through pagination and incremental syncing, ensuring that the list view remains fast regardless of how many invoices are in the system.

## 10. Future Enhancements

Several enhancements are planned for the Invoices Upload feature in upcoming sprints. Batch upload support will allow users to select and upload multiple invoice files at once, with each file creating a separate invoice record — this is particularly useful for teams that process invoices in bulk at the end of the month. OCR improvements will enhance the accuracy of data extraction from scanned and photographed invoices, reducing the need for manual corrections. An approval workflow will allow uploaded invoices to go through a review step before being finalized, which is important for teams with financial controls. Integration with email is also being explored, where invoices received as email attachments could be automatically ingested into the system without any manual upload step. These enhancements will build on the solid foundation of the current upload mechanism to further streamline the invoicing workflow.

---

### Modules Architecture (Context)
**Source**: https://onchainro.atlassian.net/wiki/spaces/SNDY/pages/175341588/Modules

The platform is a Finance SaaS application built around a modular architecture. The Invoices module sits at the center of the billing flow:

```
Dashboard ◄── Aggregates data from all modules
    │
    ▼
Invoices ◄────► Settlements
    │                │
    ▼                ▼
  Addons          Storage
```

**Data Flow**: Addons → Invoices (line items) → Settlements (payment reconciliation) → Dashboard (revenue metrics). All modules use Storage for file persistence.

**Shared Patterns**: Event-driven communication, centralized storage, subscription-centric model.

---

## Jira Stories (Completed)

### SNDY-20: Set up Invoices list view with data table
**Status**: Done | **Priority**: Medium | **Created**: 2026-02-07

As a user, I want to see all invoices in a table with columns for Invoice #, Client, Amount, Status, and Date so that I can quickly browse and find specific invoices.

**Acceptance Criteria:**
- Table displays all invoice records
- Columns: Invoice #, Client, Amount, Status, Date
- Data is refreshed daily
- Table supports pagination for large datasets

---

### SNDY-21: Implement invoice detail panel on row click
**Status**: Done | **Priority**: Medium | **Created**: 2026-02-07

As a user, I want to click on an invoice row and see a detail panel on the right side showing full invoice information so that I can preview invoice details without leaving the list.

**Acceptance Criteria:**
- Clicking a row opens a right-side detail panel
- Panel shows: Invoice #, Client, Amount, Status badge, Issue Date, Due Date, Description
- Status is color-coded (Paid=green, Pending=yellow, Overdue=red)
- Clicking another row updates the panel

---

### SNDY-22: Build Add Invoice modal with drag-and-drop upload
**Status**: Done | **Priority**: Medium | **Created**: 2026-02-07

As a user, I want to click an "Add Invoice" button and upload an invoice file via drag-and-drop so that I can create new invoice records from existing documents.

**Acceptance Criteria:**
- "Add Invoice" button in top-right corner of Invoices page
- Opens a modal with a drag-and-drop zone
- Drop zone labeled "Click to upload or drag and drop"
- Cancel and Upload buttons at the bottom
- Modal closes on Cancel or successful upload

---

### SNDY-23: Support PDF, DOC, and image file uploads for invoices
**Status**: Done | **Priority**: Medium | **Created**: 2026-02-07

As a user, I want to upload invoices in PDF, DOC, or image formats so that I can onboard invoices regardless of the file type I received.

**Acceptance Criteria:**
- Accepted formats: PDF, DOC/DOCX, JPG, PNG
- Client-side file type validation before upload
- Server-side file type validation as a security measure
- Clear error message when unsupported file type is selected
- Drop zone subtitle shows accepted formats

---

### SNDY-24: Integrate invoice file upload with Storage module
**Status**: Done | **Priority**: Medium | **Created**: 2026-02-07

As a developer, I want uploaded invoice files to be persisted via the Storage module so that files are stored durably and can be retrieved later.

**Acceptance Criteria:**
- Uploaded files are sent to Storage module API
- Storage returns a unique file reference ID
- File reference is linked to the invoice record
- Files can be retrieved by reference for preview/download
- File versioning is supported

---

### SNDY-25: Auto-generate invoice records from uploaded files
**Status**: Done | **Priority**: Medium | **Created**: 2026-02-07

As a user, I want the system to automatically create an invoice record when I upload a file so that I don't have to manually enter all invoice data.

**Acceptance Criteria:**
- New invoice record created after successful upload
- Sequential invoice number assigned (e.g., INV-0011)
- Initial status set to "Pending"
- Invoice date set to current date
- Due date calculated from default payment terms
- Metadata extraction pre-populates client, amount, description where possible

---

### SNDY-26: Implement color-coded status badges for invoices
**Status**: Done | **Priority**: Medium | **Created**: 2026-02-07

As a user, I want invoice statuses to be displayed as color-coded badges so that I can quickly identify the payment state of each invoice.

**Acceptance Criteria:**
- Paid status: green badge
- Pending status: yellow badge
- Overdue status: red badge
- Badges displayed in both the list view and detail panel
- Consistent styling across the application

---

### SNDY-27: Add file validation and error handling for invoice uploads
**Status**: Done | **Priority**: Medium | **Created**: 2026-02-07

As a user, I want clear error messages when my upload fails so that I know what went wrong and how to fix it.

**Acceptance Criteria:**
- Error shown for unsupported file types
- Error shown when file exceeds size limit
- Network errors display retry prompt
- Failed extraction still creates record with empty fields for manual entry
- No partial or corrupted data persisted on failure — full rollback

---

### SNDY-28: Implement security measures for invoice file uploads
**Status**: Done | **Priority**: Medium | **Created**: 2026-02-07

As a developer, I want uploaded files to be scanned and validated securely so that the platform is protected from malicious uploads.

**Acceptance Criteria:**
- Server-side file type validation (not just client)
- Malware scanning before persisting to Storage
- File names sanitized to prevent path traversal
- Files stored with system-generated names
- Access control tied to invoice record permissions
- All transfers over encrypted connections

---

### SNDY-29: Implement daily refresh mechanism for invoices list
**Status**: Done | **Priority**: Medium | **Created**: 2026-02-07

As a user, I want the invoices list to refresh daily so that I always see up-to-date invoice data when I open the page.

**Acceptance Criteria:**
- Invoice data syncs on a daily schedule
- Incremental sync to avoid reloading the full dataset
- Pagination support for growing invoice volume
- List view remains performant regardless of total invoice count
- Last refresh timestamp displayed

---

## Jira Epics

### SNDY-14: Implement Invoices Page
**Status**: To Do | **Priority**: Medium | **Created**: 2026-02-04

**Business Objective:** Provide comprehensive invoice management with creation, editing, and sending capabilities.

**Success Metrics:**
* Support creating/sending 1000+ invoices per month
* Reduce invoice creation time from 15 minutes to <3 minutes

**Scope:**
* In: Invoice list, create/edit invoice, send invoice, invoice templates, status tracking
* Out: Recurring invoices, payment processing (future phase)

**Dependencies:**
* SNDY-11 (Epic 3: Admin Settings) - requires invoice templates and settings configuration
* SNDY-13 (Epic 5: Platform Connections) - requires platform data for invoice population

**Timeline:** April 1-30, 2026
