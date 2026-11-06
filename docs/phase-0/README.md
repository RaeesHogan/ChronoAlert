# ChronoAlert — Phase 0 Design Package

**Status:** Draft for review. **Phase 1 authorization:** Not granted.

## Scope
Define and validate requirements, architecture, clock-aligned scheduling, state transitions, UI behavior, failure handling, and acceptance tests. Phase 0 is documentation only.

## Documents
1. [Requirements](01-requirements.md)
2. [Architecture](02-architecture.md)
3. [Scheduling Specification](03-scheduling.md)
4. [State Machine](04-state-machine.md)
5. [UI/UX](05-ui-ux.md)
6. [Reliability and Risks](06-reliability-risks.md)
7. [Test and Acceptance Plan](07-test-plan.md)
8. [Open Decisions and Approval](08-open-decisions.md)

## Phase 0 exit gate
- Resolve or explicitly defer every open decision.
- Ensure requirements, scheduling rules, architecture, and tests are consistent.
- Verify .NET/Windows compatibility and Windows notification behavior.
- User explicitly approves the final Phase 0 baseline.

## Strict boundary
No C#, XAML, solution/project files, scripts, application assets, build pipeline, or EXE may be created during Phase 0. Phase 1 remains blocked until explicit approval.