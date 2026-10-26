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


