# Guardian Fundraising Operating Model

Session-derived reference for the Wix fundraising build. Treat business rules as requirements to confirm with the organization before automating.

## Campaign hierarchy

```text
Program or organization
  → School or group, if applicable
    → Campaign
      → Student seller, if enabled
        → Customer order
```

Not every campaign has a school or student sellers. All Stars may be organization-level. Every campaign needs a configurable bulk destination type and address.

Use globally unique IDs rather than names alone:

```text
PROGRAM-SCHOOL-YEAR-SEQUENCE
ASOM-TXJR-2026-001
ASOM-ALLSTARS-2026-001
```

## Shipping

Both options must be offered:

- Bulk shipment to the campaign's configured school, organization, warehouse, or coordinator destination.
- Direct shipment anywhere in the United States, with the customer paying approximately $9 extra.

Bulk shipping is free at 50 items or more, regardless of number of boxes. Under 50 items, shipping is deducted from student profit when student tracking applies.

```text
shipping deduction per item = total bulk shipping cost / total items
student deduction = student units × deduction per item
```

## Student attribution

Student tracking applies to some campaigns but not all. Prefer student-specific links or seller IDs over free-text seller names. A campaign record should enable or disable student tracking.

## Product/catalog notes

Product-program pages in the inherited site are not necessarily campaign pages. The catalog includes spices, tumblers/water bottles, candles, coffee, apparel-in-development, and outside-vendor products such as popcorn, cookie dough, and Boost My Group.

The same product may be active in multiple campaigns simultaneously. Do not rely on school-specific duplicate product names as the long-term attribution solution. Use campaign IDs and campaign-specific product mapping/artwork. Engraved tumbler and water-bottle images may vary by campaign and require authorized editing.

## Wix build posture

Preserve the inherited template and approved content. Use the Editor as the source of truth while changes are unpublished. Keep assistance read-only when inspecting a shared editor. Work one confirmed action at a time and reserve Publish for a deliberate launch gate.
