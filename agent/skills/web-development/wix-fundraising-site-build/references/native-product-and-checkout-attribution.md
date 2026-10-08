# Native Product Page and Checkout Attribution Findings

## Verified behavior

- The fundraiser Storefront's `#studentNameInput` is passed as Wix Stores product custom text through `currentCart.addToCurrentCart()`.
- Pilot order 10001 exported `Custom text: Student Name:452`.
- Controlled order 10003 deliberately used different values: Storefront `Bingus`, checkout custom field `Wambus`.
- The export retained `Custom text: Student Name:Bingus`; `Additional checkout info` was blank. This proves the verified CSV path is the Storefront/product custom-text path, not the checkout custom field.
- A checkout custom field may appear on confirmation/order flow without appearing in the Orders CSV export.

## Direct Product Page limitation

- Wix's native Product Page is exposed as a single `#productPage1` component; its internal Add to Cart control is not a normal selectable Velo button.
- A separate custom input on that page does not automatically alter the native cart payload.
- A session-storage handoff is not a reliable solution unless custom code actually executes, the flow remains same-tab/same-origin, and the Storefront route reruns its restore logic. Browser history/native navigation can bypass those assumptions.
- Do not call native Product Page Student Name tracking verified unless a direct Product Page purchase is independently checked through order and CSV.

## Safe decision rule

1. Prefer the verified Storefront path for tracked purchases.
2. Before scaling native product custom text, open the native Product Page for a configured pilot product and confirm the native custom-text control actually renders.
3. If direct native purchases are mandatory, choose between a proven native custom-text option, a custom cart/Product Page flow, or an explicitly documented reporting workaround. Do not assume a checkout field fixes CSV export.

## Bulk catalog rollout

- Do not manually edit hundreds of products by default.
- Export the existing Wix product CSV and preserve Wix's exact headers and all unrelated values.
- Add/update the supported custom-text columns in the exported schema; product CSV schemas can vary by Wix catalog/editor version.
- Pilot-import one or two products, verify native Product Page rendering and cart/order/CSV persistence, then scale.
- Wix supports up to two custom text fields per product, subject to the active product CSV schema.
