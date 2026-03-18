# ai_context/agent-startup.md

# AI Guardrails Platform — Agent Startup Guide

## Purpose

This file tells a coding agent how to acquire context and work safely in this repository.

Read this before making implementation decisions.

---

## Read Order

Agents should read files in this order before proposing or making changes:

1. `README.md`
2. `docs/architecture.md`
3. `docs/implementation-phases.md`
4. `ai_context/project-overview.md`
5. the active GitHub issue or story being implemented

If additional architecture decision records or phase files exist later, read those next.

Do not start coding from the README alone.

---

## Core Interpretation Rules

### 1. This is a guardrails platform, not just an app
Do not collapse the architecture into a single controller or single service that directly calls an LLM.

Guardrails are the product.

### 2. Preserve the hybrid architecture
The system is intentionally designed as:

- staged request flow
- plus a policy engine abstraction
- plus pluggable checks

Do not simplify this into hardcoded if/else chains scattered across the request path.

### 3. Keep concerns separate
Maintain clear boundaries between:

- request ingress
- guardrail checks
- policy decisions
- provider interaction
- response validation
- observability
- evaluation concerns

### 4. Favor explicit decision models
Checks should not only return true/false.

Future-friendly work should lean toward explicit result objects such as:

- check outcome
- severity
- rationale
- recommended action
- allow / block / escalate

### 5. Respect phased delivery
Not every documented future capability belongs in the current implementation story.

Implement the active story cleanly without dragging in every deferred concern.

### 6. Do not assume one domain
This platform protects a generic customer-facing assistant and agentic workflows.

Avoid business-specific naming unless a later story intentionally introduces it.

### 7. Observability matters early
Even if observability is partial at first, do not design in a way that makes policy decisions opaque.

Guardrail decisions should remain inspectable.

---

## What to Optimize For

When implementing stories in this repo, optimize for:

- architectural clarity
- extensibility
- explicit contracts
- safe defaults
- readable design
- future evaluation compatibility

Do not optimize for feature count.

Do not prematurely optimize for production scale.

Do not hide architectural decisions inside framework-specific magic.

---

## What to Avoid

Avoid these failure modes:

- direct provider calls from the edge without policy orchestration
- hardcoded moderation logic tangled with transport or controller code
- rigid pipelines that cannot support new checks later
- undocumented assumptions about providers or deployment model
- coupling response validation directly into provider clients
- implementing speculative features not required by the active story

---

## Expected System Shape

The platform should evolve toward this logical flow:

- request enters through an ingress layer
- request moves through a guardrail pipeline
- checks emit structured outcomes
- policy logic decides allow / block / escalate
- allowed requests go through a provider adapter
- model responses go through validation
- all major stages emit observable events
- evaluation can later replay or score decisions

That is the baseline architectural path.

---

## Current Priority Areas

The current near-term priority guardrails are:

- input moderation / prompt safety
- jailbreak / prompt injection detection
- rate limiting / abuse prevention
- human-in-the-loop escalation

These are the most likely early implementation targets.

Future-aware but lower-priority areas include:

- PII controls
- tool authorization
- grounding checks
- hallucination detection
- citation verification

---

## Guidance for Story Execution

Before implementing a story, the agent should answer:

1. Which architectural lane does this story touch?
2. Is this story introducing a new concept or filling in an existing seam?
3. What is intentionally out of scope for this story?
4. How does this change preserve the hybrid architecture?
5. What future extension point should remain visible after this work?

If those questions cannot be answered clearly, the agent should revisit the docs before coding.

---

## Definition of Good Work in This Repo

A good change in this repository usually has these characteristics:

- small enough to review clearly
- consistent with documented architecture
- leaves room for future checks and policies
- does not overfit to one provider or one use case
- makes the system easier for the next agent to understand

The best contributions make the architecture more obvious, not less.