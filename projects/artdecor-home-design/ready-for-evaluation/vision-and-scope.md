# Vision and Scope — Rumica (formerly proposed as "ArtDecor Home Design")

## Purpose

Rumica is a web-based platform that connects people planning a home interior with the real furniture, decor, and design services needed to realize it — closing the loop between design visualization and e-commerce purchase in a single flow. Today, interior-planning tools (SketchUp, Planner 5D, Floorplanner, Homestyler, and regional competitors such as RemPlanner) let users lay out a room but do not let them buy the products they've placed; Rumica's differentiator is direct, in-platform purchase of the exact items used in a design.

**Business context (background only — not a source of functional requirements):** The global interior-design software market is estimated at $5.4B (2024) growing to $9.7B (2030, 10.3% CAGR); the Russia/Belarus target market is estimated at $103–195M with comparatively low competitive supply. The long-term monetization model is multi-channel — user subscriptions (Free/PRO), commission on in-platform product and design-pack purchases, B2B subscriptions/licenses for designers and furniture brands, platform connection/promotion fees, a future project marketplace, and advertising — with 5-year financial projections (Year 1 gross profit ~$343K combined B2C/B2B, scaling toward ~$1.34M revenue by 2031). These figures inform *why* the product is being built and its longer-term direction; they are not translated into MVP functional scope.

**Target users:**
- **Home-design enthusiasts / B2C end users** — homeowners, renters, people moving into a new home or renovating, who want a professional-grade planning and shopping experience without design expertise.
- **Design-minded professionals** (secondary audience for MVP, not a distinct role/feature set) — interior designers and similar users who use the same Free/Paid tiers as any other end user in MVP; a dedicated professional/B2B tier is not in MVP scope.
- **Internal ArtDecor/Rumica staff (Admins)** — Super Admin, Catalog Manager, User Manager, System Admin — who manage users, catalog, and published content via the admin panel.

Note on naming: the product was originally proposed to the client as "ArtDecor Home Design" (Intetics commercial proposal, Aug 2025). Per client confirmation, "Rumica" (Oct 2025) is the same product rebranded, not a separate initiative, and "Rumica" is used as the product name throughout this document and going forward.

## Scope & Boundaries

### In Scope (MVP)
- Account registration/login via email+password or Google OAuth; password recovery; user profile management.
- Free and Paid user subscription tiers with tier-based limits (e.g., number of saved projects, access to public-design duplication).
- Interactive 2D room planner (Three.js-based) — start a project from scratch, from an uploaded floor plan, from a flat-plan gallery template, or by duplicating an existing public design.
- Floor-plan upload and automated recognition (walls, doors, windows) via a third-party recognition service, with **mandatory user confirmation/adjustment of real dimensions before catalog items can be placed** in the design.
- Flat-plan gallery (pre-configured floor plans, organized by house series/building type/square footage/tags) and a public Design Projects gallery (community-shared designs, duplicable per subscription tier).
- Product catalog: browse/search/filter individual products and curated design packs; product detail pages; "place in interior" and "buy" actions.
- Admin-managed catalog CRUD (manual entry, CSV import, pricing rules, brand/priority placement) — centralized to internal Admin roles only.
- Project management: save, duplicate, edit, export (PDF/image), share via link, and publish as a public design.
- Cart and checkout via the Bitrix24 payment module, single currency.
- "My Orders" page — order/purchase history, order status, and access to receipts/invoices, sourced from Bitrix24 CRM order/deal data.
- Admin panel: dashboard (KPIs, recent activity), user management, catalog management, public-design moderation, basic reports/analytics.
- Analytics/tracking: internal usage tracking plus external analytics integration (Google Analytics / Mixpanel).
- Transactional email (registration, password recovery, order confirmation).

### Out of Scope for MVP (explicitly deferred — Post-MVP)
- **AI capabilities** of any kind: natural-language design assistant, AI-based personalized recommendations, automated styling/space-optimization suggestions. (The Rumica deck's AI-assistant mockup is aspirational and not part of MVP build scope.)
- **3D visualization / 3D design constructor** for end users. (3D asset upload — .OBJ/.GLTF — may exist in the admin/inventory layer for future readiness, but is not rendered to end users in MVP.)
- **Seller role / external self-service catalog management.** MVP catalog management is centralized to internal Admin roles (Super Admin, Catalog Manager, User Manager, System Admin) only; no external seller onboarding, seller dashboards, or seller CRUD access.
- **Shopify integration and multi-gateway/multi-market payments.** MVP uses Bitrix24 payments only; Shopify is planned for a later phase supporting US-market expansion.
- Multi-currency and multi-language support (MVP is single-currency, single-language per the original proposal's Discovery assumptions — no Q&A decision reopened this, so it remains as originally scoped).
- Marketplace for finished design solutions, B2B brand-partnership tooling, and advertising placements (part of the long-term monetization model, not MVP functionality).
- Apple/iOS OAuth (Google is the only social login provider in MVP).

## Business Requirements
- BR-1: Enable non-specialist users to plan a room layout and furnish it with real, purchasable products, reducing the gap between inspiration and purchase.
- BR-2: Provide a direct, in-platform purchase path from a design to real products, as the platform's core competitive differentiator versus existing planning-only tools.
- BR-3: Support a freemium growth model (Free/Paid tiers) to drive user acquisition while creating a monetization path via subscriptions and commerce.
- BR-4: Centralize commerce, CRM, and catalog administration in Bitrix24 to minimize custom backend build/maintenance cost for MVP.
- BR-5: Build a community/inspiration loop by allowing users to publish and duplicate public designs.

## Functional Requirements

**Authentication & User Management**
- FR-1: The system SHALL allow account registration and login via email+password.
- FR-2: The system SHALL allow login via Google OAuth as the sole social login option in MVP.
- FR-3: The system SHALL provide a password-recovery flow.
- FR-4: The system SHALL let users view and update their profile data and view their current subscription tier and its limits.
- FR-5: The system SHALL enforce Free-tier limits (maximum 2 saved projects; no duplication of public designs) and Paid-tier full access, per the current subscription tier.

**2D Room Planner**
- FR-6: The system SHALL allow users to start a new project from: a blank canvas, an uploaded floor plan, a flat-plan gallery template, or a duplicated public design.
- FR-7: The system SHALL allow users to upload a floor plan (JPG, PNG, or PDF) and SHALL send it to a floor-plan recognition service to detect walls, doors, and windows.
- FR-8: The system SHALL require the user to review and confirm/adjust the recognized real-world dimensions (wall lengths, door/window sizes) before the design proceeds.
- FR-9: The system SHALL NOT allow catalog items to be added to a design until dimensions have been confirmed (FR-8).
- FR-10: The system SHALL allow users to add, move, rotate, and delete items and to edit walls, doors, and windows within the 2D planner, with basic collision/overlap detection on item placement.
- FR-11: The system SHALL provide a Flat Plans gallery (pre-built floor plans organized by house series, building type, and square footage, with filtering) that a Paid user can select to pre-load into the planner.
- FR-12: The system SHALL provide a public Design Projects gallery, filterable by the same attributes as the Flat Plans gallery, from which users may duplicate a design (subject to tier limits per FR-5).

**Product Catalog**
- FR-13: The system SHALL let users browse, search, and filter the product catalog by category, dimensions, and style, and view a product detail page (images, specs, pricing).
- FR-14: The system SHALL allow a user to place a catalog product directly into an active design from the product detail page ("Place in Interior") or from within the planner.
- FR-15: The system SHALL allow Admin roles (Catalog Manager and above) to create, read, update, and delete products manually and via CSV import, and to configure pricing and brand/priority placement rules.

**Project Management**
- FR-16: The system SHALL allow users to save, duplicate, update, and delete their own projects, subject to tier limits.
- FR-17: The system SHALL allow users to export a project as an image or PDF and to share it via a direct link.
- FR-18: The system SHALL allow users to mark a project as public, making it visible in the Design Projects gallery for other users to view and (per tier) duplicate.

**Commerce**
- FR-19: The system SHALL allow users to add individual products or entire design packs to a cart and complete checkout via the Bitrix24 payment module, in a single currency.
- FR-20: The system SHALL update the user's subscription tier automatically upon successful subscription payment via Bitrix24.
- FR-21: The system SHALL provide a "My Orders" page listing the user's past orders with status and access to the corresponding receipt/invoice, sourced from Bitrix24 CRM order/deal records.

**Admin Panel**
- FR-22: The system SHALL provide an admin dashboard showing KPIs, recent projects, and most-used items.
- FR-23: The system SHALL allow Admin roles to view, assign roles to, deactivate, and ban users (User Manager and Super Admin).
- FR-24: The system SHALL allow Admin roles to approve/feature public designs (project moderation).
- FR-25: The system SHALL allow Admin roles to download usage reports.

**Analytics**
- FR-26: The system SHALL track internal usage events (project creation, item usage, time in planner) and forward equivalent events to an external analytics service (Google Analytics and/or Mixpanel).

## Non-Functional Requirements
No project-specific compliance, data-residency, or scale targets were provided by the client (per Q&A: standard NFRs only, no special constraints). The following are general best-practice placeholders, to be refined once real load and infrastructure decisions are made during architecture:
- NFR-1: Standard production uptime target (e.g., 99.5%+) — to be confirmed during architecture.
- NFR-2: Reasonable page-load and interaction responsiveness (e.g., core pages loading within a few seconds under normal load) — to be confirmed during architecture.
- NFR-3: Standard web-application security practices (encrypted transport, credential hashing/OAuth token handling, role-based access control for Admin functions) — to be confirmed during architecture.
- NFR-4: Standard backup/recovery practices for user-generated data (projects, uploaded plans) — to be confirmed during architecture.
- NFR-5: Single-currency, single-language (Russian) operation for MVP, consistent with the original proposal's Discovery-phase assumptions — no additional localization/compliance NFRs apply.

## Modules
- **Authentication & User Management**: registration, login (email/password + Google OAuth), password recovery, profile, subscription tier state.
- **Room Planner (Design Constructor)**: 2D planning canvas, entry points, floor-plan upload/recognition/dimension-confirmation flow, item placement.
- **Flat Plans & Design Projects Catalogs**: browsing/filtering of pre-built floor-plan templates and public community designs.
- **Product Catalog**: browsing, product detail, admin CRUD, pricing/priority rules.
- **Project Management**: save/duplicate/update/delete, export, share, publish.
- **E-Commerce**: cart, checkout, subscription payment, via Bitrix24; "My Orders" history.
- **Admin Panel**: dashboard, user management, catalog management, project moderation, reports.
- **Analytics & Tracking**: internal event tracking + external analytics integration.
- **Integration/Bridge Layer**: thin custom services connecting the front end to Bitrix24 and the floor-plan recognition vendor (see Presumed Architecture Direction).

## Use Cases

### UC-1: Register and log in
**Description:** As a prospective user, when I choose to create an account or sign in, then I gain access to the platform under my own identity and subscription state, so that I can save and manage my own designs.
**Roles:** End User (Free/Paid, pre-authentication)
**Pre-conditions:** User has a valid email address or a Google account; user is not already logged in.
**Main scenario:**
1. User selects "Sign up" and provides email + password, or selects "Continue with Google."
2. System validates credentials/OAuth token and creates or retrieves the user account.
3. System assigns the user to the Free tier by default.
4. User is logged in and redirected to the home screen.
**Alternative scenario:**
1. User selects "Forgot password," provides their email, and receives a password-reset link via the email service.
2. User sets a new password and logs in.
**Post-conditions:** User has an authenticated session and a profile record with tier = Free (or existing tier, on subsequent logins).
**Assumptions:** Google OAuth is the only social login in MVP (per Q&A decision #4).

### UC-2: Create a design from an uploaded floor plan
**Description:** As a user, when I upload my own floor plan, then the system recognizes its layout and I confirm its real dimensions, so that I can accurately furnish my actual room.
**Roles:** End User (Free/Paid)
**Pre-conditions:** User is logged in; user has a floor-plan image or PDF file.
**Main scenario:**
1. User selects "Start new project" → "Upload plan" and selects a JPG/PNG/PDF file.
2. System sends the file to the floor-plan recognition service and receives back detected walls, doors, and windows.
3. System renders the recognized layout in the planner and prompts the user to confirm or adjust dimensions (wall lengths, door/window sizes).
4. User confirms the dimensions.
5. System unlocks catalog-item placement for this project.
**Alternative scenario:**
1. Recognition fails or produces an unusable result; system informs the user and offers manual wall/door/window drawing as a fallback within the 2D editor.
**Post-conditions:** A project exists with a confirmed, dimensioned floor plan; the user may now place products.
**Assumptions:** CubiCasa is the working-assumption vendor for floor-plan recognition, per the estimation and proposal documents; per Q&A decision #7, this is **not yet finalized** — a vendor evaluation/PoC is required before final commitment, and the "CubiCasa Adapter" referenced in this document is a placeholder for whichever vendor is confirmed.

### UC-3: Start a design from a gallery template or public design
**Description:** As a user, when I don't have my own floor plan, then I can start from a pre-built flat plan or duplicate a public community design, so that I can begin furnishing without uploading anything.
**Roles:** End User (Free/Paid)
**Pre-conditions:** User is logged in.
**Main scenario:**
1. User browses the Flat Plans gallery or the public Design Projects gallery, filtering by house series/building type/square footage/style as needed.
2. User selects a plan or design and chooses "Use this plan" / "Duplicate."
3. System pre-loads the selected layout into the planner as a new project owned by the user.
**Alternative scenario:**
1. A Free-tier user attempts to duplicate a public design; system blocks the action and prompts an upgrade to Paid (per tier limits).
**Post-conditions:** A new project exists in the user's account, pre-loaded from the selected template/design.
**Assumptions:** None beyond FR-5 tier limits.

### UC-4: Furnish a design with catalog products
**Description:** As a user, when I have a dimensioned project open, then I can browse the product catalog and place real products into my design, so that I can visualize and later purchase them.
**Roles:** End User (Free/Paid)
**Pre-conditions:** An active project exists with confirmed dimensions (per UC-2/UC-3).
**Main scenario:**
1. User browses or searches the product catalog, optionally filtering by category, dimensions, or style.
2. User selects a product and chooses "Place in Interior."
3. System adds the product to the active project's canvas; user positions/rotates it, with basic collision detection.
**Alternative scenario:**
1. User opens a product from the catalog page directly (not from within a project) and is prompted to select or create a project to place it into.
**Post-conditions:** The project's item list includes the placed product(s).
**Assumptions:** None beyond the dimension-confirmation gate in UC-2.

### UC-5: Save, duplicate, share, and publish a project
**Description:** As a user, when I've finished or paused work on a design, then I can save it, duplicate it, export/share it, or publish it publicly, so that I retain my work and optionally contribute it to the community gallery.
**Roles:** End User (Free/Paid)
**Pre-conditions:** User has an open project.
**Main scenario:**
1. User saves the project (auto-save or explicit save action).
2. User optionally exports the project as an image/PDF or generates a shareable link.
3. User optionally marks the project as "Public," making it visible in the Design Projects gallery.
**Alternative scenario:**
1. User duplicates an existing project of their own to iterate on a variant, subject to their tier's project-count limit.
**Post-conditions:** Project state is persisted; if published, it appears in the public gallery.
**Assumptions:** Free-tier project count limit (2) applies per FR-5.

### UC-6: Purchase products/design packs (cart and checkout)
**Description:** As a user, when I've selected products or a design pack I want to buy, then I can check out and pay, so that I receive the real items shown in my design.
**Roles:** End User (Free/Paid)
**Pre-conditions:** User is logged in; cart contains at least one item.
**Main scenario:**
1. User adds one or more products (individually or as part of a design pack) to the cart.
2. User reviews the order summary and proceeds to checkout.
3. System routes payment through the Bitrix24 payment module.
4. On successful payment, system creates/updates the corresponding order/deal in Bitrix24 CRM and confirms the order to the user.
**Alternative scenario:**
1. Payment fails or is declined; system returns the user to checkout with an error and preserves the cart contents.
**Post-conditions:** A completed order exists in Bitrix24 CRM and is visible to the user via "My Orders" (UC-7).
**Assumptions:** Single currency, Bitrix24-only payment gateway for MVP (per Q&A decision #2); Shopify/multi-gateway support is Post-MVP.

### UC-7: View order history ("My Orders")
**Description:** As a user, when I want to check on a past purchase, then I can view my order history with status and receipts, so that I have visibility into my transactions without contacting support.
**Roles:** End User (Free/Paid)
**Pre-conditions:** User is logged in and has at least one prior order (for a non-empty state).
**Main scenario:**
1. User navigates to "My Orders."
2. System retrieves the user's order/deal records from Bitrix24 CRM and displays them as a list with status (e.g., pending, paid, fulfilled).
3. User selects an order to view details and access its receipt/invoice.
**Alternative scenario:**
1. User has no past orders; system shows an empty state with a prompt to browse the catalog.
**Post-conditions:** None (read-only view).
**Assumptions:** Order/receipt data is fully sourced from Bitrix24 CRM; no separate order database is maintained by custom code (per Q&A decision #9 and the Bitrix24-centric architecture direction).

### UC-8: Subscribe to a paid tier
**Description:** As a Free-tier user, when I choose to upgrade, then I can subscribe and pay for the Paid tier, so that I unlock full platform limits (unlimited projects, public-design duplication).
**Roles:** End User (Free)
**Pre-conditions:** User is logged in on the Free tier.
**Main scenario:**
1. User navigates to subscription/upgrade options and selects the Paid tier.
2. User completes payment via the Bitrix24 payment module.
3. On successful payment, system updates the user's tier to Paid and lifts Free-tier limits.
**Alternative scenario:**
1. Payment fails; user remains on the Free tier and is shown an error with a retry option.
**Post-conditions:** User's subscription tier reflects Paid status; tier-based limits (FR-5) are updated accordingly.
**Assumptions:** Only two tiers (Free/Paid) exist in MVP; no B2B/professional tier is in MVP scope.

### UC-9: Manage catalog and users (Admin)
**Description:** As an Admin (Catalog Manager, User Manager, System Admin, or Super Admin), when I need to maintain the platform's content or user base, then I can perform CRUD and moderation actions through the admin panel, so that the catalog and community content stay accurate and appropriate.
**Roles:** Admin (Super Admin, Catalog Manager, User Manager, System Admin)
**Pre-conditions:** Admin is authenticated with an appropriate role.
**Main scenario:**
1. Admin logs into the admin panel.
2. Admin (Catalog Manager) creates, updates, or deletes products/categories manually or via CSV import, and configures pricing/priority rules.
3. Admin (User Manager) views users, assigns roles, deactivates, or bans accounts as needed.
4. Admin reviews the dashboard (KPIs, recent activity) and downloads usage reports.
**Alternative scenario:**
1. Admin reviews a public design flagged or submitted for feature/approval and approves or rejects it (project moderation).
**Post-conditions:** Catalog, user, and published-content state reflects the admin's changes.
**Assumptions:** No external Seller role exists in MVP (per Q&A decision #6); all catalog management is performed by internal Admin roles only.

## User Flows

### Floor-plan upload → confirmed design (main flow)
1. User logs in and selects "Start new project" → "Upload plan."
2. User uploads a JPG/PNG/PDF floor plan.
3. System calls the floor-plan recognition service and returns detected walls/doors/windows.
4. Planner displays the recognized layout; user reviews and adjusts real dimensions.
5. User confirms dimensions.
6. Catalog-item placement becomes available; user furnishes the room.
7. User saves the project.

### Start from gallery template (alternative flow)
1. User logs in and selects "Start new project" → "Choose from gallery."
2. User filters the Flat Plans gallery by building type/size/tags and selects a plan.
3. Plan pre-loads into the planner with dimensions already set (no confirmation step needed, since the template's dimensions are pre-validated).
4. User furnishes the room and saves the project.

### Browse-to-purchase (main flow)
1. User browses the product catalog or an active design.
2. User adds one or more products/design packs to the cart.
3. User proceeds to checkout and completes payment via Bitrix24.
4. Order is created in Bitrix24 CRM; user sees an order confirmation.
5. User later revisits "My Orders" to check status and access the receipt.

### Subscription upgrade (alternative flow)
1. Free-tier user hits a tier limit (e.g., attempts to duplicate a public design or save a 3rd project).
2. System shows an upgrade prompt.
3. User proceeds to the subscription page, selects Paid tier, and pays via Bitrix24.
4. System updates the user's tier; the previously blocked action becomes available.

### Community publish-and-discover (alternative flow)
1. User completes a design and marks it "Public."
2. Design appears in the public Design Projects gallery.
3. A different user browses the gallery, finds the design, and duplicates it (if their tier allows), starting their own project from it.

## Presumed Architecture Direction

Rumica's MVP architecture is **Bitrix24-centric**: Bitrix24 hosts admin/back-office functions, the product/catalog data, CRM (orders/deals), and payment processing, so that this functionality does not need to be custom-built. A thin custom bridging/adapter layer sits between the React/Three.js front end and Bitrix24, comprising an API Gateway/BFF and a small set of services — an Auth Bridge (Bitrix24 OAuth), a Commerce Bridge (cart/checkout/orders into Bitrix24 CRM and payments), a Catalog Proxy/synchronizer (reading Bitrix24 catalog data for the front end), a Projects Service (planner save/load state, stored in an application database and object storage, since projects are not native Bitrix24 data), and a CubiCasa Adapter (or equivalent, pending vendor validation) for floor-plan recognition calls. The earlier, more granular estimate sheet's custom Java/Spring Boot + PostgreSQL microservices narrative is treated as a superseded early technical exploration and is **not** the current direction; where that sheet's granularity is still useful (e.g., specific field-level behavior), it is read as detail on top of the Bitrix24-centric direction, not as a competing architecture. Full component/data-flow/deployment design is the system architect's next deliverable — this section exists only to bound scope and cost assumptions for this Vision & Scope.

## Required Integrations
- **Bitrix24** — OAuth/identity for admin and (per the proposal's architecture) bridged end-user auth; CRM for orders/deals (backing checkout and "My Orders"); product/catalog data; payment processing via Bitrix24 payment apps. Central to nearly all MVP commerce and admin functionality.
- **Floor-plan recognition service (working assumption: CubiCasa)** — receives uploaded floor-plan images/PDFs and returns detected walls/doors/windows for the planner. **Not yet finalized** — a vendor evaluation/PoC is required before commitment (per Q&A decision #7); architecture and integration code should be built behind an adapter to allow vendor substitution if the PoC fails.
- **Google OAuth** — social login option alongside email+password, per Q&A decision #4.
- **External analytics (Google Analytics and/or Mixpanel)** — receives usage events (project creation, item usage, planner engagement) for product analytics.
- **Email service** — transactional email for registration confirmation, password recovery, and order confirmations.
