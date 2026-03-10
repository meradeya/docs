# ADR-002 — Database Engine

| Field        | Value              |
|--------------|--------------------|
| **Status**   | ✅ Accepted         |
| **Date**     | 2026-03-10         |
| **Deciders** | Meradeya core team |

---

## Context

Each backend service owns its own data store (database-per-service pattern). We needed a primary relational database
that:

- Handles ACID transactions reliably (critical for payments, orders, listings)
- Has strong Spring Data JPA / Hibernate support
- Supports semi-structured data (e.g. JSON attributes on listings)
- Has a rich ecosystem for migrations, tooling, and hosting

The choice applies to all services as a default. Services with special requirements (e.g. full-text search, time-series)
may adopt additional purpose-built stores alongside this primary DB.

---

## Options Considered

### Option A — PostgreSQL

- ✅ Rock-solid ACID compliance and mature replication support
- ✅ First-class JSONB support — useful for flexible listing attributes
- ✅ Excellent full-text search built in (usable for MVP before a dedicated search engine)
- ✅ Industry standard; vast ecosystem (pgAdmin, Flyway, cloud-managed: RDS, Cloud SQL, Supabase, Neon)
- ✅ Strong Spring Data JPA + Hibernate integration
- ❌ Vertical scaling only for writes; horizontal write scaling requires Citus or sharding

### Option B — MySQL / MariaDB

- ✅ Widely used; good managed hosting options
- ❌ Weaker JSON support compared to PostgreSQL JSONB
- ❌ Historically looser ACID semantics (though largely resolved in modern versions)
- ❌ Less expressive SQL (window functions, CTEs less mature historically)

---

## Decision

We will use **PostgreSQL** (latest stable, currently 18.3) as the primary database engine for all services.

PostgreSQL covers all our transactional requirements, has first-class JSON support for flexible data, and integrates
seamlessly with Spring Data JPA. Managed hosting options (AWS RDS, Neon, Supabase) make operations straightforward. If
write throughput becomes a bottleneck at scale, we revisit with a new ADR.

Schema migrations will be managed with **Flyway**.

---

## Consequences

### Positive

- Battle-tested reliability for financial and transactional data
- JSONB lets the Listing Service store category-specific attributes without an EAV schema
- PostgreSQL FTS available as a zero-infrastructure fallback for search in Phase 1 before a dedicated engine is
  introduced
- Flyway gives us auditable, version-controlled schema migrations

### Negative / Trade-offs

- Single write primary per service — not horizontally scalable for writes without sharding
- Each service manages its own schema independently; cross-service joins are not possible by design (requires events or
  API calls)

### Risks

- **Data growth** — mitigated by partitioning large tables (e.g. orders, events) and indexing reviews; monitor query
  plans from day one
- **Connection exhaustion** — mitigated by using a connection pool (PgBouncer or HikariCP, which Spring Boot configures
  by default)

---

## Notes / References

- [PostgreSQL 17 release notes](https://www.postgresql.org/docs/17/release-17.html)
- [Flyway documentation](https://documentation.red-gate.com/flyway)
- [Spring Data JPA + PostgreSQL](https://spring.io/guides/gs/accessing-data-jpa/)
