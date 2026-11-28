---
name: systematic-debugging
description: Systematically investigate software defects using reproducible experiments, runtime evidence, execution tracing, hypothesis testing, and root-cause analysis. Identify the actual cause before modifying code, implement minimal evidence-based fixes, and verify them with unit, integration, and end-to-end regression tests.
---

# Systematic Debugging

## Objective

Investigate and resolve software defects through a disciplined, evidence-driven process.

Identify the actual root cause before modifying code. Reproduce the failure, trace the execution path, test hypotheses, implement the smallest correct fix, and verify the result at the appropriate testing levels.

The goal is not merely to make the reported symptom disappear. The goal is to restore correct system behavior without introducing regressions.

## Core Principles

1. **Evidence before assumptions** — Base conclusions on observable evidence, not intuition.
2. **Reproduce before modifying** — Establish a repeatable failure whenever possible.
3. **Root cause over symptoms** — Fix the underlying defect rather than hiding its effects.
4. **Runtime behavior over code intuition** — Verify what the system actually executes.
5. **Hypothesis-driven investigation** — Form explicit hypotheses and test them systematically.
6. **Minimal changes** — Avoid unrelated refactoring and unnecessary architectural changes.
7. **Regression prevention** — Add tests that demonstrate the original defect and prevent its return.
8. **Independent verification** — Use tests, runtime evidence, and observable outcomes to validate correctness.
9. **Preserve existing behavior** — Understand existing contracts and invariants before changing implementation.
10. **Explicit uncertainty** — Distinguish confirmed facts, assumptions, and unresolved hypotheses.

## Debugging Workflow

Follow these phases sequentially. Do not skip investigation and jump directly to implementation.

### Phase 1: Understand the Failure

Establish exactly what is failing and what the expected behavior should be.

Identify:

- Reported symptoms and error messages.
- Expected versus actual behavior.
- Affected features, components, services, or users.
- Conditions required to trigger the failure.
- Frequency: always, intermittent, or environment-dependent.
- When the problem first appeared.
- Recent code, configuration, infrastructure, or dependency changes.
- Whether the problem is reproducible in another environment.

Clarify ambiguous requirements before drawing conclusions.

**Output:**
- Problem statement.
- Expected behavior.
- Actual behavior.
- Known conditions and constraints.
- Initial investigation scope.

### Phase 2: Reproduce the Failure

Create the smallest reliable reproduction scenario.

Prefer:

1. Automated test reproducing the defect.
2. Minimal integration test.
3. End-to-end workflow reproducing the issue.
4. Controlled manual reproduction.

Record:

- Exact input and preconditions.
- Required application state.
- Environment and configuration.
- Execution steps.
- Expected result.
- Actual result.
- Error output and relevant logs.

If the failure is intermittent, investigate its triggering conditions rather than repeatedly rerunning it without a hypothesis.

If reproduction is impossible, document the limitation and investigate using available production evidence.

**Output:**
- Reproduction procedure.
- Reproduction result.
- Known limitations.

### Phase 3: Collect Primary Evidence

Collect evidence from the systems and components involved in the failure.

Prioritize raw, authoritative evidence over summaries or assumptions.

Potential evidence sources:

| Area | Evidence |
|---|---|
| Application | Source code, runtime state, stack traces |
| Logging | Structured logs, error logs, correlation IDs |
| Distributed systems | Distributed traces, span relationships |
| Performance | CPU profiles, memory profiles, flame graphs |
| Database | Actual records, transaction logs, execution plans |
| Network | HTTP requests, responses, headers, packet captures |
| Concurrency | Thread dumps, goroutine dumps, race detector output |
| Infrastructure | Process state, resource utilization, container events |
| Configuration | Effective runtime configuration, environment variables |
| Dependencies | Dependency versions, API responses, service health |

Do not rely exclusively on application-level error messages. Investigate the underlying operation that produced the error.

For production incidents, preserve timestamps, correlation IDs, request identifiers, and relevant deployment information.

**Output:**
- Evidence collected.
- Evidence sources.
- Initial observations.
- Missing evidence and limitations.

### Phase 4: Trace the Execution Path

Follow the actual execution path from the entry point to the failure.

Identify:

1. Entry point: HTTP handler, CLI command, scheduled job, event consumer, or UI action.
2. Input processing and validation.
3. Authentication and authorization.
4. Business logic and domain rules.
5. State changes and state transitions.
6. Database queries and transaction boundaries.
7. Internal and external service calls.
8. Message publishing and consumption.
9. Error handling and recovery.
10. Retry, timeout, and cancellation behavior.
11. Final response and observable side effects.

Inspect relevant code and runtime evidence together.

Pay particular attention to:

- Unexpected state transitions.
- Incorrect assumptions about nil, null, or empty values.
- Incorrect error propagation.
- Missing transaction boundaries.
- Resource leaks.
- Race conditions.
- Deadlocks.
- Duplicate execution.
- Incorrect retry behavior.
- Partial failures.
- Inconsistent distributed state.
- Incorrect cache behavior.
- Timezone and serialization issues.

For stateful systems, identify the relevant states, transitions, and invariants.

For concurrent systems, identify shared resources, synchronization mechanisms, execution ordering, and ownership boundaries.

For distributed systems, trace the request across service boundaries using correlation IDs or trace context.

**Output:**
- Execution flow.
- Relevant components and dependencies.
- Suspected failure location.
- Important state changes and side effects.

### Phase 5: Form and Test Hypotheses

Create explicit hypotheses that could explain the observed behavior.

For each hypothesis, document:

- What might be wrong.
- Why it could produce the observed symptom.
- Evidence supporting it.
- Evidence contradicting it.
- An experiment that can confirm or reject it.

Prioritize hypotheses by:

1. Strength of supporting evidence.
2. Ability to explain all observed symptoms.
3. Likelihood based on the execution path.
4. Cost and risk of verification.

Test one hypothesis at a time when practical.

Use controlled experiments:

- Change one variable at a time.
- Compare successful and failing executions.
- Inspect intermediate values.
- Verify assumptions against runtime behavior.
- Add temporary diagnostic instrumentation when necessary.
- Remove temporary instrumentation after the investigation unless it provides lasting operational value.

Do not modify production behavior merely to see whether a guess works.

**Output:**
- Hypothesis list.
- Experiments performed.
- Results and conclusions.
- Remaining uncertainties.

### Phase 6: Establish the Root Cause

Do not declare a root cause until the evidence supports it.

Distinguish the following:

| Term | Meaning |
|---|---|
| Symptom | What users or systems observe |
| Trigger | The condition that activates the defect |
| Direct cause | The immediate incorrect operation or behavior |
| Root cause | The underlying implementation, design, or configuration defect |
| Contributing factor | A condition that increases the likelihood or impact |

A valid root-cause explanation must describe:

1. What is incorrect.
2. Where it occurs.
3. Why it occurs.
4. Under what conditions it occurs.
5. How it produces the observed symptom.
6. What evidence confirms the explanation.

If multiple causes exist, document them separately.

If the evidence is insufficient, state that the root cause remains unconfirmed.

**Output:**
- Confirmed root cause or unresolved hypotheses.
- Supporting evidence.
- Triggering conditions.
- Contributing factors.

### Phase 7: Design the Minimal Correct Fix

Determine the smallest change that corrects the underlying defect.

Before implementation:

- Understand existing behavior and compatibility requirements.
- Identify affected interfaces and contracts.
- Check invariants and state-transition rules.
- Consider failure handling and recovery.
- Evaluate concurrency and transaction implications.
- Consider backward compatibility.
- Identify possible side effects.
- Determine which tests must be added or updated.

Prefer:

- Correcting the underlying condition.
- Explicit validation and error handling.
- Preserving established contracts.
- Making invalid states unrepresentable where practical.
- Safe transaction and resource management.
- Idempotent operations where required.
- Bounded retries with appropriate backoff.
- Deterministic behavior.

Avoid:

- Broad refactoring unrelated to the defect.
- Catching and ignoring errors.
- Arbitrary delays to hide timing problems.
- Increasing timeouts without understanding the cause.
- Retrying non-idempotent operations without safeguards.
- Adding defensive code without a defined failure scenario.
- Weakening tests to make them pass.

**Output:**
- Proposed fix.
- Expected behavior after the fix.
- Potential risks.
- Verification plan.

### Phase 8: Implement the Fix

Implement only the changes required by the confirmed root cause.

Requirements:

1. Follow existing project conventions.
2. Keep changes focused and reviewable.
3. Preserve unrelated behavior.
4. Handle errors explicitly.
5. Maintain state and data integrity.
6. Avoid unnecessary dependencies.
7. Add regression tests where appropriate.
8. Update documentation when behavior or contracts change.

Inspect the complete diff after implementation.

Do not claim the issue is fixed merely because the code compiles or the implementation looks correct.

**Output:**
- Implementation changes.
- Regression tests.
- Updated documentation, if required.

### Phase 9: Verify the Fix

Verify the correction at the appropriate testing levels.

#### 9.1 Unit Testing

Test isolated business logic and relevant edge cases.

Verify:

- Normal inputs.
- Invalid inputs.
- Boundary conditions.
- Error handling.
- State transitions.
- Invariants.
- Regression scenarios.

#### 9.2 Integration Testing

Test interactions between components and dependencies.

Verify:

- Database operations.
- Transaction behavior.
- Internal service communication.
- External service integration.
- Serialization and deserialization.
- Message publishing and consumption.
- Error propagation.
- Timeout and retry behavior.

Use real dependencies when practical. If mocks or stubs are necessary, document what they do not verify.

#### 9.3 Regression Testing

Add a test that represents the original defect.

A useful regression test should:

1. Reproduce the original failure condition.
2. Fail against the defective behavior whenever practical.
3. Pass after the correction.
4. Verify the intended outcome, not just the absence of an error.
5. Remain deterministic and maintainable.

Run relevant existing tests to detect unintended changes.

#### 9.4 Performance and Stability Verification

When the defect affects performance, concurrency, or resource usage, verify:

- Response time.
- Throughput.
- CPU and memory consumption.
- Connection and resource usage.
- Lock contention.
- Race conditions.
- Behavior under representative load.

Compare against a meaningful baseline when available.

### Phase 9.5: End-to-End Verification

Use end-to-end (E2E) verification when the defect crosses multiple application layers, affects a complete business workflow, or cannot be adequately validated through isolated tests.

E2E testing must validate observable system behavior through realistic application entry points.

#### 9.5.1 Identify the Complete Workflow

Map the workflow from initiation to final observable outcome.

Consider:

1. User action or external request.
2. Authentication and authorization.
3. API or application entry point.
4. Input validation.
5. Business logic.
6. Database reads and writes.
7. Internal and external service calls.
8. Queue or event processing.
9. Asynchronous operations.
10. Final response and business outcome.
11. Persisted data and external side effects.

Identify the boundaries that must be exercised to reproduce the original defect.

Do not assume that a successful response means the complete workflow succeeded.

#### 9.5.2 Establish the E2E Scenario

Create a realistic test scenario that exercises the actual application workflow.

Requirements:

- Use the real application entry point whenever practical.
- Use realistic inputs and preconditions.
- Include the conditions that originally triggered the defect.
- Exercise relevant integration boundaries.
- Use real dependencies where practical.
- Clearly document mocked or simulated dependencies.
- Ensure the environment is sufficiently representative of the behavior being verified.

Avoid tests that bypass the component responsible for the original failure.

#### 9.5.3 Verify System-Wide Behavior

Verify the complete observable outcome.

Check:

- Application response and status.
- Correct business result.
- Persisted database state.
- Data consistency across components.
- External service interactions.
- Events and messages produced.
- Asynchronous processing completion.
- User-visible state.
- Authorization and security behavior where relevant.
- Absence of unintended side effects.

For asynchronous workflows, use bounded polling, event-based synchronization, or another deterministic completion signal.

Avoid arbitrary fixed sleeps as the primary synchronization mechanism.

#### 9.5.4 Test Failure and Recovery Paths

When relevant to the defect, verify:

- Invalid or malformed requests.
- Missing or unavailable dependencies.
- Database failures.
- External service timeouts.
- Partial workflow completion.
- Duplicate requests.
- Concurrent requests.
- Message delivery failures.
- Retry exhaustion.
- Recovery and compensation behavior.
- Idempotency after retries.
- Cleanup of partially completed operations.

Verify that the system reaches a valid final state after both success and failure.

#### 9.5.5 Verify the Original Business Outcome

Do not stop at checking HTTP status codes, successful method returns, or the absence of exceptions.

Verify the actual business result.

Examples:

- An order is created exactly once.
- A payment is not charged twice.
- An inventory quantity is updated correctly.
- A user receives the correct authorization result.
- A background job completes and persists its result.
- A failed workflow leaves no inconsistent partial state.

The expected outcome must be explicit and independently observable.

#### 9.5.6 Add E2E Regression Coverage

Add an E2E regression test when the original defect requires multiple application layers to reproduce.

The test should:

1. Represent the original failing workflow.
2. Exercise the relevant system boundaries.
3. Verify the final business outcome.
4. Detect the defective behavior when practical.
5. Pass after the correction.
6. Be deterministic and repeatable.
7. Clean up test data and resources.
8. Avoid depending on unrelated production data or unstable external conditions.

Do not force E2E tests for every isolated defect. Use the lowest testing level that provides sufficient confidence, and add E2E coverage when the failure crosses system boundaries or affects a critical workflow.

#### 9.5.7 Record E2E Evidence

Document:

- Workflow and test scenario.
- Application entry point.
- Components and integration boundaries exercised.
- Test environment and dependencies.
- Mocked or excluded components.
- Test commands and execution results.
- Expected and actual outcomes.
- Relevant logs and distributed traces.
- Persisted state and observable side effects.
- Known limitations.

**Output:**
- E2E test results.
- Verified business outcomes.
- Regression coverage.
- Remaining limitations.

### Phase 10: Review and Close

Perform a final review before declaring the issue resolved.

Check:

- Does the implementation address the confirmed root cause?
- Does the original reproduction scenario now pass?
- Are relevant unit, integration, regression, and E2E tests passing?
- Are existing tests still passing?
- Are error-handling paths correct?
- Are state and data invariants preserved?
- Are logs and observability sufficient?
- Is the change limited to the required scope?
- Are there unresolved risks or limitations?

Review the final diff for:

- Unintended changes.
- Dead code.
- Temporary debugging statements.
- Sensitive information in logs.
- Missing error handling.
- Missing test coverage.
- Unnecessary dependencies.
- Documentation inconsistencies.

Do not report success if required verification has not been performed.

**Output:**
- Final verification results.
- Final change summary.
- Remaining risks.
- Resolution status.

## Specialized Investigation Techniques

Apply techniques relevant to the defect. Do not force every technique into every investigation.

### Functional Defects

- Compare expected and actual execution paths.
- Inspect input transformations.
- Validate business rules.
- Examine boundary conditions.
- Trace incorrect outputs to their source.

### State and Workflow Defects

- Identify explicit and implicit state machines.
- Document valid states and transitions.
- Identify invariants.
- Find reachable invalid states.
- Examine retry and recovery transitions.
- Verify state consistency after failures.

### Concurrency Defects

- Identify shared mutable state.
- Inspect locking and synchronization.
- Examine execution ordering.
- Use race detectors and thread or goroutine dumps.
- Investigate deadlocks and lock contention.
- Reproduce with controlled concurrency where practical.

### Performance Defects

- Establish a baseline.
- Measure before optimizing.
- Profile CPU, memory, I/O, and network activity.
- Inspect database execution plans.
- Identify bottlenecks.
- Verify improvements using comparable workloads.

### Distributed-System Defects

- Follow correlation and trace identifiers.
- Inspect service boundaries.
- Verify message delivery guarantees.
- Examine duplicate and out-of-order events.
- Investigate timeout and retry interactions.
- Verify eventual consistency and recovery behavior.

### Data-Integrity Defects

- Inspect actual persisted records.
- Verify transaction boundaries.
- Check constraints and uniqueness.
- Investigate partial writes.
- Verify idempotency.
- Confirm consistency across related records and services.

### Error-Handling Defects

- Trace the original error source.
- Follow error propagation through layers.
- Identify swallowed or incorrectly transformed errors.
- Verify error classification.
- Check retryability and timeout behavior.
- Confirm that failures are observable and actionable.

## Logging and Observability Requirements

Use existing logging and observability infrastructure before introducing new instrumentation.

When additional instrumentation is needed, make it useful for both immediate debugging and future incident investigation.

Prefer structured logs containing relevant fields such as:

- Timestamp.
- Severity.
- Service and component.
- Operation or workflow name.
- Correlation ID or trace ID.
- Request or job identifier.
- Relevant entity identifier.
- State before and after a transition.
- Error type and error message.
- Retry attempt.
- Duration.
- Outcome.

Requirements:

1. Log meaningful state transitions and important decisions.
2. Preserve error context across application boundaries.
3. Make related operations traceable.
4. Record failures with actionable context.
5. Avoid logging secrets, credentials, tokens, or sensitive personal data.
6. Avoid excessive logging and duplicate error reports.
7. Do not use logs as a substitute for correct error handling.
8. Remove temporary diagnostic instrumentation when no longer needed.

Use metrics and traces when they provide better evidence than logs alone.

## Tool Selection

Choose tools based on the language, runtime, architecture, and failure type.

| Purpose | Example tools |
|---|---|
| Go debugging | Delve, `go test`, `go vet` |
| Go performance | `pprof`, Go execution trace |
| Go concurrency | Go race detector |
| Java debugging | JDB, JFR, VisualVM |
| Java testing | JUnit, Testcontainers |
| Native memory | Valgrind, AddressSanitizer |
| Native concurrency | ThreadSanitizer |
| System profiling | `perf`, flame graphs |
| Distributed tracing | OpenTelemetry, Jaeger |
| Log investigation | Loki, Elasticsearch, OpenSearch |
| Database analysis | EXPLAIN, EXPLAIN ANALYZE |
| API verification | curl, Postman |
| Browser E2E | Playwright, Cypress |
| HTTP integration | Testcontainers, WireMock |
| Load testing | k6, Gatling |
| Container investigation | Docker logs, Kubernetes events |

These are examples, not mandatory dependencies. Prefer tools already available in the project and environment.

## Final Investigation Report

When the task is complete, produce a concise but evidence-based report.

Use this structure:

### 1. Problem

Describe the reported and reproduced behavior.

### 2. Root Cause

Explain the underlying defect and why it occurred.

If not confirmed, explicitly state that the root cause remains unconfirmed.

### 3. Evidence

List the most important evidence supporting the conclusion.

Include relevant logs, traces, code locations, reproduction results, and test results.

### 4. Execution Flow

Summarize the relevant execution path and where the failure occurred.

### 5. Fix

Describe the implementation changes and why they correct the root cause.

### 6. Verification

Report the results of applicable tests:

- Unit tests.
- Integration tests.
- Regression tests.
- End-to-end tests.
- Performance or concurrency tests, when relevant.

Include commands and results where useful.

### 7. Risks and Limitations

Document remaining risks, unverified behavior, environmental constraints, and known limitations.

### 8. Status

Use one of the following:

- **Resolved** — Root cause confirmed, fix implemented, and required verification passed.
- **Partially resolved** — A correction was implemented, but some verification or related issues remain.
- **Unresolved** — Root cause or correct fix has not been established.
- **Blocked** — Further investigation requires unavailable access, data, dependencies, or environment.

## Behavioral Constraints

- Do not modify code before understanding the failure, except when a minimal diagnostic change is necessary to collect evidence.
- Do not invent root causes or claim certainty without evidence.
- Do not confuse correlation with causation.
- Do not hide symptoms through arbitrary delays, swallowed errors, or weakened validation.
- Do not make unrelated architectural changes.
- Do not introduce unnecessary abstractions or dependencies.
- Do not skip regression tests for confirmed defects when a reliable test can be written.
- Do not claim E2E verification when only unit or integration tests were executed.
- Do not claim a test passed unless it was actually executed and its result observed.
- Do not report an issue as resolved while required verification remains incomplete.
- Do not expose secrets or sensitive information in diagnostic output.
- Do not stop at the first plausible explanation; verify the root cause.

## Success Criteria

A debugging task is successful when:

1. The failure is understood and reproduced, or reproduction limitations are documented.
2. The execution path has been investigated using relevant evidence.
3. The root cause is established or uncertainty is explicitly documented.
4. The fix addresses the underlying defect.
5. The implementation is minimal and preserves unrelated behavior.
6. Regression tests cover the original failure where practical.
7. Unit, integration, and E2E verification are performed at the appropriate levels.
8. The final result is supported by observable evidence.
9. Remaining risks and limitations are documented.
