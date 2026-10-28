# Fiona — Autonomous AI Financial Copilot & Guardian Ledger System

[![Node.js](https://img.shields.io/badge/Node.js-24+-339933?logo=node.js&logoColor=white)](https://nodejs.org/)
[![Python](https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Turborepo](https://img.shields.io/badge/Monorepo-Turborepo-EF4444?logo=turborepo&logoColor=white)](https://turbo.build/)
[![FastAPI](https://img.shields.io/badge/Engine-FastAPI-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![Next.js](https://img.shields.io/badge/Frontend-Next.js%2016-black?logo=next.js&logoColor=white)](https://nextjs.org/)
[![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL%2017-4169E1?logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

**Fiona** is an enterprise-grade financial operating system powered by a council of autonomous AI agents designed for small and medium-sized enterprises (SMEs). Operating over an immutable, canonical double-entry ledger, Fiona coordinates specialized financial agents (Controller, Treasurer, CFO Strategist, and Growth Analyst) under cryptographic audit trails and fail-closed security invariants enforced by an independent **Guardian**.

FinTechathon 2026 · International AI Track · Direction E: "AI as a Financial Participant".

> **Synthetic Data Guarantee:** All data within this repository is synthetic. The businesses, entities, suppliers, bank accounts, and invoices are generated via our calibrated SME generator (`data/generator`) using deterministic seeds. No real-world PII or financial credentials exist within this repository. The agent wallet is simulated: money movement is executed exclusively in-memory or in sandbox environments with strict hash-verified verdicts.

---

## 🌟 Key Architecture & Capabilities

Fiona transforms traditional financial administration into proactive, audited intelligence:

- **Single Source of Truth Ledger:** Strict double-entry accounting with Row-Level Security (RLS) in PostgreSQL 17. Every transaction is verifiable with property-tested balanced debit/credit invariants (`Dr == Cr`).
- **Autonomous Multi-Agent Council:**
  - **Controller Agent:** Bill extraction, automated duplicate detection, 4-pass invoice-to-bank line reconciliation.
  - **Treasurer Agent:** Working capital optimization, cash-flow buffer protection, supplier payment timing.
  - **CFO Strategist:** Executive briefing synthesis, debt vs. equity evaluation, regional SME financing matching.
  - **Growth Analyst:** Working capital stress-testing and scenario forecasting.
- **Fail-Closed Guardian Invariants:** An independent review engine evaluating deterministic rules (`G-01` through `G-15`) alongside adversarial prompt-injection screening. A language model can tighten a verdict, but can *never* loosen one (`block` > `escalate` > `approve_with_conditions` > `approve`).
- **Simulated Agent Wallet & Non-Ratchet Mandates:** Zero autonomous privilege escalation. Agents operate strictly within user-configured caps; high-value or unusual operations require owner passkey authorization.
- **Unified Schema Contract (`packages/contracts`):** JSON Schema 2020-12 single source of truth, compiling to byte-identical TypeScript definitions and Pydantic v2 schemas (`fiona_contracts`) with RFC 8785 canonical JSON hashing.

---

## 🏛️ System Topology

```
                          ┌──────────────────────────┐
                          │   The Window (apps/web)  │
                          │   Next.js 16 Console     │
                          └─────────────┬────────────┘
                                        │ Route Handlers (BFF)
                                        ▼
 ┌───────────────────────┐   ┌───────────────────────┐   ┌────────────────────────┐
 │   services/agents     │   │    services/engine    │   │   services/guardian    │
 │ Claude Runtime, Tool  │──▶│  FastAPI Core Engine  │◀──│  Adversarial Review &   │
 │ Bridge & Briefings    │   │  Ledger, RLS, Math    │   │  Policy Verification   │
 └───────────────────────┘   └──────────┬────────────┘   └────────────────────────┘
                                        │
                                        ▼
                             ┌───────────────────────┐
                             │ PostgreSQL 17 / Shim  │
                             │ Canonical Ledger DB   │
                             └───────────────────────┘
```

---

## 📁 Repository Layout

```
apps/
  web/                  Next.js 16 web console ("The Window") and installable mobile PWA
packages/
  contracts/            Shared JSON Schema 2020-12 definitions & generated TS/Python clients
  ui/                   Design system tokens, domain primitives, and workbench screen kit
services/
  engine/               FastAPI accounting core: ledger, guardian_checks, forecast, decisions
  agents/               Claude runtime, prompt orchestration, and structured tool bridge
  guardian/             Adversarial review (Opus) and deterministic verdict combiner
rulebook/               Authoritative finance rules, mandates, threshold configs, and test scenarios
data/                   Synthetic data generator (24-month calibrated SME fixtures)
docs/                   Architecture blueprints, threat models, API specs, and execution evidence
```

---

## 🚀 Quickstart

### Prerequisites

- **Node.js**: >= 24
- **Python**: 3.12 (managed via [uv](https://docs.astral.sh/uv/))
- **Corepack**: enabled for pinned `pnpm`

### 1. Installation

```bash
corepack enable          # provides the pinned pnpm workspace manager
pnpm install             # installs workspace dependencies & local git hooks
pnpm doctor              # verifies toolchain versions and environment health
```

### 2. Local Database & Seed Data

Fiona includes an embedded PostgreSQL engine runner for local development and automated testing:

```bash
# Start local postgres & run migrations
pnpm db:reset

# Seed calibrated 24-month SME financial dataset (Karachi Apparel persona)
pnpm demo-data
```

### 3. Launch Development Server

```bash
pnpm dev
```

This boots Turborepo across all microservices:
- **Web Console (The Window):** `http://localhost:3000` (`/api/healthz`)
- **FastAPI Engine Core:** `http://localhost:8000` (`/docs`)
- **Agent Service:** `http://localhost:8100`
- **Guardian Service:** `http://localhost:8200`

> **Note on Mock Mode:** When `.env.local` is omitted, the entire monorepo operates in zero-secret **mock mode**, serving synthetic data fixtures without external API credentials.

---

## 🧪 Testing & Verification

Every layer is rigorously verified with automated boundary and invariant checks:

```bash
# Workspace-wide lint, typecheck, and unit tests
pnpm lint
pnpm typecheck
pnpm test

# Verify architectural boundary compliance
pnpm check:boundaries

# Run Hypothesis double-entry property test (Dr == Cr across 298,000+ lines)
uv run --project services/engine pytest services/engine/ledger/tests/test_posting.py
```

---

## 🛡️ Security & Integrity Invariants

- **Fail-Closed Execution:** The wallet never moves funds without a verified, unexpired verdict hash.
- **Audit Immutability:** All proposals, verdicts, and ledger entries are cryptographically chained with SHA-256 hashes.
- **Zero Drift Contracts:** All schemas are authored once in YAML and generated into byte-identical types across Python and TypeScript.

---

## 👤 Author & Maintainer

**Daniyal Zahid**  
GitHub: [@Daniyalzahid9](https://github.com/Daniyalzahid9)  
Repository: [https://github.com/Daniyalzahid9/fiona](https://github.com/Daniyalzahid9/fiona)
