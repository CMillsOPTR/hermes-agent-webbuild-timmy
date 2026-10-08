# Wix–Shopify product CSV migration

Use this reference when migrating legacy Shopify fundraising products into Wix Stores.

## Safe workflow

1. Download Wix's filled product CSV template from **Store Products → More Actions → Import**. Preserve its header row and column order.
2. Obtain a Shopify **product catalog** export, not customers or orders.
3. Parse Shopify as a real CSV; products may span multiple rows because additional images appear on blank-title continuation rows.
4. Select a small pilot subset by an explicit campaign code/handle (for example, `TXJASOM`) rather than importing the entire legacy catalog.
5. Map into Wix columns and write a separate review/import file. Never upload the raw Shopify CSV directly.
6. Validate required length limits, unique handles, prices, image URLs, visibility, and category names before upload.
7. Import the pilot first, then verify product cards, images, prices, variants, inventory behavior, and category filtering in Wix Preview.

## Mapping notes

- Shopify `Title` → Wix `name`
- Shopify `Body (HTML)` → Wix `description` after removing/normalizing HTML as appropriate
- First Shopify `Image Src` → Wix `productImageUrl`; retain additional images separately if the Wix importer supports them
- Shopify `Variant Price` → Wix `price`
- Shopify `Handle` → a shortened unique Wix `handleId`; do not assume Shopify handles fit Wix limits
- Wix `collection` → an existing Wix Store category, such as the pilot fundraiser category
- Shopify status/active products → review before setting Wix `visible=true`; do not infer inventory quantity when the export does not contain it
- Preserve SKU, cost, weight, and inventory as blank or explicitly reviewed when the source does not provide reliable values; never invent operational data

## Validation rules learned from Wix import

Wix rejected the initial pilot because:

- `handleId` has a maximum length of 50 characters
- `name` has a maximum length of 80 characters

Use concise, unique handles such as `txjasom-logo-tumbler`, and shorten only overlong names while keeping the campaign/product identity clear. Re-run a CSV round-trip parse and check every handle/name length before packaging the file.

## Example pilot outcome

A Texas Jr. All Stars pilot can be isolated to the four Shopify master rows whose titles contain `TXJASOM`, rather than importing hundreds of historical/current records. The Shopify export may contain many more rows than products because of image continuation rows; count nonblank-title master rows separately.

## Attribution warning

A Wix Store category can isolate products for a pilot storefront, but category membership alone is not a guaranteed order-level campaign attribution field. Before production, add explicit campaign/order metadata through Velo or a supported mapping workflow and verify it in an actual test order/export.

## Form construction and testing

- On a regular CMS setup page, use actual Wix Input elements (Text Input, Text Box/Paragraph Input, Date Picker, Dropdown, Checkbox, Upload Button) connected to a Read & Write dataset. Ordinary text elements are labels only and are not fillable in Preview.
- A Read & Write dataset can load an existing record into inputs. Put a clearly labeled `New` dataset action at the top and a `Submit` action at the bottom; test in Preview while signed in as an authorized manager member.
- Restrict the setup page to a Manager role before exposing a write-enabled dataset. Hiding the page from navigation is not sufficient protection.
