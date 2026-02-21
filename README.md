# Meradea — Documentation

> **Meradea** is an open, community-driven marketplace where anyone can buy, sell, or discover used, handmade, vintage,
> and unique items — with a focus on convenience, trust, and great user experience.

---

## Table of Contents

<!-- TOC -->
* [Meradea — Documentation](#meradea--documentation)
  * [Table of Contents](#table-of-contents)
  * [What is Meradea?](#what-is-meradea)
  * [Core Principles](#core-principles)
  * [Planned Features](#planned-features)
    * [🛒 Marketplace Core](#-marketplace-core)
    * [🔍 Discovery & Search](#-discovery--search)
    * [💳 Payments & Transactions](#-payments--transactions)
    * [🤝 Trust & Safety](#-trust--safety)
    * [💬 Communication](#-communication)
    * [👤 User Profiles](#-user-profiles)
    * [📦 Shipping & Logistics](#-shipping--logistics)
    * [📊 Analytics & Insights](#-analytics--insights)
    * [🔧 Administration](#-administration)
  * [Tech Stack](#tech-stack)
  * [Architecture Overview](#architecture-overview)
  * [Documentation Structure](#documentation-structure)
  * [Roadmap](#roadmap)
<!-- TOC -->

---

## What is Meradea?

Meradea is a full-featured web marketplace service — similar in spirit to eBay, Avito, and Vinted — designed to make it
easy for individuals and small businesses to:

- **List** items for sale (used goods, handmade crafts, vintage finds, collectibles, and more)
- **Discover** listings with powerful search, filtering, and recommendation tools
- **Transact** safely through integrated payments, escrow, and buyer/seller protection
- **Connect** with a community of buyers and sellers through reviews, messaging, and profiles

The platform aims to be **secure**, **scalable**, and **delightful to use** — on both web and mobile browsers.

---

## Core Principles

| Principle             | Description                                                                             |
|-----------------------|-----------------------------------------------------------------------------------------|
| 🛡 **Trust & Safety** | Robust fraud prevention, identity verification, secure payments, and dispute resolution |
| 🎨 **Great UX/UI**    | Clean, intuitive interfaces that work beautifully on desktop and mobile                 |
| ⚡ **Performance**     | Fast search, real-time notifications, and low-latency transactions                      |
| 🔒 **Privacy**        | User data handled with care; GDPR-compliant by design                                   |
| 🌍 **Inclusivity**    | Multi-language, multi-currency, and accessible to all users                             |
| 🔓 **Openness**       | Transparent development process, documented APIs, and community input                   |

---

## Planned Features

### 🛒 Marketplace Core

- Item listings with rich media (photos, video previews)
- Categories, tags, and custom attributes per category
- Draft, active, paused, and archived listing states
- Bulk listing management for power sellers

### 🔍 Discovery & Search

- Full-text search with typo tolerance
- Faceted filtering (price range, condition, location, category, etc.)
- Personalized recommendations based on browsing and purchase history
- Saved searches and instant alerts

### 💳 Payments & Transactions

- Integrated payment processing (cards, digital wallets)
- Escrow-based buyer protection
- Seller payouts with configurable schedules
- Multi-currency support

### 🤝 Trust & Safety

- User identity verification (email, phone, optional KYC)
- Seller and buyer ratings & reviews
- Dispute resolution center
- Automated fraud detection
- Reporting & moderation tools

### 💬 Communication

- In-app real-time messaging between buyers and sellers
- Offer and counter-offer negotiation flow
- Push and email notifications

### 👤 User Profiles

- Public seller storefronts
- Purchase and sale history
- Wishlist / saved items
- Following sellers

### 📦 Shipping & Logistics

- Shipping label generation (integrated carriers)
- Shipping cost estimation
- Tracking integration
- Local pickup option

### 📊 Analytics & Insights

- Seller dashboard (views, conversions, revenue)
- Platform-level analytics for admins
- A/B testing infrastructure

### 🔧 Administration

- Content moderation tools
- User management and banning
- Category and attribute management
- Fee and commission configuration

---

## Tech Stack

> Detailed decisions and rationale for each choice will be recorded as [ADRs](./architecture/adr).

| Layer                  | Technology                | Status             |
|------------------------|---------------------------|--------------------|
| **Backend**            | Java + Spring Boot        | ✅ Decided          |
| **Database**           | PostgreSQL                | 🔶 Likely          |
| **Messaging**          | Apache Kafka              | 🔶 Likely          |
| **Frontend**           | Modern JS framework (TBD) | 🗓 To be decided   |
| **Other dependencies** | TBD                       | 🗓 To be discussed |

> 🔶 *Likely* = strong preference within the team, not yet formally committed.  
> ✅ *Decided* = agreed upon; changes require an ADR.

---

## Architecture Overview

> Detailed architecture documents will live in [`/architecture`](./architecture).

Meradea is designed as a **distributed system** composed of loosely-coupled services that communicate both
synchronously (REST/gRPC over an API gateway) and **asynchronously via a message broker** (most likely Apache Kafka).
This enables independent scaling, resilience, and clear domain boundaries between services.

```
┌──────────────────────────────────────────────────┐
│                    Clients                       │
│          Web App  ·  Mobile Browser              │
└────────────────────┬─────────────────────────────┘
                     │ HTTPS
┌────────────────────▼─────────────────────────────┐
│               API Gateway / BFF                  │
└──┬──────────┬───────────┬───────────┬────────────┘
   │          │           │           │   (synchronous)
┌──▼──┐  ┌───▼───┐  ┌────▼───┐  ┌───▼──────┐
│Users│  │Listing│  │Payments│  │Messaging │  ...
│ Svc │  │  Svc  │  │  Svc   │  │   Svc    │
└──┬──┘  └───┬───┘  └────┬───┘  └───┬──────┘
   │          │           │           │   (async events)
┌──▼──────────▼───────────▼───────────▼──────────┐
│                  Kafka Cluster                  │
│   user-events · listing-events · order-events  │
└─────────────────────────────────────────────────┘
   │          │           │           │
┌──▼──┐  ┌───▼────┐  ┌───▼────┐  ┌──▼──────────┐
│Notif│  │Search  │  │Analyt- │  │  Other       │
│ Svc │  │Indexer │  │ics Svc │  │  Consumers   │
└─────┘  └────────┘  └────────┘  └─────────────┘

Each service: Java · Spring Boot · PostgreSQL (own schema)
```

Technology decisions will be documented and discussed in [Architecture Decision Records (ADRs)](./architecture/adr).

---

## Documentation Structure

```
docs/
├── README.md                  ← You are here
├── architecture/
│   ├── overview.md            ← System architecture overview
│   └── adr/                   ← Architecture Decision Records
├── api/
│   └── openapi.yaml           ← API specification
├── features/
│   └── *.md                   ← Feature specs & user stories
├── design/
│   └── *.md                   ← UX/UI guidelines & wireframes
├── security/
│   └── *.md                   ← Security model & policies
├── runbooks/
│   └── *.md                   ← Operational guides
└── contributing.md            ← How to contribute to this project
```

> 📌 This structure will grow as the project evolves. Feel free to propose additions via a pull request.

---

## Roadmap

| Phase       | Focus                                              | Status         |
|-------------|----------------------------------------------------|----------------|
| **Phase 0** | Project setup, documentation scaffolding, ADRs     | 🔄 In Progress |
| **Phase 1** | Core marketplace (listings, search, user accounts) | 🗓 Planned     |
| **Phase 2** | Payments, shipping, messaging                      | 🗓 Planned     |
| **Phase 3** | Trust & safety, reviews, dispute resolution        | 🗓 Planned     |
| **Phase 4** | Recommendations, seller analytics, personalisation | 🗓 Planned     |
| **Phase 5** | Internationalisation, multi-currency, scale        | 🗓 Planned     |

For the full phase-by-phase task breakdown see **[ROADMAP.md](./ROADMAP.md)**.

