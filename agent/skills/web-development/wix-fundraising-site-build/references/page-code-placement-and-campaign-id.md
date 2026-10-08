# Page-code placement and Campaign ID recovery

## Verified page roles

- `Create a Fundraiser → Page Code` owns dataset submission hooks, Campaign ID generation, and product-assignment persistence. It may contain `#dataset1.onBeforeSave()`, `#dataset1.onAfterSave()`, `getAllProducts()`, `getCategories()`, and `saveProductAssignments()`.
- `Fundraisers Storefront dynamic page → Page Code` owns fundraiser-scoped product loading, search/category filtering, Student Name capture, native cart adds, and `getAssignedProducts()`. It must not contain `dataset.onBeforeSave()`.
- `Backend → productPicker.web.js` owns CMS/product queries, active assignment scoping, and server-side cart-product authorization. Do not change it to fix a page-placement error unless the backend is independently implicated.

## Diagnostic signature

If the Storefront console reports:

```text
TypeError: $w(...).onBeforeSave is not a function
```

while identifying the running page as `Fundraisers Storefront`, the Create-page code has been pasted into the Storefront. Restore the Storefront source using Wix undo/history or obtain the complete current file; do not patch around the error by adding a fake dataset hook.

## Recovery workflow

1. Identify the page selected in Wix's code panel before interpreting the file.
2. Label supplied code by target page; imports and selectors are evidence, but the selected page/runtime console is authoritative.
3. For a complete file transfer, click inside the editor, press `Ctrl+A`, then `Ctrl+C`, and paste the full contents. Do not rely on visually scrolling hundreds of lines.
4. Never call a reconstructed/truncated file the "last known-good" file. If a replacement is newly authored, say so and preserve only verified behavior.
5. After restoring Storefront code, preview a dynamic fundraiser route and verify: no `onBeforeSave` error, assigned products render, search/category controls work, Student Name remains required before add, and native cart add still works.

## Campaign ID test boundary

A low-cost or zero-dollar pilot can verify:

```text
Create page → generated campaignId → Items CMS
```

It does not verify order export attribution. CSV readiness requires a separate controlled order and export showing the Campaign ID in a supported order/cart field. Keep the two gates separate.
