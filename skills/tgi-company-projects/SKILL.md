---
name: tgi-company-projects
description: >
  TGI/FOW/B&B/Overwatch agent map, brains, and hosts.
owner_seat: shared
approval_boundary: Do not override Vultr agent prompts
---

# TGI Agent System — 59 Agents

## Infrastructure
- **Agent/brain host (BRAIN):** Vultr **45.63.39.217** (`ssh vultr`) — NOT the ECP app box
- Orchestrator: Custom Node.js at /opt/tgi-agents/
- Agent prompts: /opt/tgi-agents/agents/*.md
- Task queue: PostgreSQL
- Services: PM2, Node.js, PostgreSQL (brains)
- **ECP app / forge-term:** Massed L40S **64.247.206.107** (`ecp-fleet-runtime`) — ecp-tools `target=vultr` resolves here. Old ## SOVEREIGN / CROWN
OVERWATCH (Cross-Company Overseer) → reports to Tony
├── ATLAS — CEO of TGI
├── VITAE — CEO of FOW
└── OPTIMUS — CEO of B&B

## CEO Direct Reports (12)
Magnus (Chief of Staff), Aria (CFO), Prism (CPO), Oracle (CDO), Vance (VP Sales), Petra (CMO), Helm (COO Production), Cargo (VP Supply Chain), Nexus (CTO), Harbor (HR), Counsel (General Counsel), Anchor (Customer Success)

## Full Departments
- EXECUTIVE: Magnus → Amy (TGI only), Summit → Alliance
- FINANCE: Aria → Ledger, Vault, Levy, Auditor
- PRODUCT: Prism → Pixel
- DATA: Oracle → Lens
- SALES: Vance → Striker, Margin
- MARKETING: Petra → Canvas, Beacon, Signal, Herald, Flare, Scout
- OPERATIONS: Helm → Forge → Gate, Haven, Manor → Seeker, Terra
- SUPPLY CHAIN: Cargo → Procure, Vendor, Ridge → Dock + Convoy, Globe
- ENGINEERING: Nexus → Coder, Infra, Cipher, Shield, Spark
- HR: Harbor → Talent, Onboard, Mentor
- LEGAL: Counsel → Meridian, Envoy, Warden
- CUSTOMER SUCCESS: Anchor → Echo, Survey
- FOW ONLY: Remedy (reports to Vitae)

## Inference Tiers
- Tier 1: Claude Code SDK Opus on Team Premium (switching to Max 20x April 10) — ALL 59 agents, zero marginal cost, 1M context window, 16K word prompts
- Tier 2: Groq / Kimi K2 — ERP Copilot, Email Scanner, Brain Chat, FOW Vitae Chat, Lab Scan/Redact
- DeepInfra K2.5 — email scanner fallback only
- Local: llama3.2 via Ollama on this Mac Mini — embeddings, 100% privacy
- NO Together AI

## Brain Databases on BRAIN (45.63.39.217) — UNDERSTAND BEFORE MODIFYING
- brain_tgi — TGI brain
- brain_fow — FOW brain
- brain_bb — B&B brain
- brain_overwatch — Overwatch brain
- ecpdb_local / ECP app DB — on **ECP fleet 64.247.206.107**, not BRAIN

## Key Principles
- Claude Code SDK Opus on Team Premium is Tier 1 for ALL agents — zero cost per query
- Study existing brain database structure before proposing changes — improvements welcome, blind rewrites are not
- Agents activate via task queue polling and scheduled triggers
- Agent prompts on Vultr are the source of truth — do not duplicate or override
