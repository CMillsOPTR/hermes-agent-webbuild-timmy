# Guardian Commerce Rules — Session Reference

## Confirmed business rules

- Each customer order remains a separate Wix order.
- Bulk fulfillment groups separate orders operationally by fundraiser; orders are not merged.
- School is the default bulk destination for school fundraisers.
- `Bulk Destination` and `Bulk Destination Address` remain configurable for organizations, warehouses, coordinators, or other approved destinations.
- At 50 or more total fundraiser items, Guardian covers bulk shipping.
- Under 50 total fundraiser items, bulk shipping cost is deducted from student profits.
- The threshold applies to the whole fundraiser, not an individual cart or order.
- Direct shipping is selected by the customer and ships to the customer's address.
- Bulk shipping joins the fundraiser shipment and goes to the configured bulk destination.
- Student tracking is optional per fundraiser through the existing `studentTracking` campaign field.
- Student name/ID must be verified through receipt, order, fulfillment/reporting, and CSV before being promised.
- Payment-provider setup is Finance/leadership-owned and must not be changed by the site implementation worker.

## Cart policy

The initial launch policy is one fundraiser per cart:

1. Inspect the current cart before adding a fundraiser product.
2. Resolve existing catalog product IDs through active `CampaignProductAssignments`.
3. Block a different-fundraiser, ambiguous, or unattributable cart.
4. Show a clear warning.
5. Offer View Cart as the safe return path.
6. Offer clearing only through an explicit customer confirmation.
7. Never clear silently.

Existing Wix cart contents should remain visible until the customer explicitly clears them. A pre-existing mixed cart is not proof that a new product was added successfully after enforcement; test the second add separately.

## Wix API notes

The current-cart API supports:

```javascript
currentCart.getCurrentCart()
currentCart.removeLineItemsFromCurrentCart(lineItemIds)
currentCart.addToCurrentCart({ lineItems })
```

Wix eCommerce catalog-reference custom text fields are represented under the catalog reference options for products that support them. A documented managed-variant shape is:

```javascript
catalogReference: {
  catalogItemId: productId,
  appId: '215238eb-22a5-4c36-9e7b-e7c08025e04e',
  options: {
    variantId: variantId,
    customTextFields: {
      'Field name': 'Customer value'
    }
  }
}
```

This shape must be tested against the actual Wix product configuration. Do not assume a generic page input becomes an order field automatically.

## Verification recipe

For shipping:

- Test bulk and direct choices with a temporary fundraiser.
- Test a school bulk destination and a configurable non-school destination.
- Verify the order remains separate while the fulfillment report groups it by fundraiser.
- Test total campaign counts below 50 and at/above 50.
- Confirm the resulting shipping/profit treatment with Finance.

For student tracking:

- Enable tracking on one temporary fundraiser.
- Use one product that visibly supports the required custom text field.
- Enter a controlled student name or seller ID.
- Verify cart line, checkout, receipt, Wix order, fulfillment view, and CSV/export.
- Repeat with tracking disabled and confirm the prompt is absent or inactive.
- Do not claim completion if any stage drops the value.

For attribution:

- Use two fundraisers with disjoint assignments.
- Test fresh cart and pre-existing cart.
- Add through the scoped storefront and through the normal Wix product page.
- Compare catalog product ID, variant/SKU, options, quantity, price, custom fields, and campaign metadata.
- Test refresh, back/forward navigation, direct product URLs, and tampered requests.
- Record Preview evidence separately from published-site and production-like QA.

## Source notes

This reference consolidates the September 16, 2026 Guardian Fundraising session and Wix eCommerce documentation checks for current-cart methods, catalog-reference custom text fields, and cart-to-checkout custom-field conversion.
