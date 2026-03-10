# Architecture Decision Records

ADRs document significant architectural decisions: the context, options considered, and consequences of each choice.

---

## Index

| ADR                                                    | Title                                   | Status             |
|--------------------------------------------------------|-----------------------------------------|--------------------|
| [ADR-001](./ADR-001-backend-language-and-framework.md) | Backend language and framework          | ✅ Accepted         |
| [ADR-002](./ADR-002-database.md)                       | Database engine                         | ✅ Accepted         |
| [ADR-003](./ADR-003-messaging-broker.md)               | Messaging broker                        | 🔵 To be discussed |
| [ADR-004](./ADR-004-frontend-framework.md)             | Frontend framework                      | 🔵 To be discussed |
| [ADR-005](./ADR-005-api-style.md)                      | API style                               | 🔵 To be discussed |
| [ADR-006](./ADR-006-authentication-strategy.md)        | Authentication & authorisation strategy | 🔵 To be discussed |
| [ADR-007](./ADR-007-deployment-platform.md)            | Deployment platform & infrastructure    | 🔵 To be discussed |

**Status legend:**

- ✅ Accepted — decision is final; changes require a new ADR
- 🟡 Proposed — under active discussion in a PR
- 🔵 To be discussed — not yet started
- ⛔ Deprecated — superseded by a later ADR

---

## How to Write an ADR

1. Copy [`ADR-000-template.md`](./ADR-000-template.md) → `ADR-NNN-short-title.md`
2. Fill in all sections; status starts as **Proposed**
3. Open a PR — on merge, update this index and set status to **Accepted**

