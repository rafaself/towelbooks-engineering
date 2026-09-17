# Decision: pnpm + Nx Monorepo

## Context

TowelBooks includes multiple browser applications, a backend API, shared schemas, a typed SDK, UI packages, email templates, infrastructure code, and repository-level validation.

These surfaces change together often enough that keeping them in one repository provides useful coordination, but the repository still needs explicit dependency boundaries to avoid becoming globally coupled.

## Decision

Use a `pnpm` workspace with Nx for project graph orchestration, task execution, caching, and boundary enforcement.

## Why pnpm

pnpm provides:

- workspace dependency management;
- deterministic lockfile-based installs;
- efficient local package linking;
- one dependency-management model for the TypeScript workspace.

## Why Nx

Nx provides:

- an explicit project graph;
- task orchestration across applications and packages;
- affected-project execution;
- caching;
- enforceable module boundaries.

The important benefit is not simply putting many projects in one repository. It is being able to express which projects are allowed to depend on which other projects.

## Shared contracts

The frontend does not import backend internals directly. Shared request/response schemas and a typed SDK provide the supported application boundary.

This makes cross-surface changes easier to coordinate while preserving ownership rules.

## Agent-assisted development implication

A monorepo gives coding agents broad visibility, which is useful but also dangerous. Without constraints, an agent can reach across boundaries to produce a fast local solution.

Project-graph and lint rules therefore act as guardrails. They turn architectural intent into checks that can fail automatically when a generated change introduces an invalid dependency.

For agentic development, that is more reliable than expecting architecture conventions to be remembered from prose alone.
