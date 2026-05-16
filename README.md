# NEXUS : Multi-Agent FinTech Decision Engine

> *An end-to-end agentic AI platform for payment network intelligence — built for large scale companies like Mastercard & American Express.*

[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://python.org)
[![Anthropic](https://img.shields.io/badge/Powered%20by-Claude%20Sonnet-orange.svg)](https://anthropic.com)
[![FastAPI](https://img.shields.io/badge/API-FastAPI-green.svg)](https://fastapi.tiangolo.com)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## Problem Statement

Global card networks process **billions of transactions daily** across merchants, issuing banks, and cardholders. The intelligence layer governing fraud decisions, risk scoring, pricing strategy, and go-to-market execution is fragmented — siloed across teams, tools, and time zones.

**NEXUS** is a multi-agent AI system that unifies this intelligence layer. It orchestrates specialized AI agents — each owning a vertical (fraud, risk, pricing, GTM, compliance, commercial strategy) — and synthesizes cross-agent insights for three distinct personas: **fraud analysts at card networks**, **enterprise merchants**, and **issuing banks**.

This is not a chatbot. This is a decision engine.

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        NEXUS PLATFORM                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   User Input (CLI / API)                                        │
│         │                                                       │
│         ▼                                                       │
│   ┌─────────────┐                                              │
│   │ ORCHESTRATOR│  ← Persona Router + Agent Planner            │
│   │   AGENT     │    (decides which agents to invoke)          │
│   └──────┬──────┘                                              │
│          │                                                      │
│    ┌─────┴──────────────────────────────────┐                  │
│    │         SPECIALIST AGENTS              │                  │
│    │                                        │                  │
│    │  🔴 Fraud Detection Agent             │                  │
│    │     └─ Real-time anomaly scoring      │                  │
│    │        Pattern recognition            │                  │
│    │        Network-level signals          │                  │
│    │                                        │                  │
│    │  🟠 Risk Analysis Agent               │                  │
│    │     └─ Portfolio risk assessment      │                  │
│    │        Credit exposure modeling       │                  │
│    │        Systemic risk flags            │                  │
│    │                                        │                  │
│    │  🟡 Commercial Strategy Agent         │                  │
│    │     └─ Revenue opportunity mapping    │                  │
│    │        Partnership & B2B strategy     │                  │
│    │        Competitive positioning        │                  │
│    │                                        │                  │
│    │  🟢 Pricing Intelligence Agent        │                  │
│    │     └─ Dynamic interchange modeling   │                  │
│    │        MDR optimization               │                  │
│    │        Segment-level pricing          │                  │
│    │                                        │                  │
│    │  🔵 GTM Planner Agent                 │                  │
│    │     └─ Market entry strategy          │                  │
│    │        Channel mix recommendation     │                  │
│    │        Launch sequencing              │                  │
│    │                                        │                  │
│    │  🟣 Compliance & Regulatory Agent     │                  │
│    │     └─ PCI-DSS checks                 │                  │
│    │        AML/KYC flag assessment        │                  │
│    │        Cross-border regulatory scan   │                  │
│    └────────────────────────────────────────┘                  │
│          │                                                      │
│          ▼                                                      │
│   ┌─────────────┐                                              │
│   │  SYNTHESIS  │  ← Cross-agent insight fusion               │
│   │   ENGINE    │    Persona-aware response formatting         │
│   └─────────────┘                                              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Agent Responsibilities

| Agent | Vertical | Key Outputs |
|-------|----------|-------------|
| **Orchestrator** | Meta-routing | Agent plan, synthesis, persona routing |
| **Fraud Detection** | Risk | Anomaly score, fraud vectors, velocity flags |
| **Risk Analysis** | Risk | Portfolio exposure, stress test, risk rating |
| **Commercial Strategy** | Strategy | Revenue mapping, segment opportunities, B2B plays |
| **Pricing Intelligence** | Monetization | Interchange models, MDR bands, pricing recommendation |
| **GTM Planner** | Growth | Market entry plan, channel strategy, launch sequence |
| **Compliance** | Regulatory | PCI-DSS status, AML flags, regulatory exposure |

---

## Personas

NEXUS adapts its output based on who is asking:

| Persona | Focus | Active Agents |
|---------|-------|---------------|
| **Fraud Analyst** (card network) | Detect, investigate, prevent fraud | Fraud, Risk, Compliance |
| **Enterprise Merchant** | Reduce fraud losses, optimize costs | Fraud, Pricing, GTM |
| **Issuing Bank** | Portfolio risk, regulatory posture | Risk, Compliance, Commercial |

---

## Tech Stack

```
Backend         FastAPI + Uvicorn
Agent Engine    Anthropic Claude (raw SDK — claude-sonnet-4-20250514)
Agent Pattern   Tool Use + Multi-turn Message Passing
Orchestration   Custom Orchestrator Agent with dynamic agent planning
Data Layer      Synthetic transaction data (JSON) + configurable inputs
CLI             Rich-powered terminal UI
API             RESTful endpoints with Pydantic schemas
Deployment      Render (live) / Docker (self-host)
Testing         Pytest with agent mock fixtures
```

---

## Product Design Decisions

### Why raw Anthropic SDK (not LangChain)?
LangChain adds abstraction that obscures agent behavior. For a decision engine at this criticality, transparency in tool invocation, message history management, and error handling is non-negotiable. Every agent call is explicit, traceable, and auditable.

### Why multi-agent over single model?
Each vertical (fraud, pricing, compliance) has distinct reasoning patterns, context requirements, and output formats. Specialist agents with focused system prompts outperform a single generalist model — and the orchestrator's job is to know *which* specialists to invoke and *how* to synthesize their outputs.

### Why persona-aware routing?
An issuing bank and a merchant asking "what's my fraud exposure?" need fundamentally different answers. The orchestrator's first job is persona classification — this shapes which agents run, in what order, and how outputs are weighted.

---

## Quickstart

### Prerequisites
```bash
python >= 3.11
pip install -r requirements.txt
export ANTHROPIC_API_KEY=your_key_here
```

### Run via CLI
```bash
python cli/nexus_cli.py
```

### Run via API
```bash
uvicorn api.main:app --reload
# Visit http://localhost:8000/docs
```

### Docker
```bash
docker build -t nexus .
docker run -e ANTHROPIC_API_KEY=your_key -p 8000:8000 nexus
```

---

## Example Queries by Persona

**Fraud Analyst:**
> "We're seeing a spike in card-not-present transactions from Southeast Asia in the last 6 hours. What's the fraud risk profile and recommended response?"

**Merchant:**
> "We're launching a B2B SaaS product targeting mid-market companies in the US. What interchange rates should we expect and how should we structure our payment stack?"

**Issuing Bank:**
> "Our subprime portfolio has 18% utilization growth QoQ. What's the risk exposure and what regulatory flags should we anticipate?"

---

## Project Structure

```
nexus/
├── agents/
│   ├── orchestrator.py       # Master routing + synthesis agent
│   ├── fraud_agent.py        # Fraud detection specialist
│   ├── risk_agent.py         # Risk analysis specialist
│   ├── commercial_agent.py   # Commercial strategy specialist
│   ├── pricing_agent.py      # Pricing intelligence specialist
│   ├── gtm_agent.py          # GTM planning specialist
│   └── compliance_agent.py   # Compliance & regulatory specialist
├── api/
│   ├── main.py               # FastAPI app
│   ├── routes.py             # API endpoints
│   └── schemas.py            # Pydantic models
├── cli/
│   └── nexus_cli.py          # Rich terminal interface
├── utils/
│   ├── anthropic_client.py   # Shared Claude client
│   ├── message_builder.py    # Message history utilities
│   └── persona_router.py     # Persona classification logic
├── data/
│   └── synthetic_transactions.json  # Sample data for demos
├── tests/
│   ├── test_orchestrator.py
│   ├── test_fraud_agent.py
│   └── test_api.py
├── docs/
│   └── architecture.md
├── requirements.txt
├── Dockerfile
├── render.yaml               # Render deployment config
└── README.md
```

---

## Roadmap

- [ ] Streaming agent responses (SSE)
- [ ] Agent memory layer (cross-session context)
- [ ] Webhook integration (real transaction feed simulation)
- [ ] Dashboard UI (React frontend)
- [ ] Fine-tuned fraud classifier as tool
- [ ] Multi-currency & cross-border agent

---

## About

Built by **Mayuk Basu** — CPO @ Classmate | Ex-APM @ Flipkart | IIM Bangalore MBA '26

This project demonstrates end-to-end product thinking applied to agentic AI systems — from problem definition and agent architecture to API design and deployment. Built to show 0-to-1 execution in the FinTech AI space.

📧 mayukbasu20@gmail.com | [LinkedIn](https://linkedin.com/in/mayuk-basu-279042194)
