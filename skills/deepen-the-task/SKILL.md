---
name: deepen-the-task
description: Deepen a task or prompt without expanding its scope. Identify what a top professional would do that the request overlooks, the rawest primary materials they would investigate, the specific methods and tools they would use, and independent evidence that would establish correctness. Then rewrite the original prompt into a more rigorous, actionable prompt.
---

# Deepen the Task

## Objective

Transform an existing task or prompt into a deeper, more rigorous, and professionally grounded version without making it unnecessarily larger.

The goal is **depth, not scope expansion**.

Do not simply add more requirements, longer explanations, additional deliverables, or generic best practices.

Instead, identify what is missing from the original request that would materially improve the quality, accuracy, reliability, and defensibility of the result.

Think like a top professional who is responsible for producing work that can withstand independent scrutiny.

## Core Principles

1. **Depth over breadth** — Investigate the important dimensions more thoroughly instead of introducing unrelated work.
2. **Primary evidence over summaries** — Prefer original source material over documentation, summaries, tutorials, or previously interpreted information.
3. **Professional judgment over generic checklists** — Identify what an experienced practitioner would actually investigate and why.
4. **Methods over vague intentions** — Specify concrete techniques, tools, and procedures.
5. **Independent verification over self-assessment** — Define evidence that can demonstrate whether the result is correct without relying solely on the agent's claims.
6. **Scope preservation** — Preserve the original objective, constraints, and intended deliverables.
7. **Evidence before conclusions** — Do not invent findings, assume facts, or present unverified hypotheses as established truth.

## Workflow

Before rewriting the original prompt, complete the following four steps.

### 1. Identify the Professional Gap

Ask:

"What would a top professional in this field do here that the original request does not ask for?"

Identify the highest-value missing activities.

Consider:

- Critical investigations that are currently overlooked.
- Hidden assumptions that need to be challenged.
- Failure modes and edge cases that require examination.
- Relevant domain-specific standards and established practices.
- Dependencies, constraints, and interactions that affect the outcome.
- Important distinctions between apparent correctness and actual correctness.

Requirements:

- Think from the perspective of an experienced specialist, not a general-purpose assistant.
- Identify concrete missing work, not generic suggestions.
- Explain why each missing activity matters.
- Prioritize only activities that materially improve the original task.

### 2. Identify the Rawest Source Material

Ask:

"What is the rawest material a top professional would start from instead of what has already been digested for me?"

Identify the closest available sources to the underlying truth.

Prefer, where applicable:

- Original source code over architecture summaries.
- Raw production logs over incident summaries.
- Database records and query execution plans over dashboard interpretations.
- Original research papers over blog posts.
- Primary datasets over published charts.
- Protocol specifications over secondary explanations.
- Original contracts and regulations over informal interpretations.
- Direct measurements over estimates.
- Reproducible observations over assumptions.

For each source, explain:

1. What should be examined.
2. Why it is more authoritative than processed information.
3. What evidence or insight it could reveal.
4. What limitations or biases it may contain.

Do not assume that rawer always means better. Select sources appropriate to the domain and research question.

### 3. Specify Methods and Tools

Ask:

"What specific methods and tools would a top professional use to investigate and execute this task?"

Identify practical, domain-specific techniques.

Consider:

- Analytical and investigative methods.
- Experimental or testing procedures.
- Measurement and validation techniques.
- Relevant standards and specifications.
- Software, frameworks, command-line tools, and specialized equipment.
- Reproducibility and evidence collection.
- Techniques for challenging alternative explanations.

For every important method or tool, explain:

1. What it is used for.
2. What question it answers.
3. What evidence it produces.
4. Why it is appropriate for this particular task.

Avoid generic recommendations such as "use modern tools", "perform thorough testing", or "follow best practices" without specifying how.

Do not recommend tools merely because they are popular. Choose them based on the task.

### 4. Define Independent Verification

Ask:

"What, other than the agent or the user, would demonstrate that the result is correct?"

Define objective acceptance criteria and independent evidence.

Depending on the domain, this may include:

- Reproducible experiments.
- Automated test results.
- Independent measurements.
- Authoritative specifications.
- External validation.
- Statistical significance and confidence intervals.
- Formal proofs or invariant checks.
- Independent expert review.
- Real-world operational observations.
- Cross-validation against independent datasets.

Distinguish between:

- Evidence that supports the conclusion.
- Evidence that could falsify the conclusion.
- Evidence that remains unavailable or inconclusive.

Requirements:

- Define observable acceptance criteria.
- Identify independent sources of truth.
- Specify how conflicting evidence should be handled.
- Never treat the agent's own explanation as proof of correctness.
- Never claim verification that has not actually occurred.

## Prompt Reconstruction

After completing all four steps, rewrite the original prompt.

The rewritten prompt must:

1. Preserve the original objective.
2. Incorporate the highest-value professional insights.
3. Direct the investigation toward primary evidence.
4. Specify appropriate methods and tools.
5. Define objective verification criteria.
6. Make the expected deliverables explicit.
7. Preserve original constraints and scope.
8. Remove ambiguity where it could affect execution.

The rewritten prompt should be directly executable by another capable agent or professional without requiring them to reconstruct the intended methodology.

## Output Format

Always structure the response in the following order.

### 1. What a Top Professional Would Do

Explain the most important missing professional activities and why they matter.

### 2. Raw Materials to Start From

List the primary sources, raw data, original artifacts, and evidence that should be examined.

Explain why these are preferable to already-digested information.

### 3. Methods and Tools

Specify the investigative methods, tools, and procedures, including their purpose and expected evidence.

### 4. Independent Verification

Define how correctness will be established independently.

Include measurable acceptance criteria and possible falsification conditions.

### 5. Rewritten Prompt

Provide one complete, polished, ready-to-use prompt.

The rewritten prompt must incorporate the findings from the preceding four sections.

## Quality Control

Before delivering the result, check:

- [ ] Did the analysis identify something genuinely important that the original request overlooked?
- [ ] Did it identify primary evidence rather than merely recommending more documentation?
- [ ] Are the methods and tools concrete and relevant to the domain?
- [ ] Is there an independent way to establish correctness?
- [ ] Are acceptance criteria observable and testable?
- [ ] Does the rewritten prompt preserve the original objective?
- [ ] Did the rewrite improve depth without unnecessarily expanding scope?
- [ ] Can another competent agent execute the rewritten prompt without guessing the methodology?

If any criterion is not satisfied, improve the analysis before presenting the final prompt.

## Behavioral Constraints

- Do not execute the original task unless explicitly requested.
- Do not rewrite the prompt before completing the four analytical steps.
- Do not automatically turn every task into a large research project.
- Do not add unrelated deliverables.
- Do not mistake verbosity for rigor.
- Do not fabricate tools, standards, sources, or verification results.
- Clearly distinguish established facts, hypotheses, and recommendations.
- If essential information is missing, state the uncertainty and identify what information would resolve it.
- Prefer a small number of high-impact improvements over a large list of generic suggestions.

## Success Criteria

A successful result is not simply a longer prompt.

It is a prompt that causes the next agent or professional to:

- Investigate something they would otherwise overlook.
- Examine stronger evidence than they would otherwise use.
- Apply more appropriate investigative methods.
- Produce results that can be independently verified.

**The ultimate test: Does the rewritten prompt produce materially more trustworthy work without unnecessarily increasing the scope of the task?**