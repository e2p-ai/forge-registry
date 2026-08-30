---
name: tgi-journal-entry
description: >
  Xero journal entries. Six-element format. Never post without Tony approval.
owner_seat: shared
approval_boundary: Tony approves every journal post
---

# TGI Xero — Advanced Accounting

## When to Use
When creating journal entries, reconciling transactions, managing invoices, or any Xero accounting operation.

## Credentials
- **Client ID:** E0169F9392B94E24A824C23BCC47DEE3
- **Client Secret:[REDACTED — infisical-get]
- **2FA TOTP Secret:[REDACTED — infisical-get]
- **Auth:** admin-bot@tiedemannglobe.com account (browser slot on Vultr)

## TOTP Generation
```python
import pyotp
totp = pyotp.TOTP('<from infisical>')
print(totp.now())  # Current 6-digit code
```

## Journal Entry Format (MANDATORY)
Every journal entry MUST include ALL six elements:

1. **Clear explanation** — what happened and why the entry is needed
2. **Accounts affected** — which go UP and which go DOWN
3. **P&L or Balance Sheet impact** — specify which statements are affected
4. **Debit and Credit amounts** — exact dollar figures
5. **Date of original transaction**
6. **Date of proposed journal entry**

Example:
```
EXPLANATION: Correct misclassified vendor payment — was coded to Office Supplies, should be Equipment
ACCOUNTS:
  - Equipment (BS Asset) ↑ DEBIT $2,500.00
  - Office Supplies (P&L Expense) ↓ CREDIT $2,500.00
ORIGINAL TRANSACTION DATE: 2026-02-15
PROPOSED ENTRY DATE: 2026-03-29
P&L IMPACT: Office Supplies expense decreases by $2,500
BS IMPACT: Equipment asset increases by $2,500
```

## Rules
- **NEVER create entries without Tony's explicit approval**
- Every account: debit/credit, increased/decreased, exact dollar amounts
- No jargon, no netting, no shorthand
- Use plain English for all financial explanations
- ARIA (CFO agent) monitors thresholds — flag anomalies
- Multi-currency: TGI deals in USD, MXN, GTQ — always specify currency
- Re-auth via admin-bot browser slot if OAuth expires (browser-session on Vultr)
- Backup email: admin-bot-backup@tiedemannglobe.com
