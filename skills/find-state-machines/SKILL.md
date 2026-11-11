---
name: find-state-machines
description: Analyze application state machines, detect reachable invalid states and transitions, investigate retry and concurrency bugs, prioritize fixes, visualize state flows, implement corrections, and add regression tests.
---

# Find State Machines

Identify the app’s explicit and implicit state machines, and document their states, invariants, and transition tables in a single file. Identify invalid states and transitions that the code can actually reach. Provide concrete code-level evidence for each finding, including execution paths involving retries, asynchronous operations, and concurrency. Avoid reporting purely theoretical issues that cannot be reached through actual code paths. Prioritize confirmed bugs by impact and group related fixes into actionable issues. Visualize the state flows, implement the fixes, and add regression tests that reproduce the original bugs. Run the relevant tests to verify the fixes and prevent regressions. Keep the documentation accurate and updated as the implementation changes.