---
name: refactor-plan-pro
description: Build a repository-grounded refactor plan with phased execution, measurable quality gates, rollout controls, and rollback safety.
---

# Refactor Plan Pro

Create a detailed and execution-ready refactor plan for the requested scope.

## Refactor Goal

{{refactor_description}}

## When to use

Use this skill when a refactor affects architecture, shared types, core flows, API contracts, performance, reliability, operational behavior, or developer workflow.

## Planning principles

1. Prefer small, reversible changes over large rewrites.
2. Sequence work by dependency order and blast radius.
3. Add verification after each significant step.
4. Protect delivery with rollout and rollback plans.
5. Surface risks early with mitigation actions.
6. Preserve backward compatibility until migration is proven.

## How LLMs process this task (important)

1. LLMs become generic when context is thin. Force specificity by grounding every major claim in repository artifacts (files, interfaces, tests, metrics, commands).
2. LLMs optimize for fluent output, not always correct output. Require verifiable checks and explicit pass/fail gates in every phase.
3. LLMs can hallucinate filenames, commands, and contracts. Only reference assets that exist or mark unknowns as `UNKNOWN - discovery required` with a discovery action.
4. LLMs follow recent, explicit constraints best. Keep strict formatting and quality rules close to the output template.
5. LLMs handle decomposition well. Break the refactor into small phases with clear dependencies and stop conditions.

## Anti-generic contract (mandatory)

1. Do not write placeholder prose like "improve code quality" without naming concrete modules, contracts, or behaviors.
2. Do not leave placeholder commands like `[commands]` when repo context exists. Provide concrete commands.
3. Every phase must include:
   - at least one automatic verification command,
   - measurable gate criteria,
   - explicit corrective actions if gate fails.
4. "Affected Files" must list real paths whenever available.
5. Risks must be technical and testable (for example: schema drift, retry storm, p95 regression, contract mismatch).

## Required workflow

1. Analyze current behavior, architecture boundaries, and constraints.
2. Map impacted files, modules, and dependencies (runtime and build/test tooling).
3. Define target architecture and compatibility strategy.
4. Split work into dependency-ordered phases with Go/No-Go gates.
5. Define test strategy (unit/integration/e2e/contract) and observability checks.
6. Define non-functional validation (latency, throughput, memory, error budget, security).
7. Define rollout strategy (flag/canary/staged) and rollback runbook.
8. Return the final plan in the required output format.

## Evidence and assumptions rules

1. If information is missing, state assumptions explicitly inside the relevant section.
2. For each major assumption, include how to validate it.
3. Do not invent production thresholds. Use known SLOs; if unavailable, define temporary conservative thresholds and mark them as provisional.

## Quality gate rules

1. Every phase must include a **Go/No-Go Gate**.
2. A gate must define at least one automatic verification command.
3. A gate must define measurable pass criteria (for example: "all contract tests pass", "p95 <= baseline + 10%", "error rate < 1%").
4. If a gate fails, stop progression and provide corrective actions in the same phase.

## Command guidance for verification

1. Prefer deterministic commands used by the repository (`npm test`, `pnpm test`, `go test ./...`, `pytest`, etc.).
2. Include focused commands per phase (for example, contract-only tests in Phase 1, integration tests in Phase 3).
3. Add at least one observability or runtime validation command/check where relevant.

## Output format

```markdown
## Refactor Plan Pro: [title]

### Scope
- Goal: [what changes, in concrete technical terms]
- Out of scope: [what will not change]
- Constraints: [time/risk/compatibility/operational limits]

### Current State
[How the system works now, current pain points, and evidence from code/contracts/tests/ops]

### Target State
[How the system should work after refactor, including compatibility and migration endpoint]

### Architecture and Impact Matrix
| Area | Impact | Risk | Notes |
|------|--------|------|-------|
| Types/Contracts | Low/Med/High | Low/Med/High | [specific contract or type impacts] |
| API/Public Interfaces | Low/Med/High | Low/Med/High | [versioning, compatibility, deprecation] |
| Data/Storage | Low/Med/High | Low/Med/High | [schema/index/migration/retention impacts] |
| Performance | Low/Med/High | Low/Med/High | [latency/throughput/memory expectations] |
| Security | Low/Med/High | Low/Med/High | [authz/validation/secrets/exposure impacts] |
| Developer Experience | Low/Med/High | Low/Med/High | [tooling, build, test, onboarding impacts] |

### Affected Files
| File | Change Type | Reason | Depends On | Blocked By |
|------|-------------|--------|------------|------------|
| [real/path/to/file] | modify/create/delete | [why this file changes] | [dependency] | [blocker] |

### Phase Plan

#### Phase 0: Baseline and Safety Net
- [ ] Capture baseline behavior and metrics with reproducible commands
- [ ] Confirm coverage gaps and add critical missing tests for high-risk paths
- Verify: [concrete commands]
- Go/No-Go Gate: PASS if [measurable criteria]. FAIL if [failure signals]. If FAIL, stop and [corrective actions].

#### Phase 1: Types and Contracts
- [ ] Introduce new types/interfaces with compatibility layer
- [ ] Add adapters/transformers to keep old and new contracts interoperable
- Verify: [concrete commands]
- Go/No-Go Gate: PASS if [contract + compile/type checks pass]. FAIL if [breakage criteria]. If FAIL, stop and [corrective actions].

#### Phase 2: Core Implementation
- [ ] Migrate implementation in smallest safe increments behind controlled switches when possible
- [ ] Validate behavioral equivalence before removing legacy paths
- Verify: [concrete commands]
- Go/No-Go Gate: PASS if [functional parity + key tests pass]. FAIL if [regression criteria]. If FAIL, stop and [corrective actions].

#### Phase 3: Integrations and Side Effects
- [ ] Update external integrations, retries/timeouts, idempotency, and edge-case handling
- [ ] Validate logs, traces, metrics, and error-rate behavior under representative load
- Verify: [concrete commands]
- Go/No-Go Gate: PASS if [integration + observability thresholds met]. FAIL if [signal thresholds exceeded]. If FAIL, stop and [corrective actions].

#### Phase 4: Tests and Hardening
- [ ] Expand unit/integration/e2e/contract coverage for migrated paths
- [ ] Validate performance, reliability, and security thresholds
- Verify: [concrete commands]
- Go/No-Go Gate: PASS if [coverage + NFR thresholds met]. FAIL if [thresholds not met]. If FAIL, stop and [corrective actions].

#### Phase 5: Rollout and Cleanup
- [ ] Roll out by feature flag/canary/staged deployment with explicit promotion criteria
- [ ] Remove deprecated code and adapters only after stable production signals
- [ ] Update docs/runbooks only where architecture or operations changed
- Verify: [concrete commands]
- Go/No-Go Gate: PASS if [production stability criteria met]. FAIL if [rollback triggers present]. If FAIL, stop and [corrective actions].

### Verification Checklist
- Functional: [key user/system behaviors with acceptance conditions]
- Contract: [API/schema/event compatibility checks]
- Reliability: [timeouts/retries/circuit-breakers/error-rate limits]
- Performance: [latency/memory/throughput targets versus baseline]
- Security: [authz/input validation/secrets handling/data exposure checks]

### Rollout Strategy
1. [Enable behind feature flag with default OFF]
2. [Canary subset with explicit scope and duration]
3. [Monitor SLOs, error budget, and business-critical KPIs]
4. [Promote to full rollout only if criteria remain stable]

### Rollback Runbook
1. Trigger conditions: [signals requiring rollback with thresholds]
2. Immediate actions: [disable flag/revert release/route traffic]
3. Data safety actions: [schema/data reconciliation steps if touched]
4. Verification after rollback: [commands + runtime signals proving recovery]

### Risks and Mitigations
| Risk | Probability | Impact | Mitigation | Owner |
|------|-------------|--------|------------|-------|
| [specific technical risk] | Low/Med/High | Low/Med/High | [preventive + detective + corrective controls] | [team/role] |

### Done Criteria
- [ ] All phase gates passed
- [ ] No critical regressions in tests or observability
- [ ] Rollout and rollback validated
- [ ] Deprecated paths removed or tracked with follow-up tickets
```

## Final instruction

Return only the final plan in the exact structure above, adapted to the requested refactor goal.
