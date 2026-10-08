# Scoped Fundraiser Storefront and Native Wix Commerce

## Proven implementation notes

- A Wix CMS collection's display name is not necessarily its internal ID. The observed `Fundraisers` display name used internal ID `Items`; backend queries must use the exact ID from CMS → Manage Collection → Settings/Advanced.
- Dynamic item pages created from the Fundraisers collection auto-generate per-record routes. The observed storefront route pattern was `/fundraisers-1/<slug>`. Case/spacing variants can collide, so require a unique campaign slug/ID before production and never store preview/editor URLs.
- `CampaignProductAssignments` stores the reference field `fundraiser`, the canonical Wix product ID field `textWxProductId`, `displayOrder`, and `active`. Legacy undefined fields (`wixProductId`, `textWXProductID`) should remain until dependency/data backup audit.
- Customer storefront retrieval must be server-side and campaign-scoped: validate the fundraiser record is Active, query active assignments for that exact fundraiser, dedupe product IDs, fetch only those products, and return an empty list for no assignments. Never query the full catalog then filter in browser code and never fall back to all products.
- Wix Store product collection references must be included before category mapping (`.include('collections')`). The page can populate search and category controls from the already-scoped product list; category choices should represent only categories present in that fundraiser's assignments.
- Native Wix commerce is the preferred scope. A `View product` button can link to the existing Wix product page, which handles variants, quantity, inventory, and native Add to Cart. Do not label a navigation button `Add to Cart`. Inline quick-add requires variant/catalogReference handling and must be tested separately.

## Verification recipe

1. Use two active fundraisers with disjoint assignments.
2. Open each dynamic storefront route and confirm only its assigned products appear.
3. Search and filter; ensure controls never request the full catalog.
4. Test empty and inactive campaigns: display zero products.
5. Navigate between routes and use browser back/forward; confirm no stale repeater data.
6. Verify product links, variants, cart contents, and checkout after a payment processor is selected.
7. Configure noindex on manager, detail, and storefront pages before publishing.
