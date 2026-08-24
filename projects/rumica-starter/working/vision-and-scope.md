# Vision and Scope — Rumica Starter (Phase 1 of Rumica)

## Purpose

Rumica Starter is Phase 1 of the Rumica platform (parent project: `artdecor-home-design`), scoped to deliver the smallest coherent, launchable slice of the product ahead of the full MVP: user accounts, two independent product catalogs (furniture and, newly, building materials), and end-to-end ordering through Bitrix24. It exists so the business can validate account creation, catalog operations at scale, and the commerce/CRM pipeline before investing in the planner and community features that make up the rest of the MVP.

Starter is explicitly a partial realization of the parent project's business case, not a redefinition of it. Per the resolved decision carried over from the parent project, the following is stated plainly rather than implied:

- **BR-4 (centralize commerce/CRM/catalog in Bitrix24) — fully satisfied.** Starter's catalog data, orders, and CRM records live in Bitrix24 from day one.
- **BR-1 (non-specialists plan and furnish with real products) — partially satisfied.** Users can browse and purchase real products across two catalogs, but Starter provides no planning capability (no 2D room planner), so the "plan" half of BR-1 is unmet.
- **BR-2 (direct purchase from a design — Rumica's core differentiator) — not satisfied.** Purchase in Starter is of standalone catalog items; there is no design/project context to purchase "from," because the planner and Design Projects features are deferred to a later phase.
- **BR-3 (freemium growth model) — not applicable to Starter.** There are no Free/Paid tiers in this scope.
- **BR-5 (community/publish loop) — not satisfied.** The publish/duplicate gallery loop depends entirely on the deferred planner and Design Projects gallery.

This limitation is accepted and carried forward as known, not silently resolved: Starter is a commerce-and-catalog foundation, not yet the differentiated "plan-then-buy" product that is Rumica's ultimate value proposition.

## Scope & Boundaries

### In Scope

- Email/password registration and login, plus Google OAuth login.
- Simplified user profile management (no subscription-tier visibility, since tiers do not exist in Starter).
- Two independent product catalogs — **Furniture** and **Building Materials** — each with its own attribute schema, browsable, searchable, and filterable independently.
- A **unified search bar** that searches across both catalogs simultaneously.
- **Catalog-specific category browsing and filtering** — browse/filter UI remains separate per catalog (not unified), even though search is unified.
- Product detail pages for items in either catalog.
- **Admin Catalog Management**: a single unified admin screen with a toggle between Furniture and Building Materials, sharing one CRUD and CSV-import UI pattern.
- **CSV import for catalog seeding**, available from day one, using **two separate CSV templates** — one matching the furniture attribute schema, one matching the building materials attribute schema. Import supports batch/async processing to handle medium-scale catalogs without blocking the admin UI.
- Shopping cart that accepts a **mixed cart** — items from both catalogs together in one cart.
- A single checkout flow: custom Rumica checkout form submitting to Bitrix24 Payment APIs, regardless of which catalog(s) the cart items came from.
- "My Orders" view, sourced from Bitrix24 CRM, showing order history for the logged-in user.

### Out of Scope

**(a) Deferred to a later phase of Rumica** (remain fully in-scope for the product; simply not built in Starter, and Starter is designed not to require rework when they are added):
- 2D Room Planner (Three.js-based room/interior design tool).
- Floor-plan upload, recognition, and dimension confirmation.
- Flat Plans gallery.
- Public Design Projects gallery, including publish and duplicate.
- Project management: save, duplicate, export, share of design projects.
- Favorites.
- Free/Paid subscription tiers and their associated usage limits.
- Subscription checkout path (Bitrix24-hosted subscription payment flow).
- Full Admin panel beyond catalog CRUD: dashboard, user management, moderation, reports.
- Analytics integration (internal + GA/Mixpanel).
- Curated design packs (tied to the planner; deferred alongside it — checkout in Starter covers individual products only).

**(b) Post-MVP for the whole Rumica product** (per the parent project's finalized scope; these are not "later phase of Starter" items — they sit beyond the entire MVP roadmap, Starter included):
- AI-assisted design/recommendation features.
- Seller role / marketplace-style multi-seller support.
- Shopify (or other third-party commerce platform) integration.
- Multi-currency support.
- Any other item the parent project's Vision & Scope classifies as Post-MVP.

## Business Requirements

- **BR-1** (partially satisfied by Starter): Enable non-specialist users to furnish with real, purchasable products. Starter satisfies the "furnish with real products" half; the "plan" half is out of scope here.
- **BR-2** (not satisfied by Starter): Enable direct purchase from within a design. Requires the deferred planner; tracked as a known gap, not to be resolved within Starter.
- **BR-3** (not applicable to Starter): Freemium growth model via Free/Paid tiers. No tiers exist in Starter.
- **BR-4** (fully satisfied by Starter): Centralize commerce, CRM, and catalog operations in Bitrix24.
- **BR-5** (not satisfied by Starter): Community/publish loop via public Design Projects. Requires the deferred planner and gallery.

## Functional Requirements

- **FR-1 (Starter)** — The system SHALL allow users to register and log in using email/password. *(corresponds to parent FR-1)*
- **FR-2 (Starter)** — The system SHALL allow users to log in via Google OAuth. *(corresponds to parent FR-2)*
- **FR-3 (Starter)** — The system SHALL maintain authenticated sessions and reject requests to protected resources without a valid session/token. *(corresponds to parent FR-3)*
- **FR-4 (Starter)** — The system SHALL allow a logged-in user to view and edit their profile, without displaying any subscription-tier information. *(corresponds to parent FR-4-starter)*
- **FR-5 (Starter)** — The system SHALL allow users to browse, search, filter, and view product detail for items in the Furniture catalog. *(corresponds to parent FR-13-starter)*
- **FR-6 (Starter)** — The system SHALL allow users to browse, search, filter, and view product detail for items in the Building Materials catalog, using that catalog's own attribute schema, independent of the Furniture catalog's schema. *(new catalog; extends parent FR-13-starter per this project's confirmed scope)*
- **FR-7 (Starter)** — The system SHALL provide a single, unified search bar that returns matching results from both the Furniture and Building Materials catalogs together. *(derived from Discovery Q&A — unified search decision)*
- **FR-8 (Starter)** — The system SHALL keep category browsing and attribute-based filtering catalog-specific: a user browsing/filtering Furniture SHALL see only furniture categories/filters, and likewise for Building Materials, with no cross-catalog category merge. *(derived from Discovery Q&A — separate browse decision)*
- **FR-9 (Starter)** — The system SHALL provide Admin users a single "Catalog Management" screen with a toggle between Furniture and Building Materials, exposing manual create/read/update/delete for products in whichever catalog is selected. *(corresponds to parent FR-15-starter; unified-screen shape derived from Discovery Q&A)*
- **FR-10 (Starter)** — The system SHALL support CSV import for catalog seeding from day one, using two separate CSV templates — one for Furniture (matching its attribute schema) and one for Building Materials (matching its attribute schema) — processed as a batch/async job so the admin UI is not blocked during import. *(corresponds to parent FR-15-starter, extended per Discovery Q&A — two-template decision and medium-scale assumption)*
- **FR-11 (Starter)** — The system SHALL allow a single cart to contain items from both the Furniture and Building Materials catalogs simultaneously (mixed cart). *(corresponds to parent FR-20, extended per Discovery Q&A — mixed-cart decision)*
- **FR-12 (Starter)** — The system SHALL provide a single checkout flow using a custom Rumica checkout form that submits the order — regardless of which catalog(s) its items came from — to Bitrix24 Payment APIs. *(corresponds to parent FR-20)*
- **FR-13 (Starter)** — The system SHALL display a "My Orders" view to the logged-in user, sourced from Bitrix24 CRM order records. *(corresponds to parent FR-22)*
- **FR-14 (Starter)** — The system SHALL provide a product/item detail page ("item card") for any catalog item, from either the Furniture or the Building Materials catalog, that displays: (a) a photo/image gallery for the item, and (b) the item's full attribute/detail set as defined by that item's catalog schema — Furniture attributes for Furniture items, Building Materials attributes for Building Materials items — consistent with the separate-schema decision in FR-6. *(new; sharpens the previously implicit "view product detail" mention in FR-5/FR-6 into an explicit requirement)*

### Traceability Mapping (Starter FR/UC → Parent FR/UC)

| Starter ID | Parent ID(s) | Notes |
|---|---|---|
| FR-1 | FR-1 | Unchanged |
| FR-2 | FR-2 | Unchanged |
| FR-3 | FR-3 | Unchanged |
| FR-4 | FR-4-starter | Unchanged (simplified profile) |
| FR-5 | FR-13-starter | Furniture portion |
| FR-6 | FR-13-starter | New catalog, extends parent scope |
| FR-7 | — | New; derived from Discovery Q&A |
| FR-8 | — | New; derived from Discovery Q&A |
| FR-9 | FR-15-starter | Unified-screen shape is new detail |
| FR-10 | FR-15-starter | CSV import mandated day-one; two-template shape is new detail |
| FR-11 | FR-20 | Mixed-cart behavior is new detail |
| FR-12 | FR-20 | Unchanged |
| FR-13 | FR-22 | Unchanged |
| FR-14 | — | New; sharpens implicit "view product detail" mention in FR-5/FR-6 into an explicit item-card requirement |
| UC-1 | UC-1 | Unchanged |
| UC-2 | — | New; browse/search across both catalogs |
| UC-3 | UC-6-starter | Individual products only, either catalog |
| UC-4 | UC-7 | Unchanged |
| UC-5 | UC-9-starter | Catalog CRUD/CSV only, no user mgmt/moderation/reports |
| UC-6 | none | New; sharpens what was previously an implicit detail of parent FR-13-starter/FR-14 into an explicit use case. View-only — parent FR-14 "Place in Interior" is explicitly NOT part of this use case (out of scope for Starter). |

## Non-Functional Requirements

- **NFR-1 (Catalog scale):** Both catalogs are expected to hold a medium/near-term volume — low thousands of SKUs each. CSV import SHALL be implemented as a batch/async process capable of handling this volume without blocking the admin UI. Catalog browse and filter SHALL use indexed filtering and proper pagination so that listing performance remains acceptable at this scale. *(derived from Discovery Q&A)*
- All other NFR categories (specific performance targets, availability, security posture beyond what Bitrix24-centric auth/commerce already implies, accessibility, localization) — not yet specified — to be confirmed.

## Modules

- **Auth Module:** Email/password and Google OAuth registration/login, session management.
- **Profile Module:** Simplified user profile view/edit (no tier display).
- **Furniture Catalog Module:** Browse/search/filter/detail for the Furniture catalog and its attribute schema, including the item card — photo/image gallery plus full attribute details (FR-14, UC-6).
- **Building Materials Catalog Module:** Browse/search/filter/detail for the Building Materials catalog and its distinct attribute schema, including the item card — photo/image gallery plus full attribute details (FR-14, UC-6).
- **Unified Search Module:** Cross-catalog search bar returning results from both catalogs.
- **Catalog Admin Module:** Unified Catalog Management screen (Furniture/Building Materials toggle), manual CRUD, and two-template CSV import with async processing.
- **Cart & Checkout Module:** Mixed-catalog cart, custom Rumica checkout form, Bitrix24 Payment API submission.
- **Orders Module:** "My Orders" view sourced from Bitrix24 CRM.

## Use Cases

### UC-1 (Starter): Register and Login
**Description:** As a prospective or returning user, when I choose to register or log in via email/password or Google OAuth, then the system authenticates me and establishes a session, so that I can access account-specific features like ordering and My Orders.
**Roles:** Anonymous visitor, Registered user.
**Pre-conditions:** The user has a valid email address or a Google account; the system is reachable.
**Main scenario:**
1. User selects "Register" and submits email/password (or chooses "Sign in with Google").
2. System validates credentials/OAuth token, creates or retrieves the account, and establishes a session.
3. User is redirected to the catalog browse/search experience, now authenticated.
**Alternative scenario:**
1. User enters invalid/duplicate credentials on registration; system rejects with a specific error and the user retries.
2. User's Google OAuth flow is cancelled or fails; system returns the user to the login screen with an error message.
**Post-conditions:** A valid session exists for the user; the account record exists in the system.
**Assumptions:** None beyond standard credential-validation behavior.

### UC-2 (Starter): Browse and Search Catalogs
**Description:** As a logged-in or anonymous user, when I use the unified search bar or browse a specific catalog's categories, then the system returns matching products from the appropriate catalog(s), so that I can find products to purchase.
**Roles:** Anonymous visitor, Registered user.
**Pre-conditions:** At least one product exists in the Furniture and/or Building Materials catalog.
**Main scenario:**
1. User enters a search term in the unified search bar.
2. System returns matching results drawn from both the Furniture and Building Materials catalogs together, indicating which catalog each result belongs to.
3. User opens a product's item card from the results (→ see UC-6).
**Alternative scenario:**
1. User instead navigates directly into the Furniture (or Building Materials) section and applies category filters specific to that catalog's schema.
2. System returns filtered results scoped only to that one catalog.
**Post-conditions:** User has located one or more products and can proceed to add them to cart.
**Assumptions:** Search and per-catalog filters operate against indexed data sufficient for low-thousands-of-SKU catalogs (NFR-1).

### UC-3 (Starter): Purchase Individual Products
**Description:** As a logged-in user, when I add individual products from either or both catalogs to my cart and complete checkout, then the system submits a single order to Bitrix24 via the custom checkout form, so that I can obtain the products I selected.
**Roles:** Registered user.
**Pre-conditions:** User is logged in; at least one product from either catalog is available for purchase.
**Main scenario:**
1. User adds one or more products — from Furniture, Building Materials, or both — to a single cart.
2. User proceeds to checkout and completes the custom Rumica checkout form.
3. System submits the order to Bitrix24 Payment APIs and confirms success to the user.
**Alternative scenario:**
1. Payment submission to Bitrix24 fails or is declined; system informs the user and returns them to checkout to retry.
**Post-conditions:** An order record exists in Bitrix24 CRM associated with the user; cart is cleared on success.
**Assumptions:** No curated design packs and no planner-originated items are part of this flow — items are standalone catalog products only.

### UC-4 (Starter): View My Orders
**Description:** As a logged-in user, when I open "My Orders," then the system displays my order history retrieved from Bitrix24 CRM, so that I can track past purchases.
**Roles:** Registered user.
**Pre-conditions:** User is logged in and has at least one prior order (for a non-empty result).
**Main scenario:**
1. User navigates to "My Orders."
2. System queries Bitrix24 CRM for orders associated with the user and displays them.
**Alternative scenario:**
1. User has no prior orders; system displays an empty state instead of an order list.
**Post-conditions:** None (read-only view).
**Assumptions:** None beyond Bitrix24 CRM being the source of truth for orders, per parent architecture.

### UC-5 (Starter): Admin Catalog Management
**Description:** As an Admin user, when I open Catalog Management and select Furniture or Building Materials via the toggle, then the system lets me manually create/edit/delete products or import them via CSV using that catalog's template, so that both catalogs stay seeded and current.
**Roles:** Admin.
**Pre-conditions:** User has Admin role; for CSV import, a CSV file matching the selected catalog's template is available.
**Main scenario:**
1. Admin opens Catalog Management and toggles to the desired catalog (Furniture or Building Materials).
2. Admin manually creates, edits, or deletes a product record using that catalog's attribute schema.
3. System persists the change and reflects it in catalog browse/search.
**Alternative scenario:**
1. Admin instead uploads a CSV file using the template matching the selected catalog.
2. System validates the file, processes it as an async batch job, and reports success/failure/row-level errors to the Admin once complete.
**Post-conditions:** Catalog data is created/updated/removed accordingly; changes are reflected in both unified search and catalog-specific browse.
**Assumptions:** CSV templates strictly correspond to each catalog's own attribute schema, per the confirmed two-template decision. No pricing/priority rule engine is included (deferred).

### UC-6 (Starter): View Item Card (Photos and Details)
**Description:** As a user browsing or searching either catalog, when I open an item from unified search results or catalog-specific browse, then the system displays that item's card showing its photo/image gallery and its full attribute/detail set per its own catalog schema, so that I can evaluate the item before adding it to my cart.
**Roles:** Anonymous visitor, Registered user.
**Pre-conditions:** The item exists in either the Furniture or the Building Materials catalog and has been reached via search (UC-2) or catalog-specific browse (UC-2, alternative scenario).
**Main scenario:**
1. User selects an item from unified search results or from catalog-specific browse (see UC-2).
2. System opens the item card and displays the item's photo/image gallery.
3. System displays the item's full attribute/detail set, drawn from the attribute schema of whichever catalog — Furniture or Building Materials — the item belongs to (per FR-6, FR-14).
4. User reviews the photos and details and may proceed to add the item to cart (UC-3).
**Alternative scenario:**
1. The item has no images on file; system displays a placeholder/fallback graphic in the gallery area instead of an empty gallery.
2. The item's attribute data is incomplete; system displays whichever attributes are available and omits blank/broken fields rather than rendering them.
**Post-conditions:** The user has viewed the item's images and details; viewing alone causes no change to catalog data or cart contents.
**Assumptions:** This use case covers viewing only. Placing an item into an interior/room (parent FR-14, "Place in Interior") is explicitly not part of Starter and not part of this use case — it depends on the deferred 2D Room Planner.

## User Flows

### Registration and Login (main flow)
1. Visitor lands on the site and selects Register or Login.
2. Visitor chooses email/password or Google OAuth.
3. System validates and creates a session.
4. User is routed to catalog browse/search, now authenticated.

### Registration and Login (alternative flow — OAuth failure)
1. Visitor selects "Sign in with Google."
2. OAuth provider returns an error or the user cancels.
3. System returns the user to the login screen with an explanatory message; user retries or falls back to email/password.

### Catalog Discovery (main flow)
1. User enters a term in the unified search bar.
2. System returns combined results from both catalogs, labeled by catalog.
3. User selects a result and views product detail.
4. User adds the product to the cart.

### Catalog Discovery (alternative flow — catalog-specific browse)
1. User selects the Furniture (or Building Materials) section directly.
2. User applies category filters specific to that catalog's schema.
3. System returns filtered results within that one catalog only.
4. User adds a product to the cart.

### Mixed-Cart Checkout (main flow)
1. User has items from both Furniture and Building Materials in the cart.
2. User proceeds to checkout and fills the custom Rumica checkout form.
3. System submits a single order (covering both catalogs' items) to Bitrix24 Payment APIs.
4. System confirms order success and clears the cart.
5. Order appears in "My Orders," sourced from Bitrix24 CRM.

### Mixed-Cart Checkout (alternative flow — payment failure)
1. User submits the checkout form.
2. Bitrix24 Payment API returns a failure/decline.
3. System displays the error and returns the user to checkout with cart intact for retry.

### Admin CSV Catalog Import (main flow)
1. Admin opens Catalog Management and toggles to Furniture or Building Materials.
2. Admin downloads/uses that catalog's CSV template and uploads a populated file.
3. System queues the file as an async batch import job.
4. System processes the batch and reports per-row success/failure to the Admin.
5. Newly imported products appear in browse/search for that catalog.

## Presumed Architecture Direction

Starter remains Bitrix24-centric, consistent with the parent `artdecor-home-design` architecture — no new backend paradigm is introduced. The same thin-bridge shape applies: an API Gateway/BFF, an Auth Bridge (email/password + Google OAuth against Bitrix24 identity/CRM), a Commerce Bridge (custom checkout form → Bitrix24 Payment APIs, mixed-cart-aware), and a Catalog Sync/Proxy layer (webhook-driven plus periodic reconciliation, per the parent's account of Bitrix24 standard-plan API limits) sized for medium-scale catalogs (low thousands of SKUs each, per NFR-1) with batch/async CSV ingestion. My Orders reads directly from Bitrix24 CRM as in the parent design.

**Open question explicitly deferred to the system-architect step (not decided here):** now that Furniture and Building Materials are confirmed as two genuinely separate catalogs with distinct attribute schemas, it is unresolved whether Bitrix24 should model them as two separate catalog entities (with the existing Catalog Sync/Proxy generalized via parameterization to handle either), or via two parallel sync pipelines, or some other mechanism. This decision, and its implications for the unified-search requirement (FR-7) and the unified admin screen (FR-9), is the system architect's to make and document.

## Required Integrations

- **Bitrix24 CRM/Catalog:** Stores both catalogs' product data (via Catalog Sync/Proxy) and order/customer records; source of truth for "My Orders."
- **Bitrix24 Payment APIs:** Receives checkout submissions from the custom Rumica checkout form for mixed-cart orders.
- **Google OAuth:** Third-party identity provider for social login, feeding the Auth Bridge.

## Clarifying Questions

None — all scope-defining questions were resolved in the Discovery-round Q&A transcript in `00-request.md`; the one remaining open point (two-catalog modeling in Bitrix24) is explicitly deferred to the system-architect step as noted above, not left ambiguous in this document.
