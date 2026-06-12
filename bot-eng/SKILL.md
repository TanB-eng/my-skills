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
  PRD.md
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

## Hard Boundary Rules

Apply these rules when splitting modules and keep them enforced through implementation.

Data ownership:

- Every database table or persistent data structure has exactly one owner module.
- Declare the owner in `architecture.md` and in the owner's module file.
- Cross-module writes must go through the owner module's service or API, never direct table access.
- In small projects, cross-module reads may query the table directly; record that relaxation in `decisions.md` and tighten it if the project grows.

One-way dependencies:

- Module dependencies may only point to modules earlier in the module order.
- Circular dependencies are forbidden. If two modules need each other, merge them or extract the shared part into an earlier module.

Public contracts:

- Write cross-module APIs and shared data types down concretely (endpoint, request/response shape, and field types) in `docs/contracts/`, not just as prose rules. `architecture.md` records which module owns which contract.
- A later module may rely only on published contracts, never on another module's internals.

File ownership:

- Each module file lists the directories and files the module owns.
- A module context may create or modify only its owned files, shared contract files, and the global docs.

## Global Docs Phase

Start from the user's idea or PRD. If Superpowers skills are available, use them in this phase: use `brainstorming` to explore intent, requirements, and design alternatives before drafting any global doc, and use its conclusions as input to the clarifying questions and the docs.

Do not generate final docs immediately if requirements are unclear. First ask clarifying questions about:

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

- `docs/PRD.md`: product goal, users, scenarios, MVP, non-goals, user journeys, product acceptance criteria.
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

- `PRD.md`: goal, users, MVP, non-goals, user journey, acceptance criteria.
- `architecture.md`: module boundaries, dependencies, data flow, missing modules, circular dependencies.
- `tech-stack.md`: support for architecture, risk, overengineering, missing auth/storage/db/test/deploy choices.

Then run cross-document consistency:

- Every core product function has a module owner.
- Every architecture module has a product reason.
- Every database table or persistent data structure has exactly one owner module.
- The module dependency graph is one-way: no cycles, no dependency on a later module.
- Every cross-module interaction has a concrete written contract.
- Each module declares the files and directories it owns.
- The tech stack can implement the architecture.
- Cross-cutting concerns are covered: auth, permissions, errors, logging, testing, deployment, environment variables, data migration.
- The first module can start without unresolved global blockers.

Finally, perform module backtesting: simulate starting each module using only the global docs and list missing information. Fix global docs until the first module is actionable.

## OpenSpec Module Engine

Use `$openspec` as the standard execution engine inside every business module. BoT_eng and OpenSpec have different ownership:

```text
BoT_eng
  Owns project-wide truth, module order, boundaries, cross-context memory,
  progress, durable decisions, and the handoff to the next module.

OpenSpec
  Owns the current module's change lifecycle: exploration, proposal/spec,
  design, tasks, implementation, verification, sync, and archive.
```

Do not let OpenSpec replace the BoT_eng global docs. Do not copy every OpenSpec artifact into `docs/modules/<module>.md`.

Each concern has exactly one authoritative source. When two files disagree, the authoritative one wins and the other must be corrected to match it:

```text
Product scope                                  docs/PRD.md
System structure, module relations,
  data ownership                               docs/architecture.md
Technical constraints                          docs/tech-stack.md
Cross-module public contracts                  docs/contracts/*
Module boundaries and final summary            docs/modules/<module>.md
Current module change process                  openspec/changes/<change-id>/
Stable module behavior specs (after sync)      openspec/specs/*
Active implementation task status              OpenSpec tasks
Project-level milestone status                 docs/progress.md
Durable decisions and reasons                  docs/decisions.md
```

Use one explicit kebab-case OpenSpec change id for the active module, normally:

```text
module-01-user-auth
module-02-file-upload
```

If a module cannot fit one conversation or one coherent OpenSpec change, split the module before implementation. If the module is cohesive but requires several delivery increments, use multiple sequential OpenSpec changes under the same module context and record them in the module file.

If `$openspec` or the OpenSpec CLI is unavailable, stop before module implementation and tell the user what is missing. Do not silently replace the required module workflow with an improvised process.

Superpowers skills may still be used for brainstorming, TDD, debugging, and verification when applicable. Spec Kit is optional for project-level specification work, but do not run a duplicate Spec Kit lifecycle inside a module already managed by OpenSpec unless the user explicitly requests it.

## Per-Module Context Flow

Process modules serially. Do not parallelize modules unless the user explicitly asks and contracts are stable.

For each new module context, load only:

```text
docs/PRD.md
docs/architecture.md
docs/tech-stack.md
docs/progress.md
docs/decisions.md
docs/modules/<current-module>.md
```

Use the following lifecycle for every module.

### 1. Recover Context

Read the global docs, current module file, Git status/diff, and the latest OpenSpec status. Determine whether this is a new module, an interrupted module, or a completed module awaiting handoff.

Do not rely on hidden conversation memory. If current state is not represented in files or Git, document it before continuing.

### 2. Refine the Module Charter

Before opening an OpenSpec change, refine `docs/modules/<current-module>.md` enough to establish the module's durable boundaries. Do not write implementation code yet.

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
## 8. Database Tables / Data Structures (Owned by This Module)
## 9. Owned Files and Directories
## 10. Public Contract (APIs and Types Other Modules May Use)
## 11. Dependencies on Existing Modules
## 12. Future Modules Depending on This Module
## 13. Error and Edge Cases
## 14. Testing Method
## 15. Acceptance Criteria
## 16. Implementation Stages (Summary Only; OpenSpec Tasks Are Authoritative)
## 17. Current Status
## 18. Open Questions / Follow-Ups
```

Run the module-boundary gate before creating implementation tasks:

- one primary business goal
- no responsibility overlap with another module
- explicit owned data and owned files
- concrete public contracts
- one-way dependencies with no cycles
- independently testable with mocks or test doubles
- small enough to understand, implement, test, and document in one context

### 3. Open the OpenSpec Change

Invoke `$openspec` for the current module.

1. Use `/opsx:explore` when requirements, boundaries, or risks still need discussion.
2. Use `/opsx:new <change-id>` to create the module change.
3. Use `/opsx:continue <change-id>` to create artifacts in dependency order, or `/opsx:ff <change-id>` only when the module is already clear and straightforward.
4. Ensure the OpenSpec proposal/spec/design/tasks are consistent with the global docs and module charter.
5. Do not start `/opsx:apply` while unresolved questions, contract ambiguity, or missing acceptance criteria remain.

The OpenSpec artifacts must cover:

- behavior and scenarios
- module-local design
- public contract changes
- data ownership changes
- implementation tasks
- testing and regression scope
- migration or rollback needs where applicable

### 4. Implement Through OpenSpec

Use `/opsx:apply <change-id>` to execute the approved tasks.

During implementation:

1. Implement only the current module.
2. Keep changes within the module's owned files, shared contract files, and global docs.
3. Keep OpenSpec tasks current as work is completed or revised.
4. If a public contract changes, pause implementation, perform impact analysis, update affected artifacts and docs, then revalidate downstream assumptions.
5. Run module-specific tests after each verifiable implementation stage.
6. Commit after each completed task group, with the module name and OpenSpec change id in the commit message. Do not leave a whole module's work in a single uncommitted pile.
7. Run the full cumulative smoke suite and affected integration tests before module completion.

### 5. Verify and Reconcile

Use `/opsx:verify <change-id>` before declaring the module complete.

Verification must confirm:

- implementation matches the OpenSpec artifacts
- module acceptance criteria pass
- public contracts match `docs/contracts/`
- file boundaries were respected: run `git diff --name-only` from the module's start commit and compare against the module's owned files, contract files, and global docs; any file outside that list must be justified in the module file or reverted
- the module's core acceptance scenarios exist as automated smoke tests in the cumulative suite
- the full cumulative smoke suite passes
- no unresolved OpenSpec task is being hidden as completed

Then reconcile durable documentation:

1. Update `docs/modules/<module>.md` with the final implemented behavior, public contract, tests, status, and remaining follow-ups.
2. Update `docs/progress.md` with completed work, test evidence, blockers, and the next module.
3. Update `docs/decisions.md` with durable product, architecture, contract, or technology decisions.
4. Update `docs/architecture.md`, `docs/tech-stack.md`, or `docs/PRD.md` only when project-wide truth changed.

### 6. Sync, Archive, and Handoff

After reconciliation:

1. Use `/opsx:sync <change-id>` when delta specs must become main OpenSpec specs.
2. Use `/opsx:archive <change-id>` only after verification and documentation updates succeed.
3. Record the archived OpenSpec change id in the module file and `progress.md`.
4. Commit at a verifiable module milestone.
5. Start the next module in a new conversation using the latest global docs, its module file, and only the public contracts it depends on.

Never enter the next module merely because implementation code exists. The current module must pass the OpenSpec verification, documentation reconciliation, and handoff gates.

## Cumulative Regression Net

Each module must leave behind automated proof that it still works, so later modules cannot silently break earlier ones.

- Before a module is completed, convert its core acceptance scenarios into automated smoke tests, kept in one cumulative suite (for example `tests/smoke/<module>/`).
- Before completing any module, run the full cumulative suite: every previous module's smoke tests, not a hand-picked "affected" subset.
- If an earlier module's smoke test fails, module completion is blocked. Treat the current module as the cause until proven otherwise.
- Keep smoke tests cheap: a few high-value end-to-end paths per module, so the full run stays fast enough to execute every time.
- Record the suite result in `docs/progress.md` as part of the module's test evidence.

## Reopening a Completed Module

When a bug or design flaw is found in an archived module, do not patch it informally from another module's context.

1. Pause the current module at a committed point and record the handoff in `docs/progress.md`.
2. Start a dedicated fix context loading the global docs plus the affected module's file.
3. Create a new OpenSpec change named `module-<nn>-<name>-fix-<short-slug>`.
4. Run impact analysis first: list downstream modules that depend on the module's public contracts or owned data, and decide whether the fix changes any contract.
5. If a contract changes, update `docs/contracts/*` first, then revalidate each affected downstream module against the new contract.
6. Implement the fix, run the module's own tests, then run the full cumulative smoke suite.
7. Reconcile docs: update the module file's final summary and status, `docs/progress.md`, and `docs/decisions.md` (record why the rework was needed), then sync and archive the fix change.
8. Resume the paused module by re-reading the updated docs, not from conversation memory.

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

Update `PRD.md` when:

- MVP scope, user roles, core journeys, or product acceptance criteria change

Keep memory files small so each new module context stays cheap to load:

- `progress.md` keeps only the current phase plus the last 2-3 modules; move older entries to `docs/progress-archive.md`.
- `decisions.md` keeps one short entry per decision; when a decision is superseded, rewrite the entry instead of appending a new contradicting one.

## Standard Prompts

Global start:

```text
Use $bot-eng. Do not write code yet. Use the Superpowers brainstorming skill to explore my project idea, then ask clarifying questions about product goal, target users, MVP scope, non-goals, user journeys, modules, data flow, risks, and acceptance criteria. After I answer, create docs/PRD.md, docs/architecture.md, docs/tech-stack.md, docs/progress.md, and docs/decisions.md.
```

Global validation:

```text
Use $bot-eng to validate docs/PRD.md, docs/architecture.md, and docs/tech-stack.md. Check file quality, cross-document consistency, missing module owners, missing product reasons, unsupported technical choices, cross-cutting concerns, and whether Module 1 can start.
```

Module start:

```text
Use $bot-eng and $openspec. Read docs/PRD.md, docs/architecture.md, docs/tech-stack.md, docs/progress.md, docs/decisions.md, and docs/modules/<module>.md. Current context handles only this module. Recover any interrupted Git/OpenSpec state, then refine the module charter. Do not code yet. Confirm responsibilities, non-responsibilities, owned data, owned files, public contracts, one-way dependencies, tests, acceptance criteria, and module size. After the boundary gate passes, start an explicit OpenSpec change for this module.
```

Module implementation:

```text
Use $bot-eng and $openspec. Continue the current module's explicit OpenSpec change. Implement through /opsx:apply, keep tasks current, commit per completed task group, and modify only module-owned files plus approved contracts/docs. Run module tests, affected integration tests, and the full cumulative smoke suite, then check the boundary diff against owned files. Then use /opsx:verify, reconcile docs/modules/<module>.md, docs/progress.md, and docs/decisions.md, sync specs when needed, and archive only after all gates pass.
```

## Completion Check

Before claiming a module is complete, verify:

- the module was executed through an explicit OpenSpec change
- OpenSpec proposal/spec/design/tasks reflect the implemented module
- `/opsx:verify` completed successfully or any accepted exception is documented
- module acceptance criteria pass
- relevant tests ran and results are known
- the module's smoke tests were added to the cumulative suite and the full suite passes
- work was committed per task group and the boundary diff check ran clean
- public contracts, data ownership, and file ownership are consistent
- `progress.md` records the result and next step
- `decisions.md` records new durable decisions
- the module file records the final behavior and archived OpenSpec change id
- global docs are updated if the module changed global truth
- the OpenSpec change was synced when needed and archived
- the next module can start from the latest docs without relying on hidden conversation context
