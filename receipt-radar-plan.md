# Receipt Radar — iOS App Plan

A privacy-first, on-device receipt scanner for freelancers and small-business
owners. Snap a receipt, get structured data, export clean reports at tax time.

---

## 1. Positioning

**Tagline:** *Your receipts, organized the moment you snap them.*

**Target users (in priority order):**
1. US freelancers / 1099 contractors (drivers, designers, consultants, trades)
2. Small-business owners with <5 employees
3. Frequent travelers who need expense reports
4. Anyone tracking warranties & returns

**Core promise:** Scan in 2 seconds, export to TurboTax/QuickBooks/Xero in 2 taps,
never send receipt images to a server.

---

## 2. Competitor gap (what we beat)

| Competitor | Price | Weakness we exploit |
|---|---|---|
| Expensify | $5–9/mo/user | Enterprise UI, cloud OCR, privacy concerns |
| Receipt Hog | Free + ads | Data-harvesting business model |
| Smart Receipts | $10 one-time | Dated UI, no iCloud sync, Android-first |
| Shoeboxed | $29+/mo | Subscription-only, cloud-dependent |
| QuickBooks Receipts | bundled | Only useful if you already pay for QuickBooks |

**Our wedge:** on-device Vision + Core ML OCR, one-time pay option, iOS-native
feel, zero account required.

---

## 3. MVP feature set (ship in 4 weeks)

### Must-have (v1.0)
- [ ] Camera capture with edge detection & auto-crop (VisionKit `VNDocumentCameraViewController`)
- [ ] On-device OCR for merchant, date, total, tax, last-4 of card (Vision `VNRecognizeTextRequest`)
- [ ] Line-item parsing (regex + heuristics, fallback: user taps to correct)
- [ ] 10 default categories (Meals, Travel, Supplies, Fuel, Lodging, Software, Utilities, Office, Other, Personal)
- [ ] Receipt list view with search and month filter
- [ ] CSV export (TurboTax / QuickBooks / Xero column presets)
- [ ] PDF export (grouped by month or category, with thumbnails)
- [ ] iCloud sync across user's own devices (CloudKit private database)
- [ ] Face ID / passcode lock on the app

### Should-have (v1.1)
- [ ] Warranty tracker — store purchase date + warranty length, notification 14 days before expiry
- [ ] Mileage log (manual entry + Siri shortcut)
- [ ] Apple Wallet-style swipe browsing of receipts
- [ ] Widgets: monthly total, deductible total, last 3 receipts
- [ ] Siri shortcuts: "Log a $X receipt at [merchant]"

### Nice-to-have (v1.2+)
- [ ] Split-with-friends mode (generate a request-for-payment link)
- [ ] Import from Gmail/Apple Mail (share-sheet for emailed receipts)
- [ ] Apple Watch quick-capture
- [ ] Multi-currency with daily FX rates
- [ ] Tax-year summary report ready for an accountant

### Deliberately out of scope
- Team/multi-user accounts (that's Expensify's market, not ours)
- Web app / Android
- Bank/card integration (regulatory & trust cost is too high for v1)

---

## 4. Tech stack

| Layer | Choice | Why |
|---|---|---|
| Language | Swift 5.10 + SwiftUI | Native, modern, fast to build |
| Min iOS | iOS 17 | VisionKit data scanners + Swift Observation |
| OCR | Vision framework | Free, fast, on-device, no API cost |
| Storage | SwiftData | Modern Core Data replacement |
| Sync | CloudKit private DB | Free up to 10GB/user, no backend needed |
| PDF | PDFKit | Native |
| Payments | StoreKit 2 | In-app purchase for Pro |
| Analytics | TelemetryDeck | Privacy-friendly, flat $10/mo |
| Crash | MetricKit + Sentry free tier | |
| CI | Xcode Cloud ($15/mo for 100 hrs) | Native, zero config |

**No backend server. No API keys. No per-user infrastructure cost.**

---

## 5. Monetization

Dual model — users pick at onboarding:

- **Pro monthly:** $2.99/mo
- **Pro yearly:** $19.99/yr (save 45%)
- **Pro lifetime:** $39.99 one-time

**Free tier limits:** 15 receipts/month, CSV export locked, no warranty tracking.

**Break-even math (solo dev):**
- Apple's 15% cut (Small Business Program) → net $17/yr per yearly sub
- 200 paying users = ~$3,400/yr → covers tools + a modest hobby income
- 2,000 paying users = ~$34,000/yr → serious side business

Compare: Expensify has >10M downloads. Capturing 0.02% of that niche is 2k users.

---

## 6. 4-week build plan

### Week 1 — Foundation
- Xcode project, SwiftUI shell, tab bar (Scan / Receipts / Reports / Settings)
- VisionKit document camera integrated; save raw image to app container
- SwiftData schema: `Receipt`, `LineItem`, `Category`, `Warranty`
- iCloud container configured, CloudKit sync working

### Week 2 — OCR & parsing
- Vision text recognition pipeline
- Merchant / date / total / tax regex extractors
- Line-item parser with confidence scores
- Manual-edit screen for corrections (taps highlight regions on the image)

### Week 3 — Reports & export
- Receipt list + detail views
- CSV exporter with TurboTax / QuickBooks / Xero presets
- PDF generator with thumbnails
- Category CRUD, search, month filter
- Face ID lock, onboarding flow

### Week 4 — Polish & submit
- StoreKit 2 paywall, Pro gating, restore purchases
- App icon, marketing screenshots (use Xcode previews + Rotato)
- Privacy manifest, App Privacy labels, Data Safety form
- TestFlight with 20 beta users from Reddit (r/freelance, r/smallbusiness)
- Submit to App Store

### Week 5+ (post-launch)
- Respond to reviews within 24h for first 30 days
- Ship warranty tracker, widgets, Siri shortcuts (v1.1)
- Launch on Product Hunt + IndieHackers after 2 weeks of stability

---

## 7. Risks & mitigations

| Risk | Mitigation |
|---|---|
| OCR accuracy on crumpled/faded receipts | Manual edit flow + show confidence; let users correct & it learns category per merchant |
| Apple rejection for subscription UX | Follow StoreKit guidelines precisely; include "Restore Purchases" + clear cancel instructions |
| Competitor (Expensify) undercuts pricing | We're not targeting their enterprise buyer; solo users don't switch back once set up |
| Low discoverability | ASO: target "receipt scanner", "mileage tracker", "tax receipts"; launch blog with tax-tip content |
| Scope creep into bank integration | Written-in-stone: v1 does not touch banks. Revisit in v2.0 if revenue justifies Plaid. |

---

## 8. Success metrics

**Month 1:** 1,000 downloads, 50 paying users, 4.5★ avg rating
**Month 3:** 5,000 downloads, 300 paying users, featured in "New Apps We Love"
**Month 6:** 20,000 downloads, 1,000 paying users, $1,500 MRR

If Month 3 numbers miss by >50%, reassess positioning or ASO, not product.

---

## 9. Immediate next steps

1. Register Apple Developer account ($99/yr) if not done
2. Reserve App Store name "Receipt Radar" (or alternatives: "Slipd", "Tally Receipt", "Stub")
3. Buy domain `receiptradar.app` for marketing page
4. Create Xcode project on the `main` branch of a new repo
5. Build Week 1 milestones

---

*Plan owner: TBD. Last updated: 2026-04-21.*
