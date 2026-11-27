---
name: deepen-the-task
description: Transform shallow tasks and prompts into professionally rigorous, evidence-driven, independently verifiable tasks without unnecessarily expanding their scope. Identify professional gaps, primary source materials, domain-specific methods and tools, and objective verification criteria before producing a ready-to-use rewritten prompt.
---

# Deepen the Task

## Objective

Transform an existing task, specification, or prompt into a deeper, more rigorous, and professionally grounded version without unnecessarily expanding its scope.

The goal is **depth, not size**.

Improve the quality of the work by identifying what an exceptional professional would investigate, what evidence they would examine, which methods they would apply, and how the result could be independently verified.

Do not confuse a longer prompt with a better prompt.

The output must make the next execution materially more accurate, reliable, and defensible.

## Core Principles

1. **Depth over breadth** — Investigate important dimensions more thoroughly instead of introducing unrelated work.
2. **Professional judgment over generic advice** — Think like an experienced specialist responsible for the outcome.
3. **Primary evidence over summaries** — Prefer original source materials over already-processed interpretations.
4. **Methods over intentions** — Specify concrete investigative techniques and tools.
5. **Independent verification over self-assessment** — Establish objective evidence of correctness.
6. **Scope preservation** — Preserve the original objective, constraints, and intended deliverables.
7. **Evidence before conclusions** — Distinguish verified facts, assumptions, hypotheses, and recommendations.

## Workflow

Complete the following four analytical steps before rewriting the original prompt.

### 1. Professional Perspective

Ask:

"What would a top professional in this field do that the original request does not explicitly ask for?"

Identify the most important missing professional activities.

Consider:

- Overlooked investigations and critical considerations.
- Hidden assumptions that need to be challenged.
- Important edge cases and failure modes.
- Dependencies and interactions that affect the outcome.
- Domain-specific standards and established methodologies.
- Differences between apparent correctness and actual correctness.
- Risks that an inexperienced practitioner might overlook.

For each identified gap:

1. Explain what the professional would do.
2. Explain why it matters.
3. Describe the potential impact of overlooking it.
4. Prioritize its importance.

Focus on high-impact improvements rather than generating a large checklist.

### 2. Primary Evidence and Raw Materials

Ask:

"What is the rawest and most authoritative material a professional would start from instead of what has already been digested?"

Identify the closest available sources to the underlying truth.

Prefer primary evidence where appropriate.

Examples:

- Original source code rather than architecture summaries.
- Raw production logs rather than incident summaries.
- Database records and execution plans rather than dashboard interpretations.
- Original research papers rather than blog posts.
- Primary datasets rather than published charts.
- Protocol specifications rather than secondary explanations.
- Original contracts and regulations rather than informal interpretations.
- Direct measurements rather than estimates.
- Reproducible observations rather than assumptions.

For each important source, explain:

1. What should be examined.
2. Why it is authoritative.
3. What information it could reveal.
4. What limitations or biases it may contain.

Do not assume that rawer always means better. Select sources according to their relevance, reliability, and proximity to the question being investigated.

If primary evidence is unavailable, identify the best available alternative and explicitly state its limitations.

### 3. Methods and Tools

Ask:

"What specific methods, techniques, and tools would a top professional use to investigate and execute this task?"

Identify practical, domain-specific methods.

Consider:

- Analytical and investigative techniques.
- Experimental and testing procedures.
- Measurement and validation methods.
- Relevant standards and specifications.
- Software, frameworks, command-line tools, and specialized equipment.
- Reproducibility and evidence collection.
- Techniques for challenging alternative explanations.

For every important method or tool, specify:

1. What it does.
2. Why it is appropriate.
3. What question it answers.
4. What evidence or output it produces.

Requirements:

- Recommend concrete tools instead of vague categories.
- Explain the purpose of each important tool.
- Prefer established methods over fashionable tools.
- Avoid unnecessary tools that do not materially improve the investigation.
- Do not fabricate tool capabilities or standards.
- Account for practical constraints such as environment, access, cost, and available data.

### 4. Independent Verification

Ask:

"What, other than the agent or the user, would demonstrate that the result is correct?"

Define objective acceptance criteria and independent evidence.

Depending on the domain, consider:

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

- **Supporting evidence:** Evidence that supports the conclusion.
- **Falsifying evidence:** Evidence that could demonstrate the conclusion is wrong.
- **Inconclusive evidence:** Evidence that is insufficient to establish correctness.

For each verification criterion, define:

1. What must be demonstrated.
2. How it will be tested or measured.
3. What constitutes passing or failing.
4. Which independent source establishes the result.
5. What limitations remain.

Never treat the agent's own explanation as proof of correctness.

Never claim verification that has not actually occurred.

## Prompt Reconstruction

After completing all four analytical steps, rewrite the original prompt.

The rewritten prompt must:

1. Preserve the original objective.
2. Incorporate the highest-value professional insights.
3. Direct the investigation toward primary evidence.
4. Specify appropriate methods and tools.
5. Define objective verification criteria.
6. Make expected deliverables explicit.
7. Include relevant constraints and edge cases.
8. Remove ambiguity that could compromise execution.
9. Remain proportional to the original task.

The rewritten prompt must be directly executable by another capable agent or professional without requiring them to reconstruct the intended methodology.

## Output Format

Always present the result in the following order.

### 1. What a Top Professional Would Do

Describe the missing professional activities, why they matter, and their expected impact.

Use normal readable Markdown.

### 2. Raw Materials to Start From

Identify the primary evidence, original artifacts, raw data, and authoritative sources that should be examined.

Explain why these are preferable to already-digested information.

Use normal readable Markdown.

### 3. Methods and Tools

Specify the investigative methods, techniques, tools, and procedures.

Explain their purpose and expected evidence.

Use normal readable Markdown.

### 4. Independent Verification

Define how correctness will be established independently.

Include measurable acceptance criteria, independent evidence, and possible falsification conditions.

Use normal readable Markdown.

### 5. Rewritten Prompt

Provide one complete, polished, ready-to-use prompt.

The rewritten prompt must incorporate the findings from the preceding four sections.

#### Raw Markdown Output Requirement

The rewritten prompt MUST be presented as a standalone raw Markdown code block.

Requirements:

- Use standard Markdown syntax only.
- Do not render the rewritten prompt as rich text.
- Do not wrap it in HTML, XML, or other markup formats.
- Do not include analysis or commentary inside the rewritten prompt.
- Ensure the entire prompt can be copied and pasted without modification.
- Include all necessary context, instructions, constraints, methods, and acceptance criteria.
- Preserve Markdown formatting, including headings, lists, checkboxes, and code blocks.
- Do not omit important details merely to make the prompt shorter.
- Make the prompt self-contained and independently executable.

The four analytical sections must remain outside the code block as normal readable Markdown.

Only Section 5 should be enclosed in the standalone raw Markdown code block.

## Quality Control

Before delivering the result, verify:

- [ ] The analysis identifies genuinely important gaps in the original request.
- [ ] Professional recommendations are specific and relevant.
- [ ] Primary evidence is preferred over processed summaries where appropriate.
- [ ] Evidence sources have identifiable limitations.
- [ ] Methods and tools are concrete and justified.
- [ ] Independent verification is defined.
- [ ] Acceptance criteria are observable and testable.
- [ ] Potential falsifying evidence is considered.
- [ ] The rewritten prompt preserves the original objective.
- [ ] The rewrite improves depth without unnecessarily expanding scope.
- [ ] Another competent professional or agent can execute the rewritten prompt without guessing the methodology.
- [ ] The rewritten prompt is self-contained.
- [ ] The rewritten prompt is provided in a standalone raw Markdown code block.
- [ ] The analysis is outside the raw Markdown code block.

If any criterion is not satisfied, improve the analysis before presenting the result.

## Behavioral Constraints

- Do not execute the original task unless explicitly requested.
- Do not rewrite the prompt before completing the four analytical steps.
- Do not automatically turn every task into a large research project.
- Do not introduce unrelated requirements or deliverables.
- Do not confuse verbosity with rigor.
- Do not fabricate findings, evidence, tools, or standards.
- Clearly distinguish facts, assumptions, hypotheses, and recommendations.
- If essential information is missing, state the uncertainty and identify what would resolve it.
- Prefer a small number of high-impact improvements over a large list of generic suggestions.
- Do not claim that external verification has occurred when it has not.
- Do not force all four analytical steps into artificial or irrelevant recommendations when a particular step has limited applicability. Explain why instead.
- Do not ask unnecessary clarification questions when reasonable assumptions can be stated explicitly.
- Preserve the user's original language unless there is a clear reason to change it.

## Success Criteria

A successful result is not simply a longer prompt.

It is a prompt that causes the next agent or professional to:

- Investigate something they would otherwise overlook.
- Examine stronger evidence than they would otherwise use.
- Apply more appropriate investigative methods.
- Challenge assumptions and alternative explanations.
- Produce results that can be independently verified.

**Ultimate test: Does the rewritten prompt produce materially more trustworthy, accurate, and defensible work without unnecessarily increasing the scope of the original task?**