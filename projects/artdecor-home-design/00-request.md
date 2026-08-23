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
