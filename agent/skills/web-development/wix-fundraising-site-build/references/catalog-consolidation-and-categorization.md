# Catalog consolidation and categorization lessons

Use this reference when migrating a Shopify catalog into Wix Stores for the Guardian fundraising workflow.

## Verified migration rules

- Parse one master row per actual Shopify product; image continuation rows are not separate products.
- Exclude products fulfilled by outside vendors when the business does not sell them through Wix (confirmed example: popcorn; also exclude Cookie Dough/Candy if present).
- Wix import weights accept at most three decimal places. Convert source grams to the Wix unit and round to three decimals before writing the CSV.
- Enforce Wix limits before upload: `handleId` <= 50 characters and product name <= 80 characters.
- External image URLs may appear blank in Preview immediately after a large import even when the image is already present in the Wix product editor. Verify the editor first and allow Wix image ingestion/cache time before re-importing.
- Preserve stable handles for pilot products when replacing a catalog, and verify category membership after import.

## Bulk versus ship-to-home

Shopify may contain separate `BULK` and `SHIP TO HOME` records for the same product identity. Consolidation is a business decision, not a cosmetic cleanup. Before merging, check price, image, description, vendor routing, reporting, and campaign assignment. If approved, remove fulfillment prefixes from customer-facing names and enforce `Direct Shipping`, `Bulk Shipping`, or `Both` at the campaign/checkout level.

## Category workflow

Inspect real source product types and vendors before creating categories. Create categories incrementally, based on observed products, and verify that each category is not a system category or a pilot dependency. Keep `All Products` and active pilot categories. Do not delete old categories until their use is checked; category deletion should not be treated as product deletion without confirming Wix's dialog.

Keep school/campaign assignment separate from product-type categories. Categories such as Custom Tumblers, Water Bottles and Thermoses, Coffee, Spices, Candles, Makeup, Skincare, Electronics, Bedding, and Tools are staff browsing aids; the Manager picker should assign products to a fundraiser only when that campaign is created.

A Wix product may appear `Active` while still being hidden from the online store. If a category has a dashboard count but the picker returns no items, check `Visible in online store` before changing code. In the verified catalog, Makeup, Skincare, Tools, and Bedding failed to appear until their products were made visible.

## Guardian pilot catalog facts

The consolidated commerce import excluded nine popcorn products and produced 282 unique customer-facing product records. Bedding was based on three actual Bomb Bedding products. The Texas Jr. All Stars pilot category must be verified after any replacement import.

## Velo picker setup and debugging

For the first product-picker UI, add the visual controls before writing page code. In the standard Wix Editor, a blank Repeater may already contain placeholder title, text, and button elements; these can be repurposed as product name, product detail/price, and `Select Product`. If the Add Elements search is unavailable, browse `Lists` or `Lists & Grids` manually. Element IDs are not exposed by the right-click context menu: select an element normally and use the Velo `Properties & Events` panel, opened from the Dev Mode/Velo toolbar if hidden, to set stable IDs for code.

A Repeater requires a unique `_id` on every data item. Returning `id` instead of `_id` leaves placeholder rows and produces Wix's `Each item in the items array must have a member named _id` error. Register `onItemReady()` before assigning repeater `.data`. Backend product queries can be paged at 100 items while the UI presents 50 per page. A multi-checkbox with one option is a workable per-product selector; track selected products by Wix product ID and show a summary before persisting assignments.

Use the published test site, authenticated as a real site member with the required Manager role, for runtime checks. Preview may show platform warnings or schema-fetching messages. Do not edit compiled files such as `p1qho.js`; diagnose through the source page/backend files and browser Console.
