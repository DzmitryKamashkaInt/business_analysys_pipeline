# Source: "ArtDecor Features and modules.pdf" — detailed feature breakdown (referenced as "detailed feature list" in the commercial proposal)

## 1. Authentication & User Management
- **1.1 Registration & Login**: Email/password; Google, Apple OAuth
- **1.2 User Profiles**: View/update profile data; subscription status and limits
- **1.3 Roles & Permissions**:
  - Free User: limited plans, no reuse (of public designs), 2 saved projects
  - Paid User: full access
  - Admin roles: Super Admin, Catalog Manager, User Manager, System Admin

## 2. Room Planner (Design Constructor)
- **2.1 Entry Points**: Start new project (blank, gallery plan, uploaded plan); from catalog (place item in interior); from project gallery (duplicate and edit)
- **2.2 Design Editor (2D)**: Add/move/rotate/delete items; wall editing; door/window editing; snap to grid
- **2.3 Upload/Parse Plan (AI)**: Supported formats JPG, PNG, PDF; recognize walls/doors/windows; user confirmation required before room generation
- **2.4 Room Plan Templates**: Personal plans; public plan gallery (limited for free users)
- **2.5 Item Placement**: Drag/drop from catalog; collision check (basic overlap detection)

## 3. Product Catalog
- **3.1 Catalog Browsing**: Categories, filters (type, dimensions, style); brand and priority rules (carousel promo)
- **3.2 Product Page**: Images (2D/3D), specs, pricing, link; "Place in Interior" button
- **3.3 Admin Management**: CRUD products manually; import from supplier feeds; assign priority brands for homepage; adjust pricing (by group, type, brand)
- **3.4 3D Asset Support**: Upload .OBJ/.GLTF; used only in admin/inventory for MVP (i.e., not yet rendered in a 3D viewer for end users at MVP stage)

## 4. Design Project Management
- **4.1 Save/Load Projects**: Multi-project per user; limited for free users
- **4.2 Project Sharing**: Make public; download/export as image or PDF; share link
- **4.3 Project Duplication**: From public designs; from personal history

## 5. E-Commerce Features
- **5.1 Cart & Checkout**: Add items (individually or from design); order summary
- **5.2 Payments**: **Stripe integration** (note: this document specifies Stripe, whereas the commercial proposal specifies Bitrix24 payment module — a discrepancy to reconcile); handle both catalog-item purchases and design-pack purchases
- **5.3 Subscription Management**: Stripe integration; update user role on payment

## 6. Admin Panel
- **6.1 Dashboard**: KPIs, recent projects, most-used items
- **6.2 Users Management**: View, assign roles, deactivate, ban
- **6.3 Catalog Management**: Product CRUD, import, price rules
- **6.4 Project Moderation**: Approve/feature public designs
- **6.5 Reports & Analytics**: Download usage reports; view room-planner interaction heatmaps (marked "future")

## 7. Analytics & Tracking
- **7.1 Internal Tracking**: Project creation, item usage, time in planner
- **7.2 External Analytics**: Google Analytics / Mixpanel integration
- **7.3 Admin Dashboards**: User activity logs; item popularity

## 8. System Services & Infrastructure
- **8.1 Cloud Storage** (e.g., Firebase, S3)
- **8.2 Email Service** (SendGrid/Mailgun)
- **8.3 AI Plan Recognition Engine**: Integration with ML service (custom or third-party)
- **8.4 Background Workers**: Asset conversion, thumbnail generation
- **8.5 DB Schema Design**: Users, roles, projects, plans, items, assets

## Cross-document discrepancies flagged for clarification
1. **Payment provider**: This document says Stripe; the commercial proposal and architecture diagram say Bitrix24 payment module (with possible external gateway integration). Which is authoritative?
2. **Admin/catalog platform**: This document's structure (Firebase/S3, SendGrid/Mailgun, generic admin panel) reads as a more generic/candidate architecture, while the commercial proposal firmly commits to Bitrix24 as "the main development framework for admin management, catalogs, and online shopping" with a custom backend integrating to it. Likely this document predates or is an alternative/earlier draft to the Bitrix24-centered proposal — needs confirmation of which is current.
3. **OAuth providers**: Google + Apple here vs. "Google" (and once "Google and iOS") in the commercial proposal — inconsistent.
