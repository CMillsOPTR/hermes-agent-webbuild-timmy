# Shipping and Student Attribution Notes

## Confirmed business rules

- Wix Payments is the selected provider; approved methods are credit/debit cards, Apple Pay, Google Pay, and PayPal. Finance/CEO owns provider settings.
- Domestic shipping is the intended scope; International/Rest of world shipping was disabled.
- Customer choices are intended to be `Bulk shipment with fundraiser` at $0 and `Direct shipping to my address` at the approved direct rate (approximately $9).
- Each customer order remains separate. Bulk orders are grouped operationally by fundraiser and destination, normally the school; do not merge Wix orders.
- The 50-item threshold applies to total fundraiser volume, not an individual cart. At 50+ items Guardian covers bulk shipping. Under 50, bulk shipping cost is divided evenly among participating students and deducted from their profits. Standard Wix shipping rules do not calculate this campaign-wide settlement; use post-order Finance/Guardian settlement until automation is proven.
- The fundraiser CMS already has `shippingOptions`, `bulkDestination`, `bulkDestinationAddress`, `directShippingPrice`, `bulkShippingThreshold`, and `studentTracking` fields. Do not create a CMS Student Name field: the name is order-level data.
- Student tracking is intended for all fundraisers and one student name applies to the full customer order.

## Wix API evidence

- `wix-ecom-backend.currentCart.getCurrentCart()` retrieves the current visitor cart.
- `currentCart.addToCurrentCart()` accepts catalog references for Wix Stores products.
- Wix eCommerce catalog references support `options.customTextFields` for products configured with matching custom text fields. Example shape:

```javascript
catalogReference: {
  appId: '215238eb-22a5-4c36-9e7b-e7c08025e04e',
  catalogItemId: productId,
  options: {
    customTextFields: {
      'Student Name': studentName
    }
  }
}
```

- Wix cart-to-checkout conversion maps line-item custom text fields to checkout description lines. This must still be tested for the site's catalog, receipt, order record, and Finance CSV before it is treated as reliable attribution.
- If a product does not support the matching custom text field, fail safely and do not claim that a page input alone will reach the order export.

## Safe implementation sequence

1. Verify Wix Payments dashboard read-back shows Checkout Active and Payouts Active for the approved methods; do not change payment settings from the build workflow.
2. Inspect and configure domestic shipping options only; do not leave free international shipping enabled.
3. Add one customer-order Student Name input outside the storefront repeater. The input can be on the storefront rather than the Details page to avoid fragile page-to-page state transfer. If Details-page entry is required, preserve it through navigation explicitly and test refresh, back/forward, new-tab, and normal product-page paths.
4. Pilot the custom text field on one product before changing the catalog broadly. Verify the exact custom-field key and follow the name through cart, checkout, receipt, order, and CSV.
5. Keep `Fundraisers.studentTracking` as a campaign setting even if all current campaigns are enabled; do not add `Student Name` to Fundraisers or CampaignProductAssignments.
6. Treat standard Wix shipping as customer-choice presentation only. Keep campaign-wide 50-item settlement and student-profit deductions in a verified post-order workflow until an order aggregation automation has been proven.

## Failure modes to avoid

- Do not merge separate customer orders into a single fundraiser order; grouping is an operational/reporting concept.
- Do not model the 50-item threshold as a per-cart or per-order shipping rule.
- Do not make the customer pay an under-50 bulk surcharge when the business rule deducts the cost from student profits.
- Do not assume a CMS boolean or a visible input automatically creates order/CSV attribution.
- Do not change product custom fields across the whole catalog before a one-product persistence test.
- Do not route bulk orders to the customer address; bulk uses the campaign's configured destination, normally the school.
