# ai_context/project-overview.md

# Project Overview — AI Guardrails Platform

## Core Purpose

The **AI Guardrails Platform** is a reference architecture for a safety-first middleware layer that sits between client applications and LLM providers.

Its primary goal is to demonstrate how to build a production-ready, domain-agnostic guardrails system that enforces policy, safety, and observability for both conversational assistants and tool-using agents.

---

## High-Level Capabilities

The platform is designed to handle:

- **Input Moderation:** Blocking unsafe or out-of-policy prompts before they reach the model.
- **Prompt Injection & Jailbreak Detection:** Identifying adversarial attempts to bypass system instructions.
- **Rate Limiting & Abuse Prevention:** Protecting provider costs and ensuring fair usage.
- **Output Validation:** Checking model responses for safety, schema compliance, and action correctness.
- **Human-in-the-Loop:** Escalating ambiguous or high-risk requests for human review.
- **Observability:** Providing full traceability of every policy decision (allow, block, escalate).
- **Evaluation:** Using regression datasets to test and improve guardrail performance over time.

---

## Technical Approach

The system follows a **Hybrid Middleware Architecture**:

1.  **Staged Pipeline:** Requests pass through a sequence of modular checks (moderation, injection, etc.).
2.  **Policy Engine:** A central decision layer evaluates check outcomes against configurable policies.
3.  **Provider Abstraction:** An adapter layer prevents coupling to specific LLM providers.
4.  **Dual-Sided Validation:** Safety checks are applied to both input (pre-model) and output (post-model).

---

## Architectural Principles

- **Guardrails as Middleware:** Safety logic is a first-class citizen, not an afterthought in a controller.
- **Observable Decisions:** If you can't see why a decision was made, the guardrail isn't doing its job.
- **Tool-Aware Safety:** Recognizing that agentic actions (tool use) require stricter validation than text alone.
- **Evaluation-Driven:** The system is designed to be measured and improved via repeatable tests.

---

## Repository Structure

- `docs/`: Detailed architectural documentation and implementation roadmaps.
- `ai_context/`: Context files designed to help AI coding agents understand and work in this repo.
- `src/` (Future): Core implementation code (following Phase 0).

---

## Current Status

We are currently in **Phase 0 — Architecture and AI Context Foundation**.

The immediate focus is on establishing the design contracts and mental models that will guide all future implementation work.