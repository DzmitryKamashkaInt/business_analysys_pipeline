# Source: "Comparative analysis of open-source vs custom implementation approach of the 3D floor planner engine"

Considered approaches:
1. **Smart Cursor Engine** (Open Source)
2. **Custom Planner** based on Three.js (or Babylon.js / native canvas engine)

## Evaluation Dimensions

| Criteria | Smart Cursor (Open Source) | Custom Planner (Three.js) |
|---|---|---|
| Cost of Implementation | Medium — reuses existing base engine, quicker setup. BUT non-maintained legacy stack, low scalability/flexibility, needs updates to align with proper design. Best for quick PoC with minimal UX and simple design. | High — full architecture, UI logic, behavior built from scratch. BUT easily updated, high scalability, adapts to new formats/features. Best for pixel-perfect design + product-oriented approach with plans for scaling. |
| Customization Level | Low — can tweak core behavior but bound to engine structure | High — UX, object logic, rendering pipeline are fully owned |
| MVP Fit | Focused on 2D editing, fast time to market | Can match, but requires splitting into iterations: Basic 2D → Advanced 2D (more features) → Basic 3D → Advanced 3D |
| 2D Editing Capabilities | Already supports walls, doors, windows; limited room for functionality improvement and available formats | Requires full development (walls, snapping, resizing) |
| Future 3D Support | Low — limited roadmap visibility | High — ready for future 3D upgrade; Three.js built for it |
| Community & Maintenance | Low — small community, uncertain maintenance | High — large ecosystem, ongoing support, community plugins |
| Integration Complexity | Medium — standalone embeddable engine; in some cases integration can take extremely large effort | Medium — requires tight integration with catalog, DB, permissions, built from scratch |
| Branding & UX Control | Low — needs UI reskinning; some default behaviors baked in; pixel-perfect design customization can be hard | High — complete brand alignment, pixel-level control |

**Conclusion implied by the document**: the custom Three.js planner was the direction ultimately chosen for the ArtDecor/Rumica proposal (matches the proposal's stated "interactive 2D planner powered by Three.js"), trading higher upfront build cost for long-term scalability, branding control, and 3D upgrade path — consistent with ArtDecor's stated Post-MVP roadmap (3D visualization, AI features).
