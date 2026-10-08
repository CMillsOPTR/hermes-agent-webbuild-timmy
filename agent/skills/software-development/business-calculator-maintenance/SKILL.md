---
name: business-calculator-maintenance
description: "Use when modifying customer-facing business calculators."
version: 1.0.0
author: Hermes Agent
license: MIT
platforms: [windows, macos, linux]
metadata:
  hermes:
    tags: [calculators, financial-correctness, legacy-ui, QA, release-gating]
    related_skills: [software-development:test-driven-development, software-development:requesting-code-review, github:github-repo-management]
---

# Customer-Facing Business Calculator Maintenance

Use this skill when modifying a calculator used by customer service or operations, especially when it contains payment, refund, allocation, percentage, or policy logic and the existing GUI must remain stable during an interim release.

## Operating contract

- Identify the authoritative repository, branch, commit, and active artifact before editing. Do not assume a similarly named file is current; inspect the tree and confirm all expected tabs/features are present.
- Preserve the existing GUI/layout unless the user explicitly requests UX changes. Put interim improvements in the calculation/validation layer and result text.
- Treat business policy as an executable contract. Record assumptions that materially affect results, especially whether a deposit counts as a payment, whether TPP is included in package totals, and how receipt payment is allocated across packages.
- Monetary values must be represented internally as integer cents. Percentages and non-monetary bases may remain numeric, but every displayed monetary result must derive from cents.
- Reject impossible states before rendering results: negative balances/payments, over-deposits, malformed numeric input, fractional payment counts, missing required policy inputs, and paid amounts above the modeled trip value.
- Preserve explainability for customer-service users: show assumptions, allocation method, non-refundable components, reconciliation totals, timestamps/rule context where useful, and final-payment adjustments.

## Workflow

1. **Confirm artifact identity**
   - Inspect remote branches/tree and locate the exact version containing the requested feature set.
   - If multiple similarly named artifacts exist, resolve the authoritative one before patching.
2. **Write policy-focused tests first**
   - Add pure calculation tests for valid paths and boundary failures.
   - Include overpayment/overdeposit, zero/one-payment, malformed input, cents rounding, missing TPP, and partial-package selection cases.
   - Verify new tests fail against old behavior before implementing when practical.
3. **Extract or centralize calculation logic**
   - Use a small ES5-compatible module if the runtime is legacy, with a Node-compatible export for tests.
   - Use strict full-string parsers; do not rely on permissive `parseFloat`/`parseInt` prefix behavior for user input.
4. **Implement the minimum safe change**
   - Preserve existing formulas unless the user confirms a policy change.
   - Guard invalid states before HTML output is generated.
   - For multi-package partial refunds, allocate receipt payment across all packages consistently, calculate each package independently, and total only explicitly selected packages.
   - Make proportional allocation deterministic and ensure allocation sums exactly to paid cents.
5. **Validate locally**
   - Run module tests, UI smoke tests using the actual artifact script, syntax checks, and whitespace/diff checks.
   - Open a local browser-preview copy so the user can inspect the candidate without relying on the legacy runtime.
6. **QA/release gate**
   - Send the exact local candidate, branch/commit identity, policy assumptions, tests, and evidence to QA JIM for independent review.
   - Do not commit or push to GitHub until QA JIM and Cody approve. If QA identifies blockers, fix and rerun tests/QA.

## Refund policy pattern

For the policy used in the Travel Calculator Suite:

- A refund requires TPP greater than zero.
- TPP plus the configured percentage of the relevant package price is non-refundable.
- Refund is the remaining paid amount after those deductions, floored at zero.
- For a single package, validate paid amount against package price plus TPP.
- For partial-package refunds, model all packages on the receipt, allocate TPP first, allocate remaining payment proportionally by package price, calculate each package's refund independently, and include only checked packages in the displayed total.

Do not silently change these rules. If a business definition is ambiguous, state the assumption and ask before release.

## Compatibility notes

- For legacy HTA/IE-compatible code, use ES5 syntax and avoid modern APIs in the runtime path.
- Keep testability by sharing the calculation module between the legacy UI and Node tests.
- A future migration to a maintained non-HTA runtime should reuse the tested calculation module rather than reimplementing policy logic.

## Related reference

See `references/tcs-v5-candidate-review.md` for the validated TCS V5 artifact-selection, cents-allocation, testing, preview, and QA-gate pattern.
