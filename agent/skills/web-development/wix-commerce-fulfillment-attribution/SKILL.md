---
name: wix-commerce-fulfillment-attribution
description: "Use for Wix fundraiser shipping and attribution."
version: 1.0.0
author: Hermes Agent
license: MIT
platforms: [windows, macos, linux]
metadata:
  hermes:
    tags: [wix, e-commerce, fulfillment, shipping, attribution, student-tracking, csv]
    related_skills: [wix-fundraising-site-build, qa-gated-delivery]
---

# Wix Commerce Fulfillment and Attribution

Use this skill when a Wix fundraising storefront must preserve campaign, student, shipping, and reporting context through cart, checkout, orders, fulfillment, and exports.

## When to Use

- A fundraiser needs bulk versus direct fulfillment choices.
- A campaign has a 50-item shipping threshold or campaign-level profit allocation.
- Customers must identify a student seller and the value must reach receipts, orders, or CSV exports.
- Cart lines need campaign validation or a one-fundraiser-per-cart policy.
- Finance needs order-level attribution without merging separate customer orders.

## Core operating contract

1. **Separate financial ownership from implementation.** Payment-provider selection, bank/payout setup, fees, refunds, and payment configuration belong to Finance or leadership. Do not connect Stripe, Wix Payments, or another provider unless the authorized owner explicitly directs that action.
2. **Treat customer orders as separate records.** Bulk fulfillment groups separate customer orders operationally by fundraiser; it must not merge them into one Wix order. Preserve individual receipts, refunds, line attribution, and reporting.
3. **Model bulk fulfillment at campaign level.** The normal school fundraiser sends bulk orders to the school. Keep the destination configurable for an organization, warehouse, coordinator, or another approved destination. The campaign threshold applies to total fundraiser volume, not an individual customer cart.
4. **Record the 50-item rule explicitly.** At 50 or more total fundraiser items, Guardian covers bulk shipping. Under 50 total fundraiser items, bulk shipping cost is deducted from student profits. Do not implement this as a per-order shipping discount.
5. **Distinguish bulk and direct fulfillment.** Bulk orders join the fundraiser shipment and go to its configured destination. Direct orders are selected by the customer and ship to the customer's address with the approved direct-shipping charge. Verify whether native Wix checkout can represent bulk delivery without requiring an individual shipping address.
6. **Treat student tracking as optional per campaign.** A campaign setting such as `studentTracking` enables the feature. Prefer controlled student IDs or student-specific links over unrestricted free text, but support a customer-entered student name only after proving that it persists through cart, receipt, order, and CSV.
7. **Verify persistence before promising reporting.** Wix eCommerce supports catalog-reference custom text fields for products that support them. A page input alone is not sufficient: test that the name survives the exact add-to-cart path, checkout, receipt/order display, and Finance export. If the product does not support the field, fail closed or clearly label the feature as unverified.
8. **Use supported Wix APIs only.** Current-cart inspection can use `currentCart.getCurrentCart()`. Explicit removal can use `currentCart.removeLineItemsFromCurrentCart(lineItemIds)`. Add catalog products with the Wix Stores app ID and product catalog ID. Verify API signatures against current Wix documentation before coding.
9. **Enforce a safe initial cart policy.** For one-fundraiser-per-cart, inspect existing line items before adding. Resolve each product ID through active campaign assignments. Block different-fundraiser, ambiguous, or unattributable lines. Show a clear warning and require an explicit confirmation before clearing; never clear a cart silently.
10. **Fail closed on unknown attribution.** A cart line with no catalog product ID, no active assignment, or multiple possible fundraiser assignments cannot be confidently attributed. Do not guess its campaign or add another campaign's item silently.
11. **Keep the storefront scoped server-side.** Product visibility must remain bounded by the current fundraiser dynamic item, active `CampaignProductAssignments`, canonical product ID field, and active Wix product. Client-side filtering is for search/categories after the server has returned the scoped set; it is not an authorization boundary.
12. **Separate customer UX from operational reporting.** Display shipping choices and student prompts in the storefront, but retain campaign-level destination, threshold, student setting, and attribution fields in CMS/order reporting. Do not rely on product names or merged order labels as the reporting mechanism.
13. **Work in reversible checkpoints.** Preserve existing working storefront/cart code. Add one field or flow at a time, test with a temporary fundraiser and one product, and avoid publishing until the full cart-to-order/export path is verified.
14. **Use full copy-safe replacements for code.** When correcting Wix page or backend code, identify the exact file and provide a complete replacement that preserves existing product loading, search, category filtering, product-page links, and native cart behavior.
15. **Maintain evidence labels.** User-reported Wix Preview results are Preview evidence only. Independent QA, published-site verification, and production-like order/export testing must be labeled separately and must not be implied by a successful Preview click test.

## Recommended implementation sequence

1. Confirm business rules and Finance ownership.
2. Audit existing CMS fields, including campaign shipping settings and `studentTracking`.
3. Verify one pilot product's support for custom text fields before adding student-name UX.
4. Add customer-facing fulfillment and student controls with exact element IDs.
5. Pass only supported fields through the native Wix cart/catalog-reference path.
6. Add server-side cart inspection and explicit-clear methods for the chosen cart policy.
7. Test separate orders, bulk grouping, direct shipping, student attribution, receipts, order records, and CSV exports.
8. Test threshold calculations at 0, 1, 49, 50, and more than 50 total fundraiser items.
9. Obtain Finance confirmation and payment access separately.
10. Run production-like checkout only after payment setup is complete and authorized.

## Verification matrix

- [ ] Bulk destination defaults to the school for school campaigns.
- [ ] Bulk destination can be configured for non-school campaigns.
- [ ] Customer can choose the supported fulfillment method.
- [ ] Bulk orders remain separate customer orders but group by fundraiser operationally.
- [ ] Direct orders use the customer shipping address and approved charge.
- [ ] The 50-item threshold is calculated across the fundraiser.
- [ ] Under-threshold bulk cost is allocated according to the approved student-profit rule.
- [ ] Student tracking can be enabled or disabled per campaign.
- [ ] Student name/ID persists to receipt, order, fulfillment view, and CSV, or the gap is explicitly documented.
- [ ] Cross-fundraiser cart additions are blocked.
- [ ] Unattributable and ambiguous cart lines fail closed.
- [ ] Clearing requires explicit confirmation and never happens silently.
- [ ] No payment or payout settings were changed during implementation.

See `references/guardian-commerce-rules.md` for the session-derived fulfillment, student-tracking, cart-policy, API, and verification notes.

## Common pitfalls

- Treating a fundraiser's 50-item threshold as an individual order threshold.
- Merging separate customer orders because they share a fundraiser.
- Assuming a shipping label or product name creates reliable attribution.
- Collecting a student name in a page input without testing order/CSV persistence.
- Adding custom text fields to products without verifying the exact Wix catalog-reference shape.
- Letting normal Wix product-page additions bypass campaign attribution.
- Clearing a visitor's existing cart automatically when they open a new fundraiser.
- Treating a Preview success as proof of production checkout, payment, order, or Finance export behavior.
- Changing payment settings while implementing fulfillment or tracking.
