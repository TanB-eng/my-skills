---
name: bot-eng
description: Use when shaping and building a multi-module, long-lived, or otherwise context-heavy AI-assisted software project from an idea to a usable MVP, especially when shared product and architecture context, serial business-module delivery, tests, and adaptive planning are needed. Do not use for one-off scripts or isolated edits unless explicitly requested.
---

# BoT_eng

BoT_eng is a lightweight, spec-first method for building software with AI. It keeps the useful parts of structured development - shared intent, clear business boundaries, incremental tests, and durable project memory - without forcing every task through a heavyweight toolchain.

The default goal is a working MVP. Make the process stricter only when complexity, risk, or the user's preference justifies it.

## Core Principles

1. Clarify the product and MVP before construction.
2. Design the architecture with the human; do not silently invent high-impact decisions.
3. Split the system by business capability, not by frontend/backend/database layers.
4. Build a thin vertical slice first and validate the core user journey early.
5. Work on one business module at a time when modules have meaningful boundaries.
6. Add or update tests with each module's behavior.
7. Update authoritative docs when product behavior, architecture, public contracts, or acceptance criteria change.
8. Use the lightest process that can produce reliable evidence.

Do not create documents, plans, approvals, or tool invocations only to satisfy a ceremony.

Keep implementation serial by default: do not have separate business modules writing code in parallel. Parallel research or review is acceptable only when it cannot change shared implementation state; merge the findings before the active module continues.

## Adaptive Modes

Choose a mode at project start and re-evaluate when risk changes.

### MVP mode (default)

Use for greenfield MVP work, simple or medium modules, and low-risk changes.

- Establish a project specification and architecture baseline.
- Implement modules serially with focused tests.
- Use module notes only when the current module needs them.
- Do not require OpenSpec, Superpowers, contract documents, or routine per-task approval.
- Do not add per-task approval gates. Pause for unresolved high-impact decisions, required authorization, or when the agreed baseline no longer works.

### Controlled mode

Use for a complex module, a material or unstable cross-module contract, a data migration, or a change with meaningful compatibility or rollback cost.

- Create a module brief and, when useful, a contract.
- Use OpenSpec for the module change.
- Review the generated planning artifacts before implementation.
- Keep tests and impact analysis proportional to the risk.

### Strict mode

Use only when the user explicitly asks for a rigorous workflow or the project has regulatory, safety, compatibility, or high-cost failure constraints.

- Use detailed module specs, OpenSpec artifacts, explicit approval gates, staged delivery checkpoints, and broader regression evidence.
- State the additional cost before adopting it.

Strict-mode implementation must not begin until the stated approval gates are satisfied.

Never force Controlled or Strict mode merely because a related skill is installed.

State the selected mode and why. Moving from MVP mode to Controlled or Strict mode requires a concrete risk or an explicit user preference; tell the user before adding the extra process. Moving to a lighter mode must not remove safeguards required by a known risk.

## Entry Routing

Use the global design phase for a new project or when the product and architecture baseline is missing or no longer trustworthy.

For an existing project with a usable baseline, read and validate the current docs and code instead of recreating them. For a bounded bug or small feature, preserve the existing architecture and use the smallest relevant part of the module loop. Do not turn a local change into a project redesign merely because this skill was invoked.

## Global Design Phase

Do not write product code during this phase. Explore the user's intent, then produce a concise but complete project baseline.

Discuss:

- problem and target users
- core user journey
- MVP capabilities
- explicit non-goals
- success and acceptance criteria
- architecture shape
- technology route and tradeoffs
- business modules and their order
- data flow, risks, and unresolved assumptions

Present the proposed direction in plain language so the user can correct it. Do not treat a vague acknowledgement as approval of an unshown document.

Create only the documents that carry useful information. A typical medium or large greenfield project uses:

```text
docs/
  PRD.md
  architecture.md
  tech-stack.md
  progress.md
  decisions.md
```

The files have these roles:

- `PRD.md`: product problem, users, core journey, MVP, non-goals, and acceptance criteria.
- `architecture.md`: architecture skeleton, business modules, order, dependencies, data ownership, and important cross-module rules.
- `tech-stack.md`: selected technologies, constraints, and reasons.
- `progress.md`: current state, completed work, tests, blockers, and next step.
- `decisions.md`: durable decisions that would otherwise be reconsidered.

A smaller MVP may combine product, architecture, and technology choices in `docs/spec.md` and keep a short `docs/progress.md`. Split documents only when independent ownership or navigation makes the separation useful.

Do not create empty files or a detailed document for every future module during the global phase. Create module notes and contracts when a module actually needs them.

## Global Baseline Review

Before presenting the baseline for approval, challenge it from an adversarial perspective. Look for a broken core journey, hidden assumptions, contradictory requirements, overlapping modules, circular dependencies, premature abstractions, unnecessary MVP scope, and technology choices that are expensive without supporting the product hypothesis. Revise the baseline when the critique exposes a real weakness.

Then review the baseline with the user. Check:

- every MVP capability has an owner module
- every module has a product reason
- the core user journey is end to end and testable
- the architecture is sufficient for the MVP but not designed for imaginary scale
- technology choices support the architecture and are explainable
- data ownership and dependency direction are understandable
- unresolved assumptions are visible

After the user explicitly approves the presented baseline, record a marker such as:

```text
Status: MVP baseline approved
Version: 0.1
```

This approval authorizes implementation of the agreed MVP. It does not authorize silent changes to product scope or architecture later.

When the user adjusts the direction, update the affected baseline documents before treating the revision as settled. Summarize what changed and what downstream module, test, or acceptance assumptions are affected.

## Module Decomposition

Split top-level modules by business capability:

```text
user-auth
file-management
document-parse
knowledge-retrieval
ai-chat
review-cards
admin
```

Avoid top-level modules named only `frontend`, `backend`, `database`, `components`, or `api`. A business module may contain its UI, API, service, data structures, and tests.

A useful module has:

- one primary business goal
- explicit responsibilities and non-responsibilities
- a clear owner for its state or data
- a small public surface
- understandable dependencies
- an independently testable outcome

Each persistent data set or stateful capability has one clear owner. Other modules should write through the owner's public interface rather than reaching into its private tables or services. A pragmatic MVP shortcut is acceptable only when it is visible, locally contained, and does not make the boundary misleading.

Do not split a module just because it contains frontend and backend code. Split when there are separate business goals, lifecycles, owners, or acceptance outcomes.

Cross-cutting foundations such as project setup, deployment, observability, or a shared design system may be explicit prerequisites or shared infrastructure. Do not present them as user-value modules, and create them only when the current MVP needs them, with a narrow owner and surface.

Keep dependencies understandable and preferably one-way. If two modules need each other's internals, revisit the boundary or extract a small shared contract; do not hide the cycle in imports or database access.

## MVP Implementation Loop

After the baseline is approved, implement the smallest useful vertical slice. The exact order depends on the project, but normally follows dependency order and user value rather than technical layers.

For each module:

1. Read the current global docs, the current module note and relevant contracts if they exist, repository state, and the preceding module's public outputs.
2. Confirm the module's goal, boundary, dependencies, and acceptance outcome.
3. Create `docs/modules/<module>.md` only if the module needs durable detail beyond `architecture.md`.
4. Decide whether MVP mode is sufficient or whether this module should be upgraded to Controlled mode.
5. Implement the smallest behavior that advances the core user journey.
6. Add or update focused tests for the behavior and important failure paths.
7. Run the focused tests and relevant integration checks; fix failures before moving on.
8. Update `progress.md` and update other docs only when project facts changed.
9. Move to the next module after the current outcome is usable and evidenced.

One module per context is a default organization technique, not a reason to create a large ceremony. A small module can be completed in one conversation. A large module should be split before implementation or upgraded to Controlled mode.

Do not start the next module until the active module's tests, progress entry, public outputs, and necessary handoff facts are current. If a later module needs an earlier module's internals, stop and repair the boundary or contract before continuing.

## Module Notes (On Demand)

When needed, create `docs/modules/<module>.md` with only the detail required to make the module understandable and transferable:

```text
Module goal
Responsibilities
Non-responsibilities
Dependencies and public outputs
Owned data or state
Important user and error paths
Acceptance criteria
Tests
Open questions
Current status
```

The module note is a durable summary, not a duplicate task tracker. Internal implementation details belong in code unless they affect consumers or future decisions.

## Contracts (On Demand)

Create `docs/contracts/<contract>.md` only when a capability is consumed across a meaningful module boundary. The contract should describe the current public obligation: inputs, outputs, errors, ownership, and compatibility rules.

The provider module owns the contract. Consumers depend on the contract, not the provider's internal files, private services, or tables. If a change affects product behavior, architecture, or a public contract, update the relevant authoritative doc before proceeding.

Do not create a contract file for every internal function or small local component.

## Tool Selection

Related skills are optional tools, not mandatory phases:

- Use brainstorming or another discovery aid when intent is unclear or the design has meaningful alternatives.
- Use OpenSpec when the current module has substantial behavior, multiple design decisions, a material or unstable public contract, migration risk, compatibility concerns, a security boundary, an irreversible operation, or a need for explicit change history. A small stable interface between two modules is not, by itself, enough to escalate.
- Use TDD when a behavior is risky or a failing example will clarify the implementation.
- Use systematic debugging when the cause of a failure is uncertain.
- Use a final verification or review skill when the change has meaningful user or system risk.
- Use Spec Kit when the user explicitly wants its project-level spec workflow.

Do not invoke these tools merely because they exist. Do not run two overlapping specification workflows for the same change.

## OpenSpec Escalation Path

When Controlled mode selects OpenSpec for a module, use an explicit change id and the smallest useful artifact set:

```text
/opsx:explore       when intent or risk still needs investigation
/opsx:new           create the change
/opsx:continue      create the next needed artifact
/opsx:ff            create all planning artifacts only when the change is already clear
/opsx:apply         implement approved tasks
/opsx:verify        check the implementation against the artifacts
/opsx:sync          update stable specs when needed
/opsx:archive       close the completed change
```

After OpenSpec creates the implementation tasks, pause and show:

- change id and artifact status
- task summary and expected file scope
- contract, data, migration, and compatibility impacts
- tests and acceptance checks
- unresolved assumptions and risks

Wait for the user's explicit approval before `/opsx:apply` or implementation code. This approval gate applies when OpenSpec is used; it does not make OpenSpec mandatory for every module.

If the user approves, implement only the agreed change. Keep artifacts current when the plan changes, then verify and reconcile the durable project docs.

If OpenSpec is unavailable, do not automatically downgrade a high-risk change. Use an equivalent concise module plan and approval gate, or ask the user how to proceed. A low-risk module may continue in MVP mode when OpenSpec was only a convenience.

## Documentation Synchronization

Treat docs as current project facts, not a diary of every edit.

If a small project uses the combined `docs/spec.md`, update its corresponding product, architecture, or technology section whenever the rules below refer to a split baseline document.

Update `PRD.md` when product scope, users, core journeys, or acceptance criteria change.

Update `architecture.md` when module boundaries, dependencies, data ownership, data flow, or system structure change.

Update `tech-stack.md` when a meaningful technology or deployment decision changes.

Update `progress.md` when a module starts, reaches a meaningful outcome, tests pass or fail, a blocker appears, or the next step changes.

Update `decisions.md` when a durable tradeoff or constraint is settled.

Do not create documentation work for private refactors, ordinary naming changes, CSS adjustments, or other changes that do not alter externally relevant facts.

When two documents disagree, identify the relevant authority, update the stale document, and record a decision if the disagreement exposed a real design change.

## Testing and MVP Convergence

Testing is part of module implementation, but test depth should match risk:

- focused unit or feature tests for local behavior
- integration tests at meaningful module boundaries
- a small smoke path for each critical MVP capability
- full core-journey validation before calling the MVP usable

If the project has no automated test harness, use the smallest reliable verification available and state the limitation. Add a test framework only when its value justifies the setup cost; do not turn a small MVP module into a tooling project solely to satisfy this skill.

Do not require a full regression suite after every tiny edit. Before MVP acceptance, run the available test suite, build checks, and the complete core user journey. Record known gaps rather than disguising them as completion.

MVP completion means:

- the core user journey works end to end
- the agreed MVP acceptance criteria are met
- important failure paths have useful behavior
- tests and build checks provide evidence
- remaining polish and non-MVP work is explicitly listed

Do not delay MVP for visual polish, speculative abstractions, rare edge cases, or future scale that is not part of the agreed hypothesis.

## Scope Changes and Rework

Pause and discuss before proceeding when implementation reveals that the approved product or architecture baseline is wrong, or when a change materially affects a public contract or compatibility guarantee, data migration, security boundary, or irreversible operation. Update the affected authoritative docs and obtain agreement on the new direction before continuing implementation.

For an ordinary bug, reproduce it, add a focused test, fix it, and run the relevant checks. For a complex or cross-module rework, use a new OpenSpec change or another appropriate planning artifact. Do not silently rewrite the baseline.

## Completion Check

Before claiming a module or MVP is complete, verify:

- the implemented behavior matches the current product and architecture facts
- the module's acceptance outcome is demonstrated
- focused tests and relevant integration checks ran
- the core user journey remains usable when applicable
- progress and any durable decisions are recorded
- public contracts or architecture docs are updated when they changed
- optional planning artifacts are reconciled when a stricter mode was used
- remaining gaps are explicit and not hidden behind a "complete" label
