# Source: "HomeDesign_Feature list_Estimates.pdf" — most granular feature spec (estimate columns were present in the original spreadsheet as Estimate_Front_h / Estimate_Back_h / Estimate_ML_h / Estimate_QA_h / Estimate_DevOps_h / Estimate_BA_h / Estimate_PM_h, but the numeric values did not extract from the PDF — only column headers and feature/detail text are captured here.)

This is the most detailed and behaviorally specific version of the feature list — likely the working estimate sheet behind the commercial proposal's rounded totals. Where it adds detail beyond `04-features-and-modules.md`, that detail is captured below.

## 1. Authentication & User Management
- 1.1 Registration & Login: Email/password; Google, Apple OAuth
- 1.2 User Profiles:
  - View/update profile data (left-side navigation)
  - **Subscription status and limits**: separate page — actual subscription status, payment method (add new / cancel / update)
  - **Created projects**: page listing project tabs (picture, name, room type/series); clickable View/Edit → redirects to project page or Room Planner; pagination
  - **Filtering/sorting with URL sync**: filter by flat type, room size, style, total price, etc.; filter state synced to URL query params (e.g. `?page=...&priceMin=...`)
  - **Project page**: project details (photos, assigned catalog items list, info/description); Edit button → Room Planner; ability to set project status to Public (then appears in the public Projects gallery)
  - **Favorites**: page listing catalog items marked as favorites, with status (pending / used in project / ordered), price, "See in catalog" and "Details" buttons
- 1.3 Roles & Permissions:
  - Non-paid User: limited plans, cannot clone a public project, 2 saved projects max
  - Paid User: full access
  - Admin roles: Super Admin, Catalog Manager, User Manager, System Admin
  - **Seller**: ability to enter the product-management system to CRUDL (create/read/update/delete/list) items — a role not mentioned in the commercial proposal's team/roles section; needs confirmation this is in MVP scope.

## 2. Room Planner (Design Constructor)
- 2.1 Entry Points:
  - Start new project from scratch (CRUD project from scratch or by uploading a plan)
  - From catalog: choose existing project (from Projects tab) or create new project to place a specific catalog item
  - From project gallery: duplicate a public project to edit (add catalog items) and save to own Projects list
- 2.2 Design Editor (2D): add/move/rotate/delete items; wall editing (set dimensions considered in later interior design); door/window editing; **save project as public**
- 2.3 Item Placement: drag/drop from catalog; collision check (basic overlap detection)
- 2.4 Upload/Parse Plan (AI):
  - Upload a project: choose file to upload for use in Design Editor
  - Supported formats: JPG, PNG, PDF
  - Recognize walls, doors, windows
  - **User confirmation before room generation**: user must adjust real dimensions (wall length, window/door sizes) after parsing. **Explicit constraint: the user must NOT be able to add catalog items in the Design Editor until dimensions are set and confirmed.**
- 2.5 Flat Plans Catalog:
  - Flat plans gallery (limited for free users): floor plans with proper dimensions/construction elements (doors, windows) usable for new design projects
  - Choose plan for edit: a PAID user clicks a plan → redirected to Design Editor with that plan pre-loaded
  - **Plans organization structure**: plans ordered by house series; parameters include square footage, building type (blocks, private house, house series), tags (e.g. "Сталинка" [Stalin-era], "хрущевка" [Khrushchyovka], "панельный" [panel], "кирпичный" [brick], etc. — Russian/CIS-market-specific housing-stock classifications)
  - Filtering plans by square size, building type, other parameters
  - APIs creation (implies a dedicated API for flat-plan catalog)
- 2.6 Design Project Catalog:
  - Designs gallery: list of public design projects usable as a starting point for new designs
  - Same organization structure (house series, square size, building type, tags) and filtering as Flat Plans Catalog

## 3. Product Catalog
- 3.1 Catalog Browsing: categories, filters (type, dimensions, style); **brand and items promo carousel** (clickable images promoting brands/items)
- 3.2 Product Page: item details (images 2D/3D, specs, pricing, link); "Place in Interior" button; **Buy button**
- 3.3 Admin Management: dedicated product-management system; CRUD items incl. add/delete photos/description; import catalog items via **CSV file**; admin can set high-priority label per brand/item to prioritize homepage carousel placement; admin can change pricing and create/assign pricing rules (fees, discounts) by item/type/group/brand
- 3.4 Assets and Images database creation (dedicated data store for product media)
- 3.5 **Shopping engine integration: "Bitrix24 for the MVP and Shopify for the US market"** — indicates a planned multi-market strategy with different commerce backends per region; not mentioned elsewhere in the proposal and needs confirmation of current relevance/priority.
- 3.6 3D Asset Support: upload .OBJ/.GLTF; used only in admin/inventory for MVP (not rendered to end users yet)

## 4. Design Project Management
- 4.1 Save/Load Projects: multi-project per user; limited for free users
- 4.2 Project Sharing: make public; download/export as image or PDF; share link

## 5. E-Commerce Features
- 5.1 Cart & Checkout: add items (individually or from a design); order summary
- 5.2 Payments: **"Payment gateway integration"** (generic — not named specifically here, unlike doc 04 which names Stripe and the commercial proposal which names Bitrix24); handles both single catalog-item purchases and design-pack purchases
- 5.3 Subscription Management: payment integration for subscription; update user role on payment

## 6. Admin Management System
- 6.1 Dashboard: KPIs, recent projects, most-used items, **number of purchases, purchases per brand, bulk vs. single-item purchases**
- 6.2 Users Management: view/assign roles/deactivate/ban; dedicated User page, Users list, **Sellers list, Seller page** (confirms the Seller role is a first-class admin-manageable entity)
- 6.3 Catalog Management: CRUD products manually; import via supplier feeds; priority-brand assignment; pricing rules
- 6.4 Project Moderation: approve/feature public designs
- 6.5 Reports & Analytics: download usage reports; room-planner interaction heatmaps (future)

## 7. Analytics & Tracking
- 7.1 Internal Tracking: project creation, item usage, time in planner
- 7.2 External Analytics: Google Analytics / Mixpanel integration
- 7.3 System Dashboards: user activity logs, item popularity

## 8. System Services & Infrastructure
- 8.1 Cloud Storage (Firebase, S3)
- 8.2 Email Service (SendGrid/Mailgun)
- 8.3 AI Plan Recognition Engine: integration with ML service (custom or third-party)
- 8.4 Background Processes: asset conversion, thumbnail generation
- 8.5 DB Schema Design: users, roles, projects, plans, items, assets
- **8.6 Backend Language & Framework (explicit tech stack, not mentioned anywhere else in the proposal set)**:
  - **Java (Spring Boot)**
  - **PostgreSQL**
  - REST API with JWT Auth
  - **Microservices architecture** (e.g., User, Planner, Catalog, Orders services)

## Major cross-document discrepancy to resolve with the client/architect
This sheet's section 8.6 describes a **custom Java/Spring Boot + PostgreSQL microservices backend** with its own admin panel, Stripe/generic payments, Firebase/S3 storage — this is a materially different technical architecture from the commercial proposal and its System Architecture diagram, which centers everything on **Bitrix24** (admin, catalog, CRM, payments) with a thin custom services layer (Auth Bridge, Commerce Bridge, Catalog Proxy, Projects Service, CubiCasa Adapter) sitting in front of it.

Two plausible explanations:
1. This detailed sheet is an **earlier/alternative technical exploration** (custom full-stack build) that was later superseded by the Bitrix24-centric approach reflected in the final commercial proposal and architecture diagram (most likely, since the commercial proposal is dated and explicit: "Bitrix24 was selected as a main development framework").
2. Or the sheet describes **components genuinely still planned to be custom-built** alongside Bitrix24 (e.g., the Projects Service / Planner / plan-recognition pieces might be the Java microservices, while Bitrix24 only handles catalog/CRM/payments) — in which case the architecture diagram's "Services" boxes (Projects Service, Catalog Proxy, etc.) may literally BE this Spring Boot microservices layer, and the two documents are actually consistent when read together.

**This needs to be confirmed with the client/stakeholder before architecture work proceeds**, since it changes cost, timeline, and the skill-set required for the back-end team materially.
