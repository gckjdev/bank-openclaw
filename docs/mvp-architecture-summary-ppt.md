
# Cover: My AI Jouney & Experience 
---

## Slide 1: AI experience before 2023

- HSBC Chat Bot Solution
  - Technology: Tensorflow, Rasa NLU
  - Role: ITSO & Platoform Lead
  - Design Platform Strategy and lead the implementation
    - Rollout to HK, UK, US market
  - Other: Gain Machine Learning & GCP Platform Cerficates
    - Deeplearning Specialization (Cousera)
    - Tensorflow Specialization (Cousera)
    - GCP Professional Cloud Architect
    - GCP Professional Data Engineer
    - GCP Professional Machine Learning Engineer

---
## Slide 2: GenAI/LLM 2024-2026

- Use GenAI / LLM to generate UI test automation cases
  - Technology: Mistral inside HSBC, LLM Fine Tuning
  - Fine tune Mistral to generate UI test automation cases inside HSBC
- AI Interview 
  - Technology: Langchain / Langgraph
  - Build an AI Interview project by generating exam question/answer to interview developers
- Agentic Coding & Harness Engineering
  - use Copilot / Opencode with guideline/skills for AI coding
  - Long-run coding task (from JIRA ticket to PR)
- Agentic Workflow & SKills in daily activities
  - Production support (SRE)
  - Automatic Weekly report Generation from confluence & Sending email 
- Banking OpenClaw Product
  - Product design
  - MVP architecture

---

## Slide 3: Banking OpenClaw Product - Brief Introduction

**What it is:**
Banking OpenClaw is not a smarter chatbot 
— it is an enterprise AI work assistant for banking staff. 
- binds to work identity
- runs around tasks
- delivers skills mapped to job actions
- operates under strict governance
- evolves toward digital workforce substitution
- proactive tasks (cron jobs)

**Architecture philosophy:**
- Assistant OS + Task Engine + Governed Skill Runtime + Banking Control Plane
- 8 logical layers: 
  - interaction access
  - identity & permissions
  - task context
  - memory & knowledge
  - skills & tools
  - agent orchestration
  - governance & audit
  - metrics & substitution analysis
- Inspired by OpenClaw's architecture but transformed for enterprise banking
  - adding formal Task objects
  - approval governance
  - enterprise tool gateway
  - labor accounting
  - FTE substitution measurement

---

## Slide 4: Banking OpenClaw Product - Architecture Overview

**One-liner:**
Assistant OS + Task Engine + Governed Skill Runtime + Banking Control Plane

**MVP Topology (4 units + 1 infra):**
- Workbench Web — session workspace + task/plan UI
- Assistant Gateway — control plane + embedded agent runtime + cron job
- Plan Agent + Execution Agent
- API Server — persistent object services (CRUD, approval, audit)
- Tool Gateway — enterprise system bridge, auth, audit, idempotency

**Core Loop:**
Session → Planning Interpretation → Task Materialization → Plan Commit → Execution Agent → Tool Gateway → Audit

**Proactive Chain:**
Cron Job → Proactive Task → Session / Task

---

## Slide 5: Future-State Architecture Logic

**Layer 1: Work Assistant**
- always-on
- multi-surface
- persistent context
- proactive support

**Layer 2: Managed Execution**
- task assignment
- lifecycle visibility
- blocker handling
- skill reuse

**Layer 3: Workforce Governance**
- digital roles
- budget control
- audit & policy
- org-level accountability


