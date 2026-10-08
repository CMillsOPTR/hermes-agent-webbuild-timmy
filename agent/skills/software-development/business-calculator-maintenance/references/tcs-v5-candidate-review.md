# TCS V5 Candidate Review Pattern

This reference captures a reusable workflow for customer-facing calculator changes:

1. Verify the exact repository branch and tree. Do not assume a file named V3 is the current V5 artifact; confirm the requested feature set, such as the Partial Package Refunds tab, is present.
2. Keep changes local and uncommitted while the user inspects an openable candidate. Provide both the legacy runtime file and a browser-preview copy when possible.
3. Share a cents-based calculation module between the legacy UI and Node tests. Use strict full-string parsing for money, integers, and percentages.
4. For partial refunds, model every package on the receipt, allocate TPP first, allocate the remaining paid amount proportionally by package price with deterministic cents allocation, calculate each package independently, and total only checked packages.
5. Verify default rendering, overpayment/overdeposit rejection, missing TPP, malformed inputs, one-payment edge cases, partial-package selection, and exact reconciliation.
6. Send the exact candidate path, branch/commit, policy assumptions, test output, and preview evidence to QA JIM. Do not push to GitHub until Cody and QA JIM approve.

Important user preference: preserve the existing GUI during interim changes; improve validation and explanatory result text without redesigning the interface.
