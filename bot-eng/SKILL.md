---
name: bot-eng
description: Use when shaping and building a multi-module, long-lived, or otherwise context-heavy AI-assisted software project from an idea to a usable MVP, especially when shared product and architecture context, serial business-module delivery, tests, and adaptive planning are needed. Do not use for one-off scripts or isolated edits unless explicitly requested.
---

# BoT_eng

BoT_eng is a lightweight, spec-first method for building software with AI. It keeps the useful parts of structured development - shared intent, clear business boundaries, incremental tests, and durable project memory - without forcing every task through a heavyweight toolchain.

The default goal is a working MVP. Make the process stricter only when complexity, risk, or the user's preference justifies it.

## Workflow at a Glance

| Stage | Working result |
| --- | --- |
| Product positioning | A concise PRD draft covering the problem, current alternatives, core journey, MVP, non-goals, constraints, and acceptance outcomes. |
| Interaction and visual direction | Relevant screens or input/output examples, important states, and enough design guidance to implement consistently. Feed discoveries back into the product draft. |
| Development baseline | Reconcile product, design, business modules, technology, data ownership, and core interfaces in `docs/`; assess readiness and present the baseline for one approval. |
| Small-step implementation | Work through an ordered task queue within serial business modules, test and integrate real components, apply the five completion checks, and retain verified Git checkpoints when authorized. |
| MVP acceptance and delivery | Validate the full core journey, reconcile docs with the actual implementation, prepare usable setup and delivery instructions, and present evidence and gaps for acceptance. Deploy only within the requested scope. |

These stages can loop back as evidence changes. Reuse settled work, omit inapplicable UI or deployment work, and avoid adding an approval between each stage. A prototype clarifies behavior and appearance; it does not prove technical feasibility or a working integration.

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

Include any important existing behavior that must be preserved in the scope. For later small tasks within the module, refer back to that context and state only changes to the goal, affected area, invariants, or verification method.

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
- how users solve it today, the shortcomings, and relevant alternatives or differentiation
- product form and delivery context, such as a website, local tool, plugin, or internal service
- core user journey
- MVP capabilities
- explicit non-goals
- success and acceptance criteria
- architecture shape
- technology route and tradeoffs
- business modules and their order
- data flow, risks, and unresolved assumptions
- material time, API, hosting, or operating-cost constraints

Start with a concise PRD draft or the product section of `docs/spec.md` before elaborating implementation details. Capture the core screens or operations, user pain, MVP boundaries, acceptance outcomes, and main risks. Mark preliminary technical ideas and unverified demand, competitor, or cost claims as assumptions; use evidence when those claims drive a decision. A small internal tool does not need a market report or a detailed cost model unless the decision depends on one.

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
- `architecture.md`: architecture skeleton, relevant directory layout, business modules and order, dependencies, core data models and ownership, service boundaries, and invariants that changes must preserve. Link to authoritative technology and interface details.
- `tech-stack.md`: selected technologies, constraints, and reasons.
- `progress.md`: current state, completed work, tests, blockers, and next step.
- `decisions.md`: durable decisions that would otherwise be reconsidered.

A smaller MVP may combine product, architecture, and technology choices in `docs/spec.md` and keep a short `docs/progress.md`. Split documents only when independent ownership or navigation makes the separation useful.

Do not create empty files or detailed notes for every future module. Capture the core interactions and necessary interface agreements during baseline design; defer unrelated interfaces and module implementation details.

Keep new planning and progress files under `docs/`, preserving existing names and casing such as `DESIGN.md` or `ARCHITECTURE.md`. Product, design, architecture, and technology are information responsibilities, not a mandatory number of files. Link to existing authorities rather than creating duplicate versions or copying the same rules into several documents.

## Interaction and Interface Design

For a product with unclear user interactions, use a simple sketch, screen outline, or existing UI reference to walk through the core journey. Show the relevant actions, input, result, and important loading, empty, and error states. For a backend or CLI project, use request/response or input/output examples instead. Skip new prototypes when the interactions are already clear; visual polish is not a prerequisite.

For a UI product, reuse its design system or derive a compact visual direction from the user's references: layout and information density, color and typography, reusable components, responsive behavior, and important interaction states. If neither exists, propose a simple direction suited to the core workflow. Capture only decisions needed for consistent implementation. Use sketches or the available design tools; do not require Figma, a particular site, a complete component library, or pixel-perfect screens before MVP work.

Walk the prototype through the actual user journey. If a screen exposes a missing rule, awkward flow, or unnecessary feature, revise the product draft and affected design or interface facts together. Resolve material choices with the user in baseline discussion; after approval, material changes follow Scope Changes and Rework.

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

Group closely related features around one business goal; a module may contain several features. Do not split a module just because it contains frontend and backend code. Split when there are separate business goals, lifecycles, owners, or acceptance outcomes.

Cross-cutting foundations such as project setup, deployment, observability, or a shared design system may be explicit prerequisites or shared infrastructure. Do not present them as user-value modules, and create them only when the current MVP needs them, with a narrow owner and surface.

Keep dependencies understandable and preferably one-way. If two modules need each other's internals, revisit the boundary or extract a small shared contract; do not hide the cycle in imports or database access.

## MVP Implementation Loop

After the baseline is approved, implement the smallest useful vertical slice. Follow dependency order and user value; finish each module's current MVP outcome, not every future feature, so the core journey becomes runnable early.

For each module:

1. Read the current baseline and interactions, module note and relevant contracts if present, repository state, and dependency modules' public outputs.
2. Check the module's goal, boundary, dependencies, behavior to preserve, acceptance outcome, and assumptions due for validation. For a low-risk module, briefly state its goal, scope, and verification method, then continue without waiting for approval.
3. Create `docs/modules/<module>.md` only if the module needs durable detail beyond the baseline.
4. Decide whether MVP mode is sufficient or whether this module should be upgraded to Controlled mode.
5. Use the current task queue to implement one small, verifiable outcome at a time, using the order described below.
6. Add or update focused tests alongside implementation for the behavior and important failure paths.
7. Connect the real components and run focused tests and relevant integration checks, including affected existing behavior; capture the results.
8. Apply Completion Check as one integrated review. Fix in-scope defects, rerun affected checks, and record the outcome in `docs/progress.md`; update other docs only when project facts changed.
9. Preserve a verified Git checkpoint when authorized, then move to the next module after the current MVP outcome passes the review. A partial or blocked result is not completion; identify the remaining work and any dependency on it.

For a UI-driven module with understood technical dependencies, the default order is frontend interaction (using clearly identified mock data if needed), backend logic and persistence, then real integration. Keep both sides aligned with the agreed interface and test while building. Do not finish all modules' frontend implementations before integrating any backend.

When the main uncertainty is an algorithm, AI output quality, an external service, or data behavior, validate that capability first. For backend-only or local tools, omit unnecessary frontend or network layers. Choose the order that resolves the biggest current uncertainty with the least work; do not create a new review gate just to select it.

One module per context is a default organization technique, not an approval gate or a reason to create a large ceremony. A small module can be completed in one conversation. A large module should be split before implementation or upgraded to Controlled mode. When changing contexts, leave a compact handoff and let the next context continue from the approved baseline without reopening settled design decisions unless new evidence conflicts with them.

Do not start the next module until the active module's tests, progress entry, public outputs, and necessary handoff facts are current. If a later module needs an earlier module's internals, stop and repair the boundary or contract before continuing.

## Small Steps and Task State

After baseline approval, keep a short, dependency-ordered queue in `docs/progress.md`. Describe the module order and only the actionable tasks needed now; refine later modules as they approach. A task should produce an observable outcome, with a goal, affected scope, relevant invariants, and a way to verify it, not a separate ticket for every file or function.

Use one authoritative task list. Reuse an existing TODO or issue tracker; use `docs/TODO.md` only if the queue outgrows progress notes. If OpenSpec supplies the module's tasks, reference them instead of maintaining duplicate checkboxes. `progress.md` still records the current result, evidence, blocker, and next action.

Keep one implementation task active at a time and mark it done only after its verification succeeds. Work through subsequent authorized tasks automatically; task transitions are not user approval gates or mandatory commits. If evidence changes the dependency order, update the queue and explain the reason; discuss changes to the approved scope under the existing rules. A finished task does not establish module completion until Completion Check passes.

## Git Checkpoints

Follow the user's repository and commit policy. At a verified module boundary, create a local checkpoint commit when authorized. Inspect the working tree and staged changes first; stage only the relevant files or hunks, preserve other work, and do not include unrelated staged changes. Include relevant docs and tests with the implementation. Keep the commit reference in the handoff or subsequent progress update, avoiding a self-referential commit-id edit.

If the project has no Git repository, commits are not authorized, or a checkpoint fails, record the verified state and checkpoint status. Offer a scoped next action without turning every module into another permission request or treating a missing commit as a failed functional check. A checkpoint is useful for comparison and recovery, but does not prove that the code passed verification.

When a later change breaks behavior, compare it with the verified checkpoint and prefer a focused fix or an authorized scoped revert. Do not discard user work, reset the whole tree, rewrite history, or bypass hooks to recover. Local checkpointing does not authorize pushing, publishing, or deploying.

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
- a small smoke path for each critical MVP capability, including relevant loading, empty, and failure states for UI flows
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

## Delivery Reconciliation

Before final MVP acceptance or a requested release, compare the actual code, configuration, and verified commands with the baseline. Update the relevant architecture or combined spec with the real module and directory structure, data and service boundaries, setup and delivery method, and environment-variable names and purposes. Keep secret values out of docs. Link to existing configuration references rather than duplicating them.

Maintain a usable root `README.md` with the implemented purpose, prerequisites, setup and configuration, run and verification commands, known limitations, and deployment or installation instructions where applicable. Check commands against the actual project scripts and record what was exercised; distinguish documented plans from verified behavior. This is a final consistency pass over continuously maintained docs, not a reason to defer updates during development.

If actual code deviates from approved scope or architecture, expose and resolve the deviation under Scope Changes and Rework; do not rewrite requirements to make an accidental implementation appear approved. Reuse the final tests and five-dimension evidence rather than adding a duplicate audit or another mandatory approval.

Present the working core journey, verification evidence, remaining gaps, and delivery state for final acceptance. Keep local usability, user acceptance, and deployment status distinct. Run a relevant smoke check on the target after an authorized deployment when possible; report an unavailable target as unverified, not successful. For a local or backend-only tool, provide its appropriate usage or input/output demonstration instead of inventing a hosted UI. Feed subsequent user feedback into the next bounded iteration and update the affected docs.

## Scope Changes and Rework

Pause and discuss before proceeding when implementation reveals that the approved product or architecture baseline is wrong, or when a change materially affects a public contract or compatibility guarantee, data migration, security boundary, or irreversible operation. Update the affected authoritative docs and obtain agreement on the new direction before continuing implementation.

For an ordinary bug, reproduce it, add a focused test, fix it, and run the relevant checks. For a complex or cross-module rework, use a new OpenSpec change or another appropriate planning artifact. Do not silently rewrite the baseline.

## Completion Check

Use these five dimensions when defining the module boundary and verification method, then review them together before claiming completion. Within the approved scope and available authorization, the AI performs this review, fixes defects, and rechecks affected evidence automatically. It is part of the module loop, not another user approval gate; existing scope-change and OpenSpec gates still apply.

| Dimension | Required evidence for the current MVP outcome |
| --- | --- |
| Functional correctness | Behavior matches the current spec and acceptance criteria. Focused checks cover the core path, important failures, and affected existing behavior; key business rules can be verified independently where practical. |
| Clear boundaries | Related business rules are cohesive; data and rules have clear owners. Review actual dependencies and public-interface usage for low coupling, hidden cycles, and private-data access. Explain the change's affected consumers and verification scope. |
| Real integration | Run the real UI, backend, persistence, and necessary external connections that the outcome actually uses. Check required data retention and the relevant available user journey; mock-only results cannot prove a real dependency works. Omit components the project does not need. |
| Diagnosable failures | Important failures provide useful error context through appropriate responses, logs, or existing diagnostics, enough to locate the failing operation or boundary. Use evidence from exercised failure paths. |
| Continuity | Record the outcome, verification commands or steps and observed results, remaining gaps, and next action. Synchronize changed authoritative facts, durable decisions, and any stricter-mode artifacts; leave enough context for a new conversation to continue. |

Architecture qualities require code and dependency review; passing tests alone does not establish cohesion or low coupling. Judge the agreed MVP scope: necessary dependencies and changes across modules are acceptable when ownership, direction, and impact are clear. Do not require zero coupling, fixed scores, new infrastructure, or a separate report for each dimension.

Keep one compact result in `docs/progress.md`, linking to evidence instead of copying it. Use an existing module note only when useful; do not create a new document just for this review:

- Passed: the current outcome is usable and all applicable dimensions have evidence; explicitly justify any inapplicable dimension.
- Partially complete: behavior or required verification is still missing, including an unavailable real dependency. Name the unverified path, its impact, and how to complete verification; do not present dependent behavior as verified.
- Blocked: a concrete dependency, authorization, or unresolved decision prevents completion. Record the blocker and next action without claiming the module is done.

Fix defects that invalidate the current acceptance outcome or its evidence before marking Passed. Recheck only affected areas unless broader testing is required by the risk. Keep non-blocking polish and future work explicit without delaying the agreed MVP; escalate material scope or architecture changes under Scope Changes and Rework. MVP completion also requires the full core-journey validation described in Testing and MVP Convergence.
