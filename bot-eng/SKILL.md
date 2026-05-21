---
name: bot-eng
description: Use when planning or executing medium-to-large AI-assisted software projects that need reliable docs, module boundaries, serial module contexts, progress memory, decisions records, Spec Kit/OpenSpec-style specs, or reusable project execution SOPs.
---

# BoT_eng

BoT_eng is a docs-driven, serial module development method for large AI-assisted projects. Use it to turn a broad idea into reliable global docs, then implement one business module per context with explicit module specs, tests, progress updates, and decision records.

## Core Rule

Do not ask an agent to build the whole project at once. Establish global docs first, validate them, then process one business module at a time.

Use this canonical document layout:

```text
docs/
  product.md
  architecture.md
  tech-stack.md
  progress.md
  decisions.md

  modules/
    00-project-setup.md
    01-<business-module>.md
    02-<business-module>.md
```

## Module Split Rule

Split top-level modules by business capability, not by technical layer.

Prefer:

```text
user-auth
file-upload
document-parse
knowledge-base
ai-chat
review-cards
admin
deployment
```

Avoid top-level splits like:

```text
frontend
backend
database
components
api
```

Reason: business modules map to user value, boundaries, and acceptance tests. Inside each business module, still specify frontend, backend, service, database, API, and tests.

Technical modules are allowed only for cross-cutting foundations such as project setup, UI system, database baseline, model router, logging, observability, or deployment.

## Global Docs Phase

Start from the user's idea or PRD. Do not generate final docs immediately if requirements are unclear. First ask clarifying questions about:

- product goal
- target users
- MVP scope
- explicit non-goals
- core user journeys
- business modules
- data flow
- technical risks
- acceptance criteria

Then create:

- `docs/product.md`: product goal, users, scenarios, MVP, non-goals, user journeys, product acceptance criteria.
- `docs/architecture.md`: system overview, business module list, module dependencies, data flow, API contract rules, directory principles, module order.
- `docs/tech-stack.md`: frontend, backend, database, storage, AI/model layer, deployment, testing, and reasons for each choice.
- `docs/progress.md`: current phase, completed work, test results, blockers, next step.
- `docs/decisions.md`: important product or technical decisions with reasons.

Mark global docs with a version and status, for example:

```text
Status: Approved for Module 1
Version: 0.1
```

Treat global docs as stage-stable, not final. Update them when later module work changes global truth.

## Global Validation Gate

Before implementing any module, validate the global docs.

Check each file:

- `product.md`: goal, users, MVP, non-goals, user journey, acceptance criteria.
- `architecture.md`: module boundaries, dependencies, data flow, missing modules, circular dependencies.
- `tech-stack.md`: support for architecture, risk, overengineering, missing auth/storage/db/test/deploy choices.

Then run cross-document consistency:

- Every core product function has a module owner.
- Every architecture module has a product reason.
- The tech stack can implement the architecture.
- Cross-cutting concerns are covered: auth, permissions, errors, logging, testing, deployment, environment variables, data migration.
- The first module can start without unresolved global blockers.

Finally, perform module backtesting: simulate starting each module using only the global docs and list missing information. Fix global docs until the first module is actionable.

## Optional Spec Tooling

If relevant project skills are available, use them as helpers, not as replacements for the `docs/` canon.

- Use Superpowers skills for brainstorming, writing plans, debugging, TDD, and verification when they apply.
- Use Spec Kit / `speckit-*` skills for specify, clarify, plan, tasks, analyze, and implement phases when the user wants spec-driven development.
- Use OpenSpec or equivalent change-spec tools for module-level capability discussion, proposal/change records, and acceptance checks if an OpenSpec skill or CLI is available.

If Spec Kit or OpenSpec is unavailable, continue with the BoT_eng markdown files. Do not block the workflow.

Mapping:

```text
product.md       ~= product spec / feature intent
architecture.md  ~= technical plan and module contracts
tech-stack.md    ~= implementation constraints
modules/xx.md    ~= module-local spec, plan, tasks, and acceptance checks
progress.md      ~= execution memory
decisions.md     ~= ADR-like decision log
```

## Per-Module Context Flow

Process modules serially. Do not parallelize modules unless the user explicitly asks and contracts are stable.

For each new module context, load only:

```text
docs/product.md
docs/architecture.md
docs/tech-stack.md
docs/progress.md
docs/decisions.md
docs/modules/<current-module>.md
```

First refine the module file. Do not write implementation code until the module file is clear enough to develop and test.

Each module file should contain:

```text
# Module Name

## 1. Module Goal
## 2. Responsible Features
## 3. Explicit Non-Responsibilities
## 4. User Journey
## 5. Frontend Pages / Components
## 6. Backend APIs
## 7. Services / Business Logic
## 8. Database Tables / Data Structures
## 9. Dependencies on Existing Modules
## 10. Future Modules Depending on This Module
## 11. Error and Edge Cases
## 12. Testing Method
## 13. Acceptance Criteria
## 14. Implementation Checklist
## 15. Current Status
## 16. Open Questions / Follow-Ups
```

After the module file is confirmed:

1. Implement only the current module.
2. Keep changes within the module's declared scope.
3. Run module-specific tests.
4. Run any integration tests affected by the module.
5. Do not start the next module until tests pass or the user accepts the known risk.
6. Update `docs/progress.md`.
7. Update `docs/decisions.md` for important choices.
8. Update `docs/architecture.md`, `docs/tech-stack.md`, or `docs/product.md` only if global truth changed.

## Update Rules

Update `progress.md` when:

- a module starts or finishes
- tests pass or fail
- a blocker appears
- the next module changes

Update `decisions.md` when:

- a product scope tradeoff is made
- a technical choice is settled
- an interface rule is chosen
- a recurring discussion should not be reopened

Update `architecture.md` when:

- modules are added, removed, or reordered
- module dependencies change
- API contracts change
- core data flow changes
- directory structure or cross-module rules change

Update `tech-stack.md` when:

- a framework, database, storage system, model provider, deployment platform, or testing strategy changes

Update `product.md` when:

- MVP scope, user roles, core journeys, or product acceptance criteria change

## Standard Prompts

Global start:

```text
Use $bot-eng. Do not write code yet. Read my project idea and first ask clarifying questions about product goal, target users, MVP scope, non-goals, user journeys, modules, data flow, risks, and acceptance criteria. After I answer, create docs/product.md, docs/architecture.md, docs/tech-stack.md, docs/progress.md, and docs/decisions.md.
```

Global validation:

```text
Use $bot-eng to validate docs/product.md, docs/architecture.md, and docs/tech-stack.md. Check file quality, cross-document consistency, missing module owners, missing product reasons, unsupported technical choices, cross-cutting concerns, and whether Module 1 can start.
```

Module start:

```text
Use $bot-eng. Read docs/product.md, docs/architecture.md, docs/tech-stack.md, docs/progress.md, docs/decisions.md, and docs/modules/<module>.md. Current context handles only this module. Do not code yet. First refine the module file: responsibilities, non-responsibilities, UI, APIs, services, data structures, dependencies, tests, acceptance criteria, checklist, and open questions.
```

Module implementation:

```text
Use $bot-eng. Implement only the current module according to the global docs and docs/modules/<module>.md. Run tests. Update docs/progress.md and docs/decisions.md. Update global docs only if product, architecture, or tech-stack truth changed.
```

## Completion Check

Before claiming a module is complete, verify:

- module acceptance criteria pass
- relevant tests ran and results are known
- `progress.md` records the result and next step
- `decisions.md` records new durable decisions
- global docs are updated if the module changed global truth
- the next module can start from the latest docs without relying on hidden conversation context
