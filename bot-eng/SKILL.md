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
- For a new project, present the baseline and readiness assessment together for one explicit approval before product implementation. For an existing usable baseline, verify it without requesting repeat approval of settled scope.
- Do not turn module boundaries, new conversations, task summaries, or low-risk implementation choices into approval gates.

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

## MVP Interaction Model

After a new MVP baseline is approved or an existing baseline is validated, proceed through low-risk modules without repeatedly asking the user to approve each module, task, file list, or test plan.

At the start of a low-risk module, briefly state:

```text
Goal
Scope
Verification
```

This is a progress update, not an approval request. Continue implementation unless the user asks to pause or a real escalation condition appears.

Surface runnable results at meaningful checkpoints, especially the first end-to-end slice, so the user can redirect early. Treat the demonstration as feedback opportunity, not a mandatory stop, unless the user requests a checkpoint.

Do not create detailed task cards, requirement IDs, traceability tables, or file-scope manifests for a simple module. Add them only when their absence would create meaningful ambiguity, coordination risk, audit needs, or expensive rework. If a short explanation in the conversation is enough, do not persist another artifact.

Pause for user input only when required authorization is missing, a high-impact decision remains unresolved, the approved product or architecture baseline must change, or Controlled or Strict mode requires its stated gate. After implementation, present the working core journey and verification evidence for final MVP acceptance.

## Entry Routing

Use the global design phase for a new project or when the product and architecture baseline is missing or no longer trustworthy.

For an existing project with a usable baseline, read and validate the current docs and code instead of recreating them. For a bounded bug or small feature, preserve the existing architecture and use the smallest relevant part of the module loop. Do not turn a local change into a project redesign merely because this skill was invoked.

## Global Design Phase

Explore the user's intent and prepare a concise project baseline before product implementation. Read-only investigation and static interaction sketches belong here. A runnable prototype or feasibility probe before baseline approval needs an explicitly authorized, bounded scope; keep it isolated from production behavior and label its simulated parts. Probe authorization does not approve the MVP implementation.

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

Separate user requirements, repository or experimental evidence, and unverified assumptions. Ask only questions whose answers materially affect scope, core interactions, architecture, risk, or acceptance. Resolve technical questions through available evidence first; explain consequential choices to the user. Avoid repeating answered questions or filling a question quota, but do not hide a blocking ambiguity just to stop asking.

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

Do not create empty files or detailed notes for every future module. Capture the core interactions and necessary interface agreements during baseline design; defer unrelated interfaces and module implementation details.

## Interaction and Interface Design

For a product with unclear user interactions, use a simple sketch, screen outline, or existing UI reference to walk through the core journey. Show the relevant actions, input, result, and important loading, empty, and error states. For a backend or CLI project, use request/response or input/output examples instead. Skip new prototypes when the interactions are already clear; visual polish is not a prerequisite.

Refine the interaction, architecture, and core interface agreements together. Define the exchanges needed by the MVP journey before the frontend and backend implement them; see Core API and Module Contracts below. Account for business rules and data ownership, rather than inventing one endpoint for every button. Technical constraints may require revising the interaction.

Keep interaction details in the existing product spec, or use `docs/design.md` only when separate navigation helps. Preserve existing document names and link to prototypes and interface definitions rather than duplicating them. These are design responsibilities, not mandatory separate agents, documents, or approval stages.

## Global Baseline Review

Before presenting the baseline for approval, challenge it from an adversarial perspective. Check the core journey against the interaction, interface agreements, and architecture. Look for contradictions, hidden assumptions, overlapping modules, circular dependencies, unnecessary MVP scope, and premature abstractions. Fix substantive gaps, then assess readiness:

- Value and scope: identify the user, problem, complete core journey, MVP boundaries, and observable acceptance outcomes.
- Interaction and interfaces: make the core actions and important failure outcomes understandable; define needed exchanges or explain why no separate interface applies.
- Ownership and feasibility: give each MVP capability and persistent data set an owner, explain dependencies and technology choices, and expose risks that could invalidate the whole MVP.
- First slice: identify a small runnable outcome, its dependencies, and a concrete way to verify it.
- Unknowns: distinguish blockers, reversible assumptions with validation plans, and intentionally deferred work.

Report one of these outcomes with evidence from the relevant spec sections, code, documentation, or actual experiments. State when evidence is missing; do not use a completeness score or document count as proof:

| Outcome | Meaning and next step |
| --- | --- |
| Ready | Readiness criteria are satisfied; no known MVP viability blocker remains, and the first slice is actionable. Present the baseline for approval. |
| Ready with assumptions | Remaining assumptions are bounded and reversible. Record what is assumed, how and when it will be tested before dependent work, and the fallback or reassessment trigger; include them in the baseline approval. |
| Not ready | A material contradiction, untestable core outcome, unclear ownership, or unresolved viability risk blocks product implementation. Name the blocker and the smallest clarification or authorized probe that can resolve it. |

An easy first screen does not offset a critical unknown about the rest of the MVP. Investigate such unknowns before investing in dependent work. Passing this review establishes readiness to attempt implementation, not proven demand, technical success, or user approval.

Keep a compact assessment in `docs/progress.md`: outcome and basis, first slice and verification, blockers or assumptions with validation timing, and the next action. Link to existing facts instead of duplicating them. After fixes, recheck the affected criteria; stop expanding planning when readiness is established.

Run this assessment at the initial baseline and after material baseline changes. A complex Controlled change may use the same criteria locally within its existing plan and approval. Do not repeat the full review or request approval merely because a module or conversation changes.

After the user explicitly approves the presented baseline, record a marker such as:

```text
Status: MVP baseline approved
Version: 0.1
```

This approval covers the presented product scope, interactions, architecture, core interface agreements, and disclosed assumptions. It authorizes implementation of that MVP, not silent scope changes, deployment, or unrelated external actions.

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

After the baseline is approved, implement the smallest useful vertical slice. Follow dependency order and user value; finish each module's current MVP outcome, not every future feature, so the core journey becomes runnable early.

For each module:

1. Read the current baseline and interactions, module note and relevant contracts if present, repository state, and dependency modules' public outputs.
2. Check the module's goal, boundary, dependencies, acceptance outcome, and assumptions due for validation. For a low-risk module, briefly state its goal, scope, and verification method, then continue without waiting for approval.
3. Create `docs/modules/<module>.md` only if the module needs durable detail beyond the baseline.
4. Decide whether MVP mode is sufficient or whether this module should be upgraded to Controlled mode.
5. Implement the smallest behavior that advances the core journey, using the order described below.
6. Add or update focused tests alongside implementation for the behavior and important failure paths.
7. Connect the real components and run focused tests and relevant integration checks, including affected existing behavior; fix failures before moving on.
8. Update `progress.md` and update other docs only when project facts changed.
9. Move to the next module after the current outcome is usable and evidenced.

For a UI-driven module with understood technical dependencies, the default order is frontend interaction (using clearly identified mock data if needed), backend logic and persistence, then real integration. Keep both sides aligned with the agreed interface and test while building. Do not finish all modules' frontend implementations before integrating any backend.

When the main uncertainty is an algorithm, AI output quality, an external service, or data behavior, validate that capability first. For backend-only or local tools, omit unnecessary frontend or network layers. Choose the order that resolves the biggest current uncertainty with the least work; do not create a new review gate just to select it.

One module per context is a default organization technique, not an approval gate or a reason to create a large ceremony. A small module can be completed in one conversation. A large module should be split before implementation or upgraded to Controlled mode. When changing contexts, leave a compact handoff and let the next context continue from the approved baseline without reopening settled design decisions unless new evidence conflicts with them.

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

## Core API and Module Contracts

During baseline design, agree on the interfaces used by the MVP's core journey, including frontend/backend exchanges inside one business module. Reuse and check existing interfaces against their implementation. Do not invent a backend or network API where the product has none.

Describe the operation and owner, endpoint/method or callable boundary, input/output shapes and field meanings, required fields, relevant access rules, and success/error examples. Include pagination, asynchronous states, retry or duplicate-submission behavior only where applicable. This must be enough to implement and test both sides without guessing core behavior; private schemas and future endpoints can wait.

Keep one authoritative definition. Use the project's existing interface schema (such as OpenAPI) if present; otherwise a short section in `docs/spec.md` or `docs/contracts/api.md` is sufficient. Split into `docs/contracts/<contract>.md` only when useful. The architecture links to interface owners and definitions; module notes reference them instead of copying field lists.

The provider owns the interface. Consumers use its public surface, not private files, services, or tables. When an interface changes, update its authority, affected consumers, mocks, and relevant tests together. Material product or compatibility changes follow Scope Changes and Rework; implementing an already approved contract does not require another approval. A small stable interface does not by itself require OpenSpec.

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

Update the interaction section or `docs/design.md` when agreed user behavior changes, and the authoritative interface definition when exchanges change. Keep both consistent with product scope and actual implementation.

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

Do not require a full regression suite after every tiny edit. Before MVP acceptance, run the available test suite, build checks, and the complete core user journey. When the journey relies on a backend, persistence, or external integration, demonstrate that connection and any required persistence; a mock-only demo is not evidence of full completion. If a real dependency is unavailable, report the result as partial and identify the unverified path. Record known gaps rather than disguising them as completion.

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
