# Stored — Project Plan

iOS gift card vault. "Where your forgotten money lives." Solo nights/weekends project, goal is App Store ship + portfolio piece, not revenue.

## Tech Stack
- Swift / SwiftUI, iOS 17+
- SwiftData + CloudKit sync
- Vision framework (OCR import)
- Core Image (barcode/QR generation)
- CryptoKit (field-level encryption for card numbers/PINs)
- WidgetKit (total-value widget), App Intents (Siri/Shortcuts), local notifications
- StoreKit 2 (free up to 5 cards, $4.99 one-time unlock)

## Data Model Principles
- `Card`, `Merchant`, `Barcode`, `ImageAsset`, `Reminder`, `BalanceEvent` as separate models
- Balances are an event log (`BalanceEvent.amount_delta`); `current_balance` is a cached derivation
- Merchant grouping is a presentation-layer query, not a model relationship

## V1 Build Order

### 1. Card vault + home screen
- Vertical overlapping card stack (~68pt peek)
- Hero total with count-up animation (once on launch, respects Reduced Motion)
- Card layout: wordmark top-left, balance top-right, last-4 bottom-left, expiration bottom-right
- Merchant color dataset (~100 US merchants) + deterministic HSL fallback
- Contrast logic for light-colored cards (dark text)
- Empty state copy ("average household has $187 in unused gift cards")

### 2. Camera/OCR import (showpiece)
- Vision framework text recognition
- One funnel, two doors: scan and manual entry converge on same confirm form
- Live card preview builds as user types
- Balance is the only required field
- "Matched X" chip = OCR correction UI
- "No barcode? Save as physical card" path

### 3. Full-brightness scannable barcode view
- Support code128, code39, qr, pdf417, aztec via Core Image
- numberOnly and physicalOnly card states (no scan view; show photo + "swipe physical card")

### 4. Expiration reminders + widget
- Local notifications for expiring cards
- WidgetKit total-value widget

### 5. Balance management + polish
- Two-tap balance decrement ("Mark amount spent")
- Search
- Face ID lock
- Merchant detail screen: horizontal card carousel (TabView .page), "Use first" badge on soonest-expiring card

## Explicitly Out of Scope
AI/email import, Apple Wallet (PassKit) export, family/shared vaults, transaction ledger, auto balance checking, loyalty/transit cards, Android/web, subscriptions, NFC, mag-stripe, merging balances across cards.

## Design Tokens
- Canvas: #F2F2F7
- Accent (interactive only): emerald #0B6E4F
- Card radius: 18pt
- Money figures: SF Pro Rounded, tabular numerals

## Documentation
Keep a running build write-up as features land — this is a deliverable alongside the app.
