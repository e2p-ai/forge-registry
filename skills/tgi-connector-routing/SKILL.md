---
name: tgi-connector-routing
description: >
  Which pipe to use: ecp-tools MCP, Plaid, Stripe, AuthNet, Xero, GitHub, Google Workspace.
owner_seat: shared
approval_boundary: No new MCP servers on seats
---

# TGI connector routing

Company work uses ecp-tools MCP (:3302, X-Agent-Key). Do not add extra MCP servers on seats.

| Connector | Pipe |
|---|---|
| ecp-tools | HTTP :3302 Streamable MCP, header X-Agent-Key. Grant-driven tool list. |
| brain-federation | brain_search / brain_recall on ecp-tools |
| TGI Plaid | Bank balances. Connector, not an ALL_TOOLS name. |
| TGI Stripe | May need operator re-auth. Do not fake live. |
| TGI Authorize.net | Card / CIM |
| TGI Xero | xero_read / xero_write on ecp-tools. Writes Tony-gated. |
| GitHub | git_exec / git_workflow on ecp-tools |
| Gmail/Drive/Calendar | Company mail = gmail_* (DWD). Operator Gmail is not the seat mailbox. |

Grok TUI also has mcp_servers: ecp-tools, brain-federation, browser, local-rag, tgi-plaid, tgi-stripe, tgi-authnet, stripe.
