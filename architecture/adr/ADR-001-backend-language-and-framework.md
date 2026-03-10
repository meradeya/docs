# ADR-001 — Backend Language and Framework

| Field        | Value              |
|--------------|--------------------|
| **Status**   | ✅ Accepted         |
| **Date**     | 2026-02-21         |
| **Deciders** | Meradeya core team |

---

## Context

Meradeya starts with a small number of broader services (Core Service covering users + listings, plus a separate
Payments Service). We needed a language and framework that offers:

- Strong ecosystem for HTTP APIs and background processing
- Mature libraries for security, persistence, and observability
- Good tooling for testing and CI
- Team familiarity — we want to ship, not learn a new stack

---

## Options Considered

### Option A — Java + Spring Boot

- ✅ We know it :)

---

## Decision

We will use **Java 21 + Spring Boot 4** for all backend services.

Java 21 virtual threads (Project Loom) give high concurrency without reactive complexity. Spring Boot 4 provides a
cohesive, production-ready foundation for REST, security, persistence, and observability. The team's existing
familiarity means we can ship faster and onboard contributors more easily.

---

## Consequences

### Positive

- Spring Security, Data JPA, and Actuator cover auth, persistence, and observability without custom plumbing
- Virtual threads give high concurrency without the complexity of reactive programming
- Large talent pool and abundant reference material

### Negative / Trade-offs

- More boilerplate than Kotlin or Go
- JVM memory overhead — mitigated by running persistent containers rather than scaling to zero

### Risks

- **Lock-in to Spring idioms** — mitigated by keeping domain logic framework-agnostic where practical
- **Java version drift** — mitigated by pinning to LTS (Java 21) and reviewing on each new LTS

---

## Notes / References

- [Spring Boot 4 release notes](https://spring.io/blog/2026/02/19/spring-boot-4-0-3-available-now)
- [Project Loom / Virtual Threads (JEP 444)](https://openjdk.org/jeps/444)
- [Spring Boot + Virtual Threads guide](https://spring.io/blog/2023/09/09/all-together-now-spring-boot-3-2-graalvm-native-images-java-21-and-virtual)
