---
name: wix-fundraising-site-build
description: "Use when guiding a Wix fundraising site build."
version: 1.0.0
author: Hermes Agent
license: MIT
platforms: [windows, macos, linux]
metadata:
  hermes:
    tags: [wix, fundraising, ecommerce, cms, velo, guided-build, accessibility]
    related_skills: [computer-use, qa-gated-delivery]
---

# Wix Fundraising Site Build

Use this skill when guiding a non-specialist through building or configuring a Wix fundraising/ecommerce site, especially when the user is working under deadline, using an inherited template, or needs incremental walkthrough support.

## Core operating contract

1. **Inspect the real editor before prescribing changes.** Public URLs may be stale because Wix editor changes are unpublished. Treat the Wix Editor view as the source of truth while building; use the public site only for published-state verification.
2. **Read-only assistance by default.** When a shared editor is visible through desktop/browser inspection, never click, type, drag, rename, delete, save, or publish unless the user explicitly changes this authorization. The user performs all edits; the agent identifies controls and gives the next action.
3. **Preserve the inherited template.** Do not redesign from scratch or replace approved layout/branding unless requested. Reuse existing header, footer, section styles, navigation patterns, and product-program pages. Remove or hide only clearly irrelevant template content, and prefer reversible hiding over deletion.
4. **Work in bite-sized checkpoints.** Give one small action at a time, with exact text when editing copy. Stop after the action and wait for a confirmation such as “done” before proceeding. Avoid dumping the full project plan during execution; keep the larger plan internally and surface only the current task.
5. **Maintain an explicit step ledger.** State the current step, what is complete, what is pending, and the exact next action. If the user reports a different UI state, adapt to the editor they actually see rather than repeating generic Wix instructions.
6. **Do not publish during construction.** Use Wix Preview for testing. Reserve Publish for an explicit launch/review gate. Warn that “changed in another browser” can indicate a concurrent editor session or autosave conflict; do not overwrite, restore, or discard without inspecting the warning.
7. **Use the visible editor state as the interaction contract.** When the user provides a screenshot, identify the actual control labels and selected state before giving the next click. Do not infer that a decorative page icon opens a page, or that a title click navigates; Wix may use the three-dot menu for page actions and the current-page selector for navigation. If a control behaves differently than expected, acknowledge the correction and switch to screenshot-led guidance.
8. **Provide complete entry values for forms and dialogs.** When a Wix dialog requires naming or configuration, give one compact fill-in table containing exact values, fields to leave blank, and fields not to change. Avoid a field-by-field interrogation unless the visible dialog makes the remaining fields conditional.
9. **Separate agent inspection from user actions.** The agent may inspect the open editor or screenshot to locate controls, but the user performs all clicks, typing, dragging, renaming, saving, and publishing. Do not claim the page changed until the user confirms the action or a new screenshot shows it.
10. **Honor momentum requests.** If the user asks for “just the next step,” reports ADHD/frustration, or asks what is taking so long, briefly acknowledge the issue and give the next concrete action immediately. Do not insert a clarification card for a low-risk pilot choice; use a clearly labeled default and let the user correct it.
11. **Make Velo edits copy-safe.** When the user is editing Wix code and asks for the full code, provide one complete replacement block, name the exact target file (`Create Fundraiser` page code versus `Backend → productPicker.web.js`), state whether existing code must be preserved, and avoid making them hunt for individual lines. After a code change, verify in the published test site and use the first concrete browser-console error rather than guessing.
12. **Use exact IDs and staged discovery before wiring form submission.** Do not invent dataset or button IDs. Have the user report the exact Velo ID for the Submit button and the connected fundraiser dataset, then provide code matching those IDs. Treat the visible selected-product summary as a completed UI feature, but keep persistence to `Campaign Product Assignments` as a separate verified step.
13. **Keep fundraising pages out of search engines.** Before launch, configure the Create a Fundraiser page and the generated fundraiser detail pages as noindex, while separately controlling member access and menu visibility. Do not confuse hiding a page from navigation with preventing search indexing; verify the SEO setting on both the manager page and its dynamic page.
14. **Diagnose dataset submission before backend persistence.** When a Wix form submit fails with `datasetApi 'save' operation failed` / `Some of the elements validation failed`, the dataset record was rejected before `onAfterSave` and product-assignment code can run; therefore, do not debug post-save assignment or URL-writing code until the initial dataset save succeeds. Inspect the form for red validation outlines and valid field formats first (especially email inputs; a placeholder such as `TEST EMAIL` is invalid). Treat repeated `message channel closed` or async-listener console messages as preview/browser noise unless they directly identify the failing field. Dataset action dropdowns expose actions such as Submit/New/Delete, not page-code event hooks; preserve the existing Submit connection and use the dataset's `onAfterSave` handler in page code. For an initial flexible staff workflow, `View: Collaborators` plus `Add: Members only` is an acceptable interim collection permission setup; defer role-specific restrictions until the staff roles are defined.

15. **Default to full copy/paste code replacements.** When the user asks for code or a correction, provide the complete target file even if only one line changed; identify the exact file and preserve working behavior. This supports rapid progress toward a customer-facing fundraising site and avoids line-hunting.
16. **Distinguish CMS field IDs from page element IDs.** When writing `wixData` records, use the exact case-sensitive CMS Field ID shown in the collection's Edit field dialog, not a guessed camel-case variant and not a page `$w()` selector. After a mismatch, preserve data, correct the full code, and verify a new record in the intended column before cleaning up undefined fields.
17. **Loop in QA-Jim for meaningful verification.** For Wix workflow/code checks, request an independent QA review before calling the result verified; relay QA findings and do not treat a user report alone as a complete QA gate when a review is practical.
18. **Treat dynamic-page storefront isolation as a release blocker.** A dynamic Fundraisers item page may be automatically connected to the Fundraisers collection and generate per-record routes, but that does not scope a Wix Store gallery or a hard-coded Shop button. Keep the details page separate from a storefront page; query `CampaignProductAssignments` server-side for the exact current fundraiser, keep only active assignments, fetch only those Wix product IDs, and return zero products when assignments are empty. Never load the entire catalog and filter only in browser code, never fall back to all products, and do not trust a user-supplied campaign query parameter as identity. Test two campaigns with disjoint products, empty/inactive assignments, deleted products, direct URL tampering, and repeated navigation for stale repeater data before customer exposure.
19. **Handle dynamic URLs explicitly.** Creating an item page from the Fundraisers collection auto-connects a read-mode dataset and produces one route per record. Wix creates a generated Page Link field in the collection for each dynamic page; its Field ID is site-specific, so inspect the collection and use the exact ID rather than guessing. A separate URL field (for example, `storefrontUrl`) does not populate itself. For a post-create share link, use the Storefront dynamic item's generated Page Link value, and only write/display it after the fundraiser dataset save and product-assignment save succeed. A separate Fundraisers List page is not required to generate or share an individual campaign link; it is only needed if customers should browse a directory. Do not store editor or Preview URLs as customer links. Verify the published URL pattern before sharing production links, and configure noindex for Create a Fundraiser, fundraiser detail, and storefront pages before launch.
20. **Use the CMS internal collection ID, not its display name.** A Wix collection can display as `Fundraisers` while its internal ID differs. A backend query using the display label can fail with `WDE0025: The Fundraisers collection does not exist`, even though dynamic datasets work. Before writing backend queries, retrieve the exact Collection ID from the collection's Advanced settings and use that literal value. Apply the same check to every custom collection. Also inspect dynamic-page URL columns for duplicate slugs (for example, case variants can resolve to the same slug); require a unique campaign ID/slug before production.
21. **Separate cart/page isolation from reporting attribution.** A user may accept the existing fundraiser-scoped storefront and one-cart behavior while still requiring an explicit fundraiser/campaign value in the Finance CSV. Do not interpret “we do not need separate carts/pages” as waiving CSV attribution. Treat the two requirements independently, confirm the distinction before updating release gates, and correct Tracker/QA records if the user clarifies it.
22. **Report delegated-agent updates concisely.** When Tracker or QA confirms an update, tell the user that it was recorded/confirmed and summarize only the decision or status they need. Do not repeat the agent's full worklog unless the user asks for it.
23. **Treat the user's live Wix observation as authoritative.** If the user says a newly created record produced a value, or that a screenshot shows a current result, accept that state first; do not reinterpret it as an older fixture or argue from assumptions. Acknowledge the correction and diagnose from the evidence shown.
24. **Label every code block by exact page before editing.** Explicitly distinguish `Create a Fundraiser → Page Code`, `Fundraisers Storefront dynamic page → Page Code`, and `Backend → productPicker.web.js`. A Create-page file containing `dataset.onBeforeSave()` must never be pasted into the Storefront. If a console says `onBeforeSave is not a function` on the Storefront, identify misplaced Create-page code as the first diagnosis.
25. **Never reconstruct a truncated Wix file as if it were the last known-good file.** If the complete Storefront source is unavailable, say so and ask the user to copy the full editor contents or use a reversible Wix undo/history action. If offering a newly authored replacement instead, label it as a new replacement—not as recovered prior code—and preserve only behavior that is verified from the available source.
26. **Use a low-friction recovery path when the user is overwhelmed.** Give one keyboard-copy step (`click inside editor → Ctrl+A → Ctrl+C`) or one page-selection step at a time, avoid repeated requests for already supplied code, and do not ask for console details until page identity and code placement are verified.
27. **Separate Campaign ID CMS verification from order/CSV attribution.** A zero-dollar or low-dollar product test can verify `Create page → generated campaignId → Items CMS`, but it does not prove `campaignId → cart → order → CSV`. State that distinction before the test and do not imply CSV readiness from a CMS pass.
28. **Respect constrained test budgets.** If the user is approved only for $1 tests, do not suggest price changes or new products/pages as the first workaround. Prefer an existing zero-dollar/low-cost pilot product and clearly state which release gate that test can and cannot verify.

## Page-code and dynamic-link verification

- **Do not confuse page code with CMS-connected page behavior:** A dynamic Details page can render its fundraiser title/content and navigate correctly through CMS bindings even when its page-code panel contains only Wix's default `$w.onReady()` starter stub. Storefront repeater/cart code belongs on the separate Fundraisers Storefront page; do not paste storefront code into the Details page merely because the Details page is being previewed.
- **Verify the current page before diagnosing missing code:** Check the Preview URL and page identity first. Details routes resemble `/items/<slug>`; storefront routes resemble `/fundraisers-1/<slug>`. Inspect the selected code tab before replacing anything. A working dynamic page with a default code stub is not evidence that code was deleted.
- **Connect dynamic buttons to the current item, not a named record:** In Wix's Connect Button panel, select the dataset already attached to the dynamic Details page (for this project, the `Items Item` dataset for the `Fundraisers` collection), then set `Click action connects to` to the Fundraisers Storefront dynamic item page. Never select a category page or a specific fundraiser record, because that recreates a hard-coded destination. The same button should resolve the matching storefront for every current fundraiser item.
- **Preview sharing is an access gate, not production verification:** An unpublished Wix Preview/feedback link may require Wix access and may not be usable by an external coworker. Do not publish solely to enable ad-hoc testing, and never treat a preview URL as a customer URL. Use an authorized collaborator/member or internal Preview for isolation checks until the staging access model is deliberate.
- **Keep user-reported evidence qualified:** When a user confirms Add to Cart/View Cart works in Preview, record it as user-reported Preview evidence. Final QA still requires production-like checkout, fundraiser isolation, attribution, and mixed-cart policy verification; do not claim independent observation.
- **Use Site Monitoring for opaque backend failures:** If the browser console only reports `Unable to handle the request` from a backend web method, do not guess at a code change. Open Wix Developer Tools/Site Monitoring logs and capture the server-side error correlated with the failing dynamic route. Compare a working fundraiser and failing fundraiser across CMS status, assignment reference, canonical product-ID field, active flag, product existence, and collection/category references. Treat browser warnings such as `Unrecognized feature: 'vr'`, Firebase duplicate-load messages, and async listener noise as unrelated unless the server log connects them to the failure.
- **Build isolation fixtures with disjoint assignments:** Use two temporary active fundraisers whose `CampaignProductAssignments` contain non-overlapping products. Verify assignment rows and canonical product IDs before opening storefronts; then test direct routes, search/category filters, refresh, back/forward, and empty/inactive cases. A correct route alone is not proof that the server-side scoped query succeeded.

## Session-learned implementation notes

- Treat Details and Storefront as separate dynamic-page responsibilities. A CMS-connected Details page may need no custom code; keep scoped repeater/cart code only on the Storefront page.
- For a staff Edit Fundraiser page, prefer a regular Read & Write page with a non-connected fundraiser selector over a dynamic customer page. Load existing active assignments asynchronously, then clear and rebind the repeater so visual checkbox state matches the selected-products summary.
- Keep mapping rows for auditability: deactivate removed assignments, update selected rows, and insert only new products instead of deleting and recreating everything.
- Native Wix cart state persists across fundraiser URLs. Product-assignment validation protects storefront adds but does not itself create order-level fundraiser attribution. Require an explicit mixed-cart policy and verify attribution through the order/export before production.
- A separate Edit page may disconnect its duplicated dataset Submit action and call `dataset.save()` explicitly; this worked for product-assignment edits. Do not assume it proves ordinary field edits are valid: if changing one apparently valid connected text field causes `datasetApi 'save' operation failed` / `Some of the elements validation failed`, inspect hidden/duplicated connected controls and obtain the exact CMS Field ID before changing code or schema.
- When a new dynamic record briefly fails with a generic backend request error, retry after Wix propagation and inspect Site Monitoring before changing code.
- Treat payment-provider confirmation as a read-back gate owned by Finance/CEO: verify the dashboard status and enabled methods, but do not alter financial settings from the webpage-build workflow.
- Separate customer-facing shipping choices from campaign-level settlement. Standard Wix shipping can present bulk/direct options, but it does not calculate a fundraiser-wide 50-item threshold or divide under-threshold shipping costs evenly among students. Keep that calculation in a post-order Finance/Guardian settlement workflow until automation is proven.
- Preserve separate customer orders for receipts, refunds, attribution, and reporting; group bulk orders operationally by fundraiser and configured destination (normally the school) rather than merging Wix orders.
- Do not create a CMS Student Name field for one-name-per-order tracking. Keep `studentTracking` as the fundraiser setting and carry the customer-entered name at cart/order level. Verify supported Wix catalog custom-text-field configuration and persistence through cart, checkout, receipt, order, and CSV before claiming Finance-ready attribution.
- Prefer collecting the one student name on the storefront next to Add to Cart rather than on the Details page, unless explicit state transfer is implemented and tested across navigation, refresh, new tabs, and normal Wix product-page paths.
- Pilot order-level custom text fields on one product before changing the whole catalog. Use the exact configured custom-field key; a page input alone is not proof that the name will appear in a receipt or export.
- Separate the two Student Name entry paths during verification. Wix's native checkout custom field can appear on the confirmation/order flow, but in the verified exports it did not populate `Additional checkout info`; the CSV retained the Storefront/product custom-text value instead. Run a discriminating test with different values (for example, Storefront=`Bingus`, checkout=`Wambus`) before claiming the checkout field is exported.
- Treat direct native Wix Product Page purchases as a separate customer path. A standalone page input and `wix-storage-frontend` handoff are not sufficient if the native Product Page does not execute the custom code or if browser history restores the Storefront without rerunning its dataset handler. Do not claim native Product Page Student Name tracking is supported unless a direct Product Page purchase independently reaches the same order/CSV field. If direct purchases must be tracked without rebuilding the Product Page, first test whether Wix's configured native product custom-text option renders on the Product Page; otherwise document the limitation and keep the Storefront path authoritative.
- When a native product custom-text field must be applied broadly, do not edit hundreds of products manually by default. Export the existing Wix product CSV, preserve Wix's exact headers and all unrelated product data, add/update the supported custom-text columns, and pilot-import one or two products before scaling. Wix supports up to two custom text fields per product; verify the exact schema in Wix's own export because product CSV schemas can differ by catalog/editor version.
- Keep customer guidance explicit when both paths exist: enter Student Name on the fundraiser Storefront and use its Add to Cart button for verified attribution; native View Product is for options/variants unless direct Product Page attribution has been separately proven.
- A second product custom-text field configured as `Campaign ID` remains customer-visible on Wix's native Product Page. The Storefront cart code can inject `customTextFields: { 'Student Name': studentName, 'Campaign ID': currentCampaignId }`, but a native Product Page purchase bypasses that dynamic CMS context and exports an empty Campaign ID when the customer leaves it untouched. Treat that route as an attribution failure, not a successful automatic test.
- When native Product Page attribution cannot be controlled, keep the Campaign ID field optional and prevent customers from reaching the native Product Page from fundraiser storefronts by hiding/disabling the repeater's View Product button. Add the product description to the scoped repeater instead; require the exact new text-element ID before writing code (for example, `#assignedProductDescription`). Do not claim native Wix custom-text inputs can be hidden or auto-populated through guessed Velo selectors or DOM injection.
- When a user is overwhelmed or asks for the next step, give one concrete editor action at a time. For Velo changes, label the exact page and provide a complete replacement file only after the full current file is available; do not reconstruct truncated Wix source.

- Wix's native Orders export can carry product custom text into the CSV. A verified pilot showed `Student Name:452` in the order, receipt, and exported CSV when `452` was the actual entered value. For fundraiser reporting, Student Name alone is insufficient: add a separate fundraiser/campaign value to the line-item/order data and independently re-export a pilot order. Do not assume the current storefront's scoped page or product assignment will create a CSV column automatically.
- When the user says separate fundraiser carts/pages are unnecessary but requires fundraiser attribution in the CSV, preserve both requirements separately: scoped storefront authorization governs what can be purchased, while explicit fundraiser metadata governs Finance reporting. Treat CSV attribution as its own release gate.
- For CSV attribution implementation, prefer a staged pilot of a second configured custom text field (for example, `Fundraiser`) on one product, passed from the current fundraiser context by backend code, before broad catalog rollout. Use a stable campaign ID/value rather than relying only on a display name. Verify cart, receipt, order detail, and `Orders → Export → Item purchased` output; if native export does not expose the value, stop and document the limitation rather than claiming Finance readiness.
- When preparing a Wix catalog update for automatic campaign attribution, preserve the source export and modify only the approved custom-text columns. Keep `customTextField1=Student Name`, set field 2 to the exact configured label (for example, `Campaign ID`), use a compatible character limit, and leave field 2 non-mandatory when the value will be injected by storefront code rather than typed on the native Product Page. Verify row count, unique `handleId` count, and zero unrelated-cell diffs programmatically before giving the user the import file. The catalog import only configures the field; it does not create automatic attribution until Storefront cart code sends the current CMS `campaignId` under the exact field label.
- Do not report a user-imported catalog as independently verified. Mark the import user-confirmed until Wix read-back or a subsequent order/export proves the field configuration and persistence.
- Keep storefront product loading and cart-status inspection in separate `try/catch` blocks. If `getCartFundraiserStatus()` fails with Wix's generic `Unable to handle the request`, do not clear the repeater or hide already-loaded assigned products; log a nonfatal warning and leave products usable. A broad catch around both calls caused products to appear only after typing in the search box because the catch erased them.
- Use `wix-storage-frontend` session storage for a fundraiser-scoped Student Name value when customers navigate from the storefront to a native Wix product page and back. Restore the value after the dynamic dataset identifies the current fundraiser, and remove it when the input is cleared. This preserves the existing mini Add to Cart path without pretending that a native product-page custom-text widget can be controlled by an invented selector.
- Native Wix Product Page custom-text fields may have no exposed Velo input ID. Do not use DOM injection or guess selectors to auto-populate them. If a separate page input is staged, keep it non-required and hidden until a supported cart connection is implemented; a standalone input does not affect Wix order data.
- When diagnosing the storefront redirecting to a product page, remember that the page code intentionally falls back to `product.productPageUrl` after `addAssignedProductToCart()` fails. Capture the first concrete backend error and inspect Wix Developer Tools → Logs → View site events (formerly Site Monitoring) before changing code. Ignore unrelated console noise such as `vr`, Grammarly unload, Firebase duplicate-load, warmup, and Swiper warnings unless server logs connect them to the failure.
- Treat Preview-versus-live cart differences as an authentication/context gate before declaring a Test Site limitation. Wix's `currentCart.addToCurrentCart()` requires visitor or member authentication; Preview may run in an authenticated owner/editor context while an anonymous live visitor does not. Confirm the test identity and compare the exact deployed backend before changing the storefront fallback or weakening cart isolation. Official Current Cart documentation supports `Permissions.Anyone` web methods but still requires a visitor/member cart context; this distinction is not solved merely by declaring the web method `Anyone`.

See `references/edit-workflow-and-cart-findings.md` for the verified edit-page, repeater-refresh, isolation, native-cart, attribution, and daily-work-tracking findings from the latest build session. See `references/page-code-placement-and-campaign-id.md` for page-code identity checks, Storefront recovery, and the CMS-versus-CSV Campaign ID test boundary. See `references/shipping-and-student-attribution.md` for the verified business rules, Wix API evidence, and staged shipping/student-attribution workflow. See `references/native-product-and-checkout-attribution.md` for the discriminating checkout-vs-storefront CSV test, native Product Page limitation, and safe bulk custom-text rollout workflow.

## Recommended build sequence

### Phase 1 — Observe and stabilize

- Confirm the correct Wix site and standard Wix Editor versus Wix Studio.
- Inventory current navigation, product/program pages, global header, global footer, Store Pages, Member Area, and Dynamic Pages.
- Preserve the existing product/program pages as source material.
- Distinguish product/program pages (Spices, Popcorn, Tumblers, Coffee, etc.) from actual school/organization campaigns.
- Create a clean `Fundraisers` hub page without deleting existing content.

### Phase 2 — Minimal customer-facing structure

- Keep the existing site template and global header/footer.
- Use a homepage that explains the service and preserves approved content/claims.
- Make only necessary copy corrections; do not alter approved buttons’ destinations merely to fit a new architecture.
- Add the `Fundraisers` page and build its body with a matching designed section.
- Keep general `Shop` navigation hidden until campaign attribution is safe, while retaining Wix Stores pages in the site.

### Phase 3 — Campaign model

Use the hierarchy:

```text
Program or organization
  → School or group, if applicable
    → Campaign
      → Student seller, if enabled
        → Customer order
```

Do not assume every campaign has a school or student sellers. Campaign records must support organization-only campaigns such as All Stars.

Use globally unique IDs, not names alone:

```text
PROGRAM-SCHOOL-YEAR-SEQUENCE
ASOM-TXJR-2026-001
ASOM-ALLSTARS-2026-001
```

A campaign should store its fulfillment destination type (school, organization, warehouse, coordinator, or other), direct-shipping availability, bulk-shipping availability, product assignments, campaign-specific imagery, dates, and payout rules.

### Phase 4 — Product and order design

- Use Wix Stores for the central catalog.
- Use CMS campaign-product mapping rather than duplicating products for every school.
- Support campaign-specific images for engraved tumblers and water bottles.
- Prove that the same underlying product can appear in two active campaigns without attribution confusion before scaling.
- Enforce one campaign per cart/order in the initial release unless mixed-campaign accounting is explicitly designed.
- Use Velo only where native Wix configuration cannot preserve campaign, seller, shipping, or reporting context.

### Phase 4B — Scoped storefront discovery and native commerce

Use a separate dynamic fundraiser storefront item page rather than a general Wix Store gallery or a hard-coded category link. A dynamic item page created from the Fundraisers collection automatically receives a read-mode dataset and a per-record route, but the storefront's product repeater must be populated from `CampaignProductAssignments` server-side. Return only active assignments for the exact current fundraiser, fetch only their Wix product IDs, and display zero products when the assignment list is empty.

Search and category controls belong to the scoped product list, not the global catalog. The backend product query must include collection references (for example, `.include('collections')`) before category filtering; otherwise product cards can render while the category dropdown is empty. Populate category choices from categories represented by the current fundraiser's assigned products only. A search input and category dropdown can filter the already-scoped list in page code without exposing the full catalog.

Keep the initial commerce scope on Wix native commerce rather than building a custom cart or payment system. A `View product` action can safely open the existing Wix product page, where Wix handles variants, quantity, inventory, and Add to Cart. A compact cart/bag action may be added later for products with a known safe variant path; do not label a button `Add to Cart` if it only navigates to a product page. Final QA must address one-campaign-per-cart attribution, cart navigation to general products, and variant handling before production.

### Phase 4A — Staff-created campaign workflow

The target user experience is a WeTravel-like staff form, not manual page construction:

```text
Staff opens Create Fundraiser
  → enters organization/campaign details
  → selects products from the Wix Stores catalog
  → chooses shipping, dates, and options
  → clicks Create
  → Wix creates/updates the CMS record, product category/mapping, and storefront link
```

Implement this as a restricted staff dashboard or member-only management page. Use a product picker backed by the Wix Stores catalog (`Stores/Products` or the current Catalog API), because a CMS Reference/Multi-reference field may expose only custom collections and cannot reliably reference Wix Store products in the editor. Store product IDs in a dedicated mapping collection for auditability and ordering.

For the no-code pilot, a dedicated Wix Store category plus a fundraiser button is acceptable. It is not the final automation: the category and button destination are otherwise manually created per campaign. For the production workflow, use Velo backend code and the Wix Stores Categories API to create/manage the category, add selected product IDs, and write the resulting storefront URL/category ID to the fundraiser record. Confirm the site's Catalog V1/V3 API version before implementing API calls.

Dynamic campaign buttons should ultimately connect to a CMS URL field such as `Storefront URL`, not a manually hard-coded category link. Do not populate that field with an editor or preview renderer URL; only use a published/valid site URL or generate it at runtime.

When building a staff form, distinguish static labels from writable controls. A label such as `School or Organization` is an ordinary text element and must remain unconnected; the input directly beneath it is the element connected to the dataset field. On a regular page, a Read & Write dataset may load an existing record into connected inputs; provide a `New` dataset action at the top so the manager can clear the current item before entering a new fundraiser. The `Submit` dataset action creates the new record. Test the form in Preview while signed in as an authorized manager member; the editor canvas itself is not a live form.

### Phase 5 — Shipping and attribution

The operating model supports both modes:

- **Bulk shipment:** to the campaign’s configured school/organization destination; free to the customer at checkout. At 50 or more total fundraiser items, Guardian covers the bulk shipping cost; below 50, the cost is divided evenly across participating students and deducted from their profits.
- **Direct shipment:** customer selects direct delivery, pays approximately $9 extra, and provides a U.S. shipping address.

Each customer order remains a separate Wix order. Bulk orders are grouped operationally by fundraiser and destination; never merge them into one order because receipts, refunds, student attribution, and reporting must remain intact. For school campaigns, the school is the default bulk destination, while `Bulk Destination` and `Bulk Destination Address` remain configurable for organizations, warehouses, coordinators, or other approved destinations.

Standard Wix shipping rules can present checkout choices such as `Bulk shipment with fundraiser` at $0 and `Direct shipping to my address` at the approved direct rate. Standard shipping rules do not calculate a campaign-wide 50-item threshold or divide shipping cost among students; treat that as a post-order Finance/Guardian settlement calculation until an end-to-end automation is proven.

For student-tracked campaigns, a student-specific link or seller ID is preferred over free-text student names. If a free-text student name is collected, verify that it survives through cart, checkout, receipt, Wix order, and CSV before relying on it for profit allocation. Do not assume a page input or a CMS `studentTracking` boolean automatically creates order attribution.

Under-50 bulk shipping is allocated by units sold and deducted from student profit when student tracking applies:

```text
shipping deduction per item = total bulk shipping cost / total items
student deduction = student units × deduction per item
```

Treat the 50-item threshold as a campaign/shipment rule, not an individual-cart threshold. Confirm how refunds, mixed products, campaigns without student sellers, and customers choosing direct shipping affect the settlement calculation before automating it.

## Wix component map

| Wix component | Use |
|---|---|
| Wix Editor / Wix Studio | Site layout, navigation, responsive design |
| Wix Stores | Products, variants, cart, checkout, payments, orders |
| Wix CMS | Programs, schools, campaigns, sellers, campaign-product mappings |
| Dynamic pages | One reusable campaign layout with many campaign records |
| Velo | Campaign/seller context, validation, cart rules, reports, calculations |
| Wix Preview | Pre-publication testing |
| Publish | Explicit launch/review gate only |

## Common pitfalls

- **Confusing public and unpublished state:** The live URL will not show editor changes until Publish. Ask what the user sees in the editor before concluding a change failed.
- **Changing approved template behavior:** Keep an existing button linked to About if that is its intended role; do not rename it just because a different CTA would be theoretically better.
- **Calling product pages campaign pages:** Existing pages may describe product programs and pricing rather than represent a specific school campaign. Inventory them before restructuring.
- **Starting with dynamic pages too early:** First make one understandable pilot flow work; then add CMS/dynamic pages. Do not click “Add to Site” for Dynamic Pages without explaining what it adds and confirming the user is ready.
- **Assuming all campaigns ship to schools:** Model a configurable bulk destination because some organizations do not have a school.
- **Using product names for attribution:** Do not create school-specific duplicate product names as the long-term solution. Use campaign IDs and order metadata/linked records.
- **Using only free-text seller names:** Typos and duplicate names corrupt student credit. Prefer student-specific links/codes or a controlled selection field.
- **Publishing after every edit:** Save/Preview while building; publish only after a deliberate QA and launch decision.
- **Overloading the learner:** Do not provide five future steps after assigning the current task. Surface one action, then wait for confirmation.
- **Confusing dynamic-page access with navigation visibility:** Dynamic page settings may offer Everyone, Site members, or Password holders without offering “Hide from menu.” Access controls who can open a page; they do not remove a navigation link. Remove the list-page link from the site menu separately, and never lock the list page with a password merely to hide it unless that is an explicit requirement.
- **Assuming a native Store Gallery will render inside a CMS item page:** Verify the category has products in the Wix dashboard, then test the gallery in Preview. If the category is populated but the dynamic-page gallery remains empty, use the dedicated category storefront as the pilot checkout path and document that campaign-product mapping/Velo is the scalable final design.
- **Pausing momentum for low-risk pilot choices:** If a product assignment is needed for a test campaign and the user has not specified a subset, default to assigning the existing pilot catalog, state the assumption, and proceed; reserve clarification for choices that materially affect production data or money.
- **Treating legacy program cards as Wix Store products:** Homepage cards such as engraved tumblers, candles, or other fundraising programs may link to informational pages rather than actual Wix Stores products. Inspect the destination in the Wix Editor and verify product name, price, variants, and Add to Cart/Buy Now before reusing it.
- **Confusing legacy Shopify products with Wix catalog products:** A product used on the old Shopify collection is not automatically available in Wix Stores. Compare the legacy campaign/product reference against the live Wix Store Products catalog before importing, recreating, or assigning anything.
- **Creating a self-referential CMS product mapping:** Wix may expose only the current collection in a multi-reference field instead of Wix Stores Products. Do not save a `Fundraiser Products` multi-reference that points back to `Fundraisers`; cancel it and use a dedicated mapping/Velo approach or the category-based pilot path.
- **Building the staff workflow around placeholder catalog items:** Keep the custom mapping collection empty until real catalog products are verified. The pilot may reuse an existing Wix category, but the production workflow must not rely on staff typing product names or maintaining duplicate product records.
- **Merging fulfillment-labelled products without business confirmation:** Shopify catalogs may contain separate `BULK` and `SHIP TO HOME` records for the same apparent item. Do not delete or consolidate them merely to improve customer-facing names. First confirm with the fundraising/fulfillment owner whether the separation drives shipping, vendor routing, pricing, campaign assignment, Finance, or reporting. If consolidation is approved, normalize fulfillment prefixes consistently, preserve one stable product identity, and verify the merged description/image/price before deleting the old records.
- **Treating fulfillment as only a product-name concern:** Customer-facing product names should not expose internal routing labels unless approved. Model campaign-level shipping (`Direct Shipping`, `Bulk Shipping`, or `Both`) separately from any product-level fulfillment metadata; do not silently discard the distinction during CSV cleanup.
- **Uploading an unverified Shopify-to-Wix CSV:** Before upload, parse only one master row per actual Shopify product, exclude outside-vendor products, round weights to at most three decimal places, enforce Wix's handle/name limits, and report included/excluded counts. Keep the existing Wix catalog untouched until the replacement import is accepted and verified; never delete the old records first.
- **Assuming imported images failed too early:** A large Wix CSV import may show products before Wix finishes ingesting external image URLs. First verify an image in the Wix product editor, then allow Preview/category caching time to catch up; do not re-import solely because Preview initially shows blank images. If the product editor is blank after processing, inspect the source URL and use a Wix-hosted media fallback.
- **Consolidating catalog records without preserving pilot identity:** When replacing a catalog, preserve stable handles/product IDs for active pilot products and verify their category assignments after import. A clean consolidated CSV can recreate products but does not automatically preserve Wix category relationships unless the import/category state is explicitly checked.
- **Treating the catalog import as the launch gate:** Payment processing is a separate required gate. Before publishing or launching a real fundraiser, select a Wix-supported processor with the business owner, verify payout destination/fees/refunds, and run a test checkout through paid status, order record, refund, and Finance reconciliation.
- **Starting Velo UI work before identifying the actual editor controls:** In the standard Wix Editor, enable Velo first, then add the visual picker controls before writing code. If element search is unavailable, browse `Add Elements (+) → Lists` or `Lists & Grids` to find a Repeater. A blank Repeater may include placeholder title/text/button elements; repurpose them as product name, detail/price, and selection control rather than assuming a checkbox is available. Do not right-click to find IDs: select an element with a normal click and use the Velo `Properties & Events` panel to set stable IDs. If that panel is hidden, open it from the Dev Mode/Velo toolbar. Verify the element IDs before providing page code. Register a repeater's `onItemReady()` handler before assigning its `.data`; otherwise Wix may leave the starter placeholder rows unchanged. Test the backend web module on the published test site while authenticated as a real site member with the required role. Wix Preview can report partial-functionality or schema-fetching errors for backend/API calls; treat the published test site as the authoritative runtime check. Do not edit compiled files such as `p1qho.js`; use the page/backend source files and browser Console only for diagnosis.
- **Inventing categories from memory:** Before creating catalog categories, inspect the actual imported product types/vendors and count the products in each group. Only create categories backed by real products; for example, create Bedding only when the source catalog actually contains the Bomb Bedding products. Keep category creation incremental and user-confirmed.
- **Deleting generic categories too early:** Remove obsolete categories only after confirming they are not used by the pilot category, buttons, or live pages. Deleting a category should not be assumed to delete products, but verify Wix's confirmation dialog and retain the system `All Products` category.
- **Giving fulfillment labels to customers:** Internal routing such as `BULK` and `SHIP TO HOME` may be useful for staff filtering but should not automatically appear in product names. If the business approves consolidation, merge only after checking product identity, price, image, description, and reporting implications; enforce fulfillment at campaign/checkout level.

## Verification gates

Before the first production campaign:

1. A campaign link opens the intended organization/campaign page.
2. Only assigned products appear.
3. Direct and bulk shipping choices are clear and correctly stored.
4. The same product can be sold in two concurrent campaigns without ambiguous credit.
5. Student attribution is optional per campaign and correct when enabled.
6. Bulk orders can be grouped by campaign and destination.
7. The 50-item rule and under-50 allocation are documented and tested.
8. Finance can use the export without rebuilding the data manually.
9. Preview and mobile layouts are reviewed.
10. Publish is approved explicitly by the user/business owner.

See `references/guardian-fundraising-operating-model.md` for the current campaign, catalog, shipping, and attribution assumptions from the project discussions. See `references/wix-shopify-csv-migration.md` for the validated Shopify-to-Wix import workflow, product-row parsing, Wix length limits, and pilot-import checks. See `references/catalog-consolidation-and-categorization.md` for the verified fulfillment-label consolidation, outside-vendor exclusions, image-processing verification, and catalog-category lessons. See `references/scoped-storefront-and-native-cart.md` for the campaign-scoped storefront, exact-ID, category filtering, dynamic URL, and native Wix commerce verification recipe.
