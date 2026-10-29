# Flutter Handoff — Vegetable Contractor

**Source of truth:** `index.html` (7946 lines, single file). **Spec:** `SCREENS_MASTER_LIST.md`. **Audit:** `AUDIT.md`.

---

## 1. Purpose & status

This is a **working single-file prototype** — HTML + CSS + vanilla JS, all state in `localStorage` under `vegContractorStateV2`, no backend, no network calls at runtime. It is hardened end-to-end: all 44 screen sections exist and render, the cart → allocate → review → confirm flow commits a real ledger, corrections produce revision snapshots, FEFO inventory reservation and dues/billing reconcile, and destructive actions are confirmed. **It is the behavioural reference for the Flutter build, not a visual mock.** Where the prototype is deliberately simple (§11), this document says whether Flutter should keep or replace the behaviour.

**Counts verified against the implementation:** 44 `section.screen` elements · 5 bottom-nav destinations · 41 `backMap` entries · 14 report types · 6 Settings hub rows · 9 More hub rows · 8 P&L tabs · 6 alert kinds computed (spec lists 13 — see §2 note).

---

## 2. Screen inventory

`#screen-id` is the DOM id; `state.currentScreen` stores it without the `#screen-` prefix. Titles come from the `titles` map inside `navigate()`.

| # | `#screen-id` | Flutter route | Screen class | Purpose | States to handle | Gate |
|---|---|---|---|---|---|---|
| 1 | `#screen-login` | `/login` | `LoginScreen` | Demo PIN gate (10-digit phone + 4-digit PIN) | empty / error / signing-in (button text "Signing in…") | pre-app |
| 2 | `#screen-splash` | `/splash` | `SplashScreen` | Brand + 1.1 s auto-route (`routeAfterSplash`) | loading only | pre-app |
| 3 | `#screen-welcome` | `/onboarding/welcome` | `WelcomeScreen` | 2 intro slides, Skip always visible | slide 1 / slide 2 | onboarding |
| 4 | `#screen-profile` | `/onboarding/profile` | `BusinessProfileScreen` | Business name*, currency ₹, default unit | empty / invalid (required) / filled | onboarding |
| 5 | `#screen-initial-items` | `/onboarding/items` | `InitialItemsScreen` | Seeded 16-item catalogue, add custom | seeded / empty-if-skipped | onboarding |
| 6 | `#screen-initial-institutions` | `/onboarding/institutions` | `InitialInstitutionsScreen` | Create institutions + tenders | 0–n institutions / duplicate-name error | onboarding |
| 7 | `#screen-setup-complete` | `/onboarding/done` | `SetupCompleteScreen` | Confirmation → Home | success | onboarding |
| 8 | `#screen-home` | `/home` | `HomeScreen` | TODAY metrics, pending split, cart card, inventory, lifetime totals, alerts | empty / active cart / completed day / correction chip / alerts empty | tab root |
| 9 | `#screen-whats-new` | `/whats-new` | `WhatsNewScreen` | Version update message, dismissible | seen / unseen | More only |
| 10 | `#screen-veg` | `/items` | `ItemGridScreen` | Photo cards + search + category filters + inline quick-add | populated / no-results / added state | tab root |
| 11 | `#screen-item-detail` | `/items/:itemId` | `ItemDetailScreen` | Qty (purchase-unit toggle), rate prefilled from `lastRates`, source, quality, paid/credit, cost preview | inventory-available / no-stock / validation error | pushed |
| 12 | `#screen-add-item` | `/items/new` · `/items/:id/edit` | `ItemEditorScreen` | Create/edit item, photo, unit, shelf-life, purchase unit | create / edit / duplicate error | pushed |
| 13 | `#screen-cart` | `/cart` | `CartScreen` | Today's purchase lines, totals, edit/remove, discard, continue | empty (`#cartEmpty`) / populated / correction mode | tab root |
| 14 | `#screen-allocate` | `/cart/allocate` | `AllocateScreen` | Per-item allocation rows, progress, remaining decision | open / partial / done / no supply | pushed |
| 15 | `#screen-review` | `/cart/review` | `ReviewScreen` | Six sections + TODAY'S FINANCIAL RESULT + warnings | populated / warnings / empty section | pushed |
| 16 | `#screen-confirm` | `/cart/confirm` | `ConfirmScreen` | Whole-cart summary + single confirm (DL-06) | populated | pushed |
| 17 | `#screen-completed` | `/cart/completed` | `CartCompletedScreen` | Success, TRX ref + revision, View Details/History/Home | success | pushed |
| 18 | `#screen-inst-list` | `/institutions` | `InstitutionListScreen` | Search, pending badge per institution | empty / populated / filtered | pushed |
| 19 | `#screen-inst-edit` | `/institutions/new` · `/institutions/:id/edit` | `InstitutionEditorScreen` | Basic fields + tender list | create / edit / duplicate error | pushed |
| 20 | `#screen-inst-profile` | `/institutions/:id` | `InstitutionProfileScreen` | Details, tenders, prices, today's supply, performance | populated / deactivated | pushed |
| 21 | `#screen-tender-mgmt` | `/institutions/:id/tenders` | `TenderListScreen` | Tender CRUD, period, alert days | empty / active / ended | pushed |
| 22 | `#screen-tender-prices` | `/institutions/:id/tenders/:tenderId/prices` | `TenderPricesScreen` | Fixed price list per tender | empty / populated / error | pushed |
| 23 | `#screen-receivables` | `/institutions/:id/receivables` | `ReceivablesScreen` | Bills, aging, payments, deductions, credit balance, "prepare bill?" chip | empty / accrued / billed / partly paid / credit balance | pushed |
| 24 | `#screen-dues` | `/dues` | `DuesScreen` | Pending split (total/billed/accrued) + per-institution rows | empty / populated | More only |
| 25 | `#screen-inventory` | `/inventory` | `InventoryScreen` | 4 metrics, 5 filters, lot list, quick actions | empty / populated / filtered | tab root |
| 26 | `#screen-inv-detail` | `/inventory/:lotId` | `LotDetailScreen` | Lot card + movement history + Quick Actions | normal / empty lot / no movements | pushed |
| 27 | `#screen-expenses` | `/cart/expenses` | `ExpensesScreen` | Day expense list + add/edit/delete | empty / populated | pushed |
| 28 | `#screen-payables` | `/payables` | `PayablesScreen` | Seller credit rows, settle | empty / open / partly settled / settled | More only |
| 29 | `#screen-history` | `/history` | `HistoryScreen` | Date cards, filters (Today/7d/Month/Custom), + Add Entry | empty / populated / filtered | More only |
| 30 | `#screen-trx-detail` | `/history/:trxId` | `TransactionDetailScreen` | Frozen snapshot tabs, revision picker, memo, photo, re-open | normal / locked / revision / with photos / outdated memo | pushed |
| 31 | `#screen-other-entries` | `/history/entries/:entryId` | `DayEntryDetailScreen` | Sale / waste / adjustment / settlement detail | normal / locked | pushed |
| 32 | `#screen-pl` | `/pl` | `ProfitLossScreen` | 8 tabs: Overall, Daily, Weekly, Monthly, Institution, Contract, Vegetable, Custom | populated / empty groups / custom range | More only |
| 33 | `#screen-reports` | `/reports` | `ReportsScreen` | List of 14 report types | populated | More only |
| 34 | `#screen-report-detail` | `/reports/:type` | `ReportDetailScreen` | Table + Export (CSV via share/clipboard) | populated / empty-with-copy / custom range | pushed |
| 35 | `#screen-settings` | `/settings` | `SettingsScreen` | 6-row hub | populated | More only |
| 36 | `#screen-item-mgmt` | `/settings/items` | `ItemManagementScreen` | All items incl. deactivated, deactivate/reactivate | populated / deactivated rows | More only |
| 37 | `#screen-business` | `/settings/business` | `BusinessSettingsScreen` | Name, currency (read-only ₹), default unit, shelf-life | populated / saved | More only |
| 38 | `#screen-app-settings` | `/settings/app` | `AppSettingsScreen` | 4 thresholds + links to editors | populated / saved | More only |
| 39 | `#screen-categories` | `/settings/app/categories` | `ExpenseCategoriesScreen` | Editable expense category set | populated / add / duplicate | More only |
| 40 | `#screen-reasons` | `/settings/app/reasons` | `ReasonsScreen` | One shared reason set (deductions, waste, rejection) | populated / add / duplicate | More only |
| 41 | `#screen-data` | `/settings/data` | `DataManagementScreen` | Backup, restore, export JSON/CSV, import, delete all | idle / downloading / import error / deleting | More only |
| 42 | `#screen-about` | `/settings/about` | `AboutScreen` | App name, version v2.9, single-user note | static | More only |
| 43 | `#screen-attention` | `/attention` | `AttentionCenterScreen` | Computed alert list | populated / "All clear ✓" | Home badge |
| 44 | `#screen-more` | `/more` | `MoreHubScreen` | 9 hub rows incl. Sign out | populated | tab root |

**Gating rules as implemented:** the only guard is `routeAfterSplash()` — `if (!state.setupDone) navigate('#screen-welcome')`. There is no per-route guard; every screen is reachable via `navigate()` from the console. Flutter must add a real `redirect` on the router for `/onboarding/*` (only while `!setupDone`) and for `/login` (only while unauthenticated). Onboarding screens are #3–7 (with #1–2 as the pre-app auth/splash gate); "More only" rows are reachable **exclusively** from the More hub (`renderMore()`), except `inst-list`, which is also linked from Settings.

**Discrepancies to carry forward:**
- `AUDIT.md` states 42 screen sections in its earlier sections; the implementation has **44** (41 spec screens + `more` hub + `login` auth gate + `whats-new`). Spec counts 41 active screens (S03 retired). See AUDIT.md §9 for the 2026-10-29 hardening pass.
- Attention Center: spec §14 lists **13** alert types; `attentionAlerts()` computes **6** — `oldStock`, `sellerDue`, `paymentDue`, `longPending`, `tenderEnd`, `backup`. Rejected-stock, incomplete-cart, missing-price, inventory-attention, price-updated, transaction-completed and waste/loss are not alert objects (some surface as toasts).
- Reports export **CSV text only** via `navigator.share`/clipboard. No PDF path exists; the UI copy was corrected to say CSV (it previously claimed "PDF/CSV").
- The Inventory filter labelled "Partially Used" matches lots with a negative movement and stock remaining (`qty > 0`). It previously filtered `status === 'Sold'`, which only occurs at qty 0, so it could never match — fixed in the hardening pass.

---

## 3. Navigation architecture

### 3.1 The five tabs

`<nav class="bottom-nav">` has exactly **5** anchors: `home`, `veg`, `cart`, `inventory`, `more`. Each is a root — tapping it calls `navigate()` and **replaces** the visible screen; there is no per-tab stack in the prototype.

| Tab | Root | Pushed screens that belong to it |
|---|---|---|
| Home | `#screen-home` | `#screen-dues`, `#screen-payables`, `#screen-pl`, `#screen-history` → `trx-detail`/`other-entries`, `#screen-reports` → `report-detail`, `#screen-settings` → 6 children, `#screen-attention`, `#screen-inst-list` → profile → tenders/prices/receivables |
| Vegetables | `#screen-veg` | `#screen-item-detail`, `#screen-add-item` |
| Cart | `#screen-cart` | `#screen-allocate`, `#screen-expenses`, `#screen-review`, `#screen-confirm`, `#screen-completed` |
| Inventory | `#screen-inventory` | `#screen-inv-detail` |
| More | `#screen-more` | `#screen-whats-new` (+ all Home-pushed hubs) |

### 3.2 Full back map (`backMap`, 41 entries)

The app bar back button is `backMap['#screen-' + state.currentScreen]`; if absent it does nothing.

| Screen | Parent | Screen | Parent |
|---|---|---|---|
| `#screen-welcome` | `#screen-splash` | `#screen-inv-detail` | `#screen-inventory` |
| `#screen-profile` | `#screen-welcome` | `#screen-expenses` | `#screen-allocate` |
| `#screen-initial-items` | `#screen-profile` | `#screen-payables` | `#screen-home` |
| `#screen-initial-institutions` | `#screen-initial-items` | `#screen-history` | `#screen-home` |
| `#screen-setup-complete` | `#screen-home` | `#screen-trx-detail` | `#screen-history` |
| `#screen-veg` | `#screen-home` | `#screen-other-entries` | `#screen-history` |
| `#screen-cart` | `#screen-home` | `#screen-pl` | `#screen-home` |
| `#screen-inventory` | `#screen-home` | `#screen-reports` | `#screen-home` |
| `#screen-more` | `#screen-home` | `#screen-report-detail` | `#screen-reports` |
| `#screen-item-detail` | `#screen-veg` | `#screen-settings` | `#screen-home` |
| `#screen-add-item` | `#screen-veg` | `#screen-item-mgmt` | `#screen-settings` |
| `#screen-allocate` | `#screen-cart` | `#screen-business` | `#screen-settings` |
| `#screen-review` | `#screen-expenses` | `#screen-app-settings` | `#screen-settings` |
| `#screen-confirm` | `#screen-review` | `#screen-categories` | `#screen-app-settings` |
| `#screen-completed` | `#screen-home` | `#screen-reasons` | `#screen-app-settings` |
| `#screen-inst-list` | `#screen-home` | `#screen-data` | `#screen-settings` |
| `#screen-inst-edit` | `#screen-inst-list` | `#screen-about` | `#screen-settings` |
| `#screen-inst-profile` | `#screen-inst-list` | `#screen-attention` | `#screen-home` |
| `#screen-tender-mgmt` | `#screen-inst-profile` | `#screen-whats-new` | `#screen-more` |
| `#screen-tender-prices` | `#screen-inst-profile` | `#screen-receivables` | `#screen-inst-profile` |
| `#screen-dues` | `#screen-home` | | |

No parent: `login`, `splash`, `home` (tab roots).

### 3.3 Rules that matter for Flutter

1. **Every push must restore state.** Screens are re-rendered from `state` on entry (`screenRenderers[id]()`), with module-level selections (`trxDetailId`, `allocItemId`, `state.selectedInstId`, `state.selectedLotId`, `state.reportType`) acting as parameters. In Flutter these become route path/query parameters, not globals.
2. **The cart is a persistent singleton**, not a per-screen object. `state.cart` survives navigation; `#screen-cart` is only a view. Never instantiate a `Cart` in a screen widget.
3. **Correction mode is a cart *mode*** (`state.cart.mode = 'correction'` + `correctionTrxId`), entered via `startCorrection()`/`startCorrectionFromHome()`, exited by `finalizeCart()` or `discardCartSetup()`. It is **not** a screen and must not become one. It renders inside `#screen-cart` with the `#correctionChip` banner.
4. `#screen-completed` deliberately backs to Home, skipping the cart flow; `#screen-setup-complete` backs to Home rather than the wizard.
5. The cart chip (`#btnAppCart`) is hidden on `login, splash, welcome, profile, initial-items, initial-institutions, setup-complete, cart, completed` (`CART_IND_HIDE_ON`).

**Recommendation:** `go_router` with a `StatefulShellRoute.indexedStack` for the five tabs, each branch owning its own `Navigator` so tab state and back stacks survive switching. Nest per-tab routes as in §2. Use a plain `Navigator.push` (or `context.push`) — not shell branches — for:
- bottom sheets and dialogs (`showModalBottomSheet`, `showDialog`), which in the prototype are DOM overlays and never touch history;
- the onboarding wizard (`/onboarding/*`), which is a linear flow better modelled as a single `PageView` route with internal steps;
- the numeric keypad and pick-list sheets, which are modal utilities;
- `#screen-completed`, which should `context.go('/home')` on back rather than pop into a dead cart.

---

## 4. Data model

`seedState()` returns the object persisted verbatim. `normalizeState(s)` heals every missing field before use. Dart equivalents below use non-nullable fields with defaults matching `normalizeState()`.

```dart
class AppState {
  int v;                    // 2 — schema version, checked on load
  bool setupDone;
  String businessName;
  String currency;          // '₹' fixed
  String defaultUnit;       // 'kg'
  Seq seq;
  List<Item> items;
  List<Institution> institutions;
  Cart cart;
  List<Transaction> transactions;
  List<InventoryLot> inventory;
  List<Payable> payables;
  List<Bill> bills;
  List<Payment> payments;
  List<Deduction> dedPending;   // deductions with no bill yet
  List<DayEntry> dayEntries;
  Map<String, LastRate> lastRates;
  Settings settings;
  String? lastBackup;
}
```

| Model | Fields (name : Dart type) | Notes |
|---|---|---|
| `Seq` | `trx, inst, item, lot, pay, bill, paym, entry, line, exp : int` | Monotonic counters; ids are `kind.charAt(0) + n` (`nextId`). Seed: `trx 1, inst 3, item 16, lot 2, pay 1, bill 1, paym 1, entry 1, line 0, exp 0`. |
| `Item` | `id, name, category, unit : String`; `image : String` (data URI or emoji); `shelfLife : int?`; `purchaseUnit : PurchaseUnit?`; `active : bool` | `shelfLife` null = groceries. `purchaseUnit = {label: 'bag', factor: 50}`. Never deleted — only `active=false`. |
| `Institution` | `id, name, contact, phone, address, location : String`; `tenders : List<Tender>`; `fixedPrices : Map<String, List<FixedPrice>>` keyed by tenderId; `active : bool` | `normalizeState` defaults `active=true` and never resurrects an explicit `false`. |
| `Tender` | `id, name, number, start, end, paymentTerms : String`; `overdueAlertDays : int`; `demandedQty : Map<String,double>`; `revisions : List` | Dates are ISO `YYYY-MM-DD` strings. |
| `FixedPrice` | `itemId : String`, `rate : double` | Live master data — never read by confirmed transactions. |
| `Cart` | `date : String`; `mode : String` ('day' \| 'correction'); `correctionTrxId : String?`; `items : List<CartLine>`; `allocations : Map<String, List<Allocation>>`; `remaining : Map<String, List<RemainingEntry>>`; `rejected : Map<String, List<Rejection>>`; `expenses : List<Expense>`; `confirmed : bool` | Singleton. Maps are keyed by `itemId`. |
| `CartLine` | `id, itemId : String`; `purchaseQty, rate : double`; `inventory : List<InventoryPick>`; `invQty, invCost : double`; `seller, payment, dueDate, quality, source, createdAt : String` | `payment` ∈ `paid-now` \| `on-credit`. |
| `InventoryPick` | `lotId : String`, `qty : double`, `rate : double` | `rate` = lot's `originalRate` at reservation time. |
| `Allocation` (cart) | `itemId, instId : String`; `qty, rate : double`; `custom : bool`; `tenderId : String?` | Confirmed rows add `netQty, rejectQty, revenue, cost`. |
| `RemainingEntry` | `itemId : String`; `qty : double`; `action : String` ('inventory' \| 'loss'); `reason : String` | Frozen copy adds `cost, rate`. |
| `Rejection` | `itemId, instId : String`; `qty : double`; `action, reason : String` | Frozen copy adds `cost, rate`. |
| `Transaction` | `id, date, status ('confirmed'), revision ('R1'), day : String`; `items : List<TransactionItem>`; `allocations : List<TransactionAllocation>`; `remaining, losses, rejected : List<...>`; `expenses : List<Expense>`; `revenue, newPurchaseCost, inventoryUsedCost, expensesTotal, lossesTotal, totalCost, profit, margin : double`; `revisions : List<TransactionRevision>`; `photos : List<Photo>`; `memos : List<Memo>`; `invPicks : List<InventoryPick>`; `invCreatedLotIds : List<String>` | Frozen snapshot. `id = TRX-YYYYMMDD[-n]`. |
| `TransactionItem` | `itemId, source, quality, seller, payment, dueDate : String`; `qty, rate, cost, purchaseQty, invQty, invCost : double` | Frozen purchase line. `qty` = total supply (`purchaseQty + invQty`). |
| `TransactionAllocation` | `itemId, instId : String`; `qty, netQty, rejectQty, rate, revenue, cost : double`; `custom : bool`; `tenderId : String?` | Frozen allocation. `netQty = qty − rejectQty`; `cost` is the blended share at commit. |
| `TransactionRevision` | `rev, note : String`; `revenue, totalCost, profit, margin : double` | Pushed before each correction; `revision` becomes `'R' + (revisions.length + 1)`. |
| `Memo` | `instId, number : String`; `shared : bool`; `sharedAt, sharedRevision : String?` | Document only — never creates a bill or moves money. |
| `Photo` | `data : String` (data URI), `date, name : String` | |
| `InventoryLot` | `id, itemId : String`; `qty, originalRate : double`; `status : String` ('Available' \| 'Sold' \| 'Waste/Loss'); `source, dateAdded : String`; `movements : List<LotMovement>` | |
| `LotMovement` | `date, source : String`; `qty : double` (negative = out) | Append-only. |
| `Payable` | `id, trxId, itemId, seller : String`; `amount, remaining : double`; `dueDate, date, status : String`; `settlements : List<Settlement>` | `status` open \| settled. |
| `Settlement` | `date : String`, `amount : double`, `note : String` | Cash-only; never P&L. |
| `Bill` | `id, instId, number, date, periodStart, periodEnd : String`; `trxIds : List<String>`; `gross, net : double`; `deductions : List<Deduction>` | `net = gross − Σ deductions`. |
| `Deduction` | `date, reason, note : String`; `amount : double`; `instId : String?` | `instId` set only while in `dedPending`. |
| `Payment` | `id, instId, date, method, against, note : String`; `amount : double`; `creditBalance : bool`; `deductions : List<Deduction>`; `allocations : List<PaymentAllocation>` | One receipt row per payment; overpay creates a **second** row with `creditBalance = true`. |
| `PaymentAllocation` | `billId : String`, `amount : double` | **Only** source of truth for "money applied to a bill" (`billPaid`). |
| `DayEntry` | `id, date, type : String` ('sale'\|'waste'\|'adjustment'\|'voucher'\|'settlement'); `itemId, lotId, seller, payableId, note, reason, cat, client, trxId : String?`; `qty, rate, amount, revenue, cost : double?` | Settlement is cash-only (`dayEntryProfit` = 0). |
| `Settings` | `overdueAlertDays 7, shelfLifeDefault 7, longPendingDays 30, tenderEndingDays 7, sellerPayableDays 3, backupInterval 7 : int`; `expenseCategories : List<String>` (6 defaults); `reasons : List<String>` (6 defaults) | |
| `LastRate` | `rate : double`, `date : String` | Feeds "Last rate: ₹30/kg · date" prefill. |

### 4.1 Frozen vs live (spec §2.4)

| Data | Confirmed transaction | Master record |
|---|---|---|
| Purchase rate, qty, cost | **frozen** in `Transaction.items` | `CartLine` is discarded after commit |
| Tender rate | **frozen** in `TransactionAllocation.rate` (+ `custom` flag) | `Institution.fixedPrices` edits apply to future transactions only |
| Item name/unit/shelf-life | resolved live by `itemId` at render time | `Item` — deactivation never removes history |
| Institution name | resolved live by `instId` | `Institution` — deactivation keeps dues collectable |
| Inventory cost | **frozen** in `remaining/losses/rejected[].cost` and `invPicks` | `InventoryLot.originalRate` |
| Expenses | **frozen** in `Transaction.expenses` | `Cart.expenses` |
| Deductions | stored on `Bill`/`dedPending` with their own date | — |

Flutter rule: `Transaction` must be **immutable** (freeze in the constructor, no setters). Corrections clone → push a `TransactionRevision` → replace the transaction with a new immutable instance.

### 4.2 Transient UI keys that leak into persistence

`state` also carries `currentScreen`, `selectedInstId`, `selectedLotId`, `editingInst`, `justCompletedId`, `reportType`, `reportRange`, `removeLineId`, `_stamp`. These are UI scratch space. **Keep them out of the persisted model in Flutter** (separate `UiState` or route params) — they are persisted today only because `saveState()` stringifies the whole object.

---

## 5. Calculation engine

All money passes through `r2(n) = Math.round(n*100)/100`. Quantities use the same 2-dp rounding. **`formatINR` uses Indian digit grouping** (last 3, then 2s: `₹5,00,000`), `formatQty` prints up to 2 dp with trailing zeros trimmed, `fmtSign` prefixes `+` or `−` (U+2212).

| # | Name | Formula (as implemented) | Where | Invariant |
|---|---|---|---|---|
| f0 | Line supply | `r2(purchaseQty + (invQty \|\| 0))` | `lineSupply()` | Inventory qty and purchase qty are one supply pool, never double-counted |
| f1 | Purchase cost | `r2(purchaseQty * rate)` | `linePurchaseCost()` | Per line, not per item |
| f1b | Inventory cost of line | `r2(Σ pick.qty * pick.rate)` | `lineInvCost()` | Uses the lot's original rate, not today's |
| f2 | Line cost | `r2(linePurchaseCost + lineInvCost)` | `lineCost()` | |
| f3 | Item aggregate | `supply = Σ lineSupply`; `lc = Σ lineCost`; `unitCost = supply > 0 ? r2(lc/supply) : 0` | `itemAgg()` | Blended delivered-cost rate; the **only** rate used for allocation cost |
| f4 | Allocation % | `min(100, allocatedQty / agg.supply * 100)` (display), exact in math | `allocVegRowHTML()` | Display 1 dp; math unrounded |
| f5 | Allocation cost share | `eff / agg.supply * agg.lc` | `cartSummary()`, `finalizeCart()` | Rounded **per row** at commit; `cartSummary` rounds the total. Keep the commit rounding order. |
| f6 | Institution revenue | `eff * a.rate` where `eff = max(0, a.qty − rejectedQty)` | `effectiveAllocQty()`, `cartSummary()` | Rejections reduce revenue, not the allocation row |
| f7 | Total cost | `investment + expenses` where `investment = Σ lineCost` | `cartSummary()` | **Losses are already inside line costs — never subtract again** |
| f8 | Profit | `r2(revenue − totalCost)` | `cartSummary()` | |
| f9 | Margin | `revenue > 0 ? r2((revenue−totalCost)/revenue*100) : 0` | `cartSummary()` | 0, rendered as `—`, when revenue is 0 |
| f10 | Overall totals | `inv = Σ(newPurchaseCost + inventoryUsedCost + expensesTotal) + dayEntry costs`; `rev = Σ revenue + dayEntry revenue`; `ded = Σ allDeductions`; `effectiveRev = rev − ded`; `profit = Σ profit + dayEntry profit − ded` | `overallTotals()` | Deductions subtracted **once** |
| f11 | Inventory value | `Σ lot.qty * lot.originalRate` over `qty > 0 && status === 'Available'` | `renderInventory()` | Neither revenue, profit, loss nor cost |
| f12 | Waste loss | `qty * lot.originalRate` | `saveWasteEntry()`, `saveWaste()` | Loss = original cost, not selling value |
| f13 | Inventory sale profit | `revenue − cost` where `cost = qty * originalRate` | `saveSaleEntry()` | Cash sale, dated |
| f14 | Bill paid | `Σ payments.allocations where billId matches` | `billPaid()` | Allocations are the only source — a receipt can never be counted twice |
| f14b | Bill pending | `r2(bill.net − billPaid(bill.id))` | `billPending()` | |
| f15 | Billed pending | `Σ bills.net − Σ billPaid` | `billedPending()` | |
| f16 | Accrued pending | `Σ allocations.revenue` for transactions with **no** bill containing that trxId | `accruedPending()` | Billed money must leave accrued |
| f17 | Credit balance | `Σ payments where creditBalance` | `creditBalance()` | |
| f18 | Total pending | `r2(billed + accrued − creditBalance)` | `instSummary()` | Credit reduces pending |
| f19 | Effective revenue | `rev − deductions` for the same scope/date window | `overallTotals()`, `groupByInst()`, `groupByContract()` | Deductions land on **their own date** (formula 15), never retro-dated |
| f20 | P&L aggregation | `revenue = Σ t.revenue`; `cost = Σ t.totalCost`; `profit = Σ t.profit`; `inv = Σ(newPurchase+invUsed)`; `exp = Σ expensesTotal`; `supply = Σ (netQty ?? qty)` | `plAggregator()` | Supply uses **netQty** where present |
| f21 | Per-unit aggregation | bucket quantities by unit, then join `" · "`: `"100 kg · 20 litre"`; never one total across units | `qtyByUnit()` | **Quantities never sum across different units** |
| f22 | FEFO reservation | Sort available lots by `dateAdded` asc; per lot `free = lot.qty − qty already picked in cart`; take `min(free, need)`; `shortfall` if `need > 0.0001` | `reserveFEFO()` | Reservation is derived from cart lines — side-effect free until commit |
| f23 | Item availability | `Σ available lot qty − Σ cart invQty for that item` | `availForItem()` | Prevents double-reserving |
| f24 | Day-entry profit | sale: `revenue − cost`; waste: `−cost`; adjustment: `−amount`; voucher: `−amount`; settlement: `0` | `dayEntryProfit()`, `dayEntriesForDate()` | Settlement never touches P&L |

**No double counting (spec §2.3).** Three separate rules enforce it:
1. `totalCost = investment + expenses` and `investment` already contains inventory-used cost and loss cost — losses are never subtracted a second time (`cartSummary()` comment at the total-cost line).
2. `overallTotals()` builds investment from `newPurchaseCost + inventoryUsedCost + expensesTotal`, never from line costs plus inventory cost again.
3. `billPaid()` reads only `payment.allocations`; the legacy `against` field is ignored (and normalised into an allocation on load).

**Deductions reduce P&L once, on their own date.** `allDeductions()` merges `bill.deductions` and `dedPending`; `overallTotals()` subtracts their sum exactly once from profit; `deductionsOn(date)` scopes it to a day for Home cards; `deductionsForInst(instId, from, to)` scopes it to an institution window. Contract P&L prorates an institution's total deductions across its contracts by revenue share (`groupByContract()`).

**Rounding order to preserve:** `finalizeCart()` rounds each frozen allocation/remaining/rejection cost share (`r2(...)` per row); `cartSummary()` accumulates unrounded and rounds only the totals. The two can differ by a paisa. Flutter should round per row at commit (authoritative) and derive display totals by summing the frozen rows.

---

## 6. State management recommendation

The prototype has one mutable `state` object, mutated in place by handlers, then `saveState()` writes the whole JSON blob. That maps cleanly onto Riverpod with an immutable state object.

```
LedgerRepository            // owns AppState, does load/save/migrate
  ├─ Future<AppState> load()          // shared_preferences key 'vegContractorStateV2'
  ├─ Future<void> save(AppState)      // debounce ~300 ms; JSON encode whole blob
  └─ AppState migrate(Map json)       // == normalizeState()
```

- **Provider shape:** `ledgerRepositoryProvider` → `ledgerProvider = NotifierProvider<LedgerNotifier, AppState>`; feature notifiers read/write through the repository so persistence stays in one place. Suggested notifiers: `CartNotifier` (cart mutations + `cartSummary` derivation), `InstitutionsNotifier` (institutions/tenders/prices), `InventoryNotifier` (lots, FEFO, quick actions), `DuesNotifier` (bills, payments, deductions), `SettingsNotifier` (thresholds, editors), `AuthNotifier` (demo gate).
- **Derived values are computed, not stored.** `cartSummary()`, `instSummary()`, `overallTotals()` become pure functions on `AppState` (or Riverpod `Provider`s). Do **not** cache them in state — the prototype recomputes on every render, which is why it never drifts.
- **Persistence:** `shared_preferences` with a single JSON string under the same key `vegContractorStateV2` matches the prototype exactly and keeps backup/import file compatibility. Move to `drift` only when you need queries (multi-year history, search, report SQL). If you do, keep the JSON blob as the backup format.
- **Versioning:** `v: 2` is checked in `loadState()` — `if (!s || s.v !== 2) return null;` (silently falls back to seed). `runImport()` rejects `bundle.v !== 2` with *"Version mismatch — import rejected."* Flutter must implement `migrate(Map)` mirroring `normalizeState()` field-by-field (it heals `cart.rejected`, `payments.allocations`, `institution.active`, `settings` defaults, `seq` defaults) and a `v` bump path rather than discarding data.
- **Auth state** lives under a separate key `vegContractorAuthV1` (`{phone, at}`); keep it separate from the ledger.
- **Backend seam:** define `abstract class LedgerStore { Future<AppState?> read(); Future<void> write(AppState); }` with `LocalLedgerStore` today and `RemoteLedgerStore` later. No screen may touch `shared_preferences` directly. The prototype has **no backend and no sync**, so the repository is the only place a server can be introduced without touching UI.
- **Bloc alternative:** if the team prefers Bloc, one `LedgerBloc` with events (`CartLineAdded`, `AllocationChanged`, `CartFinalized`, …) and a single immutable `LedgerState` works equally well; the important part is one owner and pure derivation functions.

---

## 7. Component mapping

| Prototype | Flutter primitive | Notes / token values |
|---|---|---|
| `.btn.btn-primary` | `FilledButton` | `minHeight 48`, `minWidth 44`, radius `pill`, bg `#4F8CFF`, fg `#FFF`, label 700 `text-sm` (13–15). Hover = darken 8 %; press = darken 14 % + `translateY(-1px)` (drop the transform on touch). |
| `.btn.btn-secondary` | `OutlinedButton` | bg `surface` (`rgba(255,255,255,.74)`), border `rgba(255,255,255,.64)` + 1 px ring, fg `#102033`; hover border/fg → accent. |
| `.btn.btn-danger` | `FilledButton` with `danger` colour | bg `#EF4444`, fg white. |
| `.btn:disabled` | `onPressed: null` | `opacity .5`, no transform, `cursor not-allowed`. |
| `.field` + `label` + `input` + `.field-error` | `Column` → `Text` label + `TextFormField` + `Text` error | Label `text-sm` 700 `#34465F`; input `minHeight 48`, radius `sm` (14–18), border `border`, **font ≥16 px** (prevents iOS zoom); error `text-xs` 600 `#EF4444`. Use `Form`/`TextFormField` validators; keep messages identical (§9). |
| `.currency-input` | `TextFormField` with `prefixText: '₹'` | Prefix `#60708A` 700, left padding `space-4 + 1.3em`. |
| `.card` | `Container`/`Card` | bg `surface`, border `border`, radius `lg` (28–36), padding `space-5` (17–22), shadow `raised + glass-lit`. Cards stack with `space-4` gap. |
| `.panel-head` | Row inside card header | padding `space-5`, bottom border `border-soft`. |
| `.list-row` (leading / main / value / badge) | `ListTile`-like custom `Row` | padding `space-4` vertical, bottom border `border-soft` (none on last); `main` flex 1, `sub` `text-sm` `#60708A`, `value` 700 and **wraps** (`softWrap`) — long names/amounts must never widen the row. Badge trailing. |
| `.metric` + `.metric-grid` | `GridView`/`Wrap` of `MetricCard` | Card: bg `surface`, radius `md` (20–26), padding `space-4`, `strong` = `text-metric` (19–24, tabular figures), `span` = `text-xs` 600 muted. Grid: 2 cols phone, 4 cols ≥640; a lone trailing card spans full width. `.green` → `#22C55E`, `.red` → `#EF4444`. |
| `.badge` (`-green/-red/-blue/-gray/-warn/-meta`) | `Container` + `Text` | `text-xs` 700, uppercase, letter-spacing `.08em`, radius pill, padding 4×8; bg = colour mixed 85 % with white; text `#15803D`/`#B91C1C`/`#1D4ED8`/`#34465F`/`#92400E`. |
| `.item-card` (photo + scrim + quick-add) | `Stack` in a `Card` | Min height 300, radius `lg`, `BoxFit.cover` image, scrim = `LinearGradient` `rgba(16,32,51,.08)` 34 % → `.5` 60 % → `.94` 100 %; content bottom-aligned white text. Quick-add row = qty field + rate field + `Add +` button (min 44 height, translucent `rgba(16,32,51,.5)` fills). `.added` = 4 px `#22C55E` ring. |
| `.alloc-veg-row` (thumb + bar + chip) | Custom `InkWell` row | 48×48 thumb radius `md`; progress bar 5 px radius pill, track accent@14 %, fill accent (green when done), animated `motion-base`; chip `Open`/`Partial`/`Done` with `text-xs` 700; selected row bg accent@8 %. |
| `.app-bar` | `AppBar` (`toolbarHeight 56`) | bg `surface` + `BackdropFilter(blur 16, saturate 1.8)`, bottom border, title `text-lg` 700 display font; back/action targets 44×44; cart chip with `bar-badge` (16 px pill, accent bg, 10 px 700). |
| `.bottom-nav` | `NavigationBar` or custom `BottomNavigationBar` | Height 72 + safe-area, bg `surface` + blur 16/180 %, top border, 5 items, icon 24 stroke 2, label `text-xs` 600; active `#4F8CFF`, inactive `#60708A`. |
| `.btn-sticky` | `bottomNavigationBar` of the route / `Stack` bottom bar | Fixed above the nav: `bottom: navHeight`, max width 1180, padding `space-3` × gutter, bg `bg`, top+bottom borders, buttons flex (secondary sizes to label, primary absorbs slack, lone button min 128). Content must reserve `nav + rows*stickyHeight + space-5`; 3+ buttons wrap to 2 rows below 560 px. |
| `.overlay` + `.modal` | `showModalBottomSheet` (forms/pickers) · `showDialog` (confirmations) | Scrim `rgba(16,32,51,.35)` + blur 4. Sheet/modal: bg `surface` + blur 20/180 %, radius `lg`, max width 420, max height full, head/body/foot with sticky foot padded by safe area; scale 0.95 → 1.0 over `motion-fast`. Use `showDialog` for destructive confirmations so they cannot be drag-dismissed accidentally. |
| `.toast` (G4) | `ScaffoldMessenger` `SnackBar` or `OverlayEntry` | Pill, bg `#102033`, white `text-sm` 600, bottom offset ~200, max 3 stacked, 2.5 s, duplicate message resets timer. Variants: success `#22C55E`, error `#EF4444`, warn `#F59E0B`. |
| Numeric keypad (G3) | `showModalBottomSheet` with 3×4 grid | 64 px keys, radius `lg`, `text-xl`; `0` spans 2 cols; `.` muted, `⌫` danger; **max 9 digits, single decimal point**; footer Clear / Done. Replace with `TextInputType.numberWithOptions(decimal: true)` **plus** the keypad for consistency with the prototype. |
| Pick-list sheet (`pickListSheet`) | `showModalBottomSheet` with `ListTile`s | Rows show label + optional sub + "Selected" badge; empty copy: *"No options yet — add some in Settings."* Use for tender picker (BS-14), category/reason pickers, lot picker. |
| `.empty-state` | `Column` centred | padding `space-12` × `space-4`; icon 64 stroke 1.5 muted @50 %; `h3` `text-xl`; body `text-base` max width 320. |
| `.skeleton` | Shimmer placeholder | Used only on splash today; wire to real loading states. |
| `.correction-chip` | Persistent `MaterialBanner`-style strip | bg `#F59E0B`, white 700 `text-sm`, fixed under the app bar. |
| `.progress` / `.progress-bar` | `LinearProgressIndicator` | 8 px track radius pill; fill accent, `motion-base`. |
| `.filter-chip` | `FilterChip` / `ChoiceChip` | Min height 44, radius pill; below 480 px chips take 50 % width. |

---

## 8. Design tokens

Light-only. There are **zero** `prefers-color-scheme` rules in the prototype — dark mode does not exist.

| Token | Value | Flutter |
|---|---|---|
| `--bg` | `#EEF6FF` | `AppTokens.bg` |
| `--surface` | `rgba(255,255,255,0.74)` | `surface` (glass) |
| `--surface-warm` | `rgba(238,246,255,0.72)` | `surfaceWarm` |
| `--fg` | `#102033` | `fg` |
| `--fg-2` | `#34465F` | `fg2` |
| `--muted` | `#60708A` | `muted` |
| `--meta` | `#4F8CFF` | `meta` (eyebrow/labels) |
| `--border` | `rgba(255,255,255,0.64)` | `border` |
| `--border-soft` | `rgba(255,255,255,0.38)` | `borderSoft` |
| `--accent` | `#4F8CFF` | `accent` |
| `--accent-on` | `#FFFFFF` | `onAccent` |
| `--accent-hover` / `--accent-active` | accent × black 8 % / 14 % | `accentHover`, `accentActive` |
| `--success` | `#22C55E` | `success` |
| `--warn` | `#F59E0B` | `warn` |
| `--danger` | `#EF4444` | `danger` |
| `--font-display` / `--font-body` | `"Plus Jakarta Sans", Inter, system-ui` | `google_fonts.plusJakartaSans` |
| `--font-mono` | `"SF Mono", ui-monospace, Menlo` | `monospace` (badges, eyebrows, numbers) |
| `--text-xs` | `clamp(11px, 11px + .1vw, 13px)` | 12 |
| `--text-sm` | `clamp(13px, 13px + .15vw, 15px)` | 14 |
| `--text-base` | `clamp(16px, 16px + .15vw, 17px)` | 16 |
| `--text-lg` | `clamp(16px, 16px + .5vw, 20px)` | 18 |
| `--text-xl` | `clamp(20px, 20px + 1vw, 27px)` | 22 |
| `--text-2xl` | `clamp(26px, 22px + 2.2vw, 42px)` | 28 |
| `--text-metric` | `clamp(19px, 5vw, 24px)` (≥640: `clamp(20px, 2.4vw, 30px)`) | 22 / 26 |
| `--text-h1` | `clamp(24px, 20px + .9vw, 28px)` (≥768: `clamp(30px, 2vw + 18px, 40px)`) | 26 / 32 |
| `--text-3xl` | `clamp(36px, 30px + 3.6vw, 58px)` | 40 |
| `--text-4xl` | `clamp(46px, 40px + 5vw, 78px)` | 52 (splash only) |
| `--leading-body` / `--leading-tight` | `1.55` / `1.04` | heights |
| `--tracking-display` | `-0.025em` | letterSpacing −0.5 |
| `--space-1…12` | `4, 8, 11–14, 14–18, 17–22, 20–26, 26–34, 38–52` | spacing scale 4/8/12/16/20/24/32/44 |
| `--radius-sm` / `-md` / `-lg` / `-pill` | `14–18` / `20–26` / `28–36` / `9999` | 16 / 22 / 32 / 999 |
| `--elev-ring` | `0 0 0 1px var(--border)` | 1 px white ring |
| `--elev-raised` | `0 24px 80px rgba(79,140,255,0.18)` | soft accent glow |
| `--elev-glass-lit` | `inset 0 1px 0 rgba(255,255,255,0.65)` | top highlight |
| `--focus-ring` | `0 0 0 4px rgba(79,140,255,0.28)` | focus |
| `--motion-fast` / `--motion-base` | `180ms` / `280ms` | durations |
| `--ease-standard` | `cubic-bezier(0.22, 1, 0.36, 1)` | `Cubic(0.22, 1, 0.36, 1)` |
| `--app-bar-h` / `--nav-h` | `56px` / `72px` | shell geometry |
| `--sticky-bar-h` | `44 + 2*space-3 + 1` ≈ 67–73 px | sticky bar row |
| `--container-max` | `1180px` | content max width |
| gutters | `36` / `24` / `clamp(16px,…,32px)` | desktop / tablet / phone |

```dart
@immutable
class AppTokens extends ThemeExtension<AppTokens> {
  final Color bg, surface, surfaceWarm, fg, fg2, muted, meta,
      border, borderSoft, accent, onAccent, accentHover, accentActive,
      success, warn, danger;
  final double space1, space2, space3, space4, space5, space6, space8, space12;
  final double radiusSm, radiusMd, radiusLg, radiusPill;
  final Duration motionFast, motionBase;
  final Curve easeStandard;
  final TextStyle textXs, textSm, textBase, textLg, textXl, textMetric, textH1;
  final List<BoxShadow> elevRaised, focusRing;
  // copyWith / lerp required by ThemeExtension
}
```

Register as `ThemeData(useMaterial3: true, extensions: [AppTokens.light()])` and read with `Theme.of(context).extension<AppTokens>()!`. Put glass treatments in a `GlassSurface` widget (`BackdropFilter` + `ClipRRect`), not in the theme.

**Dark mode slot:** add `AppTokens.dark()` with `bg #0B1522`, `surface rgba(18,28,43,.72)`, `fg #EAF2FF`, and flip the scrim/ring alphas; every widget that hardcodes white text over photos (`.item-card-content`) already sits on its own scrim, so it survives. The only non-token colour usages to fix first are the badge text hexes and the item-card scrim stops.

---

## 9. Interaction & validation rules

Messages are verbatim from `showToast(..., 'error')` / `authError()`.

| Field / action | Rule | Message | Enforced in |
|---|---|---|---|
| Login phone | exactly 10 digits after stripping non-digits | `Enter a valid 10-digit mobile number.` | `signInSubmit()` |
| Login PIN | exactly 4 digits | `Enter your 4-digit PIN.` | `signInSubmit()` |
| Login PIN value | must equal `DEMO_PIN` (`1234`) | `Mobile number or PIN is incorrect. Demo PIN is 1234.` | `signInSubmit()` |
| Business name | non-empty | `Enter your business name to continue.` | `profileContinue()` |
| Item name (onboarding) | non-empty | `Enter an item name.` | `saveCustomItem()` |
| Item name (editor) | non-empty | `Item name is required.` | `saveAddItem()` |
| Item name | unique among active items, case-insensitive | `An item named "X" already exists.` | both |
| Purchase unit factor | `> 0` when a unit is chosen | `Purchase unit factor must be greater than zero.` | `saveAddItem()` |
| Institution name (onboarding) | non-empty | `Enter institution name.` | `saveQuickInstitution()` |
| Institution name (editor) | non-empty | `Institution name required.` | `saveInstEdit()` |
| Institution name | unique, case-insensitive | `An institution named "X" already exists.` | both |
| Tender name | non-empty | `Tender name required.` | `saveTender()` |
| Tender period | `end >= start` | `End date must be on or after the start date.` | `saveTender()`, tender editor |
| Tender price rate | `> 0` | `Rate must be positive.` | `saveTenderPrice()` |
| Item detail qty | `> 0` (unless inventory-only mode) | `Enter a quantity.` / `Enter {item} quantity on the card.` | `addToCart()` |
| Item detail rate | `> 0` when buying | `Enter today's purchase rate.` / `Enter today's {item} rate.` | `addToCart()` |
| Inventory-only add | stock available | `No inventory available to use.` | `addToCart()` |
| Inventory reservation | enough free lots | `Not enough inventory available.` | `addToCart()` |
| On-credit line | seller name present | `On-credit purchase needs a seller name.` / `On-credit needs seller name.` | `addToCart()`, `saveCartLine()` |
| Cart line qty | `> 0` | `Quantity must be positive.` | `saveCartLine()` |
| Cart line rate | `> 0` | `Rate must be positive.` | `saveCartLine()` |
| Cart line shrink | new supply ≥ already allocated + remaining | `Reduce allocations first — N unit already allocated.` | `saveCartLine()` |
| Allocation qty | clamped to remaining; warn | `Only N unit of X left to allocate.` | `updateAllocQtyLive()`, `updateAllocQty()` |
| Allocation overrun (commit) | covered == supply ± 0.01 | `X: N unit left to allocate (or account as waste).` | `validateCart()` |
| Allocation rate | `> 0` per row | `X: an institution has rate 0.` | `validateCart()` |
| Purchase rate on commit | `> 0` per new-purchase line | `X: purchase rate missing on a new purchase line.` | `validateCart()` |
| On-credit on commit | seller **and** due date | `X: on-credit line needs seller and due date.` | `validateCart()` |
| Rejection qty | `> 0`, ≤ allocated | `Rejected cannot exceed allocated N.` | `saveRejected()` |
| Remaining decision | remainder > 0 | `Nothing unallocated.` | `remainingToInventory()`, loss handler |
| Split evenly | active institutions exist | `Add an active institution first.` | `autoSplit()` |
| Expense amount | `> 0` | `Enter amount.` | expense editor |
| Deduction amount | `> 0` | `Enter amount.` | deduction editors |
| Deduction cap | ≤ open-bill pending, else accrued | `Deduction exceeds pending ₹N.` | `saveDeduction()`, OE deduction |
| Payment amount | `> 0` | `Enter amount.` | `savePaymentWithCheck()` |
| Payment overpay | Received + deductions ≤ pending, else intent step | *"₹X exceeds pending — it becomes a credit balance…"* | `savePaymentWithCheck()` |
| Seller settlement | ≤ payable remaining | `More than owed — recording up to it.` / `More than remaining payable.` | settlement handlers |
| Bill gross | `> 0` | `Nothing to bill.` | `issueBill()` |
| Inventory add stock | qty and rate `> 0` | `Fill both quantity & rate.` / `Quantity & rate required.` | `saveNewStock()` |
| Inventory sale | qty ≤ lot qty | `Not enough quantity in this lot.` | `saveInventorySale()` |
| Inventory waste | qty ≤ lot, reason required | `Not enough in lot.` / `Reason required.` | `saveWaste()` |
| Lot empty | `lot.qty > 0` | `Lot empty.` | lot actions |
| Day entry date | not in the future | `Entries can't be booked in the future.` | all OE handlers |
| Day entry sale | lot + qty + rate | `Pick lot, quantity, rate.` | `saveSaleEntry()` |
| Day entry waste | lot + qty | `Lot & quantity required.` | `saveWasteEntry()` |
| Day entry settlement | payable + amount ≤ remaining | `No payable selected.` / `More than remaining payable.` | `saveSettlementEntry()` |
| Day entry voucher | amount `> 0` | `Enter amount.` | `saveVoucherEntry()` |
| Day entry deduction | institution + amount ≤ pending | `Pick institution & amount.` | `saveDeductionEntry()` |
| Post-lock adjustment | qty or amount `> 0` | `Enter quantity or amount.` | `savePostLockAdjustment()` |
| Correction window | `daysBetween(trx.date, TODAY) <= 1` | `Correction window closed.` / `Correction window closed — locked deal.` | `startCorrection()`, `trxRevisionMenu()` |
| Memo | at least one allocation | `Nothing allocated to bill yet.` | `memoShare()` |
| Photo attach | a file was chosen | `No photo selected.` | `saveTrxPhoto()` |
| Category / reason add | non-empty | `Name required.` | editors |
| Import | valid JSON, `v === 2`, `transactions` array | `Not valid JSON.` / `Version mismatch — import rejected.` | `runImport()` |
| Delete all | typed text equals `DELETE` | `Type exactly "DELETE" to confirm.` | `deleteAllConfirmed()` |
| Image pick | ≤ 120 KB | `Image over 120KB — using icon fallback.` | image picker |
| Review → confirm | zero warnings | `Resolve warnings first.` / `Allocate everything first — see warnings.` | review buttons |

### 9.1 Destructive confirmations (exact copy)

| Action | Title | Body | Buttons |
|---|---|---|---|
| Remove cart line (DL-01) | `Remove Item` | `Remove {item} from today's cart? Allocated qty (if any) is discarded.` | Cancel / **Remove** (danger) |
| Discard cart (DL-10) | `Discard Cart` | `Discard today's cart? Entered items will be removed and inventory reservations released.` | Cancel / **Discard** (danger) |
| Stale cart on launch (DL-09 reuses DL-10) | `Discard Cart` | `You have an unconfirmed cart from {date}. Start today fresh (old cart will be discarded) or open the old cart?` | **Start Fresh** / **Open Old Cart** |
| Delete all data (DL-07) | `Delete All Data` | `This will permanently erase all transactions, inventory, institutions, and settings. Cannot be undone.` + typed `DELETE` gate | Cancel / **Delete All** (danger) |
| Deactivate institution (DL-04) | `Deactivate Institution (DL-04)` | `Deactivate {name}?` + if open business: `Open business: N active tender(s), ₹X accrued dues.` + `New carts and allocations to this institution will be blocked. Receivables, contract P&L, and history stay intact and collectable.` | Cancel / **Deactivate** (danger) |
| Deactivate item (DL-05) | `Deactivate Item (DL-05)` | `Deactivate {name}? The item will be hidden from lists. Old transactions still show the item — history is preserved.` | Cancel / **Deactivate** (danger) |
| Delete tender | `Delete Tender (DL-03)` | `Delete {name} ({number})? Its N fixed prices will be removed too.` + `Historical transactions are not affected — they keep the rates frozen on the day of supply.` | Cancel / **Delete Tender** (danger) |
| Delete tender price | `Delete Tender Price (DL-03)` | `Remove the fixed price for {item} — ₹X per {unit}?` + same historical note | Cancel / **Delete Price** (danger) |
| Delete expense (DL-02) | `Delete Expense (DL-02)` | `Delete {type} — ₹X? This removes the expense from today's entry and from P&L.` | Cancel / **Delete** (danger) |
| Sign out | `Sign out?` | `Your ledger stays saved on this device; you'll need your mobile number and PIN to get back in.` | Cancel / **Sign out** (danger) |
| Confirm cart (DL-06) | `Confirm & Complete` | `Confirm the entire daily cart once. This cannot be undone per item.` | Cancel / **Confirm & Complete** (primary) |

---

## 10. Empty / loading / error states

**Explicit empty states today** (copy verbatim):

| Screen | Copy |
|---|---|
| Home (no cart) | `Your cart is empty` / `Select vegetables to begin today's purchase.` + Browse Vegetables + Open Cart |
| Cart | `Your cart is empty` / `Start by selecting vegetables.` + Browse Vegetables |
| Dues (no institutions) | `No institutions yet` / `Add an institution to start tracking dues.` |
| Inventory | `No stock here` / `Day remainings and manual stock appear as lots here.` |
| Lot detail movements | `No entries` |
| Sell from inventory (no lots) | `No stock available to sell.` / `Stock arrives from a confirmed cart's remaining quantity, or use Add Stock Manually on the Inventory screen.` |
| Waste (no lots) | `No stock available to mark as waste.` / same second line |
| History | `No transactions in this range` / `Complete a cart or add a day entry.` |
| Attention Center | `All clear ✓` / `No active alerts right now.` |
| Report detail | Per-report empty heading/body from the `empty` tuple (e.g. custom range offers **Change range**) |
| Tender prices (no context) | Guarded empty state (added for crash B-01 in `AUDIT.md`) |

**No empty state** (renders blank or zeroes): Vegetables no-results (grid simply has no cards), Item detail (always has an item), Allocate (needs a cart line), Expenses (empty list), Payables (empty list), P&L group tabs (rows filtered out), Settings children (static).

**Loading:** the prototype is fully synchronous. The only loading affordances are the splash skeleton and button text swaps (`Signing in…`, disabled during a 480 ms `setTimeout`). Everything else renders instantly from memory.

Once a backend exists, these need real loading states: login (auth round-trip), splash (state hydration), Home (aggregate fetch), every list screen (items, institutions, inventory, history, dues, payables, reports), transaction detail (snapshot + photos), report detail (server-side aggregation), data management (backup/restore/import progress), photo upload. Use skeletons that mirror the final layout (`.skeleton` already exists as a style) rather than spinners, and keep the prototype's empty copy as the "loaded but empty" state.

**Error:** today every failure is a toast. There is no retry UI, no offline banner, no partial-failure state. Add: an error state per async screen (message + Retry), a global offline indicator (the app is local-first today, so offline = normal), and field-level errors via `TextFormField` validators instead of toasts for form validation.

---

## 11. What is mocked

| Feature | Prototype reality | Flutter target |
|---|---|---|
| Auth | Demo PIN. `DEMO_PIN = '1234'`, `DEMO_PHONE = '9876543210'`, any 10-digit phone accepted. "Forgot PIN" opens a modal saying there is no server. | Real auth (phone OTP or device biometrics + PIN hash). Keep the `AuthNotifier` seam; do not ship the demo bypass. |
| Network | **None.** Zero `fetch`/XHR at runtime. Google Fonts + Hugeicons CDN are the only external URLs (and item photos are embedded precisely to avoid remote loads — see `AUDIT.md` §8). | Optional sync API behind `LedgerStore`; fonts/icons bundled. |
| Backup | `dataBackup()` builds `{meta, state}` JSON and triggers a browser download (`a.download`), sets `lastBackup = TODAY`. | `path_provider` + `share_plus` file export, or OS document picker; keep the same JSON envelope (`meta.dataVersion = 2`) for compatibility. |
| Restore / import | Textarea paste of JSON, validated for `v === 2` and `transactions` array, then replaces all state. | `file_picker` + same validation + a destructive confirm. |
| Export CSV | Client-side CSV of the ledger (`purchase, allocation, remaining, loss, expense, entry, payable, payment` rows), downloaded via Blob. | `csv` package + file share; columns must mirror the ledger exactly (spec §2.11). |
| Report export | `exportReport()` scrapes rendered DOM rows into CSV text, then `navigator.share` or clipboard. | Generate CSV from the model, not the widget tree. Add PDF via `pdf`/`printing` (not present today). |
| Share memo | `navigator.share({title, text})`, else `navigator.clipboard.writeText`, else no-op; stamps `memo.shared/sharedAt/sharedRevision`. | `share_plus`; same memo text template; persist the same shared-revision stamp so the outdated-memo warning still works. |
| Photo evidence | `FileReader` → data URI stored in `trx.photos[]`; 120 KB cap; viewed in DL-14. | `image_picker` + compress to ≤120 KB (or store a file path and copy into app documents); upload when a backend exists. |
| Item photos | 16 embedded 256 px JPEG data URIs in `ITEM_IMAGE_DATA`; ~300 KB of the file. Custom items fall back to an emoji. | Bundle the same 16 images as assets; store `assetName` or file path instead of a data URI. |
| Alerts | Computed client-side on every render (`attentionAlerts()`) from inventory age, payables, tender dates, bill age, `lastBackup`. No scheduler, no OS notifications. | Same pure function; optionally add `flutter_local_notifications` later. Keep in-app as the source of truth. |
| Currency | Hard-coded `₹`; `formatINR` Indian grouping; `currency` field is read-only in Business Settings. | Keep ₹ and Indian grouping; make the formatter a single utility so localisation is possible later. |
| "What's New" | `UPDATE` constant + `localStorage` seen-flag (`vegContractorUpdateSeenV1`). | Remote config or bundled changelog with the same dismiss semantics. |
| Toast / dialogs | DOM overlays, no routing. | `ScaffoldMessenger` + `showDialog`/`showModalBottomSheet`. |

---

## 12. Known gaps / next steps

1. **No backend.** Single device, single ledger, no accounts beyond the demo gate.
2. **No multi-device sync or conflict resolution.** Two devices diverge silently.
3. **Single user.** No roles, no audit of *who* changed what (revisions record *what*, not *who*).
4. **Light theme only.** No dark mode, no theme switch (`Appearance` is mentioned in spec S37 but not implemented).
5. **English only.** No i18n scaffolding; all copy is inline strings.
6. **No OS notifications.** Attention Center is in-app only; alerts are recomputed on render, so nothing fires while the app is closed.
7. **No PDF export.** CSV and JSON only; the UI copy overpromises PDF.
8. **Currency fixed to ₹**, Indian digit grouping, no locale switch.
9. **Rounding order differs** between `cartSummary()` (round totals) and `finalizeCart()` (round per row) — unify at commit.
10. **Alert coverage is 6 of the spec's 13 types** (see §2).
11. **No image compression/downscaling pipeline** beyond the 120 KB reject-with-fallback.
12. **No pagination or virtualisation.** History and P&L render everything; fine for a demo, not for years of data.
13. **UI scratch keys persist** in the state blob (§4.2).
14. **`#screen-completed` and setup-complete have non-standard back targets** — encode deliberately in the router.
15. **Accessibility is partial.** Focus rings, 44 px targets, `aria-label`s and `role="dialog"` exist; no screen-reader pass, no dynamic type scaling beyond CSS clamps.

---

## 13. Suggested folder structure

```
lib/
├── main.dart
├── app.dart                      # MaterialApp.router, theme, tokens
├── router.dart                   # go_router: StatefulShellRoute + redirects
├── core/
│   ├── theme/
│   │   ├── app_tokens.dart       # ThemeExtension (light + dark slot)
│   │   ├── app_theme.dart
│   │   └── glass_surface.dart
│   ├── format/
│   │   ├── money.dart            # formatINR, fmtSign, r2
│   │   ├── quantity.dart         # formatQty, qtyByUnit
│   │   └── dates.dart            # todayISO, addDays, daysBetween, fmtDate
│   ├── validation/messages.dart  # verbatim copy from §9
│   └── ids.dart                  # nextId(seq, kind)
├── data/
│   ├── models/                   # Item, Institution, Tender, Cart, Transaction, …
│   │   ├── app_state.dart
│   │   ├── cart.dart
│   │   ├── transaction.dart
│   │   ├── inventory.dart
│   │   ├── dues.dart
│   │   └── settings.dart
│   ├── ledger_store.dart         # abstract + LocalLedgerStore (shared_preferences)
│   ├── migrate.dart              # normalizeState() equivalent, v2
│   ├── seed.dart                 # seedState() equivalent
│   └── engine/
│       ├── cart_calc.dart        # cartSummary, itemAgg, lineCost, lineSupply
│       ├── allocation.dart       # effectiveAllocQty, cost share
│       ├── inventory.dart        # reserveFEFO, availForItem, lot value
│       ├── dues.dart             # billPaid, billPending, instSummary, accruedPending
│       ├── pnl.dart              # overallTotals, plAggregator, groupBy*, deductionsOn
│       └── alerts.dart           # attentionAlerts
├── state/
│   ├── ledger_repository.dart
│   ├── ledger_notifier.dart
│   ├── cart_notifier.dart
│   ├── institutions_notifier.dart
│   ├── inventory_notifier.dart
│   ├── dues_notifier.dart
│   ├── settings_notifier.dart
│   └── auth_notifier.dart
├── widgets/
│   ├── app_bar.dart              # title, back, cart chip
│   ├── bottom_nav.dart
│   ├── sticky_actions.dart
│   ├── app_button.dart           # primary/secondary/danger/disabled
│   ├── app_field.dart            # label + input + error + currency prefix
│   ├── app_card.dart
│   ├── list_row.dart
│   ├── metric_card.dart / metric_grid.dart
│   ├── status_badge.dart
│   ├── item_card.dart            # photo + scrim + quick add
│   ├── alloc_veg_row.dart
│   ├── progress_bar.dart
│   ├── empty_state.dart
│   ├── numeric_keypad.dart
│   ├── pick_list_sheet.dart
│   ├── confirm_dialog.dart       # copy from §9.1
│   └── toast.dart
└── features/
    ├── auth/login_screen.dart
    ├── onboarding/{splash,welcome,profile,initial_items,initial_institutions,setup_complete}_screen.dart
    ├── home/home_screen.dart
    ├── items/{item_grid,item_detail,item_editor,item_management}_screen.dart
    ├── cart/{cart,allocate,expenses,review,confirm,completed}_screen.dart
    ├── institutions/{list,editor,profile,tender_list,tender_prices,receivables}_screen.dart
    ├── inventory/{inventory,lot_detail}_screen.dart
    ├── dues/{dues,payables}_screen.dart
    ├── history/{history,transaction_detail,day_entry_detail}_screen.dart
    ├── reports/{reports,report_detail,profit_loss}_screen.dart
    ├── settings/{settings,item_management,business,app,categories,reasons,data,about}_screen.dart
    ├── attention/attention_center_screen.dart
    └── more/more_hub_screen.dart
```

**Build order**

1. **Models + JSON codec** — port `seedState()` field-for-field; write `fromJson`/`toJson`; round-trip the seeded JSON in a test.
2. **Engine** — port `r2`, `formatINR`, `qtyByUnit`, `lineCost`/`lineSupply`/`itemAgg`, `cartSummary`, `reserveFEFO`, `billPaid`/`instSummary`, `overallTotals`, `plAggregator`. Unit-test against the seed: ₹34,000 investment, ₹43,500 revenue, ₹3,000 expenses, ₹37,000 total cost, ₹6,500 profit, 14.94 % margin.
3. **Repository + migrate** — `normalizeState()` equivalent, `v: 2` gate, import/export envelope.
4. **Theme + tokens + shared widgets** — `AppTokens`, buttons, fields, cards, list rows, metrics, badges, empty state, toast.
5. **Shell + router** — five tabs, back map, redirects for auth/onboarding.
6. **Tabs** — Home, Items, Cart, Inventory, More (static-first, then wired).
7. **Cart flow** — item detail → cart → allocate (FEFO) → expenses → review → confirm → completed, including correction mode and revision snapshots.
8. **Dues** — receivables, bills, payments with allocation FIFO, deductions, credit balance, payables.
9. **Reports / P&L** — 8 P&L tabs, 14 report types, CSV export.
10. **Settings + data management** — thresholds, editors, backup/restore/import, delete-all gate.
11. **Polish** — loading/error states, a11y pass, dark tokens, PDF export, alert coverage to 13 types.
