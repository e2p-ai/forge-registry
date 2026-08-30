---
name: tgi-handoff-9
description: >
  Verify Desktop handoff has all 9 sections and non-empty Next.
owner_seat: shared
approval_boundary: Handoff is a record, not a grant
---

# TGI handoff (9 sections)

Source: Grok workflow tgi-handoff-check.rhai and Claude /finish 9-section format.

```rhai
let meta = #{
  name: "tgi-handoff-check",
  description: "Verify latest Desktop handoff has all 9 sections and non-empty Next"
};

let r = agent(#{
  prompt: "Find the newest markdown under ~/Desktop/handoff/. Check it has ## Summary Done Failed Needs Next Decisions State Files Changed Icebox. Next must be non-empty after non-trivial work. Report PASS/FAIL with missing sections.",
  description: "Handoff 9-section gate",
  subagent_type: "general-purpose"
});
print(r);

```

## 9-section format (from /finish)

## Summary
## Done
## Failed
## Needs
## Next
## Decisions
## State
## Files Changed
## Icebox

Next must be non-empty after non-trivial work. Empty Failed/Needs = Nothing. Empty Next/Decisions/State = —.
