# Meradeya

> An open marketplace for used, handmade, vintage, and unique items — built for convenience, trust, and great UX.

Meradeya lets individuals and small businesses **list**, **discover**, and **buy** items safely — think eBay or Vinted,
but simpler and developer-friendly from day one.

## Core Principles

| Principle             | Description                                                  |
|-----------------------|--------------------------------------------------------------|
| 🛡 **Trust & Safety** | Secure payments, buyer/seller protection, dispute resolution |
| 🎨 **Great UX**       | Clean, intuitive interfaces on desktop and mobile            |
| 🔒 **Privacy**        | GDPR-compliant by design                                     |
| 🔓 **Openness**       | Transparent development, documented APIs, community input    |

## Planned Features

See the [Roadmap](./ROADMAP.md) for phased delivery. Core areas:

- **Listings** — rich-media listings, lifecycle states (draft → active → sold), categories
- **Search** — full-text search and faceted filters (PostgreSQL FTS to start)
- **Payments** — card/wallet checkout, escrow, seller payouts
- **Trust** — ratings/reviews, dispute resolution, basic fraud prevention
- **Messaging** — buyer–seller chat, offer/counter-offer flow
- **Seller Profiles** — public storefronts, history, follow sellers

## Tech Stack

> Decisions and rationale are recorded as [ADRs](./architecture/adr).

| Layer        | Technology                | Status    |
|--------------|---------------------------|-----------|
| **Backend**  | Java 21 + Spring Boot 4   | ✅ Decided |
| **Database** | PostgreSQL                | ✅ Decided |
| **Frontend** | Modern JS framework (TBD) | 🗓 TBD    |

## Architecture

We start with a pragmatic **service-per-domain-group** approach — broader services that can be split later if and when
bottlenecks appear. No premature decomposition.

```
┌───────────────────────────────────┐
│            Clients                │
│    Web App · Mobile Browser       │
└─────────────────┬─────────────────┘
                  │ HTTPS
┌─────────────────▼─────────────────┐
│           Core Service            │
│   users · listings · search       │
└─────────────────┬─────────────────┘
                  │
       ┌──────────┴──────────┐
  ┌────▼────┐           ┌────▼──────┐
  │Payments │           │PostgreSQL │
  │  Svc    │           │           │
  └─────────┘           └───────────┘

Future: Notifications · Messaging · Analytics
```

Payments is a separate service from day one — financial isolation is non-negotiable. Everything else starts in Core and
is extracted only when there's a concrete reason.

See [`/architecture`](./architecture) for more detail and [ADRs](./architecture/adr) for decisions.

## Docs Structure

```
docs/
├── README.md              ← You are here
├── ROADMAP.md             ← Phased delivery plan
├── CONTRIBUTING.md        ← How to contribute
├── architecture/
│   ├── overview.md                ← System architecture overview
│   ├── database-schema.md         ← DB schema & ER diagram (Core Service)
│   └── adr/                       ← Architecture Decision Records
├── api/
│   └── openapi.yaml       ← API specification
└── features/
    └── *.md               ← Feature specs
```
