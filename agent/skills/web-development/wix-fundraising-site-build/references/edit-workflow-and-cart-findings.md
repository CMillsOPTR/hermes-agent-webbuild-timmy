# Edit Workflow and Cart Findings

## Verified pilot findings

- A shared dynamic storefront template applies to every fundraiser record; customer-facing Details and Storefront pages are separate. The Details page can work entirely through CMS bindings and a dynamic button connection even when its page-code panel contains only the default `$w.onReady()` stub.
- The Details page Shop button must connect through the current fundraiser dataset to the Fundraisers Storefront dynamic item page. Selecting a Wix category page or a named fundraiser recreates a hard-coded destination.
- `CampaignProductAssignments` is the source of truth for storefront visibility. Active assignments appear; inactive assignments disappear; zero assignments return zero cards and never fall back to the full catalog.
- An isolation fixture using disjoint assignments passed user-reported checks: TEST 3 exposed only its powerbanks, ISOLATION TEST B exposed only three different products, and direct URL switching, Back navigation, and refresh showed no stale product leakage.
- A new dynamic item may briefly return a generic backend `Unable to handle the request` error in Preview while Wix propagates CMS/assignment/product data. Refreshing later succeeded; treat this as a warning and use Site Monitoring logs rather than guessing at code changes.

## Edit-page pattern

- Preserve the Create a Fundraiser page. Build a separate regular (not dynamic) Edit a Fundraiser page for staff.
- Use a Read & Write dataset against the Fundraisers collection, a non-CMS-connected fundraiser selector dropdown, and a status text element.
- Load all existing products for the picker, load the selected fundraiser's active `textWxProductId` assignments, then clear and rebind the repeater after the selection map is populated. Without the clear/rebind, the summary can show selected products while checkbox controls remain visually unchecked because `onItemReady()` rendered before asynchronous selection loading completed.
- Preserve assignment rows when editing: mark unselected rows inactive and update selected rows' display order; insert only newly selected products. Avoid delete-and-reinsert when audit history or rollback matters.
- Validate the fundraiser selector, assignment IDs, and canonical field casing before saving. The original Create page's Submit connection must remain untouched. On a separate Edit page, an explicit save handler can be used after disconnecting the duplicated Submit action, but verify both dataset and assignment persistence.
- A successful no-change dataset save is not sufficient edit verification. If changing one apparently valid connected text field causes `datasetApi 'save' operation failed` / `Some of the elements validation failed`, inspect all connected and hidden duplicated controls and obtain the exact CMS Field ID before changing code or CMS schema. Distinguish page-level Required flags from CMS Required flags: this pilot's `School or Organization` field was CMS field ID `subtitle`, type Text, CMS-required, while the duplicated Edit-page input Required flags caused partial-edit saves to fail; disabling those page-level flags on Edit only resolved the issue. The original Create page retained its required validation.
- The CMS mapping collection is an acceptable emergency admin fallback for adding a missed product, but a staff Edit page is the intended reusable workflow.

## Cart and attribution findings

- Wix native cart state persists for the browser/visitor or account, so a cart from another fundraiser can remain when a different fundraiser URL is opened. This does not imply product leakage in the scoped storefront, but it creates a mixed-cart attribution risk.
- The storefront add method validates the current fundraiser and product assignment before adding, but validation alone does not attach a fundraiser ID to a Wix cart line/order. Do not claim Finance attribution is complete until cart, checkout, order, fulfillment, and export data prove it.
- Separate native-cart lines can be expected when two Wix products have different catalog product codes, variants, options, or payload identities even when their names/images look similar. Do not merge lines automatically.
- The compact storefront add button adds one quantity and the native cart can change quantity. The normal Wix product page may add a distinct product-code line and supports quantity controls.
- For a pilot, the business may operationally expect one fundraiser link per school and accept the native cart's homepage Continue Browsing behavior. For production, explicitly choose and test either one-fundraiser-per-cart enforcement or supported per-line campaign attribution; never silently clear a customer's cart.

## QA evidence discipline

Record Wix Preview behavior as user-reported evidence unless QA independently observes it. Final approval requires two-fundraiser isolation, empty/inactive assignment behavior, navigation/stale-state checks, direct/tampered add attempts, and production-like order attribution. SEO noindex reduces discovery but does not enforce access control or campaign isolation.

## Daily work tracking

When the user requests workday tracking, record system-local start/end timestamps, completed deliverables, blockers, and next steps. Pass the same concise status to the designated work tracker; do not treat a paused break as a workday close unless the user calls the day finished.
