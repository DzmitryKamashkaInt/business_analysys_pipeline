# Project Request — ArtDecor Home Design / Rumica

## Original Request (translated from Russian)
The user attached six documents describing an existing project (not a from-scratch requirement) and asked:

> "The attached documents contain details and a description of the project that needs to be studied, analyzed, and given a detailed description of its content and architecture. Next, I'll need to answer a series of questions about the project, and possibly generate additional documents. Study the project, ask clarifying questions using the appropriate agentic pipeline."

This is a **retrofit / reverse-engineering case** for the business-analysis-pipeline: rather than starting from a blank requirement, the business-analyst must read a client-facing commercial proposal (already delivered/priced) plus several supporting technical/business documents, reconcile inconsistencies between them, and produce a clean, internally-consistent Vision & Scope (and later System Architecture Specification) — surfacing every discrepancy as a clarifying question rather than silently picking one interpretation.

## Supporting Documents (transcribed into this project's `source-docs/` folder)
1. `source-docs/01-commercial-proposal.md` — Intetics commercial proposal (RU + EN versions) for "ArtDecor Home Design web application," Aug 2025. Client: LLC ArtDecor Group. Budget USD 119,279, 6–7 months. This is the primary authoritative scope/budget/architecture source.
2. `source-docs/02-planner-engine-comparison.md` — comparison of open-source ("Smart Cursor") vs. custom Three.js floor-planner engine; supports the proposal's choice of a custom Three.js planner.
3. `source-docs/03-rumica-presentation.md` — Oct 2025 business/investor presentation for the **same product rebranded as "Rumica"** (rumica.app), adding market sizing, competitor analysis, monetization model, and 5-year financial projections. Shows an AI assistant in the prototype UI that the commercial proposal scopes as Post-MVP only.
4. `source-docs/04-features-and-modules.md` — detailed feature/module breakdown ("ArtDecor Features and modules.pdf"). Specifies Stripe for payments (conflicts with proposal's Bitrix24).
5. `source-docs/05-detailed-feature-estimate-sheet.md` — most granular feature/estimate sheet ("HomeDesign_Feature list_Estimates.pdf"). Describes a custom Java/Spring Boot + PostgreSQL microservices backend with Firebase/S3 storage — a materially different architecture from the Bitrix24-centric proposal.

## Known Cross-Document Discrepancies (to resolve via clarifying questions — do not silently pick one)
1. **Product naming**: "ArtDecor Home Design" (Intetics proposal, Aug 2025) vs. "Rumica" (presentation, Oct 2025) — same product evolved/rebranded, or two different things?
2. **Payment backend**: Bitrix24 payment module (proposal + architecture diagram + presentation) vs. Stripe (features doc) vs. generic "payment gateway" (estimate sheet) vs. "Bitrix24 for MVP and Shopify for the US market" (estimate sheet, catalog section).
3. **Core backend architecture**: Bitrix24-centric (admin/catalog/CRM/payments in Bitrix24, thin custom services layer) vs. fully custom Java/Spring Boot + PostgreSQL microservices with Firebase/S3 — these are not obviously reconcilable and materially change cost/timeline/team skillset.
4. **OAuth providers**: Google only vs. Google+Apple vs. "Google and iOS" (inconsistent across proposal RU/EN and features doc).
5. **AI assistant timing**: Presentation shows a natural-language AI design assistant in the prototype; commercial proposal scopes ALL AI features as Post-MVP.
6. **Additional roles not in original team/roles list**: "Seller" role (manage own catalog items) appears in the detailed estimate sheet's Roles & Permissions and Admin sections but is absent from the commercial proposal's stated admin roles (Super Admin, Catalog Manager, User Manager, System Admin) and team composition.
7. **Floor-plan recognition vendor**: CubiCasa is named consistently but qualified as "preliminary"/"for MVP" in the RU proposal — is this vendor choice final or still to be validated?
8. **Market/monetization scope**: The Rumica presentation's market analysis, multi-channel monetization model, and 5-year financials are entirely absent from the Intetics technical proposal — are these to be treated as authoritative business context for the Vision & Scope, or as separate/aspirational material not yet approved for this engagement's scope?

## Q&A Transcript
(To be appended by the orchestrating agent as the business-analyst's clarifying questions are answered.)

**Q:** Is "Rumica" the rebranded continuation of the same "ArtDecor Home Design" project from the Aug 2025 proposal, or a separate venture? → **A:** Same project, rebranded — use "Rumica" as the product name; treat the Oct 2025 deck as an evolution of the same MVP scope.

**Q:** Which payment backend should the Vision & Scope treat as authoritative for MVP checkout/subscriptions? → **A:** Bitrix24 for MVP, Shopify added for a later US-market expansion.

**Q:** Which backend/architecture direction should the Vision & Scope and System Architecture Specification assume? → **A:** Bitrix24-centric — admin, catalog, CRM, and payments live inside Bitrix24; custom code is a thin bridging/adapter layer (Auth Bridge, Commerce Bridge, Catalog Proxy, Projects Service, CubiCasa Adapter).

**Q:** Which social login providers are in MVP scope alongside email+password? → **A:** Google only.

**Q:** Should any AI capability (natural-language design assistant or recommendations) be in MVP scope? → **A:** Strictly Post-MVP — no AI assistant or recommendation engine in MVP; the Rumica deck's mockup is aspirational only.

**Q:** Is the "Seller" role (external parties managing their own catalog items) part of MVP scope? → **A:** No, Post-MVP — MVP keeps catalog management centralized to internal Admin roles only.

**Q:** Is CubiCasa the final, confirmed vendor for floor-plan recognition, or still a placeholder pending evaluation? → **A:** Still to validate — CubiCasa is a working assumption for estimation, but a vendor evaluation/PoC is needed before final commitment.

**Q:** Should the Rumica deck's market analysis, competitive landscape, monetization model, and financial projections be treated as authoritative business context for the Vision & Scope? → **A:** Include as business context — summarize market opportunity and monetization model in the Purpose section, without turning financial projections into functional requirements.

**Q:** Should a customer-facing order history feature be added to MVP scope? → **A:** Yes — add a basic "My Orders" page (order list, status, receipt/invoice access) backed by Bitrix24 CRM order/deal data.

**Q:** Should the Vision & Scope include specific NFRs (performance, uptime, data residency)? → **A:** Standard NFRs only — assume no special compliance constraints; use general best-practice placeholders (standard uptime, reasonable page-load times), to be refined during architecture.

**Q:** [Architecture round] Which Bitrix24 edition/plan will Rumica run against? → **A:** Cloud, standard commercial plan — moderate REST API limits; the current sync-cache design as specified is sufficient.

**Q:** [Architecture round] Should checkout use Bitrix24's hosted payment page/widget, or a custom Rumica-built checkout form calling Bitrix24 payment APIs directly? → **A:** Custom Rumica-built checkout form — full UX control and branding consistency; adds custom-code surface (form validation, PCI-adjacent handling) not currently costed in the original estimate.

**Q:** [Evaluation round 1] The Vision & Scope still describes checkout generically as "via the Bitrix24 payment module," not reflecting the architecture round's final custom-checkout-form decision. How should this be resolved? → **A:** Update Vision & Scope to match — revise FR-19, UC-6, the "Browse-to-purchase" flow, and Required Integrations to explicitly state checkout uses a custom Rumica-built form calling Bitrix24 Payment APIs directly.

**Q:** [Evaluation round 1] The "Favorites" feature (from the detailed estimate sheet) is absent from both documents with no exclusion decision on record. How should this be handled? → **A:** Add to MVP scope — add a Favorites use case/FR to the Vision & Scope (plus a minor architecture note).

**Q:** [Evaluation round 1] Should self-service payment-method management (view/add/cancel/update payment method, from the detailed estimate sheet) be added to FR-4? → **A:** Defer to Post-MVP — add "self-service payment-method management" explicitly to Post-MVP scope; support-handled or re-subscribe-manually in MVP.

**Q:** [Evaluation round 1] The original proposal's architecture diagram includes an "Observability: Logs, Metrics, Traces" layer, absent from the finalized Architecture Specification. Should it be added back? → **A:** Omit it — treat basic observability as an implicit, standard part of any cloud deployment not requiring explicit architectural documentation for MVP; leave the spec as-is.

**Q:** [Evaluation round 2] UC-8 ("Subscribe to a paid tier") and the "Subscription upgrade" flow still say payment is "via the Bitrix24 payment module," conflicting with UC-6's updated custom-checkout-form language and the architecture spec's UC-8 diagram (which shows the same custom-form/tokenization mechanism). How should this be resolved? → **A:** Keep as-is, but clarify the distinction — subscription payment intentionally uses a different mechanism (a Bitrix24-hosted flow) than product/cart checkout; this distinction must be stated explicitly in both documents, not left ambiguous.

**Q:** [Evaluation round 2] UC-10's main scenario and its Assumptions note give opposite precedence rules for a favorited item that is both used-in-project and ordered. Which takes priority? → **A:** "Ordered" takes priority — reword UC-10's main scenario so the check order matches (ordered checked/stated first), and add an explicit precedence sentence to the architecture spec's Data Flow.
