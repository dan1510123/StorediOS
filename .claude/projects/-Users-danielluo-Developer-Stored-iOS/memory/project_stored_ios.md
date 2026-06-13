---
name: project-stored-ios
description: Key facts about the Stored iOS gift card vault app — tech stack, design tokens, scope, and build order
metadata:
  type: project
---

Solo nights/weekends iOS app, goal is App Store ship. Xcode project at `/Users/danielluo/Developer/Stored iOS/StorediOS/`.

**Why:** Portfolio piece + solving real need (forgotten gift card money).

**Stack:** SwiftUI iOS 17+, SwiftData + CloudKit sync, Vision (OCR), Core Image (barcodes), CryptoKit (field-level encryption), WidgetKit, StoreKit 2 (free ≤5 cards, $4.99 unlock).

**Data layer (completed 2026-06-13):** 6 `@Model` classes written to worktree branch `claude/focused-buck-acf7eb`:
- `Card` — central model; `cachedBalanceCents: Int` (cents); `encryptedCardNumber/PIN: Data?`
- `Merchant` — name, domain, brandColorHex
- `Barcode` — value + BarcodeFormat enum (code128/code39/qr/pdf417/aztec)
- `BalanceEvent` — event-log delta; `amountDeltaCents: Int`
- `Reminder` — triggerDate, isDelivered
- `ImageAsset` — filename (file stored in documents/card-images/)
- `CryptoService` — AES-GCM singleton, key in Keychain (`kSecAttrAccessibleAfterFirstUnlockThisDeviceOnly`)
- `StoredContainer` — factory with `.cloudKit(.automatic)` config; `make(inMemory:)` for tests

**Money representation:** All balances stored as `Int` cents (1050 = $10.50). No floating-point anywhere.

**Design tokens:** Canvas #F2F2F7, accent emerald #0B6E4F, card radius 18pt, SF Pro Rounded tabular numerals.

**V1 build order:** (1) Home screen card stack ← next, (2) OCR import, (3) Barcode view, (4) Reminders + widget, (5) Balance management + polish.

**Out of scope:** AI import, PassKit, family vaults, transaction ledger, Android, subscriptions, NFC.

**How to apply:** Check this before suggesting architectural changes — scope and design are locked. Balances are always cents (Int), never Double/Decimal.
