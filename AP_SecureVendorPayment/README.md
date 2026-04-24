
 #  Project 2: AP_SecureVendorPayment: Financial Integrity & Payment Security
 
This project implements a critical security layer within the Accounts Payable (AP) module of Dynamics 365 Finance & Operations. It ensures that no vendor payment can be processed or saved without a verified Method of Payment, preventing financial leakage and ensuring audit compliance.
---
## 1. Business Requirement
In large-scale enterprise environments, payments to vendors must be traceable and categorized (e.g., Electronic, Check, or Wire).
The Problem: Users often forget to select a "Method of Payment" in the Vendor Payment Journal, leading to reconciliation errors and incomplete bank exports.
The Solution: A mandatory validation at the Database Engine level that blocks the saving of any Vendor Payment line if the Method of Payment field is empty.

---

## Architecture & Design Pattern

### Pattern: 
Chain of Command (CoC).

### Target:
LedgerJournalTrans (The core table for all financial transactions in D365).

### Reasoning:
By extending the validateWrite() method on the table level, we ensure the rule is enforced regardless of the UI (Journal Form, Excel Add-in, or OData API).

---

## Integration Workflow
The validation acts as a gatekeeper in the following data flow:

```mermaid
Trigger: Data is sent to LedgerJournalTrans via OData API, Data Entities, or Manual Entry.

Execution: The next validateWrite() is called (Standard MS logic).

Custom Logic: Our Extension checks:

Is the AccountType == Vendor?

Is the PaymMode (Method of Payment) empty?

Outcome: If criteria match, a checkFailed warning is issued, and the SQL transaction is aborted.

```

---
## API Test Payload (OData)
To test this validation via integration (e.g., Postman), use the following JSON for the VendorPaymentJournalLines entity:

JSON Payload:

JSON
{
    "JournalBatchNumber": "VNP00045",
    "LineNumber": 1,
    "AccountDisplayValue": "VEND-1001",
    "CreditAmount": 1500.00,
    "CurrencyCode": "USD",
    "MethodOfPayment": "",  <-- This will trigger the Security Alert
    "PaymentReference": "REF-2026"
}

Expected Response: 400 Bad Request with the message: "Security Alert: You cannot process a vendor payment without selecting a Method of Payment."

---

## 5. Technical Q&A

Q: Why use validateWrite instead of validateField?

A: validateField only triggers when that specific field changes. validateWrite triggers before the entire record is saved to the DB, ensuring that the state of the record is valid as a whole.

Q: Does this affect General Ledger or Customer journals?

A: No. The code explicitly checks this.AccountType == LedgerJournalACType::Vend to ensure it only impacts Vendor-related transactions.

##  Deployment Instructions

Build: In Visual Studio, run a Full Build on the model containing AP_SecureVendorPayment.

Sync: Perform a Database Synchronization (essential when adding business logic to core financial tables).

Verify: Navigate to Accounts payable > Payments > Vendor payment journal and attempt to save a line without a Method of Payment.
