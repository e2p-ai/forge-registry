---
name: tgi-email-gate
description: >
  Validate draft email subject against TGI reply/new rules before send. Fail closed.
owner_seat: shared
approval_boundary: Do not send unless subject passes this gate
---

# TGI email gate

Source: Grok workflows tgi-email-gate-check.rhai + tgi-inbox-triage.rhai

```rhai
let meta = #{
  name: "tgi-email-gate-check",
  description: "Validate a draft email subject against TGI reply/new rules before send"
};

let r = agent(#{
  prompt: "Apply tgi-email-gate rules to any draft subject/body in the user message. Reply vs new. Fail closed. Return APPROVE or REJECT with exact fixed subject if rejected.",
  description: "Email subject gate",
  subagent_type: "general-purpose"
});
print(r);

```

```rhai
let meta = #{
  name: "tgi-inbox-triage",
  description: "Triage tony@ inbox: classify, draft actions, no send without gate"
};
let r = agent(#{
  prompt: "Search Gmail inbox (ecp-tools gmail_search). Classify urgent/money/orders/noise. Propose actions. Do not send mail unless subject passes tgi-email-gate and Tony already approved class. Return markdown table.",
  description: "Inbox triage",
  subagent_type: "general-purpose"
});
print(r);

```
