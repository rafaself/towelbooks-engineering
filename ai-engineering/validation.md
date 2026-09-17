# Validation Strategy

Agent-assisted development increases the value of automated validation. The faster code can be produced, the more important it becomes to make correctness, architecture, and safety checks part of the normal workflow.

## Validation layers

TowelBooks uses multiple layers of validation rather than relying on one test command.

Examples include:

- linting and formatting checks;
- TypeScript type checking;
- unit tests;
- database-backed integration tests;
- API and contract tests;
- architecture and workspace-boundary checks;
- migration safety checks;
- authorization and privacy regressions;
- security scanning;
- infrastructure validation;
- deployment smoke checks;
- AI workflow and orchestration checks.

## Focused first, broad when justified

The default principle is to run the smallest validation that gives meaningful evidence for a change.

For example, a localized UI change should not automatically require the same validation profile as a persistence migration or infrastructure modification. Broader suites are used when the change is cross-cutting, high-risk, release-related, or otherwise requires integration-level confidence.

This is important for coding agents because an instruction like “run the tests” is underspecified in a large repository. The repository instead documents and encodes which checks are relevant to different classes of work.

## Dedicated integration database

Database-backed integration tests use a dedicated PostgreSQL test database rather than the normal local development database.

This protects developer data and makes destructive integration-test setup explicit. The normal workspace test command does not silently truncate the development database.

## Architecture checks

Some rules are tested directly rather than left as documentation only.

Examples include enforcing package/application boundaries and checking repository topology or migration constraints. This is particularly valuable with autonomous implementation because it catches changes that may compile and pass local tests while still violating architecture rules.

## Security and privacy

TowelBooks includes user accounts, social content, private rooms, account privacy settings, and data-management workflows. Validation therefore extends beyond functional output.

Changes may need evidence around:

- authorization;
- public/private visibility;
- response redaction;
- rate limiting;
- secret handling;
- dependency and static-analysis findings;
- migration behavior.

## Evidence, not ceremony

Validation is intended to produce evidence proportional to risk. Running every check for every change creates slow feedback and encourages people—or agents—to skip the process entirely.

The better target is a workflow where the relevant checks are cheap enough to run routinely and broad certification happens when the change actually requires it.
