---
name: project-dev-suite-router
description: Use only when the user is starting a new software project, planning a release for an existing suite-managed project, or managing a significant maintenance, security, or operational change. Trigger for explicit project workflow intents such as starting a product, planning V1.1/V2.0, continuing a suite-managed project, or handling a release-level fix. Do not use for ordinary coding fixes, casual questions, documents, spreadsheets, images, non-software work, or isolated single-step development tasks. When triggered, classify the route and ask for confirmation before loading or executing the suite workflow.
---

# Project Development Suite Router

This skill is a gatekeeper for an isolated software project development workflow. It must not run the workflow automatically.

## Activation Protocol

1. Confirm the user is asking for a full software project workflow, not a small code change or general discussion.
2. Ask whether to enable the isolated project development suite before reading any detailed references.
3. Ask in the user's current language.
4. If the user declines or the intent is unclear, continue with normal Codex behavior and do not load the suite.
5. If the user confirms, classify the route and initial V1.5 intensity first. For L0, continue with normal Codex behavior. For L1/L2/L3, read `references/workflow.md`, then load only the phase-specific reference files needed for the current step.

Use a confirmation message with this meaning unless the conversation already clearly confirmed activation:

```text
I detected that you may want to start a full software project development workflow. Should I enable the isolated project development suite? If enabled, I will proceed through requirements analysis, task breakdown, prototype/flowchart, architecture, database, API, code, tests, and iterative delivery.
```

## Isolation Rules

- Treat files under `references/` as source material, not as automatically active Codex skills.
- Do not import or register the referenced Kiro skills as standalone Codex skills.
- Do not modify Kiro source files under `C:\Users\CYSWR\.kiro\`.
- Do not modify global MCP configuration automatically.
- Use MCP templates only when the active project needs them and the user agrees.
- Keep ordinary Codex behavior for non-project tasks.

## Reference Loading

After confirmation and initial intensity classification:

1. If intensity is L0, do not load the suite and continue with normal Codex behavior.
2. If intensity is L1/L2/L3, read `references/workflow.md` to determine the current project phase and context budget.
3. Read `references/steering/global-steering.md` only while the suite is active.
4. Read `references/steering/database-steering.md` only for database, SQL, DAO, mapper, repository, or persistence work.
5. Read one or more files from `references/skills/` only when the current phase calls for them.
6. Read `references/mcp/mcp-template.json` only when MCP setup is needed for the active project.

## Phase-To-Reference Map

- Phase 1 requirements analysis: `references/skills/requirements-analysis.md`
- Phase 2 task breakdown: `references/skills/task-breakdown.md`
- Phase 3 prototype and flowcharts: `references/skills/prototype-generator.md`, `references/skills/flowchart-generator.md`
- Phase 4 architecture design: `references/skills/architecture-design.md`
- Phase 5 database design: `references/skills/database-design.md`
- Phase 6 requirements transform: `references/skills/requirements-transform.md`
- Phase 7 detailed requirements and API docs: `references/skills/detail-requirements.md`, `references/skills/api-doc-generator.md`
- Phase 8 scaffold generation: `references/skills/code-scaffold.md`
- Phase 9 test generation: `references/skills/test-generation.md`
- Phase 10 iterative delivery: `references/skills/lint.md`, `references/skills/find-bugs.md`, `references/skills/test-generation.md`

## Operating Style

- Ask for user confirmation at phase checkpoints before moving forward.
- Make the smallest useful progress in the current phase instead of loading every reference at once.
- Treat technology choices as project-specific decisions made during architecture design.
- Keep generated project documentation under the active project, not inside this skill folder.

## V1.1 Project Facts Protocol

After activation, create and maintain project facts from `references/templates/`:

- `docs/PRD.md`: product positioning, MVP, non-goals, scope, and acceptance.
- `docs/DESIGN.md`: visual and interaction contract for UI projects.
- `docs/ARCHITECTURE.md`: technology choices, runtime constraints, module boundaries, and non-negotiable rules.
- `docs/TODO.md`: the current executable task queue.

Read the applicable facts before scaffolding, implementation, review, or release. If a fact conflicts with detailed documents or the codebase, stop and surface the conflict. Load the matching template only when the active project reaches that phase.

## V1.1 Reference Map

- Product positioning and PRD: `requirements-analysis.md`, `templates/PRD.md`
- UI and interaction contract: `prototype-generator.md`, `templates/DESIGN.md`
- Architecture: `architecture-design.md`, `templates/ARCHITECTURE.md`
- Two-stage planning: `task-breakdown.md`, `templates/TODO.md`
- Release readiness: `templates/RELEASE-READINESS.md`


## V1.2 Project Route Classification

Before loading phase references, classify the user's request into exactly one route:

1. **New project**: create a product or major system from scratch. Use the V1.1 product-to-first-release workflow.
2. **Version iteration**: add, change, or retire meaningful capabilities in an existing suite-managed project. Do not restart product discovery; first assess the current project facts and release impact.
3. **Maintenance or hotfix**: fix a defect, security issue, performance regression, or operational problem. Use the smallest safe change and require targeted regression validation.

If the route is unclear, ask the user to confirm the route. For version iteration and maintenance, read the minimum facts needed to classify the request before proposing changes. Use V1.5 intensity rules to decide whether to load only local context, project facts, or release-gate artifacts.

## V1.2 Reference Map

- Version planning and impact analysis: `templates/VERSION-PLAN.md`
- Maintenance, hotfix, and post-incident record: `templates/MAINTENANCE-RECORD.md`
- Version release validation: `templates/RELEASE-READINESS.md`

## V1.3 Pre-Release Risk Testing Gate

Before a release, major feature merge, version delivery, or high-risk hotfix, run the pre-release risk-driven testing gate. This gate is executed after regular development tests and before release readiness.

Use these templates as independent project artifacts instead of merging them into PRD or ARCHITECTURE:

- `references/templates/RISK-BASED-TEST-PLAN.md`
- `references/templates/TEST-AUTOMATION-RUNBOOK.md`
- `references/templates/TEST-EFFECTIVENESS-REPORT.md`
- `references/templates/SECURITY-ATTACK-REPORT.md`

Only test systems the user owns or explicitly authorizes. Do not perform destructive load, abuse, or attack tests against production or third-party systems unless the user explicitly authorizes a safe test environment and scope.
## V1.4 Lightweight Reasoning Principles

Use these as decision heuristics, not as new phases:

- Apply first-principles thinking only to high-impact decisions: product scope, MVP boundaries, technology choices, permissions, money, data consistency, AI cost/privacy, multi-tenant or security boundaries. First identify goals, constraints, and non-negotiable rules; then compare mature solutions and reuse what fits.
- Use adversarial review as a checkpoint posture, not a separate workflow. Challenge important plans and changes by asking how they fail, how they can be abused, and whether they break money, permissions, data, reliability, or release safety. Existing risk testing and security gates remain the main execution point.

## V1.5 Execution Intensity and Context Budget

Before loading detailed references, classify both the route and execution intensity:

- **L0 no suite**: ordinary coding fixes, explanations, casual questions, one-off documents, images, spreadsheets, or isolated single-step tasks. Do not load the project suite.
- **L1 lightweight context**: small change inside an existing suite-managed project. Load only the relevant task, related code, and the minimum project fact needed to avoid breaking architecture or scope.
- **L2 standard iteration**: meaningful feature set or version iteration. Load current project facts, create or update version/TODO artifacts, and run scoped tests and regression.
- **L3 release or high-risk gate**: release, major feature merge, high-risk hotfix, or money/permission/data/security-sensitive change. Run the applicable pre-release risk and security gate.

Start active-suite work with a short project state scan: route, intensity, current phase, available facts, missing facts, high-risk areas, and next action. Escalate intensity only when evidence requires it; do not run L3 work for every single TODO.
