# Dynamics 365 F&O Architecture Portfolio 

Welcome to my Dynamics 365 Finance & Operations development portfolio. This repository contains three modular mini-projects demonstrating my ability to design, develop, and integrate D365 solutions using Microsoft Best Practices.

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

### 2. [Accounts Payable (AP) - Secure Payment Gateway](./AP_SecureVendorPayment)
A UI-driven process for vendor payments. Showcases Form Patterns, Action Menu Items, and highly resilient backend processing using `ttsbegin`/`ttscommit` and robust Exception Handling (Deadlock retries).

### 3. [General Ledger (GL) - Strict Journal Control](./GL_StrictJournalControl)
An invasive yet safe modification of the standard Microsoft posting engine. Uses Class Extensions (Chain of Command) to intercept the `LedgerJournalCheckPost` class, enforcing mandatory project dimensions before allowing a journal to post.

---
*All codes are written in X++ and follow Microsoft's ALM and Extension-based architecture guidelines.
