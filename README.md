# Skills-Book

A curated collection of agent skills for software engineering workflows. Each skill is a self-contained `SKILL.md` file that extends an agent's capabilities with a focused engineering practice.

## Installation

Install all skills from this repository with the skills CLI:

```shell
npx skills add wattanar/skills-book
```

## Included Skills

| Skill | Description |
| --- | --- |
| [`find-state-machines`](skills/find-state-machines/SKILL.md) | Identifies explicit and implicit state machines in application code, detects invalid states and transitions that the code can actually reach, and implements prioritized fixes with regression tests. |
| [`meaningful-application-logging`](skills/meaningful-application-logging/SKILL.md) | Audits application logging and execution flows, identifies observability gaps, and implements structured, high-signal log events that support incident investigation. |
| [`single-agent-execution`](skills/single-agent-execution/SKILL.md) | Enforces a strictly single-agent engineering workflow: no subagents, delegation, or parallel execution, with sequential implementation, self-review, and evidence-based verification. |

## Recommended External Skills

The following third-party skills complement this repository:

| Skill | Installation command |
| --- | --- |
| [modern-web-guidance](https://github.com/GoogleChrome/modern-web-guidance) | `npx modern-web-guidance@latest install` |
| [pstack](https://github.com/backnotprop/pstack) | `npx skills add backnotprop/pstack` |
