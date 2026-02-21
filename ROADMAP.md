# Meradea — Roadmap & Planning

> This document is a living planning artifact. It is intentionally **balanced between high-level intent and actionable
> tasks**. As the project matures, phases will be broken down further into GitHub Issues and Milestones.
>
> **Status legend:**
> 🔄 In Progress · 🗓 Planned · ✅ Done · ⏸ Blocked · 💬 Needs discussion

---

## Phase 0 — Foundation *(current)*

> Set the stage: align the team, make key decisions, scaffold the project.

### Documentation & Planning

- [x] Create `docs` repository
- [x] Write initial `README.md` with project vision and principles
- [x] Define high-level feature list
- [x] Sketch initial architecture diagram
- [ ] Write `CONTRIBUTING.md` guide
- [ ] Define branching and versioning strategy (e.g. trunk-based, GitFlow)

### Architecture Decisions (ADRs)

- [ ] ADR-001 — Backend language & framework (Java + Spring Boot)
- [ ] ADR-002 — Database engine
- [ ] ADR-003 — Messaging broker
- [ ] ADR-004 — Frontend framework
- [ ] ADR-005 — API style (REST vs GraphQL vs gRPC — or hybrid)
- [ ] ADR-006 — Authentication & authorisation strategy (OAuth2 / OIDC?)
- [ ] ADR-007 — Deployment platform & infrastructure approach

### Team Alignment

- [ ] Agree on code style guides and linting rules (per language/framework)
- [ ] Agree on repository structure (monorepo vs polyrepo)
- [ ] Agree on API design conventions (versioning, naming, error format)
- [ ] Agree on event schema format and registry strategy (Avro / JSON Schema / Protobuf?)
- [ ] Define Definition of Done (DoD) for features

---

## Phase 1 — Core Marketplace

> Users can register, create listings, and browse the catalogue. No payments yet.

### User Service

- [ ] User registration & login (email + password)
- [ ] JWT / session management
- [ ] Basic user profile (display name, avatar, location)
- [ ] Email verification flow
- [ ] Password reset flow

### Listing Service

- [ ] Create / edit / delete a listing (title, description, price, condition, photos)
- [ ] Listing states: `DRAFT` → `ACTIVE` → `PAUSED` → `SOLD` / `ARCHIVED`
- [ ] Category tree (admin-managed)
- [ ] Photo upload & storage (decide on object storage solution)

### Search & Discovery

- [ ] Basic full-text search over listings
- [ ] Filter by: category, price range, condition, location
- [ ] Sorting: newest, price asc/desc, relevance
- [ ] Decide and integrate search engine (e.g. Elasticsearch / OpenSearch / PostgreSQL FTS)

### API Gateway

- [ ] Set up API Gateway / reverse proxy
- [ ] Route traffic to User and Listing services
- [ ] Centralised authentication middleware
- [ ] Basic rate limiting

### Infrastructure & DevOps (baseline)

- [ ] Local development environment (Docker Compose with all services)
- [ ] CI pipeline: build, lint, test on every PR
- [ ] Basic deployment pipeline to a staging environment
- [ ] Centralised structured logging
- [ ] Health check endpoints on all services

---

## Phase 2 — Transactions, Messaging & Shipping

> Money moves. Buyers and sellers can communicate and complete deals.

### Payments Service

- [ ] Integrate payment provider (to be decided — Stripe, Adyen, etc.)
- [ ] Buyer checkout flow (card, digital wallet)
- [ ] Escrow hold on payment until delivery confirmed
- [ ] Seller payout flow (configurable schedule)
- [ ] Basic refund handling
- [ ] Webhook processing from payment provider

### Order / Transaction Service

- [ ] Order lifecycle: `PENDING` → `PAID` → `SHIPPED` → `DELIVERED` → `COMPLETED` / `DISPUTED`
- [ ] Link listings to orders; mark listing as sold on completion
- [ ] Kafka events: `order.created`, `order.paid`, `order.completed`

### Messaging Service

- [ ] In-app chat between buyer and seller per listing
- [ ] Real-time delivery (WebSocket or SSE)
- [ ] Message persistence & history

### Offer & Negotiation

- [ ] Buyer can make an offer on a listing
- [ ] Seller can accept, decline, or counter-offer
- [ ] Offer expiry and status lifecycle

### Shipping

- [ ] Seller inputs tracking number manually (MVP)
- [ ] Buyer confirms delivery
- [ ] Research carrier API integrations for Phase 4+

### Notifications Service

- [ ] Email notifications (order updates, messages, offers)
- [ ] In-app notification centre (bell icon)
- [ ] Notification preferences per user
- [ ] Kafka consumer for all relevant domain events

---

## Phase 3 — Trust, Safety & Community

> Build confidence in the platform; protect both sides of every transaction.

### Reviews & Ratings

- [ ] Buyer leaves review of seller after completed order
- [ ] Seller leaves review of buyer
- [ ] Public rating displayed on profiles
- [ ] Prevent review manipulation (one review per transaction)

### Dispute Resolution

- [ ] Buyer can open a dispute within a time window
- [ ] Dispute states: `OPEN` → `UNDER_REVIEW` → `RESOLVED`
- [ ] Admin moderation interface for disputes
- [ ] Automated escrow release / refund on resolution

### Trust & Verification

- [ ] Phone number verification
- [ ] Optional identity verification (KYC — provider TBD)
- [ ] Trust badges on profiles ("Verified seller", "Active since …")

### Moderation & Safety

- [ ] Listing reporting (prohibited items, scam, duplicate)
- [ ] User reporting and blocking
- [ ] Admin content moderation queue
- [ ] Basic automated content checks (prohibited words, image moderation API)

### Seller Profiles & Storefronts

- [ ] Public seller page with active listings, rating, reviews
- [ ] "Follow seller" functionality
- [ ] Seller response rate & average ship time displayed

---

## Phase 4 — Personalisation & Insights

> Make the platform smarter and give sellers the data they need.

### Recommendations

- [ ] "Similar listings" on listing detail page
- [ ] Personalised feed based on browsing/purchase history
- [ ] "Trending in your area / category" section

### Saved Searches & Alerts

- [ ] User can save a search query
- [ ] Notify user when a new listing matches saved search (email + in-app)

### Seller Analytics Dashboard

- [ ] Views, watchers, and click-through rate per listing
- [ ] Revenue summary and payout history
- [ ] Listing performance tips

### Admin Analytics

- [ ] Platform GMV, active users, listing volume (time-series)
- [ ] Funnel analysis (view → message → purchase)

### Search Improvements

- [ ] Typo tolerance & synonym support
- [ ] Promoted / boosted listings (for monetisation)
- [ ] Search query analytics to inform improvements

---

## Phase 5 — Scale, Internationalisation & Hardening

> Prepare the platform for growth, more markets, and production-grade reliability.

### Internationalisation (i18n)

- [ ] UI localisation framework (language selection, locale-aware formatting)
- [ ] Multi-currency pricing & display
- [ ] Country-specific category trees and shipping rules

### Carrier & Shipping Integrations

- [ ] Integrated shipping label generation (carrier TBD)
- [ ] Real-time tracking updates pulled from carriers
- [ ] Estimated delivery date display

### Performance & Reliability

- [ ] Load testing and capacity planning
- [ ] Caching strategy (Redis or similar — TBD)
- [ ] Database query optimisation and indexing review
- [ ] Circuit breakers and graceful degradation between services

### Security Hardening

- [ ] Full security audit
- [ ] Penetration testing
- [ ] GDPR compliance review (data export, right to erasure)
- [ ] Secrets management (Vault or cloud KMS — TBD)

### Observability

- [ ] Distributed tracing (OpenTelemetry)
- [ ] Metrics dashboards (Prometheus / Grafana or cloud-native — TBD)
- [ ] Alerting and on-call runbooks

---

## Backlog / Future Ideas

> Items not yet scheduled into a phase. Up for discussion.

- Mobile app (iOS / Android — native or cross-platform)
- Auction / bidding mode for listings
- Buyer protection insurance product
- Subscription / premium seller tier
- Seller bulk import (CSV / API)
- Integration with social login (Google, Apple, etc.)
- Embedded financing / BNPL option at checkout
- Loyalty / points programme
- Gift cards
- B2B / business seller accounts

---

*Last updated: February 2026. Owned by the Meradea core team — PRs welcome.*

