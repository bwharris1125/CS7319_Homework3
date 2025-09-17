# Microservices Architecture (Chapter 18)
Subtitle: From Bounded Contexts to Service Meshes  
Your Name · Course · Date

---

## Motivation & Definition
- Pressure: scaling teams/features; monolith friction
- Microservices = independently deployable, single-purpose services
- Encapsulate code + data + schema + runtime config
- Distribution is a COST accepted for autonomy & velocity

---

## Core Principles
- Bounded context isolation (DDD influence)
- Strong internal cohesion; loose external coupling
- Polyglot freedom (tech, data stores) within guardrails
- Explicit contracts (APIs/events), not shared DB tables
- Automate quality: tests, CI, fitness functions

---

## Bounded Context Alignment
- Service boundary ≈ domain/subdomain/workflow
- Internal model owned; external models translated
- Avoid leaking ubiquitous language across contexts
- Revisit boundary if constant cross-service changes

---

## Granularity Heuristics
- Purpose: single meaningful capability
- Transactions: entities that must change together stay together
- Choreography cost: excessive chat => merge
- Name test: needs "and"? boundary likely wrong
- Too small: network latency & coordination overhead

---

## Data Ownership & Topologies
- Database-per-service: autonomy, schema agility, blast radius reduction
- Monolithic DB problems: contention, lockstep deploys, scaling bottleneck
- Limited shared DB (≤5–6 services) only within broader context (trade-offs)
- No integration via shared tables; prefer events or APIs

---

## API Gateway / Edge
- Central ingress: routing, authn/z, rate limiting, observability
- Zero business logic; pass-through + cross-cutting concerns
- Facilitates gradual evolution of internal services

---

## Sidecars & Service Mesh
- Sidecar: offload infra (logging, metrics, mTLS, retries)
- Mesh: control plane + data plane (uniform policy, traffic shaping)
- Enables consistency without centralizing domain logic

---

## Communication Styles
- Synchronous (REST/gRPC) for request/response immediacy
- Asynchronous (events, messaging) for decoupling & resilience
- Event-driven for state change propagation
- Prefer async when temporal coupling hurts scalability

---

## Choreography vs Orchestration
- Choreography: services react to events (emergent flow)
- Orchestration: explicit workflow coordinator (clarity, central logic)
- Start with choreography; introduce orchestrator for complex sagas/visibility

---

## Transactions & Sagas
- Avoid distributed ACID across services (latency, fragility)
- Saga: sequence + compensating actions on failure
- Many sagas => boundary/aggregate rethink
- Each step ideally idempotent & emits domain events

---

## Governance & Coupling
- Static coupling: deps, shared libs, build graphs (track SBOM)
- Dynamic coupling: runtime call graph, fan-out, latency chains
- Fitness functions: automated checks (max downstream calls, forbidden deps)
- Guardrails > gatekeeping; paved paths & templates

---

## Team Topologies
- Stream-aligned teams own bounded contexts end-to-end
- Platform team: mesh, CI/CD, shared tooling (enable not block)
- Enabling team: coaching, practices (observability, resiliency)
- Complicated subsystem team: deep specialist domain

---

## Risks & Mitigations
- Chatty calls => aggregate services / batch / cache
- Latency: circuit breakers, retries, backoff, caching
- Data inconsistency: design for eventual consistency, idempotency
- Over-sharing libraries: use contracts; version events
- Observability gaps: tracing + structured logs + metrics baseline

---

## Strengths / Weaknesses
**Strengths**: modularity, independent deploys, scalability, fault isolation, testability
**Weaknesses**: operational complexity, network & consistency latency, boundary errors costly

---

## Key Takeaways & Q&A
- Boundaries > technology choices
- Optimize granularity for cohesion & low communication
- Autonomy demands disciplined contracts & observability
- Use automation (mesh, fitness functions) to sustain velocity
Questions?
