---
name: repository-quality-review
description: Review repositories and produce verified recommendations.
version: 0.1.0
author: Cody, Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [repository-review, quality-assurance, GitHub, validation, security]
    related_skills: [github-repo-management, codebase-inspection, requesting-code-review, dogfood]
---

# Repository Quality Review Skill

Review a user-supplied repository as an evidence-driven engineering assessment. Inspect the exact source the user names, validate important behavior with executable probes where possible, and separate confirmed defects from recommendations. This skill produces an assessment; it does not modify the repository unless the user separately requests implementation.

## When to Use

- The user supplies a GitHub or Git URL and asks for changes, updates, risks, or recommendations.
- The user wants a lightweight product or code review of an existing project.
- A project is intended for operational users and needs usability, correctness, reliability, or security scrutiny.
- A completed deliverable must be independently reviewed before being treated as final.

Do not use this as a substitute for a full penetration test, formal compliance assessment, or a complete PR review when those scopes are explicitly requested.

## Prerequisites

- The repository URL, path, or unambiguous repository identity.
- Read access to the source.
- A clean temporary checkout for inspection; never modify the user's working tree for a review.
- If the repository is private, use an existing authorized credential flow. Never request or handle passwords in chat.

## Procedure

1. **Resolve the source.** Treat the latest explicit URL or repository identifier from the user as authoritative. If the user corrects an earlier URL, discard the earlier target. Verify access with `terminal` using `git ls-remote <url>.git` before attempting a clone.
   - Completion criterion: record the exact repository, accessible branches/tags, and whether authentication was required.

2. **Select the review revision.** Prefer the branch or tag named by the user. Otherwise use the default branch. Clone only the selected branch when practical, then record the commit SHA.
   - Completion criterion: the checkout, branch, and commit are known and reproducible.

3. **Inventory the project.** Inspect the tree, entry points, documentation, dependencies, test harness, build/run instructions, and recent history. Use `search_files` and `read_file` for source inspection and `terminal` for Git metadata or project commands.
   - Completion criterion: every relevant source file and execution path is accounted for; missing tests or documentation are explicitly noted.

4. **Model intended behavior.** Identify inputs, outputs, state, side effects, user roles, business rules, and failure boundaries. For calculators and other business-logic tools, write down the formula and invariants before judging implementation.
   - Completion criterion: expected behavior is stated for normal, boundary, invalid, and adversarial inputs.

5. **Validate behavior.** Run the existing tests. If tests are absent, execute deterministic probes against pure functions or a controlled harness without changing the repository. Include zero, one, maximum, malformed, negative, overflow, rounding, and cross-field boundary cases where relevant.
   - Completion criterion: each material finding has a reproduction input, observed output, and expected output or invariant.

6. **Review across six dimensions.** Check correctness, security, reliability, maintainability, usability, and observability. Prioritize defects affecting customer outcomes or financial/business calculations above cosmetic changes.
   - Completion criterion: findings are classified as confirmed defect, likely risk, possible concern, or recommendation.

7. **Check deployment suitability.** Identify obsolete runtimes, unsupported platforms, unsafe defaults, dependency or supply-chain concerns, missing rollback/recovery guidance, and operational training gaps. Do not treat a legacy technology as automatically exploitable; state the concrete maintenance or security impact.
   - Completion criterion: deployment risks are tied to an affected user, operator, or failure mode.

8. **Package the candidate review.** Include repository URL, revision, files/locations, reproduction evidence, severity, impact, recommendation, and validation guidance. Do not claim a fix unless code was actually changed and retested.
   - Completion criterion: another engineer can reproduce every material finding from the report.

9. **Independent QA gate.** For Cody's completed project deliverables, BT performs the initial inspection and validation, then sends the candidate assessment and evidence to QA JIM for independent review. QA JIM is an independent gate, not a rubber stamp. Relay pass, findings, blockers, and unresolved risks before presenting the final assessment.
   - Completion criterion: the final response identifies whether QA JIM passed the assessment or what remains unresolved.

## Review Heuristics

### Business calculations

- Enforce domain invariants explicitly: non-negative monetary values, deposits not exceeding package price, positive integer counts, and schedules that sum exactly to the package total.
- Test one-item schedules separately; a UI option that duplicates a deposit as the first payment can accidentally leave a balance uncollected.
- Use cents/integer arithmetic for money and make the rounding policy visible, especially when adjusting the final payment.
- Reject malformed numeric input instead of relying on permissive `parseFloat` or `parseInt` behavior.

### Customer-service tools

- Prefer clear labels, instructions near the input, visible formulas, and explainable output over feature breadth.
- Make invalid or contradictory combinations actionable: state what is wrong and how to correct it.
- Ensure the displayed result can be reconciled manually by a customer-service employee.
- Provide a small, repeatable test matrix and a short operator guide.

### Security and reliability

- Check injection, unsafe DOM rendering, command execution, file access, secrets, external calls, and privilege boundaries appropriate to the technology.
- Distinguish safe numeric interpolation from genuinely user-controlled HTML; do not report a theoretical XSS without tracing the data path.
- Treat missing tests as a reliability gap, not proof that the implementation is wrong.

## Pitfalls

- Do not search public GitHub broadly when the user has supplied a direct URL.
- Do not guess the user's account or repository from a name fragment.
- Do not ask for a password before testing public read access.
- Do not review an unverified branch or silently substitute the default branch for a versioned branch.
- Do not report a mathematically plausible output as correct without checking the business invariant and total reconciliation.
- Do not let an async QA handoff block the user indefinitely; report the preliminary assessment, then relay the independent QA result when it arrives.

## Verification

Before finalizing, verify that:

- The exact source URL, branch/tag, and commit are recorded.
- Material findings have executable evidence or a clearly stated limitation.
- Normal and boundary behavior were tested.
- Recommendations are ranked and tied to user impact.
- No credentials or secrets were exposed.
- QA JIM received the candidate review when the deliverable is project-level, safety-sensitive, ambiguous, security-impacting, or intended as final.

For reusable calculator-specific probes and invariants, see `references/calculator-review-patterns.md`.
