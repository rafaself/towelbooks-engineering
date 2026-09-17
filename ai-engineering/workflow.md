# AI Engineering Workflow

TowelBooks was developed extensively with coding agents. The workflow evolved from direct task prompting into a repository-native system designed to make agent-assisted engineering more predictable and reviewable.

## What agents are used for

Coding agents have been used across:

- feature planning;
- implementation;
- debugging and refactoring;
- test creation;
- API and schema work;
- database and migration changes;
- infrastructure changes;
- code review;
- release preparation.

The important distinction is that agents operate inside an engineering system. They do not replace architecture, validation, or ownership.

## Simplified delivery flow

```text
Issue / requirement
       |
       v
   Planning
       |
       v
 Implementation
       |
       v
 Focused validation
       |
       v
    Review
       |
       v
 Integration validation
       |
       v
 Human release decision
```

For larger work, the repository supports a more explicit orchestration model with coordinator, planner, executor, and reviewer responsibilities.

## Repository-native context

One of the largest improvements came from moving important context out of chat history and into the repository.

The private codebase contains:

- repository-wide and subtree-specific agent instructions;
- reusable engineering skills and checklists;
- workflow definitions;
- architecture and product source-of-truth documentation;
- validation commands and automated policy checks;
- structured contracts for larger orchestrated tasks.

This reduces dependence on a single prompt containing everything an agent needs to know.

## Smallest-relevant validation

Not every code change should run every test in the repository.

The workflow prefers the smallest validation set that gives meaningful evidence for the change. Broader suites are reserved for cross-cutting, high-risk, integration, release, or explicitly required work.

This keeps feedback loops practical while still making validation part of the definition of done.

## Structured handoffs

When work is split across roles, the output of one agent becomes an explicit input to another rather than an informal assumption.

A planner should produce an executable plan. An executor should report what changed and what was validated. A reviewer should evaluate the resulting change independently.

This separation helps prevent one agent from both creating a change and implicitly declaring its own work correct.

## Human responsibility

The workflow is intentionally agent-heavy, but human responsibility remains central.

I retain ownership of:

- product direction;
- architecture decisions;
- scope and trade-offs;
- security and privacy decisions;
- release decisions;
- production behavior.

The goal is leverage, not removing engineering judgment from the loop.
