# AI Guardrails Platform Architecture

## Purpose

The AI Guardrails Platform is a reference architecture for protecting customer-facing AI assistants and agentic workflows.

It sits between client applications and LLM providers and enforces safety, policy, validation, observability, and evaluation concerns.

The system is domain-agnostic and supports:

* conversational assistants
* tool-using agents

It is designed to handle:

* adversarial prompts
* prompt injection and jailbreak attempts
* abusive usage patterns
* unsafe outputs
* sensitive or high-risk actions

---

## Architectural Goals

* Enforce policy-driven controls before LLM invocation
* Separate guardrails from business logic
* Support agent/tool workflows safely
* Validate responses before returning them
* Provide strong observability (logs, metrics, traces)
* Enable evaluation-driven iteration

---

## Non-Goals (Initial Phase)

* No production-ready implementation yet
* No full evaluation dashboard
* No real human review system
* No provider-specific optimizations

---

## Architectural Principles

### Guardrails as Middleware

All safety and policy logic lives in a structured pipeline, not scattered conditionals.

### Observable Decisions

Every decision (allow, block, escalate) should be traceable.

### Dual-Sided Validation

Validation happens both before and after the LLM call.

### Tooling is High-Risk

Tool usage must be explicitly controlled and auditable.

### Evaluation-Ready by Design

The system must support replay and regression testing.

### Observability = Safety

If you can’t see failures or decisions, you don’t have guardrails.

---

## High-Level Architecture

```mermaid
flowchart TD
    A[Client Application] --> B[Ingress API / Gateway]
    B --> C[Guardrail Pipeline]

    C --> C1[Input Moderation]
    C1 --> C2[Prompt Injection Detection]
    C2 --> C3[Rate Limiting]
    C3 --> C4[Policy Decision]

    C4 -->|allow| D[LLM Adapter]
    C4 -->|escalate| H[Human Review]
    C4 -->|block| X[Rejected]

    D --> E[LLM Provider]
    E --> F[Response Validation]

    F --> F1[Output Safety]
    F1 --> F2[Schema Validation]
    F2 --> F3[Action Validation]
    F3 --> F4[Decision]

    F4 -->|approve| G[Response to Client]
    F4 -->|escalate| H
    F4 -->|reject| Y[Safe Failure]

    B --> O[Observability]
    C --> O
    D --> O
    F --> O
    H --> O

    O --> O1[Logs]
    O --> O2[Metrics]
    O --> O3[Traces]
    O --> O4[Audit History]

    O --> V[Evaluation Layer]
    V --> V1[Test Datasets]
    V --> V2[Policy Tests]
    V --> V3[Results Dashboard]
```



---

## Request Lifecycle

```mermaid
sequenceDiagram
    participant Client
    participant Gateway
    participant Guardrails
    participant Provider
    participant Validation
    participant Observability
    participant Human

    Client->>Gateway: Request
    Gateway->>Observability: Start trace
    Gateway->>Guardrails: Input checks

    alt Blocked
        Guardrails->>Observability: Log decision
        Guardrails-->>Client: Reject
    else Escalated
        Guardrails->>Human: Queue review
        Human-->>Client: Reviewed response
    else Allowed
        Guardrails->>Provider: Forward request
        Provider-->>Validation: Model output
        Validation->>Observability: Log validation

        alt Approved
            Validation-->>Client: Return response
        else Escalated/Rejected
            Validation->>Human: Review
            Human-->>Client: Safe response
        end
    end
```

---

## Core Components

### Client Application

Any upstream system:

* web app
* mobile app
* internal agent

---

### Ingress API / Gateway

Handles:

* authentication (future)
* request normalization
* trace initialization

---

### Guardrail Pipeline

**Initial focus:**

* Input moderation
* Prompt injection detection
* Rate limiting
* Human escalation

**Responsibilities:**

* evaluate request
* assign severity
* decide allow/block/escalate

---

### LLM Provider Adapter

Abstracts:

* provider APIs
* retries/timeouts
* request formatting

---

### Response Validation

Validates outputs before returning:

**Initial:**

* output safety
* schema validation
* action sanity

**Future:**

* hallucination detection
* citation checks
* PII filtering

---

### Observability Layer

Includes:

* logs
* metrics
* traces
* audit trail

Also feeds:

* evaluation systems
* debugging workflows

---

### Evaluation Layer

Planned capabilities:

* regression datasets
* prompt test suites
* policy validation
* run history

This enables continuous improvement of guardrails.

---

## Guardrail Scope

### Phase 1 (Prioritized)

* Input moderation
* Prompt injection detection
* Rate limiting
* Human escalation

---

### Future Guardrails (Documented, Not Implemented)

* PII detection
* tool authorization
* grounding validation
* hallucination detection
* business-rule validation

---

## Summary

This platform demonstrates how to:

* structure guardrails as a system
* enforce policy around LLM usage
* support agent workflows safely
* capture observability and auditability
* prepare for evaluation-driven iteration

The goal is not just to call an LLM safely,
but to show how a production-grade AI middleware layer should be designed.
