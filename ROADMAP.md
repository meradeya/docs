# Meradeya — Roadmap

> Living document. Broken down into GitHub Issues and Milestones as the project matures.
>
> **Status:** 🔄 In Progress · 🗓 Planned · ✅ Done · ⏸ Blocked · 💬 Needs discussion

---

## Phase 0 — Foundation *(current)*

> Align the team, make key decisions, scaffold the project.

### Documentation & Planning

- [x] Create `docs` repository
- [x] Write initial `README.md` with project vision
- [x] Sketch initial architecture
- [ ] Write `CONTRIBUTING.md`
- [ ] Define branching strategy (trunk-based vs GitFlow)

### Architecture Decisions (ADRs)

- [x] ADR-001 — Backend: Java 21 + Spring Boot 4
- [x] ADR-002 — Database: PostgreSQL + Flyway
- [ ] ADR-003 — Messaging broker (if/when needed)
- [ ] ADR-004 — Frontend framework
- [ ] ADR-005 — API style
- [ ] ADR-006 — Authentication strategy
- [ ] ADR-007 — Deployment platform

### Team Alignment

- [ ] Code style guides and linting rules
- [ ] Repository structure (monorepo vs polyrepo)
- [ ] API design conventions (versioning, error format)
- [ ] Definition of Done (DoD)

---

## Phase 1 — Core Marketplace

> Users can register, post listings, and browse the catalogue. No payments yet.

### Core Service *(users + listings)*

- [ ] User registration & login (email + password)
- [ ] JWT-based auth with refresh tokens
- [ ] Basic profile (display name, avatar, location)
- [ ] Email verification & password reset
- [ ] Create / edit / delete listings (title, description, price, condition, photos)
- [ ] Listing states: `DRAFT` → `ACTIVE` → `PAUSED` → `SOLD` / `ARCHIVED`
- [ ] Category tree (admin-managed)
- [ ] Photo upload & storage (object storage TBD)

### Search

- [ ] Full-text search over listings via PostgreSQL FTS
- [ ] Filter by: category, price range, condition, location
- [ ] Sorting: newest, price asc/desc, relevance

### Infrastructure

- [ ] Local dev environment (Docker Compose)
- [ ] CI pipeline: build, lint, test on every PR
- [ ] Centralised structured logging
- [ ] Health check endpoints

---

## Phase 2 — Transactions & Communication

> Money moves. Buyers and sellers can communicate and complete deals.

### Payments Service *(separate service — financial isolation)*

- [ ] Integrate payment provider (Stripe or Adyen — TBD)
- [ ] Buyer checkout (card, digital wallet)
- [ ] Escrow hold until delivery confirmed
- [ ] Seller payout (configurable schedule)
- [ ] Basic refunds & webhook handling

### Orders *(in Core Service)*

- [ ] Order lifecycle: `PENDING` → `PAID` → `SHIPPED` → `DELIVERED` → `COMPLETED` / `DISPUTED`
- [ ] Mark listing as sold on order completion

### Messaging *(in Core Service)*

- [ ] In-app chat between buyer and seller per listing
- [ ] Offer / counter-offer flow with expiry
- [ ] Seller inputs tracking number; buyer confirms delivery

### Notifications *(in Core Service, basic)*

- [ ] Email notifications for order updates, messages, offers
- [ ] In-app notification badge

---

## Phase 3 — Trust & Safety

> Build confidence in the platform on both sides of every transaction.

### Reviews & Ratings

- [ ] Buyer reviews seller (and vice versa) after completed order
- [ ] Public rating on profiles; one review per transaction

### Disputes

- [ ] Buyer can open a dispute within a time window
- [ ] Dispute states: `OPEN` → `UNDER_REVIEW` → `RESOLVED`
- [ ] Admin interface for moderation; automated escrow release on resolution

### Moderation

- [ ] Listing and user reporting
- [ ] Admin moderation queue
- [ ] Phone number verification; optional KYC (provider TBD)

---

## Phase 4 — Polish & Growth

> Make the platform smarter; give sellers actionable data.

- [ ] Seller analytics dashboard (views, CTR, revenue, payout history)
- [ ] Saved searches with new-listing alerts
- [ ] Typo tolerance & synonym support in search
- [ ] Personalised feed (browsing/purchase history)
- [ ] Distributed tracing (OpenTelemetry), metrics (Prometheus/Grafana)
- [ ] GDPR compliance review (data export, right to erasure)
- [ ] Load testing and caching strategy (Redis — TBD)

---

## Backlog / Future Ideas

- Mobile app (native or cross-platform)
- Social login (Google, Apple)
- Auction / bidding mode
- Shipping label generation (carrier integrations)
- Multi-currency & i18n
- Premium seller tier / promoted listings

---

*Last updated: March 2026. Owned by the Meradeya core team — PRs welcome.*
