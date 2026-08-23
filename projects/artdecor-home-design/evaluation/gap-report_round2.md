# Gap Report — Evaluation Round 2 — Rumica (ArtDecor Home Design)

Round 1's 3 required edits were verified as correctly applied (FR/UC numbering consistent, payment-method deferral clean, observability correctly left untouched). Two new issues surfaced from the rework itself:

## Findings

**1. Gap / inconsistency — UC-8 and "Subscription upgrade" flow not updated with checkout-language fix**
- **Where:** Vision & Scope — UC-8 ("Subscribe to a paid tier") main scenario step 2 ("User completes payment via the Bitrix24 payment module"), and the "Subscription upgrade" alternative flow ("pays via Bitrix24").
- **Conflicts with:** UC-6's updated language (custom Rumica-built checkout form calling Bitrix24 Payment APIs directly) and the Architecture Specification's UC-8 sequence diagram, which already labels the front-end participant "Front End (custom checkout form)" and routes subscription payment through the same Commerce Bridge/tokenization mechanism as cart checkout.
- **Decision:** Keep as-is, but clarify the distinction — subscription payment intentionally uses a **different mechanism** (a Bitrix24-hosted flow) than product/cart checkout. This must be stated explicitly in both documents (Vision & Scope and Architecture Specification), not left ambiguous as it currently is.

**2. Self-contradiction — UC-10 (Favorites) status-precedence rule**
- **Where:** Vision & Scope UC-10 ("Bookmark and view favorites") — main scenario step 3 (checks "used-in-project" before "ordered") contradicts the UC-10 Assumptions note ("priority is ordered > used-in-project > pending when both conditions could apply"). The Architecture Specification's Data Flow addition doesn't state a precedence rule at all.
- **Decision:** "Ordered" takes priority — reword UC-10's main scenario so the check order matches (ordered checked/stated first), and add an explicit precedence sentence to the Architecture Specification's Data Flow so the derivation logic is unambiguous for implementers.

## Decision Summary
Both findings require document edits:
1. UC-8 + "Subscription upgrade" flow (Vision & Scope) need to explicitly state subscription payment uses a Bitrix24-hosted flow, distinct from cart/product checkout's custom form — and the Architecture Specification needs a corresponding note distinguishing the two payment paths (UC-8 sequence diagram and/or Commerce Bridge component description).
2. UC-10 main scenario (Vision & Scope) needs reordering to check "ordered" before "used-in-project"; Architecture Specification Data Flow needs an explicit precedence sentence.

This is evaluation cycle 2 of a maximum 2 automatic cycles. If this rework introduces further findings, the automatic loop stops and the user is shown the remaining gaps directly.
