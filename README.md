# TowelBooks Engineering

Public engineering case study of [TowelBooks](https://towelbooks.com), a production reading and social platform built with extensive use of AI coding agents.

This repository documents the architecture, infrastructure, engineering decisions, and agentic software-development workflows behind the product. The production source code remains private.

## Why this repository exists

TowelBooks became more than a product project. It also became a practical environment for learning how to build and operate software with coding agents as part of the engineering workflow.

The interesting part is not simply that AI generated code. The engineering challenge was making agent-assisted development reliable inside a real codebase with architecture boundaries, tests, migrations, security constraints, deployment workflows, and production releases.

This repository explains that process publicly without exposing the private production codebase.

## Product scope

TowelBooks currently includes:

- account registration, authentication, email verification, password recovery, and session management;
- book discovery and catalog data backed by Open Library plus curated records;
- personal libraries, wishlists, and reading-progress tracking;
- ratings, reviews, comments, reactions, and public profiles;
- private reading rooms, memberships, roles, invitations, and room-owned readings;
- account privacy controls, data export, and account deletion workflows;
- a separate operator/admin application.

## Current architecture

```text
Browser
  |
  +-- Main web app -------- React + Vite -------- Cloudflare Pages
  |
  +-- Admin app ----------- React + Vite -------- Cloudflare Pages (operator surface)
  |
  +-- API ----------------- NestJS + Prisma ----- AWS ECS Fargate
                                      |
                                      +----------- PostgreSQL / RDS
                                      |
                                      +----------- S3 / SES / CloudWatch

Public assets ------------------------------------ Cloudflare R2
Edge / DNS / Tunnel / WAF ------------------------ Cloudflare
```

The codebase is a `pnpm` + `Nx` monorepo with shared schemas, a typed SDK, design-system packages, transactional email templates, infrastructure code, and architecture documentation.

More detail: [Architecture Overview](./architecture/overview.md)

## Agentic software development

TowelBooks was developed extensively with coding agents across:

- feature planning;
- implementation;
- debugging and refactoring;
- test creation and validation;
- database and migration work;
- infrastructure changes;
- code review;
- release preparation.

As the repository grew, I moved from ad-hoc prompting toward repository-native engineering workflows with explicit roles, scoped permissions, validation rules, structured handoffs, and source-of-truth documentation.

A simplified flow looks like this:

```text
Requirement / issue
      |
      v
   Planner
      |
      v
   Executor
      |
      v
 Validation gates
      |
      v
   Reviewer
      |
      v
 Human engineering judgment
      |
      v
 Merge / release / production
```

The current orchestration model separates planner, executor, reviewer, and coordinator responsibilities. Agent permissions are intentionally constrained by role, and validation is selected according to the risk and scope of the change.

More detail: [AI Engineering Workflow](./ai-engineering/workflow.md)

## Repository map

```text
architecture/
  overview.md          Production architecture and codebase boundaries
  infrastructure.md    AWS and Cloudflare deployment topology

ai-engineering/
  workflow.md          How agent-assisted development is structured
  agents.md            Agent roles, permissions, and handoffs
  validation.md        Testing and validation strategy

decisions/
  modular-monolith.md  Why the system is not split into unnecessary services
  monorepo.md          Why pnpm + Nx is used
```

## What I learned

A few conclusions from building TowelBooks this way:

1. **Agent capability is not the main bottleneck. Context quality is.** Clear boundaries, documentation, and acceptance criteria materially improve implementation quality.
2. **Validation must be part of the agent workflow, not an afterthought.** Agents are more useful when tests, type checks, architecture checks, and migration checks are explicit parts of task completion.
3. **Permissions and scope matter.** A planner should not behave like an executor, and an automated reviewer should not silently mutate the code it is evaluating.
4. **Large tasks benefit from orchestration, but orchestration itself creates complexity.** The workflow needs to remain simpler than the engineering problem it is solving.
5. **Human ownership does not disappear.** Product decisions, architecture, trade-offs, release decisions, and responsibility for production behavior still belong to the engineer.

## Related projects

- [AWS MCP Gateway](https://github.com/rafaself/aws-mcp-gateway) — security-focused, read-only MCP access to selected AWS account data.
- [OpenCode Gateway](https://github.com/rafaself/opencode-go-gateway) — local Go gateway translating Codex Responses traffic to alternative model backends.

## About

I am a Software Engineer and Electrical Engineer working primarily with TypeScript, Python, cloud infrastructure, developer tooling, and AI-agent-driven engineering workflows.

GitHub: [@rafaself](https://github.com/rafaself)
