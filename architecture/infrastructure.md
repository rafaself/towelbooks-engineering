# Infrastructure

TowelBooks uses AWS for application runtime services and Cloudflare for the public edge and web delivery layer.

## Production topology

```text
Users
  |
  +--> Cloudflare Pages -------- Main web application
  |
  +--> Cloudflare Pages -------- Admin application
  |
  +--> Cloudflare Tunnel ------- API entrypoint
                                  |
                                  v
                            AWS ECS Fargate
                                  |
                    +-------------+-------------+
                    |             |             |
                    v             v             v
                  RDS            S3            SES
               PostgreSQL     private data    email
                    |
                    +---------------------------+
                                                |
                                           CloudWatch

Public assets ------------------------------> Cloudflare R2
Container images ---------------------------> AWS ECR
```

## AWS responsibilities

The production repository currently uses AWS for:

- **ECS Fargate** for the API runtime;
- **RDS** for PostgreSQL persistence;
- **ECR** for container images;
- **S3** for private storage use cases;
- **SES** for transactional email;
- **CloudWatch** for runtime observability;
- **SSM Parameter Store** for runtime configuration/secrets integration.

Infrastructure definitions live with the codebase and are validated as part of the engineering workflow.

## Cloudflare responsibilities

Cloudflare owns much of the public-facing delivery layer:

- Pages for browser applications;
- DNS;
- Tunnel for API connectivity;
- R2 for public assets;
- WAF and rate limiting at the edge.

This split keeps static/browser delivery close to the edge while the application API and persistence remain on AWS.

## Deployment as an engineering boundary

Deployment configuration is treated as part of the application contract. Changes to infrastructure are reviewed with the same concern for reproducibility, least privilege, secret handling, and rollback as application code.

The private repository includes dedicated deployment and smoke-test workflows, infrastructure validation, and environment-specific controls. Production changes are not treated as an implicit side effect of editing application code.

## Why this matters for AI-assisted development

Infrastructure is an area where an apparently small autonomous change can have a large blast radius. The agent workflow therefore benefits from explicit boundaries:

- production mutation is not assumed to be allowed;
- secrets and credentials are outside source control;
- infrastructure changes require focused validation;
- deployment state and code state are treated separately;
- runtime topology is documented and checked.

The goal is not to prevent agents from working on infrastructure, but to make the permitted surface and validation requirements explicit.
