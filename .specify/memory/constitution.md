<!--
## Sync Impact Report
Version change: (none) → 1.0.0 — initial constitution creation
Added sections:
  - Core Principles (I–V)
  - Monetization & Entitlement Gates
  - Development Workflow
  - Governance
Modified principles: N/A (initial)
Removed sections: N/A (initial)
Templates reviewed:
  - .specify/templates/plan-template.md ✅ (Constitution Check uses dynamic placeholder; no update needed)
  - .specify/templates/spec-template.md ✅ (no constitution-specific references; no update needed)
  - .specify/templates/tasks-template.md ✅ (mobile path conventions align with StorediOS/ structure)
Deferred TODOs: none
-->

# Stored Constitution

## Core Principles

### I. Native-First (NON-NEGOTIABLE)

SwiftUI and SwiftData are the exclusive UI and persistence layer. CloudKit is the sole sync
backend. Third-party frameworks MUST NOT be added for networking, persistence, or animation.
UIKit is permitted only where SwiftUI has documented platform gaps (e.g., `AVCaptureSession`
for camera input). Any exception requires an explicit constitution amendment.

**Rationale**: Stored is a portfolio piece and App Store submission. Native-only keeps the
binary lean, guarantees forward compatibility, and demonstrates SwiftUI mastery.

### II. Privacy & Encryption by Default (NON-NEGOTIABLE)

Card numbers and PINs MUST be encrypted with CryptoKit before any write — to SwiftData or
CloudKit. No sensitive field is ever stored or transmitted as plaintext. Biometric (Face ID)
lock MUST guard vault access; bypasses are permitted only in explicit test-scheme builds and
MUST NOT ship in App Store builds.

**Rationale**: A gift card vault holds financial data. Plaintext PINs or card numbers
exposed via CloudKit inspection is a reputational and legal liability.

### III. Lean Data Model

The canonical entity set is: `Card`, `Merchant`, `Barcode`, `ImageAsset`, `Reminder`,
`BalanceEvent`. No new top-level entities MUST be added without a constitution amendment.

Balance MUST be stored as `BalanceEvent` deltas (event log); `current_balance` is a cached
derivation — it is NEVER the source of truth. Merchant grouping is a presentation-layer
query, not a SwiftData relationship.

**Rationale**: Event-sourced balance avoids drift bugs and keeps history auditable. Keeping
merchant grouping in the presentation layer prevents it from coupling into every card query.

### IV. UX Delighters Ship With Features

Camera OCR import, full-brightness barcode scan, count-up hero animation, merchant color
extraction, and contrast-aware text rendering are first-class deliverables — not post-launch
polish. A feature is NOT considered done until its designed UX behavior is implemented and
respects system accessibility settings (Reduce Motion, Dynamic Type, High Contrast).

**Rationale**: The app's differentiation is the feel. Shipping a feature without its
designed delighter is shipping half a feature.

### V. Hard V1 Scope Boundary (YAGNI)

The following are explicitly out of scope for V1 and MUST NOT be implemented:
AI/email import, PassKit (Apple Wallet) export, family/shared vaults, full transaction
ledger UI, automated balance checking, loyalty/transit cards, Android, web, subscriptions,
NFC, mag-stripe, or cross-card balance merging.

Scope additions require an explicit constitution amendment with rationale.

**Rationale**: Solo nights/weekends project. Scope creep is the #1 risk to shipping.
The out-of-scope list is a covenant, not a suggestion.

## Monetization & Entitlement Gates

StoreKit 2 governs access tiers: free tier allows up to 5 cards; the one-time $4.99
in-app purchase unlocks unlimited cards.

Every card-creation code path MUST check the current entitlement before proceeding. No
debug bypass, developer override, or backdoor MUST be present in App Store builds.
Entitlement behavior MUST be verifiable against the local StoreKit configuration file
and StoreKit 2 sandbox during development.

## Development Workflow

V1 ships in this order — each stage MUST be demonstrable before the next begins:

1. Card vault + home screen
2. Camera/OCR import (showpiece)
3. Full-brightness scannable barcode view
4. Expiration reminders + WidgetKit widget
5. Balance management + polish (search, Face ID, merchant detail)

A running build write-up MUST be maintained as features land. This write-up is a
deliverable alongside the app and is part of the portfolio artifact.

Source lives under `StorediOS/` (Xcode project). Design tokens — canvas `#F2F2F7`,
accent `#0B6E4F`, card radius 18pt, SF Pro Rounded tabular numerals — MUST be defined
as named constants and MUST NOT be hardcoded inline.

## Governance

This constitution supersedes all other practices, preferences, or prior conventions for
Stored. Any amendment must document: (a) the rationale, (b) impact on in-flight features,
and (c) the version bump type per the policy below.

**Version Policy**:
- MAJOR: Principle removal, redefinition, or backward-incompatible governance change.
- MINOR: New principle or section added, or materially expanded guidance.
- PATCH: Clarifications, wording, typo fixes, non-semantic refinements.

As a solo project, the author (Dan Luo, dan1510123@gmail.com) is the sole approver of
amendments. All implementation decisions MUST be verified against this constitution before
committing.

**Version**: 1.0.0 | **Ratified**: 2026-06-13 | **Last Amended**: 2026-06-13
