# Feature: User Accounts & Profiles

> **Status:** 🔄 In Progress — Phase 1
> **Owner:** TBD
> **Related service:** Core Service
> **API spec:** [`/api/users.yaml`](../api/users.yaml), [`/api/auth.yaml`](../api/auth.yaml)

---

## Overview

Users can register with email + password, verify their address, manage a public
profile, and reset their password. Authentication uses short-lived JWT access
tokens rotated with long-lived refresh tokens.

---

## User Stories

| ID | As a… | I want to… |
|----|-------|-----------|
| U-01 | Visitor | Register with email + password |
| U-02 | Registered user | Verify my email address |
| U-03 | Registered user | Log in and receive tokens |
| U-04 | Logged-in user | Refresh my access token silently |
| U-05 | Logged-in user | Log out |
| U-06 | Registered user | Request a password-reset email |
| U-07 | Logged-in user | View and edit my profile |
| U-08 | Any visitor | View another user's public profile |

---

## Business Rules

- Email addresses are lowercased and must be unique across all accounts.
- Passwords are hashed with bcrypt; minimum length 8 characters.
- `email_verified` must be `true` before a listing can be published.
- `SUSPENDED` accounts can log in but receive `403` — they cannot perform
  write actions.
- `DELETED` accounts are soft-deleted; email is retained (blocked for re-use).
- Refresh token rotation: every `/auth/refresh` call revokes the old token and
  issues a new pair. Detected reuse of a revoked token invalidates all tokens
  for the user (detect-and-revoke pattern).
- Password reset: all existing refresh tokens are revoked on success.
- Only `displayName`, `avatarUrl`, `location`, and `bio` are user-editable.
  Email and password changes are separate flows (not yet in Phase 1 scope).

---

## API Endpoints (Phase 1)

| Method | Path | Auth | Summary |
|--------|------|------|---------|
| `POST` | `/auth/register` | No | Create account + send verification email |
| `POST` | `/auth/login` | No | Issue access + refresh tokens |
| `POST` | `/auth/refresh` | No | Rotate token pair |
| `POST` | `/auth/logout` | Yes | Revoke refresh token |
| `POST` | `/auth/verify-email` | No | Confirm email with single-use token |
| `POST` | `/auth/password-reset/request` | No | Send reset email |
| `POST` | `/auth/password-reset/confirm` | No | Set new password via token |
| `GET` | `/users/me` | Yes | Get own full profile |
| `PATCH` | `/users/me` | Yes | Update own profile (partial) |
| `GET` | `/users/{userId}/profile` | Optional | Get public profile |

---

## Error Codes

| Code | Trigger |
|------|---------|
| `EMAIL_ALREADY_EXISTS` | Registration with a taken email |
| `ACCOUNT_SUSPENDED` | Login or action by suspended user |
| `TOKEN_INVALID` | Expired, used, or forged auth/verify token |
| `OPTIMISTIC_LOCK_CONFLICT` | Profile updated concurrently — re-fetch and retry |

---

## Out of Scope (Phase 1)

- Social login (Google, Apple) — Backlog
- Email change flow
- Phone number / KYC verification — Phase 3
- Two-factor authentication
