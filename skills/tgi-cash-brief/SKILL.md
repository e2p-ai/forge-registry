---
name: tgi-cash-brief
description: >
  Read-only cash snapshot via TGI Plaid + Stripe. No transfers.
owner_seat: shared
approval_boundary: Read-only
---

# TGI cash brief

Source: Grok workflows tgi-cash-brief.rhai + tgi-cash-brief-daily.rhai

```rhai
let meta = #{
  name: "tgi-cash-brief",
  description: "Read-only cash snapshot via TGI Plaid + Stripe status tools"
};

// Lightweight orchestration: ask parallel status agents then synthesize.
// Agent budget: 3.
let plaid = agent(#{
  prompt: "Call tgi-plaid plaid_status and plaid_balances. Return institutions, available totals only. No transfers.",
  description: "Plaid cash read",
  subagent_type: "general-purpose"
});
let stripe = agent(#{
  prompt: "Call tgi-stripe stripe_account_status and stripe_get_balance. Summarize livemode and balances. No charges.",
  description: "Stripe balance read",
  subagent_type: "general-purpose"
});
let out = agent(#{
  prompt: `Using these results write a CEO cash brief (markdown table, 7-day note if missing say so). Plaid: ${plaid}\nStripe: ${stripe}`,
  description: "Synthesize cash brief",
  subagent_type: "general-purpose"
});
print(out);


```

```rhai
let meta = #{
  name: "tgi-cash-brief-daily",
  description: "Daily CEO cash brief from Plaid + Stripe + optional Xero"
};
let r = agent(#{
  prompt: "Run tgi-cash-brief skill using TGI Plaid balances/txns and TGI Stripe balance. Optional xero_read for AR summary. Read-only. Output cash brief table.",
  description: "Daily cash brief",
  subagent_type: "general-purpose"
});
print(r);

```
