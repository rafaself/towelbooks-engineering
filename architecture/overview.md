# Architecture Overview

TowelBooks is a production reading and social platform organized as a `pnpm` + `Nx` monorepo. The architecture is intentionally explicit about application boundaries, shared contracts, persistence, and deployment ownership.

## Runtime surfaces

| Surface | Technology | Production role |
| --- | --- | --- |
| Main web app | React + Vite | Authenticated product and public site |
| Admin app | React + Vite | Operator-only administration surface |
| API | NestJS + Prisma | Canonical application API |
| Database | PostgreSQL / AWS RDS | Runtime persistence |
| Public assets | Cloudflare R2 | Public asset delivery |
| AWS runtime | ECS Fargate, RDS, S3, ECR, CloudWatch, SES | Application runtime and operations |
| Edge | Cloudflare Pages, DNS, Tunnel, R2, WAF, rate limiting | Web delivery and edge controls |

## Monorepo shape

The private production repository is organized around deployable applications and shared packages:

```text
apps/
  api/            NestJS API and domain modules
  web/            Main React application
  admin/          Operator application
  ui-gallery/     Development-only UI gallery

packages/
  config/         Shared tooling/configuration
  design-tokens/  Shared brand and design primitives
  schemas/        Shared request/response contracts
  sdk/            Typed API client
  ui/             Shared UI components
  email-templates/

infra/
  AWS and Cloudflare infrastructure

docs/
  Product, architecture, API, privacy, ADRs, and operational docs
```

## Boundary rules

A major concern is preventing the monorepo from becoming one globally coupled application.

The repository enforces module boundaries so that:

- applications may depend on shared packages;
- shared packages do not depend on applications;
- frontend and backend application internals do not directly cross-import each other;
- the web application consumes backend behavior through shared schemas and a typed SDK.

These boundaries matter even more with coding agents. Without mechanical constraints, an agent can easily solve a local task by introducing an architectural dependency that is convenient in the moment but expensive later.

## API and persistence

The API is built with NestJS and Prisma on PostgreSQL. Product behavior includes authentication, catalog access, libraries, reading rooms, social features, privacy workflows, and administration.

Database-backed integration tests use a dedicated test database rather than the developer database. Migration and persistence checks are treated as part of repository validation rather than as an isolated deployment concern.

## Architecture as executable policy

Documentation is useful, but architectural rules are stronger when the repository can test them.

TowelBooks therefore combines written architecture guidance with automated checks for areas such as:

- workspace boundaries;
- migration safety;
- database isolation;
- authorization and privacy regressions;
- deployment topology;
- security scanning;
- AI orchestration contracts.

This approach is especially useful in agent-assisted development: the repository can reject some classes of incorrect changes even when the implementation was produced autonomously.
