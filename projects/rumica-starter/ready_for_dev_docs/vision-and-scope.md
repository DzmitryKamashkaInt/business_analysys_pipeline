# Vision and Scope — Rumica Starter (Phase 1 of Rumica)

## Purpose

Rumica Starter is Phase 1 of the Rumica platform (parent project: `artdecor-home-design`), scoped to deliver the smallest coherent, launchable slice of the product ahead of the full MVP: user accounts, two independent product catalogs (furniture and building materials), and end-to-end ordering. This rework replaces Starter's original Bitrix24-centric direction with a fully custom-built backend — catalog, identity/authentication, and CRM/orders are all owned in-house — and expands ordering to support checkout and payment across six countries (Kazakhstan, Uzbekistan, Armenia, Georgia, Belarus, Russia), each routed to its own local payment provider(s) and priced in its own local currency. It exists so the business can validate account creation, catalog operations at scale, multi-country commerce, and the country-specific payment-provider abstraction before investing in the planner and community features that make up the rest of the MVP.

Starter is explicitly a partial realization of the parent project's business case, not a redefinition of it. Per the resolved decisions carried over from the parent project (and updated by this rework), the following is stated plainly rather than implied:

- **BR-1 (non-specialists plan and furnish with real products) — partially satisfied.** Users can browse and purchase real products across two catalogs, in whichever of the six supported countries they are in, but Starter provides no planning capability (no 2D room planner), so the "plan" half of BR-1 remains unmet.
- **BR-2 (direct purchase from a design — Rumica's core differentiator) — not satisfied.** Purchase in Starter is of standalone catalog items; there is no design/project context to purchase "from," because the planner and Design Projects features are deferred to a later phase.
- **BR-3 (freemium growth model) — not applicable to Starter.** There are no Free/Paid tiers in this scope.
- **BR-4 (own the full commerce/CRM/catalog stack, rather than a third-party platform, to control cost/timeline and to support country-specific payment orchestration) — fully satisfied.** See the reworded BR-4 below; this replaces the prior "centralize in Bitrix24" wording now that Bitrix24 has been dropped entirely.
- **BR-5 (community/publish loop) — not satisfied.** The publish/duplicate gallery loop depends entirely on the deferred planner and Design Projects gallery.

This limitation is accepted and carried forward as known, not silently resolved: Starter is a commerce-and-catalog foundation, not yet the differentiated "plan-then-buy" product that is Rumica's ultimate value proposition.

## Scope & Boundaries

### In Scope

- Email/password registration and login, plus **Google OAuth login** (explicit — see FR-2).
- Simplified user profile management (no subscription-tier visibility, since tiers do not exist in Starter).
- Two independent product catalogs — **Furniture** and **Building Materials** — each with its **own attribute schema**, browsable, searchable, and filterable independently, end to end (catalog storage, browse/filter UI, CSV import, item card rendering — no collapsing into one undifferentiated schema at any layer). Catalog contents are **identical across all six supported countries** — every SKU is purchasable in every country; only price/currency and payment routing vary by country, not product availability.
- A **unified search bar** that searches across both catalogs simultaneously.
- **Catalog-specific category browsing and filtering**, now including **nested category hierarchy with breadcrumb navigation** per catalog — browse/filter UI remains separate per catalog (not unified), even though search is unified.
- Product detail pages ("item card") for items in either catalog, rendering that item's photo gallery and its full attribute set per its own catalog's schema; for an out-of-stock item, the item card replaces the "Add to Cart" action with a "Notify me when available" affordance (see FR-18, UC-7).
- **Admin Catalog Management**: a single unified admin screen with a toggle between Furniture and Building Materials, sharing one CRUD and CSV-import UI pattern, Admin role only (no Editor/Viewer role hierarchy).
- **CSV import for catalog seeding**, available from day one, using **two separate CSV templates** — one matching the furniture attribute schema, one matching the building materials attribute schema. Import supports batch/async processing to handle medium-scale catalogs without blocking the admin UI, and reports **per-row success/failure** to the Admin on completion.
- **Stock/inventory management**: stock quantity tracked per SKU in both catalogs, decremented on a successfully paid order, and out-of-stock items handled explicitly (blocked from checkout / flagged to the user).
- Shopping cart that accepts a **mixed cart** — items from both catalogs together in one cart — available to **both anonymous (guest) and logged-in users**, with the guest cart **merging into the user's account cart on login/registration**.
- A single checkout flow, now **country/currency-aware**: a custom Rumica checkout form that routes payment through a **payment orchestration layer** to the appropriate country-specific provider adapter(s), based on the cart's country/currency context, for all six supported countries. Prices are displayed and stored in each country's own local currency (true multi-currency — no single reference currency).
- "My Orders" view, sourced from the custom-built Orders/CRM module, showing order history for the logged-in user.

### Out of Scope

**Deferred to a later phase of Rumica** (remain fully in-scope for the product; simply not built in Starter, and Starter is designed not to require rework when they are added):
- 2D Room Planner (Three.js-based room/interior design tool).
- Floor-plan upload, recognition, and dimension confirmation.
- Flat Plans gallery.
- Public Design Projects gallery, including publish and duplicate.
- Project management: save, duplicate, export, share of design projects.
- Favorites.
- Free/Paid subscription tiers and their associated usage limits.
- Subscription checkout path.
- Full Admin panel beyond catalog CRUD: dashboard, user management, moderation, reports. (Role hierarchy beyond a single Admin role is likewise out of scope — explicitly rejected for this rework.)
- Analytics integration (internal + GA/Mixpanel).
- Curated design packs (tied to the planner; deferred alongside it — checkout in Starter covers individual products only).
- Multi-language localization (Starter's UI remains single-language, Russian, across all six supported countries — only currency and payment routing vary by country, not language).

## Business Requirements

- **BR-1** (partially satisfied by Starter): Enable non-specialist users to furnish with real, purchasable products. Starter satisfies the "furnish with real products" half, across six countries; the "plan" half is out of scope here.
- **BR-2** (not satisfied by Starter): Enable direct purchase from within a design. Requires the deferred planner; tracked as a known gap, not to be resolved within Starter.
- **BR-3** (not applicable to Starter): Freemium growth model via Free/Paid tiers. No tiers exist in Starter.
- **BR-4** (fully satisfied by Starter, reworded): Own the full commerce, CRM, and catalog stack in a custom-built backend rather than a third-party platform. This is the rationale replacing the prior Bitrix24-centralization wording: a custom stack gives Rumica direct control over cost and delivery timeline, and — critically — is what makes the country-specific payment-provider orchestration (six independently-swappable local payment adapters, per-country currency) practical. A third-party platform such as Bitrix24 could not accommodate that degree of payment-routing customization.
- **BR-5** (not satisfied by Starter): Community/publish loop via public Design Projects. Requires the deferred planner and gallery.

## Functional Requirements

- **FR-1 (Starter)** — The system SHALL allow users to register and log in using email/password. *(unchanged)*
- **FR-2 (Starter)** — The system SHALL allow users to log in via Google OAuth. *(unchanged; made explicit per this rework's closed estimate-coverage gap — this requirement already existed, it is now stated unambiguously)*
- **FR-3 (Starter)** — The system SHALL maintain authenticated sessions and reject requests to protected resources without a valid session/token. *(unchanged)*
- **FR-4 (Starter)** — The system SHALL allow a logged-in user to view and edit their profile, without displaying any subscription-tier information. *(unchanged)*
- **FR-5 (Starter)** — The system SHALL allow users to browse, search, filter, and view product detail for items in the Furniture catalog. *(unchanged)*
- **FR-6 (Starter)** — The system SHALL allow users to browse, search, filter, and view product detail for items in the Building Materials catalog, using that catalog's own attribute schema, independent of the Furniture catalog's schema. *(unchanged; independence reaffirmed as an explicit, non-negotiable property of this rework's data model)*
- **FR-7 (Starter)** — The system SHALL provide a single, unified search bar that returns matching results from both the Furniture and Building Materials catalogs together. *(unchanged)*
- **FR-8 (Starter)** — The system SHALL keep category browsing and attribute-based filtering catalog-specific, and SHALL support a nested category hierarchy with breadcrumb navigation within each catalog: a user browsing/filtering Furniture SHALL see only furniture categories/filters (navigable via breadcrumbs through the category tree), and likewise for Building Materials, with no cross-catalog category merge. *(changed — extended per this rework's accepted scope-creep item: nested category hierarchy + breadcrumbs)*
- **FR-9 (Starter)** — The system SHALL provide Admin users a single "Catalog Management" screen with a toggle between Furniture and Building Materials, exposing manual create/read/update/delete for products in whichever catalog is selected, restricted to the Admin role only (no Editor/Viewer roles). *(unchanged in shape; role-hierarchy rejection made explicit)*
- **FR-10 (Starter)** — The system SHALL support CSV import for catalog seeding from day one, using two separate CSV templates — one for Furniture (matching its attribute schema) and one for Building Materials (matching its attribute schema) — processed as a batch/async job so the admin UI is not blocked during import, and SHALL report per-row success/failure results to the Admin once the batch completes. *(changed — per-row reporting made an explicit requirement, closing a previously-implicit gap)*
- **FR-11 (Starter)** — The system SHALL allow a single cart to contain items from both the Furniture and Building Materials catalogs simultaneously (mixed cart), and SHALL support an anonymous/guest cart that persists pre-login and merges into the user's account cart upon login or registration. *(changed — extended per this rework's accepted scope-creep item: guest cart with merge-on-login)*
- **FR-12 (Starter)** — The system SHALL provide a single, country/currency-aware checkout flow using a custom Rumica checkout form: the cart's country/currency context SHALL determine which country-specific payment provider adapter(s), behind the payment orchestration layer, the order is routed to, and prices SHALL be displayed and charged in that country's local currency. *(changed — replaces "submits to Bitrix24 Payment APIs"; now multi-country, multi-currency, orchestration-layer-based)*
- **FR-13 (Starter)** — The system SHALL display a "My Orders" view to the logged-in user, sourced from the custom-built Orders/CRM module. *(changed — source of truth is now the custom backend, not Bitrix24 CRM)*
- **FR-14 (Starter)** — The system SHALL provide a product/item detail page ("item card") for any catalog item, from either the Furniture or the Building Materials catalog, that displays: (a) a photo/image gallery for the item, and (b) the item's full attribute/detail set as defined by that item's own catalog schema — Furniture attributes for Furniture items, Building Materials attributes for Building Materials items, never a generic shared shape — consistent with the separate-schema decision in FR-6. *(unchanged; per-catalog-schema rendering made explicit per this rework's closed estimate-coverage gap)*
- **FR-15 (Starter)** — The system SHALL track a single global stock quantity per SKU (not per-country), shared across all six supported countries, and SHALL decrement that stock on a successfully paid order regardless of which country the order came from. Out-of-stock prevention SHALL primarily occur at browse/item-card/add-to-cart time — an out-of-stock item SHALL NOT be addable to the cart in the first place (see UC-6, FR-18) — with checkout-time stock verification retained only as a fallback for the rare race condition where a cart item's stock is exhausted after it was already added (see UC-3). *(new — accepted scope-creep item: stock/inventory management; confirmed as a single global pool, not per-country pools; reworded this cycle to align with the confirmed add-to-cart-time prevention decision — see Evaluation Round 3)*
- **FR-16 (Starter)** — The system SHALL store and display catalog prices, cart totals, and order totals in the local currency of the country associated with the shopping session/cart, independently per country, with no single reference currency and no automatic cross-currency conversion at checkout. *(new — true multi-currency model)*
- **FR-17 (Starter)** — The system SHALL expose an internal payment orchestration interface (createPayment, getStatus, refund, webhook) behind which a separate provider adapter plugs in for each of the six supported countries (Kazakhstan, Uzbekistan, Armenia, Georgia, Belarus, Russia), and SHALL allow any single country's adapter to be replaced without requiring changes to checkout UI, order flow, or other countries' adapters. *(new — payment orchestration layer, direction only; detailed component design is the system-architect's to make)*
- **FR-18 (Starter)** — The system SHALL let a user (guest or logged-in) request a one-time restock notification for an out-of-stock SKU from that item's card, capturing the user's email address (from their account if logged in, or entered manually if a guest); the system SHALL send exactly one notification email to that address when the SKU's stock quantity transitions from zero to an available (positive) quantity, and SHALL NOT send any further notification for that same request after it has fired once. *(new — accepted scope-creep item from this rework cycle: "Notify me when available" restock notification, part of the confirmed out-of-stock UX decision)*

### Traceability Mapping (Starter FR/UC → Parent FR/UC, and prior-version Starter FR/UC)

| Starter ID | Status (this rework) | Parent ID(s) / Prior-Starter mapping | Notes |
|---|---|---|---|
| FR-1 | Carried over, unchanged | Parent FR-1 | — |
| FR-2 | Carried over, made explicit | Parent FR-2 | Already existed; explicitness was a closed estimate-coverage gap, not new scope |
| FR-3 | Carried over, unchanged | Parent FR-3 | — |
| FR-4 | Carried over, unchanged | Parent FR-4-starter | — |
| FR-5 | Carried over, unchanged | Parent FR-13-starter | — |
| FR-6 | Carried over, unchanged | Parent FR-13-starter | — |
| FR-7 | Carried over, unchanged | — (new in prior Starter version) | — |
| FR-8 | Carried over, extended | — (new in prior Starter version) | Adds nested category hierarchy + breadcrumbs |
| FR-9 | Carried over, unchanged | Parent FR-15-starter | Admin-only reaffirmed |
| FR-10 | Carried over, extended | Parent FR-15-starter | Adds explicit per-row CSV success/failure reporting |
| FR-11 | Carried over, extended | Parent FR-20 | Adds guest cart + merge-on-login |
| FR-12 | Changed | Parent FR-20 (prior Starter: Bitrix24 Payment APIs) | Bitrix24 dropped; now orchestration-layer, multi-country, multi-currency |
| FR-13 | Changed | Parent FR-22 (prior Starter: Bitrix24 CRM) | Bitrix24 dropped; now custom Orders/CRM module |
| FR-14 | Carried over, unchanged | — (new in prior Starter version) | Per-catalog-schema rendering made explicit |
| FR-15 | New | — | Stock/inventory management (accepted scope-creep item) |
| FR-16 | New | — | Multi-currency pricing model |
| FR-17 | New | — | Payment orchestration layer / six country adapters |
| FR-18 | New — this rework cycle, out-of-stock UX decision | — | "Notify me when available" one-time restock notification |
| UC-1 | Carried over, unchanged | Parent UC-1 | — |
| UC-2 | Carried over, extended | — (new in prior Starter version) | Adds nested category hierarchy + breadcrumbs |
| UC-3 | Changed / extended | Parent UC-6-starter (prior Starter: Bitrix24 checkout) | Bitrix24 dropped; adds guest cart merge, country/currency-aware checkout; out-of-stock handling reframed this cycle as a rare race-condition case only (prevention now happens at add-to-cart time, see UC-6/UC-7), with a link to the new "Notify me when available" flow |
| UC-4 | Changed | Parent UC-7 (prior Starter: Bitrix24 CRM) | Bitrix24 dropped; now sourced from custom Orders/CRM module |
| UC-5 | Carried over, extended | Parent UC-9-starter | Adds explicit per-row CSV import success/failure reporting |
| UC-6 | Carried over, extended | — (new in prior Starter version) | Alternative scenarios (image placeholder; omit blank/broken attributes) reaffirmed as explicit; this rework cycle adds the out-of-stock branch in the main scenario (Add to Cart unavailable → Notify me when available, UC-7) |
| UC-7 | New — this rework cycle, out-of-stock UX decision | — | Request Restock Notification ("Notify me when available") |

## Non-Functional Requirements

- **NFR-1 (Catalog scale):** Both catalogs are expected to hold a medium/near-term volume — low thousands of SKUs each. CSV import SHALL be implemented as a batch/async process capable of handling this volume without blocking the admin UI. Catalog browse and filter SHALL use indexed filtering and proper pagination so that listing performance remains acceptable at this scale. *(unchanged; backend now custom-built rather than Bitrix24-hosted, but the scale target is the same)*
- **NFR-2 (Multi-currency data integrity):** All monetary values (catalog prices, cart totals, order totals) SHALL be stored per-country in that country's own currency with an explicit currency code attached to every stored amount, using integer/minor-unit precision (no floating-point rounding of currency values). The system SHALL NOT aggregate, compare, or convert amounts across currencies implicitly; any cross-currency reporting is out of scope for Starter. *(new — derived from the confirmed true multi-currency, no-single-reference-currency decision)*
- **NFR-3 (Payment adapter swap resilience):** The payment orchestration layer's per-country provider adapter SHALL be replaceable within weeks, for any single country, without requiring changes to the checkout UI, the order/cart flow, or the adapters of the other five countries — reflecting the confirmed regional payment/compliance instability across the six supported countries. *(new — derived from the confirmed adapter-swap requirement)*
- **NFR-4 (Restock notification at-most-once dispatch):** For any given restock-notification request (SKU + email pair, FR-18), the system SHALL guarantee that at most one notification email is ever sent for that request, even under concurrent or near-simultaneous stock updates for the same SKU (e.g., an Admin edit and a CSV import touching the same SKU in quick succession). *(new — derived directly from FR-18's "SHALL NOT send any further notification after it has fired once"; added this cycle to keep this NFR paired between the Vision & Scope and the Architecture Specification, matching NFR-1/2/3's convention)*
- All other NFR categories (specific performance targets, availability, security posture, accessibility) — not yet specified — to be confirmed.

## Modules

- **Auth Module:** Email/password and Google OAuth registration/login, session management — now backed by the custom identity store, not Bitrix24.
- **Profile Module:** Simplified user profile view/edit (no tier display).
- **Furniture Catalog Module:** Browse/search/filter/detail for the Furniture catalog and its attribute schema, including nested category hierarchy/breadcrumbs and the item card (FR-14, UC-6); for out-of-stock items, the item card's "Add to Cart" action is replaced by "Notify me when available" (FR-18, UC-7).
- **Building Materials Catalog Module:** Browse/search/filter/detail for the Building Materials catalog and its distinct attribute schema, including nested category hierarchy/breadcrumbs and the item card (FR-14, UC-6); for out-of-stock items, the item card's "Add to Cart" action is replaced by "Notify me when available" (FR-18, UC-7).
- **Unified Search Module:** Cross-catalog search bar returning results from both catalogs.
- **Catalog Admin Module:** Unified Catalog Management screen (Furniture/Building Materials toggle), manual CRUD, two-template CSV import with async processing and per-row success/failure reporting, Admin-only.
- **Inventory/Stock Module:** Per-SKU stock quantity, decrement-on-paid-order, out-of-stock handling for both catalogs (FR-15).
- **Cart & Checkout Module:** Mixed-catalog cart, guest cart with merge-on-login, custom Rumica checkout form, country/currency-aware routing into the Payment Orchestration Module.
- **Payment Orchestration Module:** Internal createPayment/getStatus/refund/webhook interface; six country-specific provider adapters (Kazakhstan, Uzbekistan, Armenia, Georgia, Belarus, Russia), independently swappable.
- **Orders/CRM Module:** Custom-built order and customer record storage, replacing Bitrix24 CRM; source for "My Orders."

## Use Cases

### UC-1 (Starter): Register and Login
**Description:** As a prospective or returning user, when I choose to register or log in via email/password or Google OAuth, then the system authenticates me and establishes a session, so that I can access account-specific features like ordering and My Orders.
**Roles:** Anonymous visitor, Registered user.
**Pre-conditions:** The user has a valid email address or a Google account; the system is reachable.
**Main scenario:**
1. User selects "Register" and submits email/password (or chooses "Sign in with Google").
2. System validates credentials/OAuth token against the custom identity store, creates or retrieves the account, and establishes a session.
3. User is redirected to the catalog browse/search experience, now authenticated.
**Alternative scenario:**
1. User enters invalid/duplicate credentials on registration; system rejects with a specific error and the user retries.
2. User's Google OAuth flow is cancelled or fails; system returns the user to the login screen with an error message.
**Post-conditions:** A valid session exists for the user; the account record exists in the system.
**Assumptions:** None beyond standard credential-validation behavior.

### UC-2 (Starter): Browse and Search Catalogs
**Description:** As a logged-in or anonymous user, when I use the unified search bar or browse a specific catalog's nested categories, then the system returns matching products from the appropriate catalog(s), so that I can find products to purchase.
**Roles:** Anonymous visitor, Registered user.
**Pre-conditions:** At least one product exists in the Furniture and/or Building Materials catalog.
**Main scenario:**
1. User enters a search term in the unified search bar.
2. System returns matching results drawn from both the Furniture and Building Materials catalogs together, indicating which catalog each result belongs to.
3. User opens a product's item card from the results (→ see UC-6).
**Alternative scenario:**
1. User instead navigates directly into the Furniture (or Building Materials) section and drills into that catalog's nested category hierarchy using breadcrumb navigation, applying category filters specific to that catalog's schema.
2. System returns filtered results scoped only to that one catalog, with breadcrumbs reflecting the user's current position in the category tree.
**Post-conditions:** User has located one or more products and can proceed to add them to cart.
**Assumptions:** Search and per-catalog filters operate against indexed data sufficient for low-thousands-of-SKU catalogs (NFR-1).

### UC-3 (Starter): Purchase Individual Products
**Description:** As a user (anonymous or logged-in), when I add individual products from either or both catalogs to my cart and complete checkout in my country's currency, then the system routes payment through the appropriate country-specific provider adapter and creates a single order in the custom Orders/CRM module, so that I can obtain the products I selected.
**Roles:** Anonymous visitor (guest cart only), Registered user (checkout).
**Pre-conditions:** At least one product from either catalog is available for purchase and in stock; checkout requires the user to be logged in (a guest cart may exist beforehand).
**Main scenario:**
1. User (anonymous or logged-in) adds one or more products — from Furniture, Building Materials, or both — to a single cart.
2. If the user was anonymous, they log in or register at or before checkout; their guest cart merges into their account cart.
3. User proceeds to checkout and completes the custom Rumica checkout form; the system determines the cart's country/currency context and displays totals in that country's local currency.
4. System routes the payment request through the payment orchestration layer to the matching country-specific provider adapter and confirms success to the user.
5. System decrements stock for each purchased SKU and creates the order record in the custom Orders/CRM module.
**Alternative scenario:**
1. Payment submission to the country-specific provider adapter fails or is declined; system informs the user and returns them to checkout to retry, cart intact.
2. Rare race condition: an item was in stock at the moment it was added to the cart, but its stock is exhausted by a concurrent order (or drops to zero via an admin/CSV stock update) before this checkout completes. System detects this at checkout, removes/flags only that item from the order, informs the user it has just sold out, offers "Notify me when available" for that SKU (UC-7), and lets the user proceed to pay for the remaining in-stock items in the cart.
**Post-conditions:** An order record exists in the custom Orders/CRM module, associated with the user, priced in the correct local currency; stock is decremented; cart is cleared on success.
**Assumptions:** No curated design packs and no planner-originated items are part of this flow — items are standalone catalog products only. Vendor-specific behavior of each country's payment provider adapter is a working assumption pending PoC/validation (see Required Integrations). Out-of-stock items cannot be added to the cart in the first place — prevention happens at browse/item-card/add-to-cart time (see UC-6), where "Add to Cart" is simply unavailable for an out-of-stock item; this use case's alternative scenario above covers only the rare race-condition case where stock is exhausted after the item was already in the cart, not the primary out-of-stock handling point.

### UC-4 (Starter): View My Orders
**Description:** As a logged-in user, when I open "My Orders," then the system displays my order history retrieved from the custom Orders/CRM module, so that I can track past purchases.
**Roles:** Registered user.
**Pre-conditions:** User is logged in and has at least one prior order (for a non-empty result).
**Main scenario:**
1. User navigates to "My Orders."
2. System queries the custom Orders/CRM module for orders associated with the user and displays them, including each order's currency and country.
**Alternative scenario:**
1. User has no prior orders; system displays an empty state instead of an order list.
**Post-conditions:** None (read-only view).
**Assumptions:** None beyond the custom Orders/CRM module being the sole source of truth for orders in this reworked architecture.

### UC-5 (Starter): Admin Catalog Management
**Description:** As an Admin user, when I open Catalog Management and select Furniture or Building Materials via the toggle, then the system lets me manually create/edit/delete products, manage their stock quantity, or import them via CSV using that catalog's template, so that both catalogs stay seeded, current, and correctly stocked.
**Roles:** Admin.
**Pre-conditions:** User has Admin role; for CSV import, a CSV file matching the selected catalog's template is available.
**Main scenario:**
1. Admin opens Catalog Management and toggles to the desired catalog (Furniture or Building Materials).
2. Admin manually creates, edits, or deletes a product record — including its stock quantity — using that catalog's attribute schema.
3. System persists the change and reflects it in catalog browse/search.
**Alternative scenario:**
1. Admin instead uploads a CSV file using the template matching the selected catalog.
2. System validates the file, processes it as an async batch job, and reports per-row success/failure results to the Admin once the batch completes.
**Post-conditions:** Catalog data (including stock levels) is created/updated/removed accordingly; changes are reflected in both unified search and catalog-specific browse.
**Assumptions:** CSV templates strictly correspond to each catalog's own attribute schema, per the confirmed two-template decision. No pricing/priority rule engine is included (deferred); no role hierarchy beyond Admin exists.

### UC-6 (Starter): View Item Card (Photos and Details)
**Description:** As a user browsing or searching either catalog, when I open an item from unified search results or catalog-specific browse, then the system displays that item's card showing its photo/image gallery and its full attribute/detail set per its own catalog schema, so that I can evaluate the item before adding it to my cart.
**Roles:** Anonymous visitor, Registered user.
**Pre-conditions:** The item exists in either the Furniture or the Building Materials catalog and has been reached via search (UC-2) or catalog-specific browse (UC-2, alternative scenario).
**Main scenario:**
1. User selects an item from unified search results or from catalog-specific browse (see UC-2).
2. System opens the item card and displays the item's photo/image gallery.
3. System displays the item's full attribute/detail set, drawn from the attribute schema of whichever catalog — Furniture or Building Materials — the item belongs to (per FR-6, FR-14).
4. User reviews the photos and details; if the item is in stock, the user may proceed to add it to cart (UC-3); if the item is out of stock, "Add to Cart" is unavailable and the system instead offers "Notify me when available" for that SKU (UC-7).
**Alternative scenario:**
1. The item has no images on file; system displays a placeholder/fallback graphic in the gallery area instead of an empty gallery.
2. The item's attribute data is incomplete; system displays whichever attributes are available and omits blank/broken fields rather than rendering them.
**Post-conditions:** The user has viewed the item's images and details; viewing alone causes no change to catalog data or cart contents.
**Assumptions:** This use case covers viewing only. Placing an item into an interior/room (parent FR-14, "Place in Interior") is explicitly not part of Starter and not part of this use case — it depends on the deferred 2D Room Planner.

### UC-7 (Starter): Request Restock Notification ("Notify me when available")
**Description:** As a user (anonymous or logged-in) viewing an out-of-stock item's card, when I select "Notify me when available" in place of "Add to Cart," then the system captures my email address and queues a one-time restock-notification request for that SKU, so that I am automatically emailed once the item becomes available again, without needing to keep checking back.
**Roles:** Anonymous visitor, Registered user (the requester); Admin (triggers restock detection via a manual stock update, UC-5); System/CSV import process (triggers restock detection via a batch stock update, UC-5 alternative scenario).
**Pre-conditions:** The item's stock quantity is zero at the time of the request; the item's card is displaying "Notify me when available" in place of "Add to Cart" (see UC-6).
**Main scenario:**
1. User opens the item card for an out-of-stock SKU (UC-6) and selects "Notify me when available" in place of "Add to Cart."
2. If the user is logged in, the system captures their account email automatically; if the user is anonymous, the system prompts for and captures a manually entered email address.
3. System records a one-time restock-notification request for that SKU and email address, and confirms to the user that the request was received.
4. At a later point, an Admin manually updates the item's stock quantity from zero to a positive value (UC-5), or a CSV import updates the item's stock quantity from zero to a positive value (UC-5, alternative scenario).
5. System detects the zero-to-available stock transition for that SKU and sends exactly one notification email to each email address with a pending request for that SKU, via the email delivery provider (see Required Integrations).
6. System marks each fired request as complete and does not send any further notification for it.
**Alternative scenario:**
1. The same email address submits a second restock-notification request for a SKU it already has a pending request for; system recognizes the existing pending request and does not queue a duplicate — only one notification will ever be sent for that SKU/email pair.
2. A user who requested notification for a SKU purchases the same or an equivalent item through another channel before the SKU is restocked; the pending request simply becomes moot — the system takes no special action, and the one-time email still fires normally (harmlessly) if/when the SKU is later restocked.
**Post-conditions:** A pending restock-notification request exists for the SKU/email pair until it fires or is superseded by a duplicate check; once fired, the request is marked complete and no further emails are sent for it.
**Assumptions:** The email delivery provider (see Required Integrations) is reliable enough for this purpose; the specific provider is an open implementation choice for the system-architect, not decided here. This use case does not reserve or hold stock for the requester — the notification is informational only, not a guarantee of availability at the moment the email is read.

## User Flows

### Registration and Login (main flow)
1. Visitor lands on the site and selects Register or Login.
2. Visitor chooses email/password or Google OAuth.
3. System validates against the custom identity store and creates a session.
4. User is routed to catalog browse/search, now authenticated.

### Registration and Login (alternative flow — OAuth failure)
1. Visitor selects "Sign in with Google."
2. OAuth provider returns an error or the user cancels.
3. System returns the user to the login screen with an explanatory message; user retries or falls back to email/password.

### Catalog Discovery (main flow)
1. User enters a term in the unified search bar.
2. System returns combined results from both catalogs, labeled by catalog.
3. User selects a result and views the item's card — photos and details (UC-6).
4. If the item is in stock, user adds the product to the cart, as either a guest or a logged-in user; if the item is out of stock, "Add to Cart" is unavailable and the user may instead request "Notify me when available" (see Request Restock Notification flow).

### Catalog Discovery (alternative flow — catalog-specific browse)
1. User selects the Furniture (or Building Materials) section directly.
2. User navigates the catalog's nested category hierarchy via breadcrumbs and applies category filters specific to that catalog's schema.
3. System returns filtered results within that one catalog only.
4. If the item is in stock, user adds it to the cart; if the item is out of stock, "Add to Cart" is unavailable and the user may instead request "Notify me when available" (see Request Restock Notification flow).

### Guest Cart, Country-Aware Checkout, and Payment (main flow)
1. Anonymous user browses both catalogs and adds items from either to a guest cart.
2. User proceeds toward checkout and is prompted to log in or register; the guest cart merges into the user's account cart.
3. System determines the cart's country/currency context and displays totals in that country's local currency.
4. User completes the custom Rumica checkout form.
5. System routes the payment request through the payment orchestration layer to the matching country-specific provider adapter.
6. Provider confirms payment; system decrements stock for purchased SKUs, creates the order in the custom Orders/CRM module, confirms success, and clears the cart.
7. Order appears in "My Orders," sourced from the custom Orders/CRM module.

### Country-Aware Checkout (alternative flow — payment failure or race-condition out-of-stock)
1. User submits the checkout form.
2. Either: the country-specific provider adapter returns a failure/decline, or — in the rare case a concurrent order/stock update exhausted an item's stock after it was already added to the cart — that item is discovered to be out of stock at checkout.
3. System displays the specific error; for the out-of-stock case, it flags only the affected item, offers "Notify me when available" for that SKU (see Request Restock Notification flow), and leaves the rest of the cart intact so the user can retry payment or adjust the cart.

### Request Restock Notification (main flow)
1. User (guest or logged-in) opens an out-of-stock item's card, where "Add to Cart" is replaced by "Notify me when available."
2. User selects "Notify me when available"; system captures the user's account email (if logged in) or prompts for a manually entered email address (if guest).
3. System records the one-time restock-notification request and confirms receipt to the user.
4. Later, an Admin manually updates the SKU's stock, or a CSV import updates it, moving stock from zero to an available quantity.
5. System detects the transition and sends exactly one notification email per pending request for that SKU via the email delivery provider, then marks each request complete.

### Admin CSV Catalog Import (main flow)
1. Admin opens Catalog Management and toggles to Furniture or Building Materials.
2. Admin downloads/uses that catalog's CSV template and uploads a populated file.
3. System queues the file as an async batch import job.
4. System processes the batch and reports per-row success/failure results to the Admin once complete.
5. Newly imported products (including stock levels) appear in browse/search for that catalog.

## Presumed Architecture Direction

Starter now moves to a fully custom-built backend, replacing every previously Bitrix24-hosted responsibility: identity/authentication, both catalogs (including their independent attribute schemas), cart/checkout, stock/inventory, and orders/CRM are all owned and stored by Rumica's own backend — there is no Bitrix24 dependency anywhere in this rework. On top of that custom backend, checkout introduces a **payment orchestration layer**: a single internal interface (createPayment, getStatus, refund, webhook) that the Cart & Checkout Module calls, which in turn routes each payment request to one of six country-specific provider adapters based on the cart's country/currency context. Each adapter is intended to be independently swappable — a given country's underlying provider can change without touching the orchestration interface, the checkout UI, or any other country's adapter — because the actual vendor per country is only a working assumption pending PoC/validation, not a locked-in choice. The detailed shape of the backend services, data schema, and the orchestration layer's internal design is intentionally left to the system-architect step; this section states the direction and constraint, not the implementation.

## Required Integrations

- **Google OAuth:** Third-party identity provider for social login, feeding the Auth Module. *(unchanged)*
- **Kazakhstan payment provider — Kaspi Pay, with fallback bank card acquiring:** Primary local payment method for the Kazakhstan checkout flow. **Working assumption pending vendor PoC/validation — not a final commitment.**
- **Uzbekistan payment providers — Payme and Click, offered in parallel:** Both offered to Uzbekistan users at checkout. **Working assumption pending vendor PoC/validation — not a final commitment.**
- **Armenia payment provider — Ameriabank vPOS or VPOS.am (multi-bank aggregator):** Candidate primary provider for the Armenia checkout flow; exact choice between the two undecided. **Working assumption pending vendor PoC/validation — not a final commitment.**
- **Georgia payment provider — TBC E-Commerce API or Bank of Georgia (Visa/Mastercard acquiring):** Candidate primary provider for the Georgia checkout flow; exact choice between the two undecided. **Working assumption pending vendor PoC/validation — not a final commitment.**
- **Belarus payment provider — bePaid (ЕРИП + cards):** Primary local payment method for the Belarus checkout flow. **Working assumption pending vendor PoC/validation — not a final commitment.**
- **Russia payment provider — ЮKassa/CloudPayments:** Primary local payment method for the Russia checkout flow, **contingent on continued legal presence in Russia** — a business/legal condition, not just a technical one. **Working assumption pending vendor PoC/validation — not a final commitment.**
- **Transactional email delivery provider (new — this rework):** Sends the one-time restock-notification email when an out-of-stock SKU transitions from zero to available stock (FR-18, UC-7). This is a genuinely new integration for Starter — it was not part of the confirmed integrations list before this rework. The specific provider (e.g., SendGrid, Mailgun, AWS SES, or an equivalent) is an **open implementation choice for the system-architect to make; it is not pre-decided here.**

## Clarifying Questions

None — both remaining open points from this rework were resolved by the user: (1) catalog contents are identical across all six countries, with no per-country availability flags (see Scope & Boundaries); (2) stock (FR-15) is a single global pool per SKU, not per-country pools (see FR-15).
