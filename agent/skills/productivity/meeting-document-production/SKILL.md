---
name: meeting-document-production
description: "Use for meeting-ready documents."
version: 0.1.0
author: webbuild-timmy
license: MIT
platforms: [windows, linux, macos]
metadata:
  hermes:
    tags: [meetings, documents, google-docs, requirements, recommendations]
    related_skills: [meeting-action-items, google-workspace, docx]
---

# Meeting Document Production

Use when a user provides meeting notes and asks for polished, meeting-ready documents, especially when they want both the original notes preserved and a separate recommendation, implementation plan, or bottom line.

## Core workflow

1. **Separate source from interpretation.** Preserve the original notes, current-state observations, wishlist, and explicit requirements in a source-notes document. Do not silently convert proposals into decisions.
2. **Create a separate recommendation document.** Organize the second artifact around executive recommendation, ordered implementation steps, decisions needed, acceptance criteria, risks, and bottom line.
3. **Resolve corrections before finalizing.** If the user clarifies that an existing platform, website, or account should be reused, revise the architecture rather than repeating an isolated-site recommendation.
4. **Use evidence labels.** Distinguish confirmed facts, recommendations, assumptions, unresolved questions, and items requiring stakeholder approval.
5. **Include actionable meeting decisions.** End the recommendation with a compact list of decisions the meeting must make, not just a long technical explanation.
6. **Verify artifacts.** Read back or extract the generated documents and confirm headings, tables, and major sections are present before delivery.

## Document pair shape

### Source notes / requirements

Include:

- Purpose and scope
- Initial logistics questions
- Current process
- Pain points and quoted observations
- Wishlist and proposed options
- Functional requirements
- Operational requirements
- Reporting/export requirements
- Original goal
- A short architecture caveat section, clearly labeled as analysis

### Meeting-ready recommendation

Include:

- Executive recommendation
- Platform and URL decisions
- Step-by-step build plan
- Data model or system components
- Attribution and workflow risks
- Pilot plan
- QA/launch acceptance checklist
- Decisions needed in the meeting
- Bottom line

Keep the second document concise enough to use in a meeting while retaining the details needed for implementation.

## Google Docs delivery

If Google Workspace OAuth is authenticated and the user authorizes document creation, create native Google Docs and return verified document URLs. Do not claim a cloud document exists until the API returns a document ID/URL and a read-back confirms the content.

If Google Workspace is not authenticated or the user has not authorized cloud writes, create polished `.docx` files instead. Verify that the files exist and that text extraction shows the expected content. Tell the user exactly how to upload each DOCX to Drive and use **Open with → Google Docs**. Do not describe DOCX files as already-created native Google Docs.

## Wix/ecommerce planning pattern

For a Wix fundraising platform, favor a reusable dynamic campaign template over one manually built page per fundraiser. Keep the existing main Wix site when the user indicates that it already hosts the organization’s websites; use a site section such as `/fundraising/{campaign-slug}` before recommending a separate subdomain. Treat campaign attribution, one-campaign-per-cart behavior, shipping mode, inventory, fulfillment exports, and Finance reconciliation as launch-critical—not optional polish.

## Pitfalls

- Mixing the user’s original notes with recommendations so stakeholders cannot tell what was actually said.
- Presenting a recommendation as an approved decision.
- Creating a separate-site architecture after the user says the existing site should host the feature.
- Claiming Google Docs were created when only local DOCX files were produced.
- Delivering a long plan without a bottom line, decision list, or pilot acceptance criteria.
- Skipping artifact read-back after generation.

## Verification checklist

- [ ] Two artifacts exist when the user requested source notes plus recommendation.
- [ ] The source document preserves the original requirements and current state.
- [ ] The recommendation reflects the latest platform/site clarification.
- [ ] Confirmed facts, recommendations, and unresolved decisions are distinguishable.
- [ ] The recommendation includes ordered steps, pilot scope, acceptance tests, and bottom line.
- [ ] Google Docs status is reported honestly: native URL verified, or DOCX conversion instructions provided.
- [ ] Generated files were read back or text-extracted before delivery.
