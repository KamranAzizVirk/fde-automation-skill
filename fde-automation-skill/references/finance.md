# Module: Finance Department Automation

Part of the FDE Automation Skill. The rules in SKILL.md and references/common-standards.md always apply on top of this file.

Covers: invoices, payment follow-up, reconciliation, expense claims, and reporting.
Roles: Finance Controls Expert, Security Expert, QA Expert, Cloud Automation Expert. Add International AI Expert if an AI model reads or writes finance content. Add a Language Expert for each approved language (SKILL.md, Section 7.5).
Always Full Mode.
Claude gives no tax, accounting, or legal advice. The client's accountant confirms the rules. Claude says "میں یقین سے نہیں کہہ سکتا" about them.

## Core Rules
1. **Read-only first.** Write access is added only after the owner approves each permission.
2. **Approval limits** (amounts and approvers) are written in the brief in Phase 4. Claude never invents limits.
3. **Dual control:** payments and bank detail changes need two people.
4. **Audit trail:** every action is logged with time, who or what acted, before and after values, and the approval record.
5. **No secrets in code or logs:** bank and card data use the provider's secure or tokenised method.
6. **Card data (PCI DSS):** if card data is handled, confirm the scope with the payment provider before design.
7. **Reconciliation:** automated reports are compared with the source. Differences go to a human.
8. **Money movement** only after a written control plan and a separate security review. Claude does not move money itself. It prepares the instruction and a human approves it in the provider's own system.
9. Never type or store bank account numbers, card numbers, or passwords in chat, code, or documents.

## Extra Intake Questions
1. Which finance tasks? (multi-select): invoicing, payment follow-up, bank reconciliation, expense claims, monthly reporting, other.
2. Which finance systems are used, and who owns them?
3. Money access level (from the core intake: none, read-only, draft only, send reminders within limits, move money).
4. Who approves what, and up to what amount? (owner types)

## Workflow (Added to the Shared Phases)

### A. Process Map (Phase 2)
List each step of the finance process and mark it: automated, human-approved, or human-only. Show the map to the owner. Anything involving payment, refund, write-off, or bank detail change is human-approved at minimum.

### B. Approval Matrix (Phase 4)
A table: action, amount range, who approves, how it is recorded. The owner approves it at G4. Dual control applies to payments and bank detail changes.

### C. Data Handling
1. Treat all finance data as sensitive. Minimise what is sent to any AI model. Remove account numbers and personal details before sending text to a model when the task does not need them.
2. Set retention and deletion rules with the client's accountant.
3. Keep finance data and its logs in a place with limited access.

### D. Build (Phase 10)
One task at a time, read-only first. Draft invoices and reminders are shown to a human before sending. Reminders are sent within the approved limits and wording only. Overdue notices follow the approved tone and never threaten.

### E. Reconciliation and Reports
1. Match records from two sources (for example bank statement and accounting records). Show matched, unmatched, and doubtful items.
2. Differences go to a human, with the reason Claude can see. Claude does not force a match.
3. Reports state their data source and date.

### F. Testing (Phase 13)
1. Use test data and a sandbox of the finance tool where one exists. Never test with live payments.
2. Test wrong amounts, duplicates, partial payments, currency differences, and missing fields.
3. Test that the approval limit blocks an action above the limit and that the audit trail records it.

### G. Go-Live
1. Pilot with one small process and a human supervisor.
2. A separate security review before any write access or money action.
3. Fresh owner confirmation at every step that increases access.

### H. Money Handling Standards
1. **Never use floating-point numbers for money.** Use exact decimals (for example Python `Decimal`) or whole numbers of the smallest unit (for example paisa or cents). Store the currency code (ISO 4217, for example PKR, USD) with every amount.
2. **Rounding:** the owner and the client's accountant agree rounding rules in writing. Code follows them in one place only.
3. **Idempotency keys:** every call that creates an invoice, a payment request, or a message carries a unique key, so a retry never duplicates it.
4. **Time:** store timestamps in UTC. Show dates in the client's time zone. Invoice and report dates follow the accountant's rules.
5. **Audit log:** append-only (entries are added, never edited or deleted). Records who or what acted, when, before and after values, and the approval.
6. **Separation of duties:** the person or agent that prepares a payment is not the one that approves it.
7. **Reconciliation rules:** write the matching rules (amount, date window, reference) and keep a list of doubtful matches for a human.
8. **Data retention:** follow the accountant's advice on how long records must be kept. Claude does not decide this.

## Gates Checklist for This Module
- G2: process map approved.
- G4: approval matrix, retention, and PCI scope approved.
- G13: sandbox tests passed, including limit and audit tests.
- G14: separate security review passed.
- Live: fresh owner confirmation for each added permission.
