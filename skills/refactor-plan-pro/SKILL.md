---
name: refactor-plan-pro
description: Build a production-safe refactor plan with phased execution, quality gates, rollout strategy, and rollback runbook.
---

# Refactor Plan Pro

Create a detailed and execution-ready refactor plan for the requested scope.

## Refactor Goal

{{refactor_description}}

## When to use

Use this skill when a refactor affects architecture, shared types, core flows, API contracts, performance, or reliability.

## Planning principles

1. Prefer small, reversible changes over large rewrites.
2. Sequence work by dependency order.
3. Add verification after each significant step.
4. Protect delivery with rollout and rollback plans.
5. Surface risks early with mitigation actions.

## Required workflow

1. Analyze current behavior and constraints.
2. List all impacted files, modules, and dependencies.
3. Split work into phases with explicit quality gates.
4. Define tests, observability checks, and non-functional validation.
5. Add rollout strategy and rollback runbook.
6. Return a final plan in the required output format.

## Quality gate rules

- Every phase must include a **Go/No-Go Gate**.
- A gate must define at least one automatic verification command.
- If a gate fails, stop progression and return corrective actions.

## Output format

```markdown
## Refactor Plan Pro: [title]

### Scope
- Goal: [what changes]
- Out of scope: [what will not change]
- Constraints: [time/risk/compatibility]

### Current State
[How the system works now and current pain points]

### Target State
[How the system should work after refactor]

### Architecture and Impact Matrix
| Area | Impact | Risk | Notes |
|------|--------|------|-------|
| Types/Contracts | Low/Med/High | Low/Med/High | ... |
| API/Public Interfaces | Low/Med/High | Low/Med/High | ... |
| Data/Storage | Low/Med/High | Low/Med/High | ... |
| Performance | Low/Med/High | Low/Med/High | ... |
| Security | Low/Med/High | Low/Med/High | ... |
| Developer Experience | Low/Med/High | Low/Med/High | ... |

### Affected Files
| File | Change Type | Reason | Depends On | Blocked By |
|------|-------------|--------|------------|------------|
| path/to/file | modify/create/delete | why | X | Y |

### Phase Plan

#### Phase 0: Baseline and Safety Net
- [ ] Capture baseline behavior and metrics
- [ ] Confirm test coverage gaps and add critical missing tests
- Verify: [commands]
- Go/No-Go Gate: [criteria]

#### Phase 1: Types and Contracts
- [ ] Introduce new types/interfaces behind compatibility layer
- [ ] Keep backward-compatible adapters if needed
- Verify: [commands]
- Go/No-Go Gate: [criteria]

#### Phase 2: Core Implementation
- [ ] Migrate implementation in smallest safe increments
- [ ] Remove dead paths only after equivalent behavior is validated
- Verify: [commands]
- Go/No-Go Gate: [criteria]

#### Phase 3: Integrations and Side Effects
- [ ] Update external integrations and edge-case handling
- [ ] Validate logs, traces, and error-rate behavior
- Verify: [commands]
- Go/No-Go Gate: [criteria]

#### Phase 4: Tests and Hardening
- [ ] Expand unit/integration/e2e coverage
- [ ] Validate performance and reliability thresholds
- Verify: [commands]
- Go/No-Go Gate: [criteria]

#### Phase 5: Rollout and Cleanup
- [ ] Rollout plan (feature flag/canary/staged)
- [ ] Remove deprecated code and adapters
- [ ] Update docs and runbooks if needed
- Verify: [commands]
- Go/No-Go Gate: [criteria]

### Verification Checklist
- Functional: [key behaviors]
- Contract: [API/schema compatibility]
- Reliability: [timeouts/retries/error rates]
- Performance: [latency/memory/throughput targets]
- Security: [authz/input validation/secrets handling]

### Rollout Strategy
1. [Enable behind feature flag]
2. [Canary subset]
3. [Monitor SLOs and error budget]
4. [Full rollout criteria]

### Rollback Runbook
1. Trigger conditions: [what signals require rollback]
2. Immediate actions: [disable flag/revert release]
3. Data safety actions: [if schema/data touched]
4. Verification after rollback: [commands + signals]

### Risks and Mitigations
| Risk | Probability | Impact | Mitigation | Owner |
|------|-------------|--------|------------|-------|
| ... | Low/Med/High | Low/Med/High | ... | ... |

### Done Criteria
- [ ] All phase gates passed
- [ ] No critical regressions in tests or observability
- [ ] Rollout and rollback validated
- [ ] Deprecated paths removed or tracked with follow-up tickets
```

## Final instruction

Return only the final plan in the exact structure above, adapted to the requested refactor goal.
