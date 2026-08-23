# Gap Report — Evaluation Round 1 — Rumica (ArtDecor Home Design)

## Findings

**1. Gap / internal inconsistency — Checkout description not updated after architecture-round decision**
- **Where:** Vision & Scope — FR-19, UC-6 ("Purchase products/design packs"), "Browse-to-purchase" user flow, Required Integrations section.
- **Conflicts with:** Architecture Specification — Commerce Bridge component, UC-6/UC-8 sequence diagrams, "Key Architectural Decisions," and "Cost & Complexity Notes," all of which treat "custom Rumica-built checkout form" as a **final, client-confirmed** decision (00-request.md architecture-round Q&A #2).
- The Vision & Scope was finalized before the architecture round and still describes checkout generically as happening "via the Bitrix24 payment module," with no mention that it's a Rumica-built form calling Bitrix24 Payment APIs directly (as opposed to a hosted redirect/widget).
- **Decision:** Update Vision & Scope to match — revise FR-19, UC-6, the "Browse-to-purchase" flow, and Required Integrations to explicitly state checkout uses a custom Rumica-built form calling Bitrix24 Payment APIs directly.

**2. Gap — "Favorites" feature absent from both documents**
- **Where:** Not present anywhere in Vision & Scope or Architecture Specification.
- **Source:** `source-docs/05-detailed-feature-estimate-sheet.md` §1.2 — a Favorites page (bookmark catalog items, status pending/used-in-project/ordered) is explicitly specified. Not mentioned in the commercial proposal or features doc, and never raised in the Q&A transcript, so there's no exclusion decision on record either — it's simply missing without a trace.
- **Decision:** Add to MVP scope — add a Favorites use case/FR to the Vision & Scope (plus a minor architecture note).

**3. Gap — Subscription/payment-method self-service missing from FR-4**
- **Where:** Vision & Scope FR-4 ("view and update their profile data and view their current subscription tier and its limits").
- **Source:** `source-docs/05-detailed-feature-estimate-sheet.md` §1.2 — a dedicated subscription-status page with add/cancel/update payment method is specified. FR-4 covers visibility of tier/limits but not payment-method self-service, and there is no Post-MVP exclusion note covering it either.
- **Decision:** Defer to Post-MVP — add "self-service payment-method management" explicitly to Post-MVP scope; support-handled or re-subscribe-manually in MVP.

**4. Gap — Observability/monitoring layer dropped from Architecture Specification**
- **Where:** Architecture Specification — no corresponding component in the Components table, Component Diagram, Deployment View, or NFR-1 (uptime) mapping.
- **Source:** `source-docs/01-commercial-proposal.md` §2.3 — the proposal's own system architecture diagram explicitly includes an "Observability: Logs, Metrics, Traces" layer in the Application tier. This was silently absent from the finalized spec, and NFR-1 (uptime target) was asserted without any monitoring mechanism to support/verify it.
- **Decision:** Omit it — treat basic observability as an implicit, standard part of any cloud deployment not requiring explicit architectural documentation for MVP; leave the spec as-is.

## Decision Summary
- 3 of 4 findings require document edits (checkout consistency fix, add Favorites to MVP, add payment-method self-service to Post-MVP list).
- 1 of 4 findings (observability) requires no edit — client accepted the current spec's silence on it.

## DECIDE Outcome: REWORK REQUIRED (Round 1)

**business-analyst** — revise `working/vision-and-scope.md`:
- FR-19, UC-6, "Browse-to-purchase" flow, Required Integrations: state checkout uses a custom Rumica-built form calling Bitrix24 Payment APIs directly (not a hosted widget).
- Add Favorites feature to MVP scope: new bullet in "In Scope (MVP)," new FR, new use case (narrative as/when/then), and a Modules-section mention.
- Add "self-service payment-method management (add/update/cancel)" to "Out of Scope for MVP (Post-MVP)."

**system-architect** — revise `working/architecture-specification.md` (after business-analyst's edit lands):
- Extend Application DB (or Catalog Sync/Proxy) component responsibility to note storage of the user↔favorited-item relationship.
- Add one sentence to Data Flow describing how favorited-item status (pending/used-in-project/ordered) is derived by cross-referencing Projects Service and Bitrix24 CRM order data.
- No new component/service/diagram mandated unless the architect judges it necessary.

No changes needed for findings 3 (never in architecture) or 4 (observability — accepted as-is).

Everything else evaluated — the Bitrix24-centric direction, Google-only OAuth, AI/Seller/Shopify/multi-currency exclusions, CubiCasa-pending-validation framing, "My Orders," standard NFRs, Bitrix24 Cloud standard-plan sizing — was found traceable cleanly to the Q&A transcript and/or a source document in both documents, with no reintroduction of excluded material.
