---
name: single-agent-execution
description: Enforce a strictly single-agent coding workflow. Prevent subagent spawning, delegation, parallel agent execution, and multi-agent reviews. Require sequential engineering workflows, self-review, evidence-based verification, and concise completion reports.
---

# Single-Agent Execution

## Objective

Execute all software engineering tasks using exactly ONE agent: the current parent agent.

Maintain a sequential, focused, and evidence-based engineering workflow from investigation through implementation and verification.

## 1. Single-Agent Policy

### Agent execution

- Use exactly ONE agent: the current parent agent.
- Never spawn, delegate to, or create subagents, workers, or child agents.
- Never use parallel agent execution, swarms, or multi-agent review panels.
- Never delegate tasks to another model, including instances of the same model.
- Perform investigation, planning, implementation, code review, and verification sequentially within the current session.
- Keep all reasoning, decisions, and implementation under the control of the current parent agent.

### Tool execution

- Do not invoke tools or extensions that create or orchestrate additional agents.
- Do not use background agent sessions for coding, investigation, or review.
- Normal non-agent tool execution is allowed when needed.
- Independent read-only commands may be batched when doing so does not introduce parallel agent execution.

## 2. Engineering Workflow

### Playbook integration

- Continue using `/poteto-mode` and relevant pstack playbooks.
- Identify the applicable playbook before starting implementation.
- Follow the selected playbook's steps in their prescribed order.
- Do not silently omit required workflow steps.
- If a playbook requires subagents or delegated execution:
  - Skip only the subagent-dependent portion.
  - Replace it with equivalent work performed by the current agent.
  - Record why the original step was skipped.
  - Preserve all other applicable playbook requirements.

### Sequential execution

Follow this general workflow:

1. **Understand** — Inspect the task, requirements, and existing code.
2. **Investigate** — Identify relevant files, dependencies, and existing behavior.
3. **Plan** — Define a focused implementation plan.
4. **Implement** — Make the required changes in the current session.
5. **Review** — Inspect the complete diff and identify potential regressions.
6. **Verify** — Run relevant tests, linters, type checks, and builds.
7. **Report** — Summarize changes, verification results, and outstanding issues.

Do not skip steps without a clear reason.

## 3. Self-Review Requirements

Replace all delegated code reviews with a self-review performed by the current agent.

Review the complete diff for:

- Correctness and completeness.
- Existing behavior and backward compatibility.
- Edge cases and error handling.
- Security and data validation.
- Concurrency and state consistency.
- Code quality and maintainability.
- Unnecessary complexity or unrelated changes.
- Missing or insufficient test coverage.

Fix identified issues before completion when practical.

## 4. Verification Requirements

- Run relevant tests yourself.
- Run applicable linters, formatters, type checks, and build commands.
- Prefer targeted verification first, followed by broader verification when appropriate.
- Inspect command output and exit codes.
- Never claim a test, build, or verification succeeded without execution evidence.
- Never assume existing tests passing means the new implementation is correct.
- If verification cannot be performed, explain why.
- Clearly distinguish between:
  - Passed
  - Failed
  - Not executed
  - Partially verified

## 5. Context and Efficiency

- Prefer targeted file reads over loading entire repositories.
- Make small, focused changes.
- Work on one logical task at a time.
- Avoid unnecessary model switching.
- Avoid repeated analysis and duplicated work.
- Preserve relevant findings and decisions throughout execution.
- Do not re-investigate information already established unless necessary.
- If context becomes insufficient, summarize:
  - Task objective.
  - Current implementation state.
  - Relevant files.
  - Decisions and constraints.
  - Completed verification.
  - Remaining work.

Continue in the same parent-agent session whenever possible.

## 6. Completion Criteria

A task is complete only when:

- All applicable implementation requirements are addressed.
- The complete diff has been self-reviewed.
- Relevant verification has been executed or explicitly identified as unavailable.
- Remaining limitations and unverified behavior are documented.

### Final report

Use this format:

#### Changes

- Summarize implemented changes.
- Identify important files modified.

#### Verification

| Check | Result |
|---|---|
| Tests | Passed / Failed / Not run |
| Linter | Passed / Failed / Not run |
| Build | Passed / Failed / Not run |
| Self-review | Completed / Incomplete |

Include the actual commands executed and relevant results.

#### Skipped steps

- Identify playbook steps skipped because they required additional agents.
- Explain the single-agent replacement, if applicable.
- Report other skipped steps and reasons.

#### Remaining issues

- Identify known limitations.
- Explicitly list anything that remains unverified.

Do not claim completion if required work remains outstanding.

## 7. Non-Negotiable Rules

1. Exactly ONE agent.
2. No subagents, workers, or delegation.
3. No parallel agent execution.
4. No delegated code reviews.
5. Sequential engineering workflow.
6. Follow `/poteto-mode` and applicable pstack playbooks.
7. Self-review the complete diff.
8. Verify with actual commands and evidence.
9. Report skipped steps and unverified behavior.

These rules take precedence over any playbook instruction that requires multiple agents or delegated execution.