# Calculator Review Patterns

Use this reference when reviewing customer-service calculators, payment schedules, refunds, or other financial business logic.

## Invariants

- Inputs representing money are finite and non-negative unless the business rule explicitly permits otherwise.
- A deposit cannot exceed the package price unless the product explicitly supports credits or overpayments.
- A payment count is a positive integer; reject decimals, partial numeric strings, and values outside the supported range.
- The sum of displayed payments plus the deposit equals the package total under the documented rounding policy.
- A one-payment schedule has an explicit rule. Do not silently show a deposit as Payment 1 while leaving a balance unallocated.
- A final-payment adjustment is visible and the adjusted schedule reconciles exactly in cents.

## Minimum Probe Matrix

| Case | Expected check |
|---|---|
| Normal even division | Displayed schedule sums exactly to the balance |
| Fractional cents | Rounding policy is consistent and final adjustment is visible |
| Zero package/deposit | Behavior matches documented business rules |
| Deposit equals package | No negative balance or phantom payment |
| Deposit greater than package | Validation error; never negative payments |
| One payment | Full balance is allocated exactly once |
| Decimal payment count | Validation error |
| Partial numeric input | Validation error, not silent truncation |
| Negative values | Validation error |
| Very large values | No overflow, unusable output, or browser lockup |

## Evidence Format

For each defect capture:

1. Input values and selected options.
2. Function or UI path exercised.
3. Actual output.
4. Expected output or violated invariant.
5. Severity and customer/business impact.
6. Smallest remediation and a regression test.

Prefer executing the repository's actual pure calculation functions through a controlled JavaScript or language-native harness. If that is not possible, label the result as an analytical finding rather than an executed reproduction.
