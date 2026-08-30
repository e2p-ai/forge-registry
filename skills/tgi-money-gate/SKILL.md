---
name: tgi-money-gate
description: >
  Fail-closed money/send/shell gate. Overwatch/CEO grant capability. Tony only for send/money/shell/tax/scanner/INV-<3000.
owner_seat: shared
approval_boundary: Tony for send/money/shell/tax/scanner/INV-<3000
---

# TGI money gate

TOOL_GATES_NOT_TONY_APPROVALS: Overwatch/CEO click capability grants. Tony only for send / money / shell / tax file / scanner insert / INV-<3000.

## Tony-required tools (do not Always without Tony)
gmail_send, xero_write, erp_write, bill_pay_schedule, po_generate, budget_set,
shell_exec, run_code, claude_code_bash, tax_form_lookup, scan_contact_intake,
voice_call, alert_send, press_release_send, social_post, deploy, db_admin.

## Rules
- Reading cash/PDF/OCR is not a Tony card.
- Never auto-Always a books-write pack.
- Use request_tool if the seat lacks the tool. Skills never grant.
