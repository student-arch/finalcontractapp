# Audit — Vegetable Contractor prototype vs. SCREENS_MASTER_LIST.md

**Artifact:** `index.html` · 363 KB · 42 screen sections · ~3.5k lines JS
**Spec:** `SCREENS_MASTER_LIST.md` (SRS v2.9) · 41 screens · 22 sheets · 14 dialogs
**Date:** 2026-10-26
**Method:** structural diff of screen IDs, overlay registry, formatter/engine functions, and destructive-action guards; JS passes `node --check`.

---

## 1. Verdict

Screen coverage is **complete** (41/41, plus one extra `More` hub). The data model and
calculation engine are genuinely implemented, not mocked: FEFO reservation, revision
snapshots (R-suffix), frozen tender-rate snapshots, blended inventory cost, correction
window, photo evidence, backup/restore, CSV export.

What's missing is narrowly scoped — **4 overlay/dialog behaviours** and **5 mislabelled
overlay titles**. Nothing structural.

| Layer | Spec | Present | Status |
|---|---|---|---|
| Full screens (S01–S42) | 41 | 41 + More hub | ✅ complete |
| Bottom sheets (BS-01–22) | 22 | 22 (some as full modals) | ✅ functional |
| Dialogs (DL-01–14) | 14 | 12 fully, 2 partial | ⚠️ 4 gaps |
| Global components (G1–G10) | 10 | G1 nav, G2 cart badge, G3 keypad, G5 attention, G6 toast | ✅ |
| Workflows (WF-01–11) | 11 | 11 reachable | ✅ |
| Universal elements (U1–U13) | 13 × 41 | header/back/nav consistent | ✅ |

---

## 2. Gaps — must fix

### P1 · DL-02 — Delete expense has no confirmation
`index.html:3977` — `Remove Expense` fires `removeExpense()` immediately (`:3983`).
Spec §15.3 requires a confirm before destroying an expense row.
**Fix:** route through a confirm modal, pattern-matching `discardCartAsk()`.

### P1 · DL-08 — Duplicate item in cart is never offered a choice
`addToCart()` (`:3392`) appends a line unconditionally.
Spec §15.2 requires: *"Edit existing entry" / "Add a separate purchase line"* when the
item is already in today's cart — and S12 must then show combined qty + weighted-avg rate.
**Fix:** detect existing line, branch to a 2-option modal.

### P2 · DL-03 — Tender price removal is unconfirmed
`index.html:4708` — `Remove Price` splices immediately from the modal footer.
Spec: confirm, noting historical transactions are unaffected.
Whole-tender delete (`:4662`) is likewise immediate.

### P2 · DL-11 — Inventory quick-action menu doesn't exist
Actions render as flat buttons on S25 lot detail (`:4992`). Spec describes a routing
menu (Use in Supply · Sell · Mark Waste). Functionally equivalent, visually a divergence.

---

## 3. Mislabelled overlay titles

Cosmetic, but they make the build hard to trace against the register.

| Location | Shows | Should be |
|---|---|---|
| `Edit Cart Line (BS-01)` | BS-01 | **BS-16** (BS-01 is Inventory Availability Choice) |
| `Record Settlement (BS-10)` | BS-10 | **BS-13** (BS-10 is Prepare Period Bill) |
| `Prepare Period Bill (BS-11)` | BS-11 | **BS-10** |
| `Post-Lock Adjustment (BS-11)` | BS-11 | **BS-11** ✅ correct |
| `Deduction (BS-12)` / `Add Deduction (BS-12)` | BS-12 | **BS-09** (BS-12 is memo/share) |
| `Add Other-Day Entry (BS-22)` | BS-22 | no BS ID — WF-09 / §10.1 |

---

## 4. Acceptable divergences (no action)

- **DL-12 generic selectors** → native `<select>` instead of sheet pick-lists. Works, and
  all sets are editable (no free text), per §2.11.2.
- **S22 receivables** → standalone screen rather than embedded in S19 profile. Both reachable.
- **Extra `screen-more`** → hub for S17/S23/S27/S31/S28/S32/S34; matches §3.1 More tree.
- **English-only** → correct; S03 retired in v2.7.
- **No AI/insight features** → correct; §21 explicit non-scope.

---

## 6. Fix log — applied 2026-10-26

All P1/P2 gaps closed in `index.html`; verified by a headless Chromium CDP suite
(11 interaction assertions across the four new flows, plus a 42-screen navigation
sweep from both cold and populated state). **0 runtime errors.**

| Item | Change |
|---|---|
| DL-08 | `addToCart()` now detects an existing line for the item and opens a choice sheet: *Add a separate purchase line* / *Edit existing entry* → routes to BS-16. Reservation stays side-effect-free because FEFO is derived from cart lines only. |
| DL-02 | `removeExpense()` now asks first (Cancel / Delete) and closes the parent editor sheet before prompting. |
| DL-03 | New `deleteTenderPriceAsk()` — confirms, and states that historical transactions keep their frozen rate. |
| DL-11 | S25 flat action row replaced with a single **Quick Actions** button opening the routing menu (Use in Supply · Sell · Mark Waste). |
| Crash **B-01** | `renderTenderPrices()` threw whenever no institution/tender was in context (`inst.tenders.find` on null). Now guards and shows an empty state. Found by the screen sweep — it was reachable from `More → …` on a fresh install. |
| Labels | BS-01→BS-16, settlement BS-10→BS-13, period bill BS-11→BS-10, deductions BS-12→BS-09, other-day entry BS-22→none. |

### Remaining known divergences
- DL-11 is offered from S25 only, not also from the S24 card (spec lists both).
- DL-12 selector sheets remain native `<select>` elements.
- Whole-tender delete (inside *Edit Tender*) is still immediate — spec's DL-03 covers price rows only.


## 6. Engine spot-checks (all correct)

| Formula | Site | Check |
|---|---|---|
| Line cost = purchase + inventory | `:3004` | ✅ |
| Blended inventory rate | `:3377` | ✅ qty-weighted |
| P&L aggregation | `:5488` | ✅ supply/revenue/cost/profit |
| Overall totals | `:5352` | ✅ |
| Dues FIFO + deductions | `savePaymentWithCheck` | ✅ |
| Overpayment → credit balance | `Overpayment intent step (G-1)` | ✅ |
| Revision R-suffix | `nextRevNumber()` `:4100` | ✅ |
| FEFO reservation | `reserveFEFO` | ✅ |

No `TODO`/`FIXME`/lorem-ipsum. JS parses clean.

---

## 7. Design pipeline pass — 2026-10-26

Commissioned an extension run through OpenDesign (OpenCode local agent; Cloud was
unavailable). Scope was constrained to visual/motion polish with the engine frozen.

First attempt returned in 30 s with **0 artifacts** — it resumed a stale agent
session that believed an unrelated task was already complete. Re-issued with an
explicit fresh-start brief plus a definition of done; that run worked properly.

**Result:** 54 changed lines, **all CSS. Zero JS diff.**

| Change | Effect |
|---|---|
| `@keyframes screen-in` + `animation` on `.screen.active` | Screens fade/rise in on navigate |
| `--elev-glass-lit` inset highlight | Glass cards get a lit top edge |
| `padding-block-end: … env(safe-area-inset-bottom)` | Sticky footers clear the home indicator |
| `.overlay-close` → 40px circle, hover state | Proper touch target on sheet close |
| `.text-xs`, `.text-2xl` utilities | Type-scale gaps filled |
| `@media (prefers-reduced-motion: reduce)` | Motion respects OS setting |
| `--text-base` clamp | Slightly larger body text at narrow widths |

**Verification after the run:** 42/42 screens present, JS diff 0 lines, all 21
interaction assertions pass, 42-screen sweep clean from cold and populated state,
0 console errors.

Backup of the pre-run file: `index.pre-pipeline-20261026-0452.html`.

---

## 8. Item photos + Allocate row list — 2026-10-26

**Allocate per Item (S13)** now opens on a card listing every vegetable in today's cart
as a row: 48 px photo, name, `allocated / total`, a progress bar, and an
Open / Partial / Done badge. The selected row is highlighted; tapping a row switches the
allocation target (keyboard Enter/Space works too). This replaces the small text-only
chip row that showed one item at a time.

**Item photos.** Several seeded photos were wrong — `Cilantro.jpg` was a rabbit,
`Sugar.jpg` was Sugarloaf Mountain, `Oil.jpg` was a pumpjack, `Potato.jpg` was an art
photograph, `White rice.jpg` was dry noodles. All 16 catalogue items now carry a
verified 256 px photo, embedded as a data URI in `ITEM_IMAGE_DATA`.

| Item | Was | Now |
|---|---|---|
| Potato | art photo | pile of potatoes |
| Coriander | a rabbit | coriander leaves |
| Sugar | Sugarloaf Mountain | white + brown sugar |
| Oil | an oil pumpjack | olive oil bottle |
| Rice | dry noodles | rice grains |
| Beans | a baked-bean dish | green beans |
| Spinach | a field | spinach leaves |

Embedding means the prototype no longer depends on Wikimedia at runtime — the remote
paths were also rate-limiting (429) under load, which is what surfaced the problem.
Cost: file grew 378 KB → 655 KB.

`ensureItemImages()` remaps any older saved state that still points at a remote Commons
URL, and new custom items get a photo automatically when their name matches a catalogue
vegetable.

**Verification:** 9 new assertions (row count, per-row photo, default selection,
Partial badge + bar width, row-tap switching, reload persistence, custom-item photo,
42-screen sweep) plus the three earlier suites re-run — all green, 0 console errors.



---

## 9. Production hardening — 2026-10-29

Full behavioural pass. The prototype was audited as an *application*, not a visual
mock: every screen, form, calculation and workflow was exercised in headless
Chromium and the defects below were fixed in place. **118 assertions across six
suites, 0 console errors, 44/44 screens clean at 320/360/390/430/768/1024.**

### 9.1 Blocking defects found and fixed

| # | Defect | Effect before | Fix |
|---|---|---|---|
| 1 | `TODAY` hardcoded to `2026-10-21` | Every "today", aging figure, correction window and alert was wrong on any other calendar day | Derived from the device clock; seed data generated relative to it (yesterday's supply, month-scoped tenders) |
| 2 | Dates built with `toISOString()` | At UTC+5:30 local midnight formats as the previous UTC day, shifting every date by one | Local-date `toISO()` formatter used everywhere |
| 3 | `₹₹40/kg` on 18 surfaces | `formatINR()` already prefixes ₹; callers added another | All duplicate symbols removed (guarded by a test) |
| 4 | Tapping any transaction in History bounced to History | `openTrx()` navigated first, so the renderer read a null id and redirected | Id is set before `navigate()`; renderer falls back to the newest transaction |
| 5 | Payment recorded once **per bill** | Dues and "collected" double-counted after any FIFO payment | One receipt + `allocations[]`; `billPaid()` / `billPending()` are the only source of truth |
| 6 | "Memo / Bill" silently created a bill for institution #1 | Sharing a document moved money and changed dues | Memos are documents on `trx.memos`; per-institution picker; outdated-memo warning reads the memo |
| 7 | Deductions hit the institution tab only | Overall / daily / weekly / monthly P&L disagreed with each other | `allDeductions()` + `deductionsOn()` / `deductionsInRange()` feed every view |
| 8 | P&L headline metrics hardcoded (`₹5,00,000`) | Screen contradicted its own tabs | Computed; "Deductions" is now a first-class row |
| 9 | Empty cart showed totals card and "Continue → Allocate" | `svgHide('.cart-totals-card')` targeted a class absent from the markup — dead button on an empty cart | Class added; totals/CTA hide when empty |
| 10 | Tender with no price array threw on "+ Add Item Price" | `inst.fixedPrices[tenderId]` was `undefined` → crash on a valid flow | Array created on demand; missing-tender case toasts instead of throwing |
| 11 | Day-entry type picker stacked a second modal | Sale/Waste form opened underneath the picker | `dayEntryFlow()` closes the picker first |
| 12 | `#btnTrxPhoto` had two click handlers | Locked transactions opened the photo modal *and* the adjustment modal | Static duplicate binding removed; renderer owns the button |
| 13 | "Delete All Data" restored the demo dataset | Wipe left you with the seed transaction and institutions | `emptyState()` keeps catalogue + settings, drops every record |
| 14 | `app.state` captured once at export | Restore / import / delete left the exposed handle pointing at a detached object | Live getter |
| 15 | "Partially Used" inventory filter could never match | It filtered `status === 'Sold'`, which only happens at qty 0 | Matches lots with a negative movement and stock remaining |
| 16 | "Export PDF/CSV" copy | No PDF export exists | Copy says CSV |

### 9.2 Logic and state fixes

- **Over-allocation** is clamped to the item's remaining supply, with a live toast; the input is rewritten to the clamped value.
- **Editing a cart line** can no longer strand allocations: new supply must cover allocated + remaining, and the rate must be positive.
- **Removing the last line** clears that item's allocations/remaining/rejections, and an emptied cart clears expenses and correction mode.
- **Discard** now also clears `rejected`.
- **`undoRemaining()` / `autoSplit()`** persist (they mutated without `saveState()`).
- **Correction mode** preserves each allocation's `tenderId`, so the tender picker and contract P&L survive a re-open.
- **`syncPayablesForTrx()`** had inverted arithmetic (`amount − settled` computed as `reduce(amount, −x)`) and zeroed `remaining` before writing the settlement that explained it. Both fixed.
- **`finalizeCart()`** is defensive: it re-validates and refuses to commit a cart with unallocated stock or a missing rate, regardless of how it was reached.
- **`quickAdd()`** routes duplicates through the same choice sheet as the detail screen (it previously appended silently).
- **Shelf-life policy** writes `settings.shelfLifeDefault` instead of overwriting `overdueAlertDays`.
- **Bills** clamp `net` at 0 and push the surplus deduction back to pending — never a negative bill.
- **Quantities never sum across units**: `qtyByUnit()` renders "100 kg · 20 litre" instead of a fake total (cart, home, inventory, institution and P&L rows).

### 9.3 Validation added

| Surface | Rule |
|---|---|
| Business profile | Name required (was silently defaulting to "My Vegetable Business") |
| Item create/edit | Trimmed, case-insensitive duplicate rejected, purchase-unit factor > 0 |
| Institution create/edit | Trimmed, duplicate name rejected |
| Tender create/edit | Name required; end date on or after start |
| Cart line | Quantity > 0; rate > 0; supply ≥ allocated + remaining |
| Allocation | Clamped to remaining supply |
| Deduction | Cannot exceed open-bill pending (or accrued when unbilled) |
| Day entries | No future dates (sale, waste, settlement, voucher, deduction) |
| Sale / waste | Quantity ≤ lot; empty-lot state explains where stock comes from |
| Expense | Amount > 0 (delete asks first) |

### 9.4 UI/UX and responsive fixes

- `.screen` had no top offset, so every heading sat under the fixed 56px app bar. One `--screen-top-offset` now clears it, including `env(safe-area-inset-top)`.
- `.metric-grid` had **no grid** below 640px — the four Today metrics rendered as bare text. It is now a 2-up card grid on phones, 4-up from 640px.
- `.btn-sticky` double-counted the safe area and reserved a fixed 136px. Reserve is now derived from the real bar height and row count, so 3- and 4-button bars (transaction detail) wrap to two rows without clipping or truncation.
- Sticky bars no longer truncate labels ("Re-open & Correct", "Memo / Bill", "Attach Photo" are full width).
- Allocation sub-rows lost their fixed `150px` indent and fixed-width inputs; `#itemGrid` quick-add wraps at 360px. **Zero horizontal overflow on all 44 screens at 320–1024px.**
- Tap targets: app-bar buttons, modal close, inline row actions and inventory `⋯` are ≥44px. Checkbox/radio *labels* are 44px tall while the painted control stays 20px.
- Modals: body scrolls, footer is sticky, `100dvh` cap, safe-area aware. `Escape` dismisses the topmost overlay.
- Closed overlays are `visibility: hidden` — they were still in the tab order and the accessibility tree.
- Focus ring extended to `.auth-link`, `.bottom-nav a`, `.alloc-veg-row`, `.list-row[onclick]`; `.item-card:active` had an invalid shadow token.
- Disabled buttons no longer lift on hover; reduced-motion also neutralises overlay transitions.
- Toast flood capped at 3 with duplicate-text coalescing.
- Sign out asks for confirmation and states the ledger stays on the device.

### 9.5 Verification suites

| Suite | Assertions | Covers |
|---|---|---|
| `suite` | 12 | Boot, auth gate, onboarding gate, seed coherence, currency |
| `flow2` | 13 | Add → allocate → expenses → review → confirm → commit, plus arithmetic invariants |
| `deep` | 26 | Institution/tender/price CRUD, stock, sales, overpayment → credit, all 14 reports, all 8 P&L tabs, settings, reload persistence, sign-out |
| `edge` | 23 | Correction window + R2 revision, post-lock adjustment, stale cross-day cart, discard, extreme values, negative/zero rejection, typed delete-all |
| `crud` | 24 | Item/category/reason/expense CRUD, duplicate guards, inventory reservation, import rejection |
| `data` + `a11y` | 17 | Backup/restore round-trip, CSV blob, Escape, focus rings, tap-target audit |

### 9.6 Known, deliberate limitations

- No backend, no sync, single user, English only, ₹ only, light theme only.
- Auth is a demo PIN (`1234`) with no server — the gate is a UX simulation.
- Backup/export are browser downloads; "share memo" uses Web Share/clipboard; photo evidence is a local data URI.
- Alerts are computed on load (in-app only, no OS notifications).
- The spec's 13 alert kinds are implemented as 6 computed kinds.
- Reports export CSV/JSON only (copy corrected).

`FLUTTER_HANDOFF.md` documents the routes, data model, formulas, component mapping,
validation contract and suggested Flutter structure derived from this build.
