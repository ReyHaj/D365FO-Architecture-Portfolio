# Dynamics 365 F&O Architecture Portfolio 

Welcome to my Dynamics 365 Finance & Operations development portfolio. This repository contains three modular mini-projects demonstrating my ability to**X++ Programming, System Architecture, design, develop, and integrate D365 solutions using Microsoft Best Practices.

##  Portfolio Objectives
The code in this repository highlights my proficiency as a Mid-level/Senior D365 Developer across the core architectural pillars:
- **Data Integrity:** Table Methods & Validation Rules.
- **System Extensibility:** Chain of Command (CoC) & Extension Classes.
- **Decoupled Logic:** Event Handling (Publisher/Subscriber model).
- **User Interface:** Form Patterns, Action Panes, & Menu Items.
- **Integration:** Data Entities, OData, and Staging concepts.
- **Resiliency:** Exception Handling (`try`, `catch`, `retry`, `throw`).

##  Projects Included

### 1. [Accounts Receivable (AR) - Smart Onboarding](./AR_SmartCustomerOnboarding)
A secure data ingestion pipeline for new customers arriving via API (OData/Data Entities), featuring strict Table validations and decoupled Event Handlers for post-insert actions.
**Focus:** Accounts Receivable, Data Quality, and Automated Workflows.
- **Business Goal:** Prevent user data-entry errors during customer creation and automate post-creation processes.
- **Technical Implementation:**
  - **Chain of Command (CoC):** Extended `CustTable` to enforce strict 10-digit validation on the Tax Exempt Number (`VATNum`).
  - **Data Event Handlers:** Implemented `onInserted` events to trigger automated background actions (e.g., Welcome Notifications) upon successful customer creation.

### 2. [Accounts Payable (AP) - Secure Payment Gateway](./AP_SecureVendorPayment)
A UI-driven process for vendor payments. Showcases Form Patterns, Action Menu Items, and highly resilient backend processing using `ttsbegin`/`ttscommit` and robust Exception Handling (Deadlock retries).
**Focus:** Accounts Payable, Audit Compliance, and 3-Tier Security Architecture.
- **Business Goal:** Eliminate unclassified vendor payments by forcing users and APIs to provide a "Method of Payment".
- **Technical Implementation:**
  - **UI Layer (Forms):** Extended `LedgerJournalTransVendPaym` to make the `PaymMode` field visually mandatory.
  - **Database Layer (Tables):** Applied CoC on `LedgerJournalTrans` to block invalid database writes (API/OData protection).
  - **Action Tools (MenuItems):** Created a custom `VendPaymAudit_Action` class to allow auditors to scan entire journals for compliance instantly.

### 3. [General Ledger (GL) - Strict Journal Control](./GL_StrictJournalControl)
An invasive yet safe modification of the standard Microsoft posting engine. Uses Class Extensions (Chain of Command) to intercept the `LedgerJournalCheckPost` class, enforcing mandatory project dimensions before allowing a journal to post.
**Focus:** General Ledger, Financial Auditing, and Core System Extensions.
- **Business Goal:** Ensure every manual financial entry has a documented justification for end-of-year auditing.
- **Technical Implementation:**
  - **Core Extension:** Intercepted the standard `validateWrite()` method on `LedgerJournalTrans`.
  - **Contextual Logic:** Built a smart filter (`LedgerJournalACType::Ledger`) to ensure the description (`Txt`) is mandatory *only* for General Ledger transactions without affecting other modules.

---

## 🛠️ Technical Skills Demonstrated
- **Languages:** X++, JSON
- **Frameworks:** Dynamics 365 F&O, .NET Framework
- **Architecture:** Chain of Command (CoC), Data Event Handlers, Extension Models
- **Tools:** Visual Studio, GitHub CLI, D365 Model Management, OData / Data Entities
  
---
*All codes are written in X++ and follow Microsoft's ALM and Extension-based architecture guidelines.
