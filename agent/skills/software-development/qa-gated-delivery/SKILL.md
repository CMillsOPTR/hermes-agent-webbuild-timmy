---
name: qa-gated-delivery
description: "Use when deliverables need independent QA before release."
version: 1.0.0
author: Hermes Agent
license: MIT
platforms: [windows, linux, macos]
metadata:
  hermes:
    tags: [quality-assurance, release-gates, independent-review, validation, artifacts]
    related_skills: [requesting-code-review, test-driven-development, github-repo-management]
---

# QA-Gated Delivery

Use this workflow when a user requires completed project work to receive an independent QA review before it is considered final, especially when the candidate must remain local until user and QA approval.

## Core Contract

1. Implement the requested change.
2. Validate it locally with deterministic tests and static checks.
3. Package the candidate artifact, tests, assumptions, and exact command output.
4. Keep the candidate uncommitted/unpushed when the user has reserved release approval.
5. Send the candidate to an independent QA agent in read-only mode.
6. Relay QA findings, severity, blockers, and residual risk.
7. Provide a directly openable local artifact or preview for user inspection when the work has a UI.
8. Only commit, push, or call the work release-ready after the user's gate and independent QA gate are satisfied.

Never treat the implementer's self-check as independent review. Never describe a pending candidate as final.

## Candidate Packaging

Include:

- Repository, branch, and commit baseline
- Exact changed and added files
- Local artifact paths or preview URL
- Business-rule assumptions
- Test commands and real outputs
- Static/syntax checks
- Known limitations and intentionally unchanged areas
- Explicit statement that no external write occurred, when applicable

For a legacy or single-file UI, preserve the existing GUI/layout unless the user authorizes redesign. Add a browser-openable preview copy when the native runtime is difficult to inspect, ensuring the preview references local dependencies from the same directory.

## Calculation-Heavy Applications

For customer-facing financial or scheduling tools:

- Extract pure calculation logic into a small testable module while preserving the current UI.
- Represent monetary values as integer cents internally; retain percentages and non-monetary bases in their natural units.
- Use strict full-string validation instead of prefix parsers such as `parseFloat`/`parseInt` on user input.
- Reject impossible states before rendering results: negative balances, negative payments, over-deposits, fractional counts, and invalid boundary combinations.
- Add reconciliation output showing the authoritative total and any final-payment adjustment.
- Encode policy rules as tests, including required prerequisites, non-refundable components, and maximum valid amounts.
- Preserve existing business formulas when the user confirms they are correct; change validation and representation around them rather than silently changing policy semantics.

## TDD and Verification

For each defect or rule:

1. Write a focused regression test first.
2. Run it against the current implementation and confirm the expected failure.
3. Implement the smallest change that makes it pass.
4. Run the focused test and then the full suite.
5. Run syntax/static checks on both the extracted module and the host UI file.
6. Perform a smoke test through the actual UI or a deterministic DOM harness.

A useful minimum for a legacy HTML/HTA calculator is:

```text
node tests/calculations.test.js
node tests/ui_smoke.test.js
node --check calculations.js
node --check extracted-inline-script.js
git diff --check
```

Do not claim UI behavior was tested merely because the module tests passed; render a preview or execute a UI smoke harness as well.

## QA Handoff

Send QA a concise, read-only brief containing:

- Candidate location
- Original requirements and policy assumptions
- Changed files
- Exact reproductions of the original defects
- Tests that passed
- What was intentionally not changed
- Explicit request for independent severity classification and release blockers

QA should independently inspect the diff and rerun or recreate critical cases. A pass is not a rubber stamp: unresolved financial, safety, security, or policy defects block release unless the user explicitly accepts the risk.

## Pitfalls

- **Pushing before QA/user approval:** keep changes local and report paths instead.
- **Changing UI while fixing calculations:** isolate the calculation module and preserve layout.
- **Floating-point money:** use integer cents end-to-end for monetary operations and formatting.
- **Prefix parsing:** reject malformed strings instead of silently accepting numeric prefixes.
- **Implicit business policy:** record assumptions such as whether a deposit counts toward payment count and whether amount paid may include TPP.
- **False reconciliation:** ensure displayed components sum exactly to the authoritative package/refund total.
- **Only testing the happy path:** include zero, exact-boundary, over-boundary, malformed, fractional, one-payment, rounding, and prerequisite-missing cases.

## Supporting Reference

See `references/tcs-v5-calculator-review.md` for a condensed example of the legacy HTA calculation workflow, policy assumptions, regression cases, and QA handoff structure.
