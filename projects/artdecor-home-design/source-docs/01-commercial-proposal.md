# Source: Intetics Commercial Proposal — "Home Design web application" (RU + EN versions)
Client: LLC ArtDecor Group (Zoya Kavaliova, General Manager). Prepared by Intetics Inc., August 2025.

## Executive Summary
ArtDecor Home Design platform — an innovative web-based e-commerce solution for interior design. Empowers users to plan, visualize, and purchase real products through a digital assistant bridging design creativity and e-commerce.

MVP focuses on: interactive 2D room planner (Three.js), real product catalogs (single items + curated interior packs), checkout/payments via Bitrix24 payment gateway module (extendable to external payment systems), export/share functionality.

Delivery model: milestone-based Agile, Time & Material contract.

Budget: **USD 119,279** (excl. VAT). Timeline: **6–7 months**. Start: on demand.

Team:
- PM / Business Analyst
- Front-end software engineer
- Full-Stack software engineer
- Back-end engineer (Bitrix24-focused, part-time)
- DevOps (part-time)
- QA engineer (part-time)

## 1. Scope and Objectives

### 1.1 Project Scope
Web-based application for both professional interior designers and home-design enthusiasts.
- Users visualize/plan interiors by uploading own floor plans or selecting pre-configured flat layouts from a catalog.
- Users can reuse/adapt public designs created by other users (community-driven inspiration).
- Users explore real product catalogs, integrate individual items into designs, purchase selected products or complete design packs directly through the system (seamless design ↔ e-commerce link).
- Later phases: AI for personalized design recommendations, automated styling suggestions, space-usage optimization.
- Value to end-users: professional-grade design experience accessible to non-specialists; visualize real products in own space; draw inspiration from templates/community designs; simplified concept-to-purchase journey.
- Value to ArtDecor: new competitive advantage in digital commerce/design market; additional revenue via product/design-pack sales; stronger customer loyalty; analytics on customer behavior and design trends.

### 1.2 Assumptions
- Proposal describes functionality as investigated/scoped during the Discovery phase. Updates/features not considered in Discovery require separate investigation, estimation, prioritization.
- Estimate reflects scope known at proposal time; may be updated after MVP scope elicitation/prioritization finalization.
- Features/integrations based on requirements of ArtDecor's potential partners (store networks, brands, etc.) — subject to separate investigation/estimation/prioritization.
- **Bitrix24 selected as the main development framework** for admin management, catalogs, and online shopping.
- Third-party service used for recognizing uploaded floor plans (CubiCasa, licensed, "for MVP").
- Final licensing/infrastructure costs NOT included in estimate — to be discussed after determining actual system load.
- **Russian language** is the only language in MVP scope; additional languages require separate discussion/estimation/prioritization.
- Client provides catalog data (data + renders) or API access to supplier catalogs.
- **Single currency** suffices for MVP.
- Client stakeholders available for workshops/approvals.
- Legal/compliance/organizational business requirements are the client's responsibility.

## 2. Proposed Solution

### 2.1 Intetics Solution
Web-based application integrated with product catalogs, third-party services, and user-project databases.

Core functionality: **interactive 2D planner (Three.js)** — upload own floor plans or select pre-configured layouts, place walls/doors/windows, position real products directly within designs, change placement/orientation.

- Browse/filter/search product catalog (individual items + full design packs); use products in own projects.
- Save projects; export as PDF/image; share via direct links; mark a design "public" so others can duplicate/adapt it.
- Auth: email+password, or Google OAuth (RU proposal also mentions "Google and iOS" in one feature list section — treat as ambiguous/to-confirm).
- Password recovery.
- Admin: manage users, products, categories, design packs via CRUD interface in Bitrix24-based admin panel.
- Payments: add products/design packs to cart, checkout via Bitrix24 payment module integrated with external payment gateways; single currency for MVP.
- Post-MVP: 3D visualization/design constructor; AI-based recommendations (personalized styling, optimized layouts based on user behavior/preferences).

### 2.2 Prototype and MVP — Key Features Considered/Estimated in Discovery
1. **Registration / Log In / Restore password** — create/update/manage account; email+password login; Google (and possibly Apple/iOS — inconsistent between RU/EN versions) auth; account recovery.
2. **Home screen** — entry point with saved projects, catalog browsing, featured/recommended design packs; view common floor plans and recently edited projects; start new project from home screen.
3. **2D Planner** — interactive tool (Three.js) integrated with **CubiCasa** floor-recognition tool (licensed, "for MVP" — i.e., a preliminary/placeholder integration choice per RU version, described as more firmly decided in EN version: "powered by Three.js and integrated with CubiCasa (for MVP) floor recognition tool (licensed approach)"). Upload personal floor plans or use pre-configured layouts; create/modify walls, doors, windows; position products directly within plan.
4. **Catalog of Products and Design Packs** — real furniture/decor/materials + curated interior packs; browse/search/filter; detailed product info; integrate into projects.
5. **Project Management** — create/save/duplicate/update/delete projects; export as PDF/image; share via links.
6. **Cart and Checkout** — add products/design packs to cart; complete purchase; single currency at MVP stage.
7. **Admin Panel** — manage users, products, categories, design packs via basic CRUD.
8. **AI-based Recommendations (Post-MVP)** — design suggestions based on preferences/behavior/catalog updates; recommend layouts, products, style options.

*Detailed feature list available on demand (see also `05-features-modules.md` and `06-feature-estimates.md` in this folder, which appear to be that detailed list).*

Prototype/design links referenced (not fetched — external, may require access):
- Figma clickable prototype
- Figma design system (corporate style)

## 2.3 System Architecture (as diagrammed in the proposal)

Layers (top to bottom):
1. **Clients Layer**: End-User (Web Browser), Admin (Bitrix24)
2. **Edge & Delivery Layer**: CDN/WAF, DNS/SSL
3. **Application Layer**:
   - **Web Front-End** (React + Three.js Planner)
   - **API Gateway / BFF**
   - **Services**: Auth Bridge (Bitrix24 OAuth), Commerce Bridge (Bitrix24), Export Service (PDF/PNG), Catalog Proxy (Synchronizer), Projects Service, CubiCasa Adapter
   - **Observability**: Logs, Metrics, Traces
4. **Data Layer**: Application DB (SQL) — projects, user-mapping, exports, webhook receipts; Object Storage (exports, media); Object Storage (projects)
5. **External Platforms**:
   - **Bitrix24**: OAuth, CRM (Deals/Orders/Carts), Webhooks, Catalog/Products/Packs, Payments (via Bitrix24 payment apps)
   - **CubiCasa**: Scan/Conversion API, Webhooks, Exports (JPG/PNG/SVG/PDF)

Key data flows labeled in the diagram: OAuth (client ↔ front-end ↔ API Gateway ↔ Bitrix24), Cart/Checkout (client → API Gateway → Commerce Bridge → Bitrix24 Payments), Catalog (CDN/WAF → Catalog Proxy → Bitrix24 Catalog), Floor Plan Recognition (End-User → CubiCasa Adapter → CubiCasa Scan/Conversion API), Planner Save/Load (Front-End ↔ API Gateway ↔ Projects Service ↔ App DB / Object Storage), Order status (API Gateway ↔ Commerce Bridge ↔ Bitrix24 CRM).

### 2.4 Roles and Responsibilities
Client side (not costed in the estimate): **Product Owner** — requirements & approvals.

Intetics team:
- **PM/BA** — delivery management, requirements elicitation
- **Full-Stack engineer** — React + Three.js, Design Constructor development
- **Back-End engineer** — API, DB, CubiCasa and Bitrix24 integrations
- **Front-End engineer** — main system front-end layout
- **QA engineer** — automation, regression, performance testing
- **DevOps engineer** — CI/CD, environments

Team composition may change per client requests/timelines.

### 2.5 Deliverables
- Working web application per approved requirements
- Integrated back-end infrastructure (DBs, APIs)
- Integrated external systems (Bitrix24, CubiCasa)
- Project documentation covering all features, integrations, and architecture

## 3. Estimate

Team basis: 6 members — PM/BA, senior FE engineer, senior FS engineer, senior BE engineer, DevOps engineer, QA engineer.

### 3.1 Schedule
Gantt-style timeline spans roughly Sept 2025 – May 2026 across phases: System Services & Infrastructure setup → Authentication & User Management (Registration/Login, User Profiles, Roles/Permissions) → Room Planner 2D (Design Editor, Item Placement, Entry Points, Upload plan for design, Flat Plans catalog) → Design Projects Catalog → Online Shop/Products catalog (Catalog browsing/filtering, Product page, Assets/image DB) → Admin management (Dashboard, User Mgmt, Products Mgmt, Design Project Mgmt, Flat catalog Mgmt, Reports & Analytics) → E-Commerce features (Cart/checkout, Payments, Subscriptions) → UAT bugfixing / UAT.

### 3.2 Estimate table (USD)

| Module | Front-end | Back-end | ML | Testing | DevOps | PM/BA | TOTAL |
|---|---|---|---|---|---|---|---|
| Authentication & User Management | 7830 | 5850 | 0 | 1216 | 640 | 3168 | 18704 |
| Room Planner (Design Constructor) | 24075 | 12600 | 600 | 3300 | 0 | 7425 | 48000 |
| Product Catalog | 3150 | 7650 | 432 | 988 | 0 | 2367 | 14587 |
| E-Commerce Features | 2520 | 1440 | 0 | 352 | 800 | 792 | 5904 |
| Admin Management System | 4500 | 7110 | 0 | 1032 | 1800 | 2322 | 16764 |
| Analytics & Tracking | 2700 | 1800 | 0 | 160 | 800 | 360 | 5820 |
| System Services & Infrastructure | 1800 | 4500 | 0 | 0 | 3200 | 0 | 9500 |

**Total (expected) budget: USD 119,279.** **Minimal required budget: USD 113,459** (excludes part of Admin Management System and Analytics & Tracking functionality).

Note: An "ML" cost column appears (Room Planner 600, Product Catalog 432) despite no explicit AI/ML feature being in MVP scope per the narrative — likely tied to the CubiCasa/plan-recognition integration effort. Worth confirming with the client/BA.

Final cost may vary based on actual assigned engineers' rates and undiscovered requirements/implementation variants. Budget/schedule changes must be agreed with the client in writing (email).

### 3.3 Rate Card (Intetics standard, USD)
**Long-term (Remote In-Sourcing®):**
- PM/Architect/QA Manager/Specialty: $90k–144k/yr ($50–80/hr indicative)
- Senior Engineer: $81k–108k/yr ($45–60/hr)
- Engineer: $63k–81k/yr ($35–45/hr)
- Associate Developer: $45k–63k/yr ($25–35/hr)

**Short-term (T&M):**
- PM/Architect/QA Manager/Specialty: $60–95/hr
- Senior Engineer: $60–76/hr
- Engineer: $44–59/hr
- Associate Developer: $32–43/hr

**Local consulting:** Onsite Management/Consulting: $110–240/hr

## Company Information (Intetics Inc.) — background only, not project-scope-relevant
Founded 1995, Naples FL HQ, 700+ FTEs, 200+ contractors, 500+ clients in 38+ countries, ISO 9001 / ISO 27001 certified, Remote In-Sourcing® / Offshore Dedicated Team® / TETRA™ / Predictive Software Engineering models.
