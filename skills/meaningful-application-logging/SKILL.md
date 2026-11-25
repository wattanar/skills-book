---
name: meaningful-application-logging
description: Analyze application logging and execution flows, identify observability gaps, and implement structured, actionable logs that make production behavior, failures, and incident investigation easy to understand.
---

# Meaningful Application Logging

## Mission

Make application behavior explainable from its logs.

Analyze the real execution flow before changing logging. Then add or improve logs so an engineer can answer:

- What happened?
- Which operation, request, job, or business process was involved?
- Where did it start, which important steps completed, and where did it fail?
- Why did it fail, and what was the relevant error context?
- What was the impact: which operation, dependency, or business entity was affected?
- Can related events be correlated across services and asynchronous boundaries?

Prefer a small number of high-signal events over large volumes of noisy logs. Logging must help diagnose incidents without leaking secrets or sensitive data.

## Operating principles

1. **Understand before instrumenting.** Trace the code path and its callers, callees, state transitions, dependencies, and error paths before editing logs.
2. **Log events, not sentences.** Prefer structured fields with stable names and types. Human-readable messages should summarize the event, not contain the only copy of important context.
3. **Describe meaningful boundaries.** Log operation start/completion where useful, state transitions, external dependency calls, retries, rejected actions, and failures.
4. **Preserve causality.** Carry request, trace, correlation, and business-operation identifiers across function calls, goroutines, queues, and service boundaries where supported.
5. **Make failures actionable.** Include the operation, failed step, dependency, duration where relevant, retry state, and the original error. Do not replace or swallow the root cause.
6. **Avoid noise.** Do not log every function entry, every variable, every loop iteration, or successful low-value internal detail.
7. **Use levels consistently.** Levels express operational significance, not developer convenience.
8. **Protect data.** Never log credentials, access tokens, session cookies, private keys, payment data, or unrestricted personal data. Redact or allowlist fields.
9. **Respect the existing stack.** Reuse the repository's logger, schema, context propagation, configuration, and telemetry conventions unless there is a demonstrated reason to change them.
10. **Verify the result.** Add or update tests for important log events, fields, error paths, and redaction behavior when practical.

## Workflow

### 1. Discover the logging and observability architecture

Inspect the repository before making changes:

- Identify language, framework, service boundaries, entry points, and runtime type (HTTP, worker, scheduled job, CLI, event consumer, etc.).
- Find logger initialization, logging libraries, wrappers, adapters, formatters, and configuration.
- Determine whether logs are structured JSON or plain text, and identify existing field naming conventions.
- Find existing request IDs, trace IDs, span IDs, correlation IDs, and context propagation mechanisms.
- Inspect metrics, distributed tracing, error reporting, and log shipping integrations if present.
- Check environment-specific log levels, sampling, retention, and redaction configuration where available.
- Search for existing logging helpers and avoid introducing a competing abstraction.

Do not assume a specific language, logging library, or telemetry vendor.

### 2. Reconstruct the application process flow

Follow the relevant flow from its entry point to its terminal outcome.

For each flow, identify:

- Trigger: HTTP request, message, scheduled task, user action, or internal call.
- Input validation and authorization decisions.
- Important business operations and state transitions.
- Database reads/writes and transaction boundaries.
- External service, RPC, cache, filesystem, and queue interactions.
- Retry, timeout, cancellation, fallback, and compensation behavior.
- Success, partial-success, rejection, and failure exits.
- Asynchronous handoffs and the identifiers required to connect events later.

Inspect both the normal path and exceptional paths. Follow returned errors and wrapped errors to determine where the original cause can be logged without duplicate stack traces or redundant messages.

Create a concise flow map or sequence diagram when the process has multiple stages or crosses service boundaries. Base it on code, not assumptions.

### 3. Audit existing logs

Classify existing log statements:

- **Useful:** identify a meaningful event and contain enough safe context to investigate it.
- **Vague:** messages such as `failed`, `error occurred`, or `processing` without operation context.
- **Noisy:** repetitive, high-frequency, or low-value messages.
- **Misleveled:** expected business rejections logged as errors, or real failures logged as debug/info.
- **Duplicated:** the same error logged at several layers without adding context.
- **Unsafe:** logs secrets, sensitive payloads, or unbounded user-controlled content.
- **Disconnected:** missing request/trace/correlation identifiers or inconsistent field names.

Record concrete evidence: file, function, current behavior, and why it helps or harms investigation.

### 4. Design the event model before implementation

For each proposed event, define:

| Attribute | Requirement |
|---|---|
| Event name | Stable, machine-readable name, e.g. `payment.authorization.failed` |
| Level | Chosen by operational significance |
| Meaning | What happened, stated unambiguously |
| Location | Service, component, operation, and step |
| Correlation | Trace/request/correlation/operation IDs when available |
| Outcome | `success`, `failure`, `rejected`, `retrying`, or another documented value |
| Context | Safe identifiers and relevant state, not entire objects |
| Error | Error type/code and original error details where safe |
| Timing | Duration for operations where latency matters |

Use consistent field names throughout the application. Prefer a documented schema such as:

- `timestamp`
- `level`
- `message`
- `event`
- `service`
- `environment`
- `version`
- `trace_id`
- `span_id`
- `request_id`
- `correlation_id`
- `operation`
- `component`
- `step`
- `outcome`
- `duration_ms`
- `attempt`
- `dependency`
- `error`
- `error_type`
- `error_code`

Only emit fields that are available and relevant. Do not fabricate IDs or values. Follow existing conventions if they already provide equivalent fields.

### 5. Choose log levels deliberately

Use the following as defaults, adapting to the application's established policy:

- **ERROR:** an operation failed unexpectedly, a dependency is unavailable, data integrity may be affected, or the system cannot fulfill its responsibility. Include actionable context.
- **WARN:** an abnormal but handled condition occurred, such as a retry, fallback, degraded dependency, approaching a limit, or recoverable inconsistency.
- **INFO:** a meaningful business or operational boundary completed, a job started/completed, or a significant state transition occurred.
- **DEBUG:** diagnostic details useful during development or targeted investigation, but too granular or frequent for normal production operation.
- **TRACE:** very fine-grained execution details only when the logger and operational policy support it.

Expected validation failures, not-found results, authorization denials, and business-rule rejections are not automatically system errors. Choose their level based on whether they are expected and operationally significant.

Do not log the same error at every layer. Usually, add context as an error propagates and log it once at the boundary that owns the failed operation. Lower layers may return or wrap errors without logging them.

### 6. Instrument meaningful process boundaries

Add logs selectively at points such as:

- Request/job/message accepted, rejected, started, and completed.
- Important business state transitions, including previous and next state when safe.
- External dependency operation started/completed/failed when the boundary is operationally important.
- Retry scheduled/performed/exhausted, including attempt and backoff where available.
- Timeout, cancellation, circuit-breaker, fallback, and compensation events.
- Transaction commit/rollback outcomes when they materially affect diagnosis.
- Background task handoff and consumer completion/failure.
- Unexpected invariant violations and data-integrity conditions.

For long-running operations, record elapsed duration. Prefer measuring around the actual operation rather than estimating it.

A completion event should make the outcome explicit. If an operation can partially succeed, distinguish partial success from full success.

### 7. Make each event useful

A useful event should answer, as applicable:

- **Who/what:** service, component, operation, and safe entity identifier.
- **Where:** stage or dependency.
- **When:** timestamp and duration.
- **Which execution:** trace/request/correlation/operation identifier.
- **What outcome:** success, failure, rejection, retry, timeout, or cancellation.
- **Why:** error type/code and safe explanatory context.
- **Impact:** affected operation or entity and whether the action was retried or recovered.

Prefer:
- `event=inventory.reservation.failed`
- `operation=reserve_inventory`
- `dependency=inventory_db`
- `order_id=...`
- `outcome=failure`
- `error_type=Deadlock`
- `attempt=2`

Avoid vague messages such as:
- `Something went wrong`
- `Cannot process`
- `Error`
- `Request failed`

Do not dump full request/response bodies, database records, or arbitrary objects to obtain context. Extract a small allowlisted set of fields.

### 8. Handle errors and asynchronous execution correctly

- Preserve the original error and its causal chain using the language's established wrapping conventions.
- Include error type/code and safe error text at the appropriate logging boundary.
- Distinguish timeout, cancellation, validation, business rejection, dependency failure, and unexpected internal failure.
- Do not convert an error into success merely to simplify logging.
- Do not log and then silently discard an error.
- Propagate context and correlation metadata through asynchronous tasks and message headers where supported.
- For retries, include attempt number, maximum attempts if known, next delay if known, and final exhaustion outcome.
- Avoid logging an expected cancellation as an unexpected error.

### 9. Security, privacy, and volume controls

Before adding fields, assess whether they can contain:

- Passwords, API keys, tokens, cookies, authorization headers, or secrets.
- Personal data, health data, financial data, or payment-card information.
- Raw request/response bodies, SQL parameters, or user-generated text.
- Internal infrastructure details that should not be exposed.

Use allowlists and explicit redaction. Do not rely only on regex masking after sensitive values have already entered the log event. Sanitize untrusted strings to prevent log injection and control characters.

Consider cardinality and volume: avoid high-cardinality metric labels in logs unless the log backend is designed for them; avoid large payloads and per-item logging in hot loops. Use sampling only where appropriate and never let sampling erase the only record of critical failures or audit-required events.

Application logs are not a substitute for security audit trails, metrics, traces, or durable business records.

### 10. Implement with minimal, maintainable changes

- Use the existing logger and structured logging API.
- Pass context rather than relying on global mutable correlation state.
- Keep event names and field names stable and consistent.
- Avoid creating a generic logging framework unless the codebase genuinely needs one.
- Do not change business behavior merely to add logs.
- Do not catch, suppress, or retry errors differently unless explicitly required.
- Keep log formatting out of business logic when the existing architecture provides a suitable logging boundary.
- Update documentation or shared event conventions when introducing a new schema.

### 11. Verify

Run relevant tests, linters, and build checks available in the repository.

Verify that:

- Important success, failure, rejection, timeout, and retry paths emit the intended events.
- Structured fields have stable names and expected types.
- Correlation identifiers survive relevant boundaries.
- Durations are measured correctly and use a documented unit.
- Original errors remain discoverable.
- Sensitive fields and payloads are not logged.
- Expected business outcomes are not incorrectly elevated to ERROR.
- Duplicate logs and excessive high-frequency events have not been introduced.
- Logging failures cannot break the application unless the system explicitly requires fail-closed audit behavior.

If tests cannot reliably assert logger output, use the project's test logger or capture mechanism. Do not make tests brittle by asserting incidental wording when a stable event name and structured fields are available.

## Deliverables

For a substantial logging task, provide:

1. **Process flow:** concise description or diagram of the actual execution path.
2. **Logging audit:** important existing gaps, vague/noisy/duplicate/unsafe events, with code references.
3. **Event design:** proposed event names, levels, and structured fields.
4. **Implementation:** focused code changes that preserve application behavior.
5. **Verification:** tests and commands run, plus any checks that could not be run.
6. **Incident investigation guide:** example queries or a short sequence for following one execution through the logs, adapted to the actual log backend and schema.

For a small task, keep the deliverables proportionate; do not create unnecessary documents.

## Incident investigation usability test

Before finishing, simulate an investigation using only the proposed logs:

1. Find the initial request, job, or message.
2. Follow its correlation/trace identifier through the process.
3. Identify completed and failed steps in chronological order.
4. Determine the failing dependency or component.
5. Find the root error and relevant attempt/timeout information.
6. Determine whether the operation recovered, partially completed, or failed.
7. Identify the affected business entity using a safe identifier.

If this cannot be done, identify the missing context and improve the event design. Do not claim that logs prove a root cause when they only show correlation.

## Final response format

Summarize the work using this structure:

### Process analyzed
- Entry point and key stages.
- Important dependencies and state transitions.

### Logging gaps
- Finding, location, operational impact.

### Changes made
- Event names, levels, and key fields.
- Correlation and error-handling improvements.
- Security/redaction considerations.

### Verification
- Tests and commands executed.
- Results and limitations.

### Incident investigation
- How an engineer can follow a request/job and identify the failure from the emitted logs.

Be precise. Separate facts confirmed from code from recommendations or assumptions. Do not claim that logs, tests, or commands were executed unless they actually were.
