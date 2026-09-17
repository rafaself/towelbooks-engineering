# Agent Roles

The current TowelBooks workflow separates responsibilities across multiple agent roles instead of treating one unconstrained agent as planner, implementer, reviewer, and release operator at the same time.

## Coordinator / orchestrator

The coordinator owns the lifecycle of larger tasks. Its responsibility is coordination rather than direct product implementation.

Typical responsibilities include:

- understanding the task graph;
- deciding whether a planning step is needed;
- assigning bounded work;
- tracking dependencies and evidence;
- integrating completed work;
- determining when final validation is required.

The orchestrator is intentionally not the default place for arbitrary code edits.

## Planner

The planner is read-oriented and turns a requirement into an implementation plan.

Its job is to identify:

- affected boundaries;
- relevant source-of-truth documentation;
- implementation steps;
- validation requirements;
- meaningful risks or unresolved decisions.

A useful plan is specific enough for another agent to execute without re-solving the entire problem.

## Executor

The executor owns one bounded implementation task at a time.

The expected behavior is:

- follow the approved scope;
- change only the required surfaces;
- preserve documented architecture boundaries;
- run the smallest relevant validation;
- report what changed and what remains uncertain.

The executor is not allowed to redefine the product requirement simply because another implementation would be easier.

## Reviewer

The reviewer evaluates changes independently and is read-only in the normal review role.

Review is findings-first and focuses on areas such as:

- correctness;
- regressions;
- architecture boundaries;
- authorization and privacy;
- migration safety;
- test quality;
- infrastructure impact.

Keeping review separate from implementation reduces the tendency for the same agent to rationalize its own choices.

## Permission boundaries

Agent capabilities are scoped by role. This is deliberate.

Examples of the principle:

- planners do not need write access to implementation code;
- reviewers should not silently fix the change they are evaluating;
- production mutation is not granted as a default engineering capability;
- shell and repository operations are constrained according to the task.

Deny-by-default boundaries are more reliable than expecting every prompt to restate what an agent must not do.

## Why multiple agents?

Multiple roles are not inherently better than one strong agent. They are useful when they create a real separation of concerns.

The trade-off is additional orchestration complexity. For small tasks, a direct implementation flow is often better. The multi-role workflow is most useful for larger or higher-risk changes where planning, implementation, and independent review benefit from separation.
