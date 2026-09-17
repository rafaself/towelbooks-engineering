# Decision: Prefer a Modular Monolith Over Premature Service Splitting

## Context

TowelBooks contains multiple product domains and deployment concerns, but that does not automatically justify decomposing the application into many independently deployed services.

The current backend is organized around explicit domain boundaries inside one primary application API rather than introducing distributed-system boundaries without an operational reason.

## Decision

Prefer clear module boundaries inside the application first. Introduce an independent service only when there is concrete evidence that the boundary needs separate deployment, scaling, availability, security isolation, release cadence, ownership, or runtime characteristics.

## Why

Splitting services too early creates costs that are easy to underestimate:

- more deployment units;
- network failure modes;
- cross-service contracts and versioning;
- additional observability requirements;
- harder local development;
- more complex data ownership and transactions;
- increased infrastructure and operational overhead.

A modular monolith can still provide meaningful separation of concerns without paying those costs immediately.

## Agent-assisted development implication

This decision also creates a useful constraint for coding agents.

An agent should not solve a local design problem by introducing a new service simply because the abstraction looks clean in isolation. The default is to preserve existing ownership boundaries and add distributed infrastructure only when the system requirements justify it.

The architectural question is therefore not “could this be a service?” but “what operational requirement makes an independent service necessary?”
