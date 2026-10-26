# VEGETABLE CONTRACTOR BUSINESS MANAGEMENT APP
## Complete End-to-End UI/UX Design & Application Build Specification (v2.9)

> **Document status:** Single source of truth. This version consolidates the earlier 101-section draft into a non-repetitive, internally consistent spec. One canonical worked example (Section 16) is used everywhere; all figures in this document derive from it.
>
> **Change summary from v1:** merged 4 duplicate workflow diagrams into 1 (§4), replaced mixed example numbers with one consistent dataset, added previously-missing rules (expense allocation, rounding, unit handling, duplicate items, cart lifecycle, offline), restructured the 313-item screen list into a grouped screen/state map, added a table of contents.
>
> **Change summary v2.1:** added **Payments & Receivables for tender-period billing/dues/delivery memos (§7.5)** — institutions pay 1–2+ months after supply; added **same-day correction window (§2.8)**; added **revenue-vs-cash rule (§2.9)**; added **irregular supply-frequency rule (§4.2)**; added **Telugu-first localization (§5.4)**; added **bag/sack purchase-unit conversion (§6.3)**; added **per-item shelf-life/old-stock alerts (§8.1, §14)**.
>
> **Change summary v2.2:** **correction window extended to end of the day after the transaction, with full revision snapshots (§2.8)**; memos/bills generated **only when the institution asks** — shared as screenshot/PDF with a Shared ✓ marker and outdated-re-share warning (§7.5.5); **Record Payment gained deduction lines** for short payments (§7.5.3, §2.2 formulas 14–15); **Post-Lock Adjustments** for rejections/weight differences discovered after lock (§7.5.7, §2.10); **multiple tenders per institution** with own price lists and dues periods (§7.2); FEFO lot consumption (§8.4); last-rate prefill + optional purchase source (§6.3); backup reminder (§13, §14).
>
> **Change summary v2.3:** **Correction Mode separated from the daily cart** — both can coexist (§2.8); Home pending figure **split: Accrued vs Billed pending** (§6.1, §7.5.6); **revision/payment reconciliation** — FIFO re-application to the latest revision + institution **credit balances** (§7.5.3); **blended delivered-cost rate** for items mixing inventory + new purchase (§2.2 formula 4, §8.4, §16.3 recomputed); **"Tender Ending Soon" alert** + period-ended billing chip (§7.2, §7.5.2, §14); deduction-reason set editable in Settings (§7.5.3, §13); map rows for Dues Overview & Tender Management (§17).
>
> **Change summary v2.4:** sharpened product intent — **contractor-only tool whose main purpose is maximizing profit on institution tenders** (§1.1, §1.4, §12); **Contract-wise P&L** — profit/loss per tender per institution incl. deductions (§11, §12, §16.8); **Other Day Entries** — sales/waste/adjustments/payable settlements on no-cart days become date-anchored History rows feeding P&L (§10.1, §16.9); **Payables** — optional seller credit (khata) on purchases with settlement tracking and cash picture (§9.1, §14); **inventory reservations** held while a cart is open, released on discard/revision (§8.4, §6.3); Home **Correction Mode chip** + TODAY-cards rule (§6.1); tender picker defined **per allocation row** with default rule (§6.5); missing-price add asks **which tender** (§6.5); search matches Telugu names (§6.2); **version-tagged backups** (§13); optional **photo evidence** per transaction/adjustment (§10); **Spoilage by Item** report (§12).
>
> **Change summary v2.5:** **Other Day Entries get the same correction window** as carts — errors fixed by end of the next day, locked after, repair-by-compensating-entry (§2.8 rule 6); **Data Recording Standard (§2.11)** — every fact stored as a structured, dated, attributed row so the ledger is AI/analysis-ready from day one; **§21 Future Roadmap** — Data → Insight → Predictive → AI assistant, explicitly outside the v1 build but reading exactly the data §2.11 guarantees; **multi-year performance** — pagination/virtualized lists and precomputed aggregates (§10, §11); **inactive institutions stay safe** — deactivation warns about active tenders/unbilled (accrued) dues (§7.4, §15.2); **quality/grade note** optional on purchase lines — the field future price-vs-quality AI learns from (§6.3); single editable **reason editor** shared by deductions, waste/loss and rejection reasons (§13); **Home pending-split example re-derived from the canonical dataset** (§6.1, §7.5.6).
>
> **Change summary v2.6:** **Iconography mandated — Hugeicons** (§5.5): single library, Stroke Rounded style family only, color/weight carries state instead of style switching, icons always paired with Telugu-first text, per-slot assignment from the §17 screen map; reflected in §19 checklist.
>
> **Change summary v2.9:** **all ten open gaps (G-0–G-10) resolved with recorded decisions**: **Record Payment overpayment intent step** — when Deductions + Received exceed pending dues, confirm "Received ₹X exceeds pending ₹Y — ₹Δ becomes credit balance. Record it?" (Record = credit path / Adjust = fix the form) between the plain validation and the overpay path (§7.5.3, §15.1); the **three alert thresholds get a named home — Settings → App — with defaults**: Long-Pending Dues 30 · Tender Ending Soon 7 · Seller Payable alert 3 days (§13, §14); **Discard cart** confirmation officially added to §17's modal list, aligning §15.2 with the DL-10 registry; **custom-item images defined** — bundled seed set or camera/gallery pick with thumbnail preview (§5.3, §17); **Welcome/Intro slide copy specified** (2 plain-English slides + always-visible Skip, §4.1); **seed catalog enumerated** — exactly the §6.2-named items ship pre-seeded, §16.1 canonical overrides (leafy 1 · Tomato 3 · Potato 14 · Onion 30 · veg default 7 · groceries none) (§6.2); **Attention Center confirmed as a full-screen list** opened by the Home badge, alerts **in-app only in v1** — OS-local notifications deferred post-v1 (§14, §21 rule 4); **no standalone expenses in v1** — expenses attach to a cart day (§9, §2.5), the four other-entry types do not grow (§10.1). This consolidates the SCREENS_MASTER_LIST gap register (document, not new screens — all counts unchanged).
>
> **Change summary v2.8:** **Mobile-phone-only — the tablet/desktop variant removed** (§3): §3.2 Tablet/Desktop (sidebar navigation, multi-column layouts) deleted; §3.1 is now the only navigation/layout surface (floating bottom nav, full-width single-column); §5.1's "2 columns on tablets/large screens" note removed; §19 checklist updated. No §16 figures or accounting rules affected. (v2.7 change-summaries remain below as history; v2.8 supersedes them for layout.)
>
> **Change summary v2.7:** **Localization removed — the app is English-only** (§5.4): Telugu default/secondary dropped everywhere; the Language selection step is removed from first-time setup (§4.1) and Settings (§13, §17); item master data carries a single name field (§5.3, §5.4, §6.2, §13); search matches item names, no secondary names (§6.2); memos/bills, alerts and all documents in English (§7.5.5, §14); icons pair with text in plain English (§5.5); alert name harmonized to "**Payment Due Alert**" §7.5.6 ↔ §14; §19 checklist updated. (Earlier change-summaries mention Telugu because they describe older versions — v2.7 supersedes them.)

---

## TABLE OF CONTENTS

1. [Product Overview](#1-product-overview)
2. [Core Business Model & Accounting Rules](#2-core-business-model--accounting-rules)
3. [Navigation](#3-navigation)
4. [The Daily Workflow (Single Canonical Flow)](#4-the-daily-workflow-single-canonical-flow)
5. [Design System](#5-design-system)
6. [Screens — Core Daily Loop](#6-screens--core-daily-loop)
7. [Screens — Institutions (incl. Payments & Receivables)](#7-screens--institutions)
8. [Screens — Inventory](#8-screens--inventory)
9. [Screens — Expenses](#9-screens--expenses)
10. [Screens — History](#10-screens--history)
11. [Screens — Profit & Loss](#11-screens--profit--loss)
12. [Screens — Reports](#12-screens--reports)
13. [Screens — Settings & Data Management](#13-screens--settings--data-management)
14. [Notifications & Attention Center](#14-notifications--attention-center)
15. [Validation, Errors & Edge Cases](#15-validation-errors--edge-cases)
16. [Worked Examples (Canonical Dataset)](#16-worked-examples-canonical-dataset)
17. [Master Screen & State Map](#17-master-screen--state-map)
18. [UX State Rule](#18-ux-state-rule)
19. [Mandatory Build Checklist](#19-mandatory-build-checklist)
20. [Final Product Experience](#20-final-product-experience)
21. [Future Roadmap — Data → Insight → AI](#21-future-roadmap--data--insight--ai)

---

# 1. PRODUCT OVERVIEW

## 1.1 What This App Is

A **mobile-phone-only management application for a single Vegetable Contractor** — phone screens only, no tablet/desktop variant. It is a **contractor-only tool** — no institution, employee, or third party ever uses it; everyone else stays on WhatsApp calls and paper. Its **main purpose: maximize the contractor's profits on institution tenders** — by protecting margins at entry (knowing each institution's fixed rate before buying), exposing loss leaks the same day (rejections, waste, unprofitable items/institutions), recovering every rupee owed (dues, deductions, margin discipline), and preventing money from silently leaking through double-counting, missed collections, or forgotten stock.

He personally uses it daily to manage:

- Vegetable/grocery purchasing
- Institution-wise supply allocation
- Tender pricing
- Inventory (rejected/remaining/reusable stock)
- Expenses
- Revenue, profit/loss (daily and overall)
- **Receivables** — dues by tender period, collectable long after supply
- **Memos/bills on demand** — generated only when the institution asks, shared as screenshot/PDF
- **Optional:** seller payables (credit/khata purchases, §9.1)
- Historical business performance

## 1.2 The Questions It Answers Every Day

The app must answer these questions with **minimal manual calculation**:

| # | Question | Answered by |
|---|----------|-------------|
| 1 | What am I purchasing today, at what quantity and price? | Item list → Cart |
| 2 | Which institutions receive what, at what fixed rate? | Allocation |
| 3 | What is my revenue? | Allocation auto-calc |
| 4 | What is my investment and total cost? | Cart + Expenses |
| 5 | Which institution is profitable/unprofitable? | Review + P&L |
| 6 | Which vegetable is profitable/unprofitable? | Review + P&L |
| 7 | What was rejected and what happens to it? | Rejected decision |
| 8 | What can I reuse from inventory next time? | Inventory module |
| 9 | Did I profit or lose today — and overall? | Home + Review + P&L |
| 10 | Who owes me how much, since when, and what have I collected? | Receivables & Payments (§7.5) |
| 11 | How much do I owe mandi sellers, and when to settle? | Payables (§9.1) |
| 12 | How do I maximize profit on today's and every day's supplies — and where is profit leaking? | Home + Review + P&L + Reports |
| 13 | For every contract (tender per institution), how much profit or loss did I make after deductions? | Contract-wise P&L (§11, §12) |

## 1.3 The Single User: Contractor

There is exactly **one user**. He can:

- Create/edit institutions and their fixed tender prices
- Add/edit/deactivate items
- Create daily carts, enter quantities, purchase rates, allocations
- Add/edit/delete expenses
- Decide rejected-quantity outcomes (inventory vs waste)
- Use, sell, or mark-waste inventory
- Confirm the daily cart once (correctable until end of the next day — §2.8)
- Generate memos/bills **on demand, only when the institution asks** — shared as screenshot/PDF
- Record received payments **with deductions** and track pending dues
- Review history, institution/vegetable performance, overall P&L

The app must be simple enough for daily use while covering the complete business workflow.

## 1.4 What This App Is NOT

| It is NOT | Therefore never include |
|-----------|------------------------|
| An institution-facing or shared tool | Institution logins, portals, dashboards for buyers, notifications to institutions |
| An e-commerce marketplace | Discounts, offers, wishlist, ratings, reviews, "Buy Now", checkout, ads |
| A multi-user system | Admin/staff/customer/institution accounts, RBAC, approval workflows |
| An ERP | Complex modules, GL code, depreciation, multi-branch |
| An inventory-heavy warehouse app | Full WMS features beyond the contractor's simple stock |

Vegetable images exist for one reason: **fast visual identification and selection** — styled like a premium produce store app (§5.3), but item cards carry **zero marketplace functions**: no Buy Now, no customer cart/checkout, ratings, reviews, offers, or shipping. Only the contractor shops here.

---

# 2. CORE BUSINESS MODEL & ACCOUNTING RULES

This section is the accounting heart of the app. Every screen must obey it.

## 2.1 The Two Price Types — Never Confuse Them

| Price | Meaning | Who sets it | Changes how often | Where entered |
|-------|---------|-------------|-------------------|---------------|
| **Purchase Rate** | What the contractor pays to buy the item | The market/seller | Daily — changes every day | Item detail entry |
| **Fixed Tender Rate** | What the institution pays the contractor | The institution contract | Fixed per contract, edited manually | Institution profile |

Example: Tomato purchase rate ₹30/kg today; Institution A tender rate ₹40/kg; Institution B ₹45/kg; Institution C ₹38/kg.

**Never assume one global selling rate. The same vegetable has different rates at different institutions.**

## 2.2 Financial Formulas

These formulas are mandatory and used by every calculation surface:

| # | Value | Formula |
|---|-------|---------|
| 1 | Purchase Cost | `Quantity × Purchase Rate` |
| 2 | Institution Allocation % | `Institution Quantity ÷ Total Item Quantity × 100` |
| 3 | Institution Revenue | `Institution Quantity × Institution Tender Rate` (or Custom Rate if overridden) |
| 4 | Allocated Purchase Cost | `Delivered Cost Pool × Allocation %` — per item. **Delivered Cost Pool = New Purchase Cost + Inventory Used Cost** for the item; **blended rate = Pool ÷ item quantity** (used when an item mixes inventory + fresh purchase — source per institution is informational; money draws from the pool, so inventory cost is recognized exactly once: §8.4, §16.3) |
| 5 | Total New Purchase Cost | Sum of all newly purchased item costs |
| 6 | Inventory Used Cost | `Inventory Quantity Used × Inventory Original Purchase Rate` |
| 7 | Total Cost | `New Purchase Cost + Inventory Used Cost + Additional Expenses` |
| 8 | Total Revenue | Sum of Institution Revenue + applicable Inventory Sales Revenue |
| 9 | Profit / Loss | `Total Revenue − Total Cost` |
| 10 | Profit Margin | `Profit ÷ Revenue × 100` (undefined/blank when revenue = 0) |
| 11 | Inventory Value | `Available Inventory Quantity × Original Purchase Rate` |
| 12 | Waste Loss | `Wasted Quantity × Original Purchase Rate` |
| 13 | Inventory Sale Profit | `Sale Revenue − Sale Quantity × Original Purchase Rate` |
| 14 | Net Collectable (Dues) | `Period Billed − Deductions − Payments Received` |
| 15 | Deduction (P&L effect) | Recorded as a negative-revenue **Deductions line on the deduction's own date** — aggregates show `Effective Revenue = Supply Revenue − Deductions` (§7.5.7) |

## 2.3 No-Double-Counting Rule (Critical)

If stock already exists in inventory (purchased on a previous day), reusing it must **NOT** create a second purchase cost:

- The original purchase cost stays attached to the inventory stock.
- When reused: revenue is generated on reuse, and the **Inventory Used Cost** (formula 6) is recognized in that day's Total Cost — once.
- Prevents artificial duplicate investment and inflated totals.

## 2.4 Historical Rate Preservation

When a cart is confirmed, the transaction stores **the actual rates, quantities, allocations, expenses, and results used at that time** — as immutable snapshots.

Example: 20 October → Tomato → Institution A → ₹40/kg. If the institution's tender rate later changes to ₹45/kg, the 20 October transaction must still show ₹40/kg. Master-data edits must **never rewrite history.** Completed transactions stay re-openable only within the correction window (§2.8); afterwards they are permanently read-only.

## 2.5 Expense Allocation Rule

Every expense belongs to exactly one of:

1. **Assigned** — the contractor explicitly assigns it to an institution or item during entry (optional field).
2. **Day-level** — the default; affects only the day's total profit/loss.

Unassigned expenses are **never auto-distributed** across institutions. Institution profit displays: `Institution Revenue − Allocated Cost − Expenses Assigned to That Institution` (assigned expenses only). Day-level expenses appear only in the day's overall totals.

## 2.6 Inventory Value, Waste and Status

- Inventory Value (formula 11) is **neither revenue, profit, loss, nor cost** — it is the value of available stock on hand.
- Rejected/remaining stock is **NOT automatically a loss**. It becomes:
  - **Inventory** (recoverable value), or
  - **Loss/Waste** (formula 12) — only when the contractor marks it so.
- The contractor always chooses. Inventory represents potentially recoverable value.

Inventory statuses: `Available`, `Partially Used`, `Sold`, `Damaged`, `Waste/Loss`.

## 2.7 Precision, Rounding & Formatting

- Quantities: allow **fractional values** (e.g., 0.5 kg). Accept up to 2 decimals.
- Currency: store exact values internally; display rounded to whole rupees (or 2 decimals if fractional); never let displayed rounding change stored totals — **totals are computed from exact values, not summed from rounded displays.**
- Allocation % displayed to 1 decimal (e.g., 37.5%), computed with full precision.
- Currency format: ₹ with Indian digit grouping (e.g., ₹5,00,000).

## 2.8 Data Integrity Rules

1. **Correction window:** a confirmed cart remains **re-openable until the end of the calendar day *following* the transaction date** — the contractor can fix typos (rates, quantities, allocations), add items/expenses, or change inventory decisions; totals recalculate and the reference gains a revision suffix (R2, R3 …). This covers the midnight seam: a cart confirmed after 00:00 still belongs to its business date (rule 4) and stays correctable through the following day. After the window, the transaction is **permanently locked** (Locked badge) — later corrections happen through **Post-Lock Adjustments** (§7.5.7), never by editing history. Corrections run inside dedicated **Correction Mode** (rule 2), and a downward revision triggers the collected-money check (§7.5.3).
2. **Two cart types:**
   - **Daily cart** — the unconfirmed workspace for the current date. **Only one daily cart exists at a time, and the Cart tab always shows the daily cart.**
   - **Correction Mode** — a re-opened confirmed transaction within its window. It is **independent of the daily cart and can coexist with it**: it opens only from Home ("Edit Today's Cart") or Transaction Detail, never via the Cart tab. Banner: *"Editing TRX 20-Oct (R2) — all changes stay date-stamped to the transaction's original date."* Recommended practice: finish a correction before starting the new day's cart.
3. All data is stored **locally-first** (works fully offline) with optional Backup / Restore / Export (Section 13).
4. A "business day" = the date the cart was **started**; a cart may be completed any time after start.
5. **Every re-confirm stores a full revision snapshot** — nothing is overwritten. Transaction Detail offers a revision selector (R1/R2/…); dues and memos always follow the **latest** revision, with an outdated-bill warning if a shared document exists (§7.5.5).
6. **Standalone entries follow the same window.** Other Day Entries — inventory sales, waste marks, post-lock adjustments, seller payment settlements (§10.1, §9.1, §7.5.7) — are re-openable **until the end of the calendar day following their date**, then locked on the same terms. After the window, a wrong money amount is repaired by a **compensating entry** (a counter-adjustment or counter-settlement) dated on the repair day — history is never rewritten (§2.10 principle).

## 2.9 Revenue vs Cash (Payment Reality)

Institutions pay **only after the tender period ends — typically one to two months after supply, sometimes longer.** Therefore:

- **Profit** is recognized when the cart is confirmed (§2.2 formulas).
- **Cash received** is tracked separately, as payment records per institution (§7.5).
- Never display the two as the same thing. **Profit ≠ money in hand**; the Home dashboard shows the pending figure split into **Unbilled (accrued)** and **Billed pending** (§6.1, §7.5.6).
- The same wall applies to the **purchase side**: on-credit purchases (seller khata, §9.1) count in Investment and profit at the purchase date, while the settlement moves only cash. Collections − payments (incl. seller settlements) − expenses show "money in hand"; profit never does.

## 2.10 Post-Lock Adjustments Principle

Locked transactions are **never edited** (§2.8). Discoveries that arrive later — next-day deliveries, late rejections, gate-weigh differences (allocated 200 kg, weighed 198 kg), quality cuts revealed at collection — become **Adjustment records** dated on the day they are discovered:

- Quantity effects → inventory (§7.5.7).
- Money effects → deductions on dues + a **Deductions line in P&L** on the adjustment's date (formula 15), keeping aggregate results truthful without rewriting the supply day.

## 2.11 Data Recording Standard (Analysis & AI-Ready from Day One)

Every fact the app stores must be **machine-usable forever** — the contractor may later train AI on this ledger (§21). Non-negotiable storage rules:

1. **Every fact is a typed row** with: date · item · quantity (base unit) · rate (₹ per base unit) · source/counterparty (seller name for purchases; institution + tender for supplies) · reason codes. Nothing numeric is ever stored as free text.
2. **Reasons come from editable dropdown sets** (deductions, waste/loss, rejection — §13), never typed prose — so patterns are countable.
3. **The ledger is append-only**: revisions and compensating entries (§2.8) are added as new records; original records survive untouched — the full history of *what happened, when* stays queryable.
4. **Reference integrity**: item, institution, tender, expense, reason — every reference resolves by ID; deactivated/deleted masters keep their IDs (§7.4, §13) so historical rows never dangle.
5. **Export mirrors the ledger**: Export Data (§13) yields structured CSV/JSON whose columns directly map to these rows (one row per purchase line, allocation, expense, payment, settlement, waste, sale, adjustment) — the exact training/analysis format; no scraping or re-keying needed.

Historical business performance (§1.1) is therefore not just viewable — it is **systematically capturable**: daily prices per seller, per item; tender outcomes per institution; payment behavior per contract; waste and rejection patterns by reason, season, and seller (§21).

---

# 3. NAVIGATION

## 3.1 Mobile (Only Experience)

Floating **frosted-glass bottom navigation** (§5.1) with 5 destinations:

| Tab | Contents |
|-----|----------|
| **Home** | Today's dashboard, financial cards, quick actions, attention center |
| **Vegetables** | Item selection (visual grid, search, filters) |
| **Cart** | Current daily cart (badge shows item count) |
| **Inventory** | Reusable/rejected/remaining stock |
| **More** | Institutions, **Payments & Dues (§7.5)**, **Seller Payables (§9.1)**, Profit & Loss, History, Reports, Settings |

- Persistent cart indicator on Home and other screens (e.g., "Cart · 3 Items · Investment ₹34,000" — Hugeicons cart icon paired with text, §5.5 — never an emoji).
- Large touch targets, sticky primary buttons, bottom sheets, large numbers, minimal typing.
- **Mobile-phone only:** the app is built exclusively for phone screens — there is no tablet/desktop sidebar variant, no multi-column desktop layout; the same bottom-nav phone UX is the whole product.

---

# 4. THE DAILY WORKFLOW (SINGLE CANONICAL FLOW)

> This is the **only** workflow diagram in this document. It appears here once and governs everything else.

## 4.1 First-Time Setup Flow (Once)

```
Splash → Welcome/Intro → Business Profile (name, currency ₹, default unit kg)
→ Initial Items (vegetables + groceries, images, units) → Initial Institutions
(+ contracts + tender period + fixed tender prices) → Setup Complete → Home
```

**Welcome/Intro (2 slides, Skip always visible):** plain-English (§5.4) —
1. *"Know your price before you buy."* — every institution's fixed tender rate is known at allocation time, so margins are protected from the first bag.
2. *"Enter once — every rupee tracked."* — today's cart, inventory, dues and waste all start from the same confirm-once record.

## 4.2 Daily Supply Flow (When Supplies Happen)

> **Supply frequency is irregular.** The contractor supplies daily only on some days; other days there is no supply at all. A cart is created **only when supplies occur** — no-supply days simply have no cart and no results. What accumulates across days regardless of frequency: inventory aging, tender-period dues, and receivables (§7.5).

```
Open App → Home (today's status at a glance)
→ Browse Vegetables → tap item image → Item Detail
    → [Inventory check: existing stock offered first]
    → choose Use Inventory / New Purchase (or both)
    → enter Quantity + (for new purchase) today's Purchase Rate
    → Purchase Cost auto-calculated → Add to Cart
→ repeat for all items
→ Open Cart (edit items freely; totals update instantly)
→ Continue → Allocate:
    → per item: allocate quantity to institutions
    → Institution Fixed Tender Rate appears automatically (or missing-price prompt)
    → Custom Rate override optional
    → Allocation % auto-calculated; revenue auto-calculated
→ Handle Remaining quantity (Move to Inventory OR Mark Loss/Waste — contractor chooses)
→ Handle Rejected quantity (same two options, with reason when wasted)
→ Add Expenses (type, amount, optional note, optional assignment)
→ Review Everything (item, institution, inventory, expense summaries + analyses)
→ Today's Financial Result (final preview)
→ Confirm & Complete (ONE confirmation for the whole cart)
→ Cart Completed ✓ → transaction saved (immutable snapshot)
→ Inventory updated → History updated → P&L updated → back to Home
```

## 4.3 Decision Points the Contractor Must Make (Never Automated)

| Point | Choice | Never auto-decided |
|-------|--------|--------------------|
| Inventory vs new purchase | Use stock on hand, buy new, or split | Never silently double-buy |
| Remaining quantity | Move to Inventory OR Mark Loss/Waste | Never auto-loss |
| Rejected quantity | Move to Inventory OR Mark Loss/Waste | Never auto-loss |
| Missing tender price | Add price to institution OR use a Custom Rate for this allocation only | Never borrow another institution's rate |
| Confirm & Complete | One confirmation per day | Never per-item / per-institution confirmation |

---

# 5. DESIGN SYSTEM

## 5.1 Visual Language & Component Library — Soft-Modern Produce-Commerce

**Style:** **Clean + Minimal + Rounded + Card-based + Soft Modern E-commerce** — a fresh, spacious, premium-vegetable-store feel with frosted-glass surfaces, large vegetable images and simple words. Functionally it stays the contractor workflow (§4.2) — the e-commerce look is visual only, never a marketplace (§1.4).

Surfaces & layout:

- **Full-width grid/rows:** content spans the screen edge-to-edge — item browsing is a **single-column, full-width card grid**; carts, institutions, dues, history render as **full-width list rows**. Nothing cramped, boxed-in, or centered-to-a-narrow-column.
- **Rounded everything:** cards 20–24 px radius (large photos clipped by the card's rounded corners); bottom sheets 24 px top radius; **pill-shaped buttons**; inputs 12–16 px radius.
- **Soft depth:** diffuse soft shadows only — elevation signals stacking, never harshness; generous 8-pt spacing rhythm, 16–24 px card padding, wide line height; lots of breathing room.
- **Palette:** light clean background (near-white with a faint cool-vegetable tint); soft green accent for freshness; **green reserved for profit, red for loss/waste** across every surface; dark readable text — always AA contrast, even over glass (§5.2).

**Glassmorphism (frosted, floating surfaces):**

- Frosted-glass treatment for: **sticky top app bar**, **floating bottom navigation**, the **floating cart indicator** (§3.1), **bottom sheets/selectors**, and **modals**.
- Recipe: translucent light fill + backdrop blur + hairline light border + soft shadow — content gently visible behind; **graceful fallback** to a solid light surface where blur isn't supported; glass never reduces legibility (strong enough fill for numbers to stay perfectly readable).

**Component library (restyle, don't shrink):** primary/secondary/danger **pill** buttons, input fields, **numeric keypads** (mandatory for money/qty — not the generic keyboard), select fields, search bars, **product-style item cards** (large image + name + unit + live price hint), financial/institution/inventory cards, tables, tabs, badges/chips, progress bars, bottom sheets, modals, confirmation dialogs, toasts, empty/skeleton/error/success states. All share one typography scale, radius system, spacing rhythm, Hugeicons iconography (§5.5), and interaction pattern.

**One CRUD experience everywhere:** every entity (items, prices, institutions, tenders, expenses, deductions) follows the same soft-modern interactions — full-width **"＋ Add"** rows/cards, tap card → detail → **Edit**, **Delete** always behind a gentle confirmation dialog; **Calendar/History** browsing renders as clean rounded date cards (§10); **Update Record** (the correction window & revisions, §2.8) presents as a friendly **"Edit" → "Update notice (R2)"** flow in plain words — the same ease as updating a shopping list, never a scary ledger edit.

## 5.2 Financial Visualization

Visual hierarchy over charts — large prominent numbers are the primary visualization:

- Profit → large number (green). Loss → large number (red). Revenue/Investment → large numbers.
- Simple charts only where they aid understanding: Daily P/L, Institution comparison, Vegetable comparison, Monthly trend. No complex dashboards.
- Progress indicators for allocation % (simple bars).

## 5.3 Image Usage (Large, Hero-Style)

- **Large, clean, realistic vegetable/grocery photos are the hero of item cards** — produce-commerce presentation: the image fills most of the card (top, consistent 4:3/1:1 crop), with name, unit and live price hint beneath.
- Item Detail shows a large hero image above quantity/rate fields. Inventory and price lists reuse the same photos for instant recognition.
- Photography rules: fresh, bright, appetizing but honest (no gloss or marketplace styling); one canonical image per item (managed in Item Master, §13); image = identification aid only — no badges, banners, or promotional overlays.
- **Custom-item image mechanism (§17 "Image mgmt"):** an item's image is set from the **bundled vegetable/grocery image set** (shipped with the seed catalog, §6.2) or **captured/picked from the camera/gallery** — thumbnail preview before save; one canonical image per item, reused everywhere; no in-app image editing.

## 5.4 Language (English-Only)

- The app ships in **English only** — a single language everywhere, with **no language selection at first-time setup (§4.1) and no Language setting (§13)**.
- All fixed strings — labels, buttons, validation messages, states — are written in plain English consistently on every screen. No other language ships.
- **Simple language rule:** short everyday words, 1–2-line sentences, zero finance jargon ("this much profit" not "net margin realization"; errors ≤ 8 words). Written for a non-technical user.
- Item master data carries a single **Name** per item (e.g., Tomato, Onion — §6.2); there is no secondary-language name field, and search matches item names (§6.2).
- Numbers stay in Western Arabic numerals; ₹ symbol everywhere.
- All generated documents (delivery memos, tender-period bills, reports, exports) are in English.

## 5.5 Iconography — Hugeicons (Mandatory Icon System)

- The app uses **Hugeicons** (hugeicons.com) as its **single icon library** — no other icon sets, framework default glyphs, or hand-drawn SVGs anywhere in the UI.
- **One style family only:** Hugeicons **Stroke Rounded** across every screen — one stroke weight, rounded caps; filled/duotone/solid variants are not mixed in for accent use. State emphasis (active tab, Completed ✓, danger) is expressed through **color and weight of use**, never by switching icon style.
- **Icons always pair with text** (plain-English labels, §5.4) — an icon is a recognition aid, never the sole label; money, quantities, and statuses are never icon-only.
- **Every icon slot is assigned by section 17's screen map:** one icon per navigation tab (Home · Vegetables · Cart · Inventory · More), each primary action, each alert type (§14 — payment/rupee, aging/clock, waste/damage, backup, tender-end/calendar, seller-payable). Same semantic → same icon file, reused everywhere.
- **Sizing & alignment:** 24 px for nav/primary buttons, 20 px inline; 4-px grid alignment; stroke color follows `currentColor` so icons adapt to text, status, and theme colors.
- **Practical rules:** import only the icons actually used; pin one library version for the whole build; respect the chosen Hugeicons license (free or pro pack); no per-screen custom icons.

---

# 6. SCREENS — CORE DAILY LOOP

## 6.1 Home Dashboard

Open the app → understand the situation **within seconds**.

### TODAY (four primary financial cards)

| Card | Example value |
|------|--------------|
| Investment | ₹34,000 |
| Revenue | ₹43,500 |
| Expenses | ₹3,000 |
| **Profit/Loss** | **+₹6,500** (negative → Loss −₹5,000, prominently red — cf. 18 Oct in History, §10) |

*Values from the canonical example (§16). Investment card = New Purchase Cost + Inventory Used Cost for the day.*

*Below the four cards: a two-part **"Pending"** figure as of 20 Dec (§16.8 state): "Pending ₹36,500 · Billed pending ₹5,550 (A's Oct bill: 21,050 − 500 deductions − 15,000 collected) · Unbilled (accrued) ₹30,950 (B ₹20,050 [Day 1 ₹15,550 + Day 2 ₹4,500] + C ₹10,900)" (§7.5.6) — profit ≠ cash received (§2.9).*

### Additional sections

- **Today's Cart** (if active): "3 Items · Total Quantity 1,200 kg · Investment ₹34,000 · Expected Revenue ₹43,500 · Expected Profit +₹9,500 (before expenses)" + button **Open Cart**. If no active cart: "Start Today's Supply" + button **Browse Vegetables**.
- **Inventory summary** (compact card): "50 kg Available · Stock Value ₹1,500 · 1 Item" + **View Inventory**. If attention needed: "N items need attention."
- **Overall Performance:** Total Investment ₹5,00,000 · Total Revenue ₹6,20,000 · Total Expenses ₹40,000 · Overall Profit ₹80,000 (if negative: Overall Loss) + **View Profit/Loss**.
- **Quick Actions:** Browse Vegetables · Open Cart · Inventory · Institutions.
- **Attention/Notifications** (badge-linked; see §14): includes payment-due and old-stock alerts.

Home exists in three variants: Empty (no data at all), Active Cart, Completed Day. **On the Completed-Day variant, while the correction window (§2.8) is open, show button "Edit Today's Cart" (re-opens the confirmed cart for fixes); after the window closes, show a "Completed & Locked" badge instead.**

**While Correction Mode is open** (even next to an active cart): a persistent chip **"Correcting TRX 20-Oct (R2)"** appears on Home. The four TODAY cards always reflect the **current date's** figures — never the corrected transaction's; its changes surface on the transaction's own date.

## 6.2 Vegetable / Item List

- Large visual cards: image, name, category, default unit, current cart quantity if added ("500 kg in Cart"), state badge ("Added ✓" when in cart), tap-to-continue action.
- Search: `Search vegetables or items...` — also matches groceries and custom items; search matches **item names** (§5.4).
- Filters: **All · Vegetables · Groceries · Other Items** (plus: show deactivated only via Settings → Item Management).
- Categories: Vegetables (Tomato, Potato, Onion, Carrot, Cabbage, Cauliflower, Brinjal, Beans, Green Chilli, Lady's Finger, Coriander, Spinach), Groceries (Rice, Dal, Oil, Sugar), Other Items (contract items). Contractor can add custom items.
- **Seed catalog (ships pre-seeded at setup):** exactly the items named above — nothing more (anything beyond is a custom item, §17 "Add/Edit Custom Item"). Seed defaults: §16.1 canonical values override the category defaults where stated — Tomato 3 · Potato 14 · Onion 30 days; leafy greens (Coriander, Spinach) 1 day; other seeded vegetables 7 days; groceries none; each editable per item (§8.1).

## 6.3 Item Detail / Quantity Entry

Example: **Tomato** (large hero image, §5.3)

| Field | Value |
|-------|-------|
| Quantity | `500` |
| Unit | `kg` |
| Today's Purchase Rate | `₹30/kg` — **prefilled with the item's last recorded rate** (shown as "Last: ₹28/kg · 19 Oct"), always editable; entered fresh daily |
| **Purchase Cost** | **₹15,000** (auto) |

- **Inventory availability check** (automatic): if stock exists, show "Existing Inventory Available: 50 kg" (availability already excludes quantity reserved by the open cart — §8.4) with options **Use Inventory / Purchase New** (not mutually exclusive).
- **Purchase-unit shortcut (optional per item, set in Item Master):** an item may define a bulk **purchase unit** (bag / sack / crate / box) with a conversion factor — e.g., Potato: 1 bag = 50 kg. The quantity field offers a toggle: enter `8 bags` → the app instantly shows **"8 bags = 400 kg."** Everything downstream (cart, allocation, inventory, revenue) uses the **base unit**; the purchase rate stays ₹/base-unit, so formulas (§2.2) are unchanged. Without a purchase unit, entry behaves as plain base-unit entry.
- **Purchase source (optional):** free-text mandi/seller name per purchase line — shown in history and reports for quick price comparison.
- **Quality/grade note (optional):** free-text quality/grade per purchase line (e.g., "A-grade", "semi-loosu") — recorded for price-vs-quality analysis (§2.11, §21); never affects calculations.
- **Purchase payment (per line, §9.1):** **Paid now** (default) or **On-credit →** seller name + optional due date; feeds the Payables module and the cash picture, never the P&L date.
- Primary button: **Add to Cart**.
- If the item already exists in the cart (duplicate add): prompt **"Edit existing entry"** or **"Add a separate purchase line."** Multiple lines per item are allowed; the cart shows combined quantity and a weighted-average rate; total cost = sum of lines. Never silently merge.

## 6.4 Active Cart (Central Daily Workspace)

Persistent indicator (Hugeicons cart icon + text, §5.5): "Cart · 3 Items · Investment ₹34,000".

### Cart screen

- Title: **TODAY'S CART** (with cart date).
- Per item (per purchase line): name, quantity (with purchase-unit hint when set — e.g., `400 kg · 8 bags @ 50 kg`), purchase rate, purchase cost, actions **Edit / Remove**.
  - Example: Tomato — 500 kg — ₹30/kg — ₹15,000.
- Bottom: Total Quantity (1,200 kg), Total Investment (₹34,000).
- Free editing before confirmation: change quantity, change rate, remove item, add item — **all totals update instantly.**
- Remove item → confirmation dialog: "Remove Tomato from today's cart?" (Cancel / Remove).
- Primary button: **Continue → Allocate**. (If cart empty → Empty State: "Your cart is empty — start by selecting vegetables." + **Browse Vegetables**.)
- Invalid/missing data on continue → validation errors listed (§15).

## 6.5 Institution Allocation

Per cart item: show total quantity, then allocate per institution.

### Automatic rate display (core rule)

Selecting Tomato + Institution A automatically finds A's fixed tender rate and shows **"₹40/kg — Fixed Tender Rate."** No manual entry required. Institution B → ₹45/kg; Institution C → ₹38/kg — each correct automatically.

### Custom Rate Override

Optional override (e.g., ₹42/kg) shown as **"Custom Rate"** for that allocation only. Never silently changes the saved institution price.

### Missing Price

No price exists → **"No fixed tender price found"** with options **Add Price to Institution** or **Use Custom Rate**. Never borrow another institution's rate automatically. With multiple tenders (§7.2), **Add Price to Institution asks which tender** the new price belongs to.

### Tender Selection (Multiple Active Tenders)

- Tender is selected **per allocation row** (item × institution) — the same cart may supply Institution A's tomato under Tender 1 and its potato under Tender 2.
- **Default rule:** the tender whose period covers today **and** holds a price for that item; latest start date wins ties. The picker sheet appears only when that default is ambiguous.
- Each allocation stores the tender it used in the frozen transaction snapshot (§2.4).

### Allocation feedback

- Allocation % computed (formula 2) with simple progress bars.
- Revenue per institution computed (formula 3).
- Allocated cost per institution computed (formula 4) → institution gross profit shown inline.
- Validation: allocations cannot exceed total item quantity (§15).
- Example result: Tomato → A 200 kg (40%) ₹8,000 · B 150 kg (30%) ₹6,750 · C 100 kg (20%) ₹3,800 · Remaining 50 kg (10%).

## 6.6 Remaining & Rejected Quantity Handling

### Remaining (unallocated)

Total 500 kg − Allocated 450 kg = **Remaining 50 kg** → prompt:

- **Move to Inventory** → confirmation sheet: item, quantity, original purchase rate (₹30/kg), original cost (₹1,500), source, date → **Add to Inventory**.
- **Mark as Loss/Waste** → reason required → loss recorded (formula 12).

### Rejected by institution

e.g., Institution A receives 200 kg Tomato, rejects 30 kg → **"What should happen to the rejected 30 kg?"**

- **Move to Inventory** (usable) → inventory +30 kg.
- **Mark as Loss/Waste** (unusable) → loss = original cost of 30 kg (₹900). Reason required (spoiled, damaged, etc.).

Support **partial rejections and partial remainders** (enter exact rejected/remaining quantity). Every decision is recorded with source institution and date.

> Rejections are normally recorded **during the day, before Confirm**. If discovered only after the cart is locked (e.g., a next-day delivery), record them through a **Post-Lock Adjustment** (§7.5.7) — never by editing history.

## 6.7 Review Everything (Most Important Screen)

Title: **REVIEW EVERYTHING** — the complete daily transaction before the single confirmation.

Sections shown:

1. **Item Summary** — per item: total quantity, purchase rate, purchase cost, inventory used (with source breakdown), new purchase quantity.
2. **Institution Summary** — per institution: items, quantities, tender rates (fixed/custom), allocation %, revenue, allocated cost, assigned expenses, profit.
3. **Inventory Movement** — remaining/rejected decisions, inventory in/out, waste.
4. **Expenses** — list with edit/delete; total.
5. **Analysis (pre-confirmation):**
   - Vegetable-wise: Tomato → +₹3,550 · Potato → +₹3,650 · Onion → +₹2,300 (canonical §16)
   - Institution-wise: A → +₹4,300 · B → +₹4,300 · C → +₹2,400
   - Overall: final financial summary.
6. **Warnings/Validation** — unallocated quantity, items without rates, missing prices, allocation overruns.

### TODAY'S FINANCIAL RESULT (bottom, canonical §16)

| Field | Value |
|-------|-------|
| Total Investment (New Purchase) | ₹34,000 |
| Inventory Used Cost | ₹0 (Day 1) |
| Additional Expenses | ₹3,000 |
| Total Cost | ₹37,000 |
| Expected Revenue | ₹43,500 |
| **Expected Profit** | **+₹6,500** |
| Profit Margin | 14.94% |

If negative: **Expected Loss −₹X** prominently.

## 6.8 Final Confirmation & Completion

- Secondary: **Edit Cart**. Primary: **Confirm & Complete**.
- A confirmation dialog summarizes the result and requires explicit confirm. **ONE confirmation for the entire daily cart — never per item, never per institution.**
- On completion → **"Cart Completed ✓"** screen with Today's Final Result (Investment / Revenue / Expenses / Profit), transaction date + reference number, and buttons: **View Details · View History · Back to Home.** (Memos/bills are *not* auto-generated — they are produced on demand, when the institution asks — §7.5.5.)
- If re-opened within the correction window (§2.8) and completed again, the reference gains a revision suffix (e.g., TRX-20261020-R2), a **full revision snapshot is stored** (§2.8 rule 5), and any already-shared memo/bill triggers an outdated warning to re-share (§7.5.5).
- Saved snapshot includes (immutable): date, items, quantities, purchase rates/costs, inventory usage & original costs, institutions, allocations, tender rates (incl. custom), revenue, expenses, total cost, profit/loss, remaining quantities, all inventory movements.

---

# 7. SCREENS — INSTITUTIONS

## 7.1 Institution List

Access: **More → Institutions** (or Settings → Institutions). Cards with name, contact, contract info, number of tender prices. Search supported. Empty state: "No institutions yet — create one to start allocating supplies." + **Add Institution**.

## 7.2 Create / Edit Institution

Form — short and practical:

| Group | Fields |
|-------|--------|
| Basic | Institution Name*, Contact Person, Phone, Address, Location |
| Tenders (list — add/edit **multiple**) | **+ Add Tender:** Tender/Contract Name, Tender Number, Start Date, End Date, Payment Terms (e.g., paid after tender period ends — usually 1–2+ months later), Overdue Alert Days (default 7; drives the Payment Due alert, §14), Optional **demanded quantities per item** (supplied-vs-required check for the period, off by default), **own Fixed Tender Price list** (§7.3) |
| Notes | Optional notes |

- Most institutions carry **one tender**, which then acts as the default everywhere.
- **Multiple simultaneous tenders work:** allocation shows a tender picker (§6.5); dues, bills, and memos group **per tender** (§7.5).
- An ended tender stops offering daily rates but keeps its dues/payments history intact.

## 7.3 Institution Profile

Contains:

- **Details** — basic + contract info.
- **Tenders** — list of tenders with periods, payment terms, alert days; add/edit (§7.2).
- **Fixed Tender Prices (per tender)** — per-item list for the selected tender (e.g., Tomato ₹40/kg · Potato ₹35/kg · Onion ₹38/kg). **+ Add Item Price** (Item, Unit, Fixed Tender Rate). **Edit** per price (e.g., ₹40 → ₹42; applies to future transactions only). **Delete** price (confirm; historical transactions unaffected).
- **Price History** — past rate changes per item, per tender.
- **Today's Supply** — current allocations if cart in progress.
- **Financial Performance** (lifetime + per period): Total Supply qty, Revenue, Allocated Cost, Assigned Expenses, Profit/Loss, Profit Margin, Supply History.
- **Receivables & Payments** — accrued dues, aging, payment records (full sub-module, §7.5).
- **Memos & Bills (on demand)** — documents generated from this institution's supplies; regenerate/create one whenever the institution asks — shared as screenshot/PDF (§7.5.5).
- **Item-wise Performance / Monthly Performance** inside analytics view.

## 7.4 Institution Rules

- The same vegetable may hold different rates at different institutions (§2.1).
- Editing current prices never rewrites past transactions (§2.4).
- Deactivating an institution: confirmation dialog that **names open business state** — "Institution A has 1 active tender and ₹21,050 accrued dues" (unbilled on 21 Oct — before its 5 Nov period bill; canonical §16.8); historical references preserved. Pending dues of a deactivated institution remain visible and collectable, and its Receivables/Contract P&L stay intact; only new carts and new allocations are blocked (§15.2).

## 7.5 Payments & Receivables (Tender-Period Reality)

Because institutions pay **only after the tender period ends — one to two months after supply, sometimes longer** (§2.9), the app must track dues separate from profit. Supplies inside a period are also **irregular** (§4.2): some days two supplies, some days none — the period bill simply aggregates whatever was supplied.

### 7.5.1 Receivables View (per institution, in profile)

- **Accrued dues** — all supplied-and-confirmed amounts not yet paid, **grouped per tender and its period**.
- Each group shows: `Billed − Deductions − Received = Pending` (formula 14) and **aging** (e.g., "Oct period · ₹21,050 billed · 45 days pending").
- Status badges per period: **Accrued (Not Billed) · Bill Issued · Partially Paid · Fully Paid**.

### 7.5.2 Tender-Period Billing

- Aggregate all confirmed supplies within a tender's period → **period bill total** (auto-stops at the period end date; supports partial-period billing when the contract requires it).
- Bill record: tender, institution, period, items summary, amounts, issue date, optional notes; generated **on demand** (§7.5.5) — a bill is not created until the contractor wants it.
- Bill display: `Supply ₹X − Deductions ₹Y = Net to collect ₹Z`.
- **When a period ends with unbilled supplies**, its Receivables entry shows the suggestion chip **"Period ended — prepare bill?"**; billing stays manual, never automatic (§7.5.5). The **"Tender Ending Soon"** alert (§14) precedes this.

### 7.5.3 Record Payment (with Deductions)

Institutions commonly **deduct money at collection time** — damaged bags, quality cut, short weight. Record Payment therefore captures both sides:

- **Amount received** + Reference (UTR / cheque / DD / voucher no.) + note + date.
- **Deduction lines (add one or more):** reason (Quality · Damage · Quantity/short weight · Return · Other — editable set), amount, optional link to a transaction or adjustment (§7.5.7).
- Net effect: **dues reduce by Received + Deductions** (formula 14); a period shows **Fully Paid only when Billed − Deductions − Received = 0**; partial payments leave a visible residual.
- Overpaying — whether by a large payment or by a **downward revision after collections exist** — never loses money: the excess becomes an **institution credit balance** ("Advance from Institution A: ₹X"), displayed in the receivables view and **auto-applied FIFO to the next period's bill**. Before re-confirming a downward revision the app warns: *"₹15,000 is already collected against this bill — revised total is ₹13,000. ₹2,000 becomes credit balance."*
- **Overpayment intent step:** when Deductions + Received would exceed the pending amount, the form shows the explicit confirm — *"Received ₹30,000 exceeds pending ₹25,000 — ₹5,000 becomes credit balance. Record it?"* — **Record it** stores the excess as credit balance (FIFO next period); **Adjust** returns to the form (the plain validation of §15.1 stays). This makes overpayment a deliberate choice, never a silent surprise.
- **Revision re-mapping:** when a cart is revised within its window (§2.8), payment/deduction applications **re-run FIFO against the latest revision** — nothing needs manual untangling.
- The reason editor (payment deductions · waste/loss marks · rejection reasons) is **editable in Settings** (§13); deduction defaults: Quality · Damage · Quantity/short weight · Return · Other.
- Applied to the **oldest pending period first (FIFO)** with manual targeting override.

### 7.5.4 Payment History

- All receipts per institution with their period allocations; running pending totals.
- Also visible institution-wide: a **Payments summary row** in the institution profile header (Total Billed · Collected · Pending).

### 7.5.5 Memos & Bills — Generated Only On Demand

- **No daily sharing.** Memo/bill documents are generated **only when the institution asks** — typically at or near payment time — and shared as a **screenshot/image or PDF** (in English — §5.4).
- Two document types:
  - **Transaction memo** (per supply): institution, supply date, memo number, items, quantity, tender rate (frozen — §2.4), amount per line, total, signature space.
  - **Period bill** (the assembled §7.5.2 record): the document used to collect payment months later; shows `Supply − Deductions = Net to collect`.
- Generated from Transaction Detail or Institution Profile, anytime; **every generation is timestamped** so re-shares can be distinguished.
- Optional **"Shared ✓" marker** per document. If the correction window revises amounts (§2.8) or a Post-Lock Adjustment (§7.5.7) changes dues afterwards, shared documents trigger the warning: *"This shared bill is outdated — regenerate and share the updated version."*
- Document numbering: sequential per institution (e.g., A-2026-10-001), revision suffixes handled per §6.8.

### 7.5.6 Overall Dues & Alerts

- Home shows the pending figure **split** (§6.1): as of 20 Dec (§16.8): `Pending ₹36,500 — Billed pending ₹5,550 · Unbilled (accrued) ₹30,950`.
- The **Payments & Dues overview** is reachable from **More**: per-institution rows (accrued, billed pending, credit balance, oldest aging) with a quick **Record Payment** action.
- Alerts (§14): **Payment Due Alert** (period end + Overdue Alert Days passed with unpaid billing) and **Long-Pending Dues** (aging threshold — default 30 days, Settings → App §13).
- Reports get a **Payment Dues report** and a **Cash-vs-Accrued summary** (§12).

### 7.5.7 Post-Lock Adjustments (after the correction window closes)

For discoveries that arrive after the lock — next-day deliveries, late rejections, gate-weigh differences (allocated 200 kg, weighed 198 kg), damage found at unloading:

- The locked transaction is **never edited** (§2.8, §2.10).
- The contractor records an **Adjustment** on the discovery date: institution, linked transaction/tender/period, type (**Rejected/Short quantity · Accepted-weight difference · Quality deduction · Other**), quantity and/or amount, reason.
- Effects, all anchored to the adjustment's own date:
  - Rejected/short quantity → **Inventory lot** added with source "Institution X — Post-Supply Rejected" (then Use/Sell/Waste as normal, §2.6).
  - Money impact → **Deduction on dues** (§7.5.3): `Billed − Deductions − Received` reconciles to zero.
  - **P&L shows a "Deductions" line** on the adjustment date (formula 15) — Effective Revenue = Supply Revenue − Deductions; the supply day's own numbers stay untouched.
- Adjustments appear as a linked list inside Transaction Detail ("Original + Adjustments") and in the institution profile.

---

# 8. SCREENS — INVENTORY

Dedicated **INVENTORY** tab. Contains physical stock the contractor owns and can reuse or sell. Sources: institution rejection, unallocated remaining, previous supply, manual add.

## 8.1 Inventory Dashboard

- Summary: total available quantity, total stock value (formula 11), item count.
- Filters: All · Available · Partially Used · **Rejected** (lots sourced from rejections — status may still be Available, §8.2) · Damaged · Waste/Loss. Search supported.
- **Attention section:** rejected items awaiting decision — "N items need attention" + **Review Inventory**; plus **old-stock detection**: any Available/Partially Used lot whose age (days since added) exceeds the item's **shelf-life alert age** (per-item setting; defaults: leafy 1 day, vegetables 3 days, groceries none) gets an **"Old Stock"** badge → quick actions **Use in Supply · Sell · Mark Waste**.
- Empty state: "No inventory available — rejected or remaining reusable stock will appear here."

## 8.2 Inventory Item Card

Example card: **Tomato** (photo reused from Item Master, §5.3)

| Field | Value |
|-------|-------|
| Available | **50 kg** |
| Original Cost | ₹30/kg |
| Stock Value | ₹1,500 |
| Source | Institution A — Rejected |
| Age | 1 day (shelf-life alert: 3 days for vegetables) |
| Status | **Available** |

Actions: **Use in Supply · Sell · Mark Waste** (each opens its own sub-flow, §8.4–8.6).

## 8.3 Add to Inventory (from Remaining/Rejected)

Confirmation sheet: item, quantity, original purchase rate, original cost, source (institution/remaining), date → **Add to Inventory**. Stock with identical item+unit from the same source/day may be merged; otherwise separate lots are kept (each lot preserves its own original rate and history).

## 8.4 Use Inventory in Next Supply (No Double Counting — §2.3)

Creating a new cart for an item with inventory:

- Show **"Existing Inventory Available: 50 kg"** → options **Use Inventory / Purchase New** (both).
- Source breakdown displayed clearly:
  - Required: 200 kg → From Inventory: 50 kg (cost ₹1,500, already invested — no new money today, even if bought on credit) + New Purchase: 150 kg @ today's rate → Total: 200 kg.
- The 50 kg creates **no duplicate investment**; its original cost is recognized once as Inventory Used Cost (formula 6).

### Reservations

While a cart is open, the inventory quantities it consumes are **reserved**: availability figures elsewhere (Item Detail check, Inventory dashboard) already deduct in-cart reservations. Discarding the cart, editing it, or changing usage releases and re-binds reservations automatically. Lots themselves **deplete only at Confirm** (§6.8) — consistent with movement history (§8.7; blended-rate money per formula 4, §16.3).

### Lot selection order (FEFO)

When multiple lots of the same item exist, the default consumption order is **oldest-lot-first (FEFO — first-expiry, first-out)**: oldest stock goes out first, which protects perishables and naturally clears old-stock alerts (§8.1). The contractor can override and pick a different lot manually; every usage is recorded against the lot it consumed, and the used cost follows **that lot's** original rate (formula 6). When a cart item mixes inventory-sourced and fresh-purchased stock, allocation money uses the item's **blended delivered-cost rate** (formula 4, §16.3) — the inventory cost is recognized exactly once.

### Partial usage

Inventory 100 kg, used 40 kg → remaining automatically 60 kg. Never delete the lot; update quantity (status → Partially Used). Every usage is appended to the lot's movement history.

## 8.5 Sell Inventory (Separate Sale)

Flow: select quantity → enter selling rate → review (revenue, original cost, profit) → confirm.

Example: 50 kg sold @ ₹35 → Revenue ₹1,750, Cost ₹1,500, **Profit ₹250**, recorded with date. Adds revenue to the day's totals (formula 8 second term).

## 8.6 Mark Damaged / Waste

Select quantity → reason (Spoiled, Damaged, …) → loss preview (formula 12) → confirm **"Are you sure this stock is unusable?"**.

Example: 20 kg @ ₹30 → **Loss ₹600** → contributes to Daily Loss and Overall Loss.

## 8.7 Inventory History

Every movement recorded and traceable per lot: date, +/− quantity, source/destination ("+50 kg · Institution A Rejected" / "−30 kg · Used for Institution B" / "−20 kg · Wasted (Spoiled)"), running remaining.

## 8.8 Inventory Accounting (Distinguish Always)

| Concept | Definition | Cash/loss effect |
|---------|-----------|------------------|
| New Purchase | Money spent today on new stock | Today's investment |
| Existing Inventory | Stock bought earlier | None (asset) |
| Inventory Used | Existing stock used today | Inventory Used Cost in today's Total Cost (once) |
| Inventory Waste | Existing stock became unusable | Loss today (formula 12) |
| Inventory Value | Available qty × original rate | Neither revenue, profit, loss, nor cost |

---

# 9. SCREENS — EXPENSES

- **Expense List** (per day): entries with type, amount, note, assignment. Empty state provided.
- **Add Expense:** Expense Type (Transport, Labour, Loading, Unloading, Packaging, Other — configurable), Amount (numeric keypad), Optional Note, **Optional assignment** to institution/item (defaults to day-level, §2.5).
- **Example day:** Transport ₹1,000 · Labour ₹1,000 · Packaging ₹500 → **Total ₹3,000**.
- **Edit / Delete** before confirmation (delete → confirm dialog). Immediate recalculation everywhere.
- **Daily Expense Breakdown** visible in Review and History.

## 9.1 Payables — Seller Credit Purchases (On by Default)

Mandi sellers commonly extend credit ("khata") — settled days later. This mirrors §7.5 for the purchase side:

- **Purchase payment flag** on every purchase line (Item Detail, §6.3): **Paid now** (default) / **On-credit** → seller name (free-text person list) + optional due date. If he always buys cash, this screen simply stays empty — zero extra effort.
- **Seller Payables screen** (More → Seller Payables): per-seller rows — amount owed, purchase date(s), oldest aging, settled status (**Open · Partly settled · Settled**); settle open lines with payment entries (amount + reference, FIFO); option alert N days around due date (default 3 — Settings → App, §13).
- **P&L semantics unchanged:** daily Investment and Total Cost still recognize purchases at the **purchase date** — payables only change the **cash picture** ("money in hand today" = receipts − settlements − expenses).
- **Seller Payable Due alert** (§14): due date approaching or passed while unsettled.
- Reports (§12) gain a **Payables report**; the Cash summary counts settlements.

---

# 10. SCREENS — HISTORY

- **History Dashboard:** completed **transactions and other entries** (carts, sales, waste, adjustments, settlements) listed by date — cart cards show Investment / Revenue / Expenses / Profit(or Loss); other entries render as compact type-badged rows. Lists are **built for years of data**: date cards load progressively (paginate by month), ledger rows virtualize, search hits fetch on demand (§2.11). Example cards:

| Date | Investment | Revenue | Expenses | Result |
|------|-----------|---------|----------|--------|
| 20 Oct | ₹34,000 | ₹43,500 | ₹3,000 | +₹6,500 |
| 19 Oct | ₹28,000 | ₹32,000 | ₹2,000 | +₹2,000 |
| 18 Oct | ₹35,000 | ₹32,000 | ₹2,000 | −₹5,000 |

- **Filters:** Today · Yesterday · Last 7 Days · This Month · Previous Month · Custom Date Range — period totals shown for selection.
- **Search** across transactions (item names, institutions, dates).
- **Transaction Detail** (frozen rates): item list, institution allocations with tender rates as used, inventory movements, expenses, final result. Sub-views: item detail, institution detail, expense detail, inventory detail, P/L detail. While the correction window (§2.8) is open: **Re-open & Correct** action — this is where corrections of any within-window transaction start (on Home the same entry point appears as "Edit Today's Cart" for today's own transaction only, §6.1); afterwards a **Locked** badge, a **revision selector (R1/R2/…)**, and any **linked post-lock adjustments** (§7.5.7).
- **Memos & Bills** — generate on demand per tender/institution from Transaction Detail (§7.5.5).
- **Photos (optional, off by default)** — attach a weight-slip / delivery / transport photo to a transaction or post-lock adjustment; visible in Transaction Detail and available when disputes arise at collection time (§7.5.5).

## 10.1 Other Day Entries (Non-Cart Financial Events)

Inventory sales (§8.5), waste (§8.6), post-lock adjustments (§7.5.7), and seller settlements (§9.1) can happen **on any day** — standalone on a no-cart day, or alongside a cart. They become lightweight, **date-anchored entries**:

- Each entry carries its own date → renders in **History** as a compact row with a type badge (**Sale · Waste · Adjustment · Settlement**) linking to its detail view — never lost just because no cart exists.
- They feed **daily/monthly P&L aggregates** exactly like cart effects:
  - Sale → revenue + lot reduction (formulas 8, 13).
  - Waste → loss (formula 12).
  - Adjustment → inventory in and/or a Deductions line (formula 15).
  - Payable settlement → cash-out only (§9.1 — never P&L; the cost was recognized at purchase).
- A **waste-only day** renders as a Loss day (red −₹X — no revenue, so margin stays blank per formula 10/§15.2); a **sale-only day** follows normal margin rules (negative margin shows as a red Loss display, §15.2). The Home TODAY cards aggregate cart + other entries for the current date.
- **No standalone expenses in v1 (decided — the former Gap G-10):** expenses attach to a **cart day** (entered via the day's Expense List, §9) and are allocated only there — assigned (§2.5) or day-level; there is no expense-only entry on a no-cart day. Every "money moved on a quiet day" need other than expenses is covered by the four other-entry types; a standalone expense row would be a v1.1+ addition touching §9/§10.1 and §2.5, not silent scope creep.

---

# 11. SCREENS — PROFIT & LOSS

- **P/L Dashboard:** Total Investment ₹5,00,000 · Total Revenue ₹6,20,000 · Total Expenses ₹40,000 · **Overall Profit ₹80,000** (or Overall Loss).
- **Tabs:** Overall · Daily · Weekly · Institution-wise · **Contract-wise (Tender)** · Vegetable-wise · Monthly (+ custom period).
- **Institution-wise:** per institution — total supply qty, revenue, allocated cost, assigned expenses, profit/loss, margin; best & worst performer highlighted.
- **Contract-wise (per tender, per institution):** each tender agreement (§7.2) gets its own P&L line — supply quantity, supply revenue at the tender's fixed rates, allocated cost, deductions from that contract's collections, **net profit/loss and margin for the contract as a whole** — so the contractor sees for every single contract exactly how much he earned or lost, and which contracts to renegotiate, reprice, or drop. Deductions and adjustments (§7.5.7) are included same as Institution-wise (Effective Revenue).
- **Vegetable-wise:** per item — total quantity, total investment, total revenue, assigned expenses (§2.5 — assigned only, never auto-distributed), waste loss, overall profit/loss; e.g., **Tomato:** 5,000 kg · ₹1,50,000 invested · ₹1,85,000 revenue · ₹5,000 assigned expenses · **+₹30,000**; supplied to A, B, C. Best & worst performer highlighted.
- **Breakdowns:** revenue breakdown, investment breakdown, expense impact, loss analysis.
- **Profit is transaction-based** (recognized on confirm, §2.9) — this dashboard never mixes in payment collections; cash belongs to Receivables (§7.5).
- **Built for years of data:** aggregates (overall/daily/monthly/per institution/contract/item) are **precomputed and incrementally updated on writes** (§2.11) — dashboards open instantly; deep histories paginate rather than reload everything.
- **Deductions surface as their own line** on the adjustment's date (formula 15) — aggregate Institution-wise/Vegetable-wise views show Effective Revenue = Supply Revenue − Deductions (§7.5.7).
- Simple charts per §5.2 only.

---

# 12. SCREENS — REPORTS

Lightweight, generated from the same transaction data (no separate calculations). Every report exists for one goal: **spotting and stopping profit leaks** — negative-margin items, loss-making institutions, uncollected dues, waste, missing tenders:

- **Report types:** Daily · Weekly · Monthly · Institution · **Contract (Tender profitability per §11)** · Vegetable · Inventory · Expense · P/L · **Payment Dues** (per institution: billed, collected, pending, aging) · **Cash-vs-Accrued** (collections vs supplied revenue by period, §7.5) · **Payables** (per seller: owed, aging, settlements, §9.1) · **Spoilage by Item** (waste/loss frequencies — the "what not to buy" view) · Custom Date Range.
- Each report = a structured summary screen; **Export** (share/print/PDF/CSV) from report detail. No report editing, no report builder.

---

# 13. SCREENS — SETTINGS & DATA MANAGEMENT

Keep settings light:

| Group | Contents |
|-------|----------|
| Institutions | Manage institution profiles, fixed tender prices |
| Items | Vegetable/grocery lists, add/edit items, images, single Name per item (§5.4), units, **purchase-unit conversion** (bag/sack/crate = N base units, §6.3), **shelf-life alert age (days)** per item, **Deactivate Item** (never delete; keeps historical references — old transactions still show the item) |
| Business | Business Name, Currency (₹ default), Default Unit (kg), default shelf-life policy |
| App | Appearance/Theme, Expense category editor, **single reason editor** — one editable set shared by payment **deductions** (§7.5.3), inventory **waste/loss marks** (§8.6) and **rejection reasons** (§6.6), **default Overdue Alert Days (default 7 — the global fallback that pre-fills the Add-Tender field, §7.2)**, **alert thresholds — Long-Pending Dues aging (default 30 days), Tender Ending Soon N days (default 7), Seller Payable alert N days (default 3)**, **backup reminder interval** (default 7 days) |
| Data | **Backup** (with confirmation → complete feedback; **version-tagged** files: schema version + timestamp), **Restore** (validates version before confirming — destructive warning), **Export Data** (CSV/JSON), **Import Data** (validation + confirm), **Delete All Data** (double confirmation, warns of permanent loss) |
| About | App info, version, About |

---

# 14. NOTIFICATIONS & ATTENTION CENTER

**Attention Center** — a **full-screen list** of every alert below (§17 Notifications row), opened by tapping the Attention badge on Home. In v1 alerts are **in-app only** (no OS notifications); OS-local notifications are deferred post-v1 (§21):

| Alert | Trigger |
|-------|---------|
| Rejected Stock Alert | Rejected quantity awaiting inventory/waste decision |
| Incomplete Cart Alert | Previous day's cart never confirmed |
| Missing Institution Price | Allocation attempted without a fixed tender price |
| Inventory Attention | Damaged stock / items awaiting decision |
| **Old Stock Alert** | Inventory lot age exceeds the item's shelf-life alert age (per item, §8.1) |
| **Payment Due Alert** | Tender period ended + institution's Overdue Alert Days passed with unpaid billing (§7.5.6) |
| **Long-Pending Dues** | Receivables aging beyond the contractor-set threshold (default 30 days — Settings → App, §13) (§7.5.6) |
| **Backup Due** | No backup performed in N days (default 7, configurable — §13) |
| **Tender Ending Soon** | N days (default 7 — Settings → App, §13) before a tender's End Date: "Tender 'X' ends 31 Oct — bill the period" (§7.2, §7.5.2) |
| **Seller Payable Due** | An on-credit purchase's due date is approaching or passed while unsettled (alert lead default 3 days — Settings → App, §13) (§9.1) |
| Price Updated | A fixed tender price was changed (informational) |
| Transaction Completed | Cart confirmed (informational) |
| Waste/Loss Alert | A waste action was recorded (informational) |

All alert text is in plain English (§5.4).

---

# 15. VALIDATION, ERRORS & EDGE CASES

## 15.1 Error Messages (shown near the relevant field)

| Situation | Message |
|-----------|---------|
| Missing quantity | "Enter a quantity before continuing." |
| Missing purchase rate | "Enter today's purchase rate." |
| Allocation exceeds quantity | "Allocated quantity cannot exceed available quantity." |
| Missing institution price | "No fixed tender price is available for this institution." |
| Invalid amount | "Enter a valid amount." |
| Quantity ≤ 0 | "Quantity must be greater than zero." |
| Quantity with too many decimals | "Up to 2 decimal places allowed." |
| Confirm with unallocated remainder / no rates / unresolved rejections | Warning list on Review with paths to fix |
| Multiple active tenders, target unknown | "Choose the tender for this supply." (tender picker, §7.2) |
| Deduction + received exceed pending dues | "Deductions plus received cannot exceed the pending amount." — unless the contractor passes the **overpayment intent step** ("Record it" → excess becomes credit balance, §7.5.3) (§7.5.3) |
| Editing a transaction whose bill was already shared | "A shared bill exists — amounts have changed. Regenerate and re-share the updated version." (§7.5.5) |
| Downward revision after collections exist | "₹15,000 is already collected against this bill — revised total is ₹13,000. ₹2,000 becomes credit balance." (§7.5.3) |

## 15.2 Edge Cases & Rules

| Case | Rule |
|------|------|
| Fractional quantities | Allowed, up to 2 decimals (0.5 kg). |
| Rounding | Exact internal values; displayed rounding never changes stored totals (§2.7). |
| Units | One unit per item (e.g., kg, litre, sack). Allocation and inventory use the item's unit only — no cross-unit allocation; unit shown with every quantity. |
| Duplicate item in cart | Prompt: edit existing line or add a separate purchase line. Multiple lines allowed; cart shows combined qty + weighted-average rate; allocations draw from the combined pool. |
| Purchase rate ₹0 | Allowed (e.g., free stock) but shown as ₹0 — flagged for review on the Review screen. |
| Same item, multiple lots in inventory | Kept as separate lots per original rate; the app picks/asks which lot to consume; each lot keeps its cost and history. |
| Cart across days | Cart date = day started. Opening the app with an unconfirmed old cart → prompt: "Continue yesterday's cart?" or "Start fresh" (old cart must be confirmed or explicitly **discarded** first — the Discard-cart confirmation, §17 modal list). |
| Offline | Fully functional offline (local-first); sync/backup optional and non-blocking. |
| Loss day (revenue < cost) | Displayed as red Loss (−₹X) everywhere consistently; margin shown as negative %. |
| Revenue = 0 but costs exist | Margin blank; Loss shown. |
| Allocations split but quantity left | Remaining prompt at allocation end (§6.6) — Review warns if unresolved. |
| Inventory more than required | Contractor enters how much of the lot to use; lot stays with remainder (§8.4). |
| Institution deactivated with open allocation | Cannot allocate to deactivated institutions; existing allocations kept. Confirmation names active tenders and unbilled (accrued) dues; Receivables & Contract P&L stay intact and collectable (§7.4). |
| Supply irregularity | Cart exists only on supply days (§4.2); dues/aging/old-stock alerts keep operating on quiet days. |
| Rate typo after confirm | Correction window: re-openable until end of the day after the transaction, then locked (§2.8). |
| Rejection discovered after lock | Post-Lock Adjustment: inventory lot + dues deduction + P&L Deductions line on the discovery date — history untouched (§7.5.7). |
| Multiple simultaneous tenders | Tender picker at allocation; dues/bills grouped per tender (§7.2). |
| Lot consumption order | Multiple lots default oldest-first (FEFO) with manual override; used cost follows the consumed lot (§8.4). |
| Purchase rate prefill | Item entry arrives prefilled with the last recorded rate (labeled "Last: ₹X/kg · date") — editable, never silent (§6.3). |
| Purchase entered in bags/sacks | Quantity may be typed in the item's purchase unit; converted live to base unit and stored in base unit (§6.3). |
| Inventory aging | Lot age is compared to the item's shelf-life alert age → Old Stock badge + alert (§8.1, §14). |
| Partial payment received | Payment may be any amount; applied FIFO (or manually targeted); residual dues remain visible per period (§7.5.3). |
| Supply once, paid months later | Revenue stays in the supply month's P&L; payment only moves cash (§2.9) — never double-counted. |
| Correction Mode beside the daily cart | Correction Mode is independent of the daily cart — both can coexist; the Cart tab always shows today's cart; changes stay date-stamped to the transaction's date (§2.8). |
| Item mixing inventory + fresh purchase | Allocation draws from the item's blended delivered-cost rate (New Purchase + Inventory Used over total qty) — inventory cost recognized exactly once (§2.2 formula 4, §16.3). |
| Revision with collections already applied | Payments/deductions re-run FIFO against the latest revision; excess becomes an institution credit balance applied to the next bill (§7.5.3). |
| Period ended, supplies not yet billed | "Tender Ending Soon" alert before end + "Period ended — prepare bill?" chip in Receivables; billing stays manual (§7.5.2, §14). |
| Money events on no-cart days | Sales, waste, adjustments, settlements become date-anchored Other Day Entries — visible in History and every P&L aggregate (§10.1, §16.9). |
| Inventory reservation | Consume-in-cart quantities are reserved; availability deducts reservations; discard/revision releases them; lots deplete only at Confirm (§8.4). |
| Credit purchases | Paid-now by default; on-credit lines need seller + optional due date; settlements move cash only, never P&L (§9.1). |
| Wrong standalone entry (sale/waste/adjustment/settlement) | Editable until end of the next day (same window as carts); thereafter locked — fixed by a compensating counter-entry, history never rewritten (§2.8, §2.10). |
| Free-text values | Never in numeric fields; reasons always from the editable dropdown set — the ledger stays countable/AI-ready (§2.11). |
| Photo evidence | Optional attachment per transaction/adjustment, off by default (§10). |

## 15.3 Confirmation States (Destructive Actions)

- Remove item from cart ("Remove Tomato from today's cart?" — Cancel/Remove).
- Mark waste ("Are you sure this stock is unusable?" — shows Qty, original cost, potential loss — Cancel/Mark as Loss).
- Delete expense, delete tender price, restore data, delete all data — all confirmed.

## 15.4 Success States

Short, unobtrusive toasts: **Added to Cart ✓ · Institution Created ✓ · Tender Price Updated ✓ · Inventory Updated ✓ · Cart Completed ✓**. No excessive animation.

---

# 16. WORKED EXAMPLES (CANONICAL DATASET)

> All numbers used elsewhere in this document come from here. Every step is verifiable with §2.2 formulas.

## 16.1 Master Data

**Institution fixed tender prices (₹/kg):**

| Item | Institution A | Institution B | Institution C |
|------|--------------|--------------|--------------|
| Tomato | 40 | 45 | 38 |
| Potato | 35 | 32 | 36 |
| Onion | 38 | 40 | 35 |

**Item purchase-units (optional shortcuts):** Potato — 1 bag = 50 kg (hence Day 1's 400 kg entered as `8 bags`); Tomato, Onion — plain kg.

**Shelf-life alert ages:** Tomato 3 days · Potato 14 days · Onion 30 days · Leafy greens 1 day · Groceries none.

## 16.2 Day 1 — 20 October 2026 (Full Daily Cart)

**Purchases** (today's market rates; rate fields arrive prefilled with each item's last recorded rate — "Last: ₹28/kg · 19 Oct" — always editable). *Day 1: tomato ₹15,000 bought **on credit** from the mandi seller (due 27 Oct — settled later, §16.9); potato and onion paid now:*

| Item | Qty | Purchase Rate | Purchase Cost |
|------|-----|--------------|---------------|
| Tomato | 500 kg | ₹30 | ₹15,000 |
| Potato | 400 kg | ₹25 | ₹10,000 |
| Onion | 300 kg | ₹30 | ₹9,000 |
| **Total New Investment** | | | **₹34,000** |

**Allocation & Revenue:**

| Item | Institution A | Institution B | Institution C | Remaining |
|------|--------------|--------------|--------------|-----------|
| Tomato | 200 kg (40%) × ₹40 = **₹8,000** | 150 kg (30%) × ₹45 = **₹6,750** | 100 kg (20%) × ₹38 = **₹3,800** | 50 kg (10%) |
| Potato | 150 kg (37.5%) × ₹35 = **₹5,250** | 150 kg (37.5%) × ₹32 = **₹4,800** | 100 kg (25%) × ₹36 = **₹3,600** | 0 kg |
| Onion | 100 kg (33.3%) × ₹38 = **₹3,800** | 100 kg (33.3%) × ₹40 = **₹4,000** | 100 kg (33.3%) × ₹35 = **₹3,500** | 0 kg |

*Note on this table: Onion's three 33.3% displays sum to 99.9% — display rounding only; the stored allocations are full-precision and total exactly 100% (§2.7).*

- Revenue: A **₹17,050** · B **₹15,550** · C **₹10,900** → **Total ₹43,500**
- Allocated cost (formula 4): A ₹12,750 · B ₹11,250 · C ₹8,500 → **₹32,500**
- Institution profit (no assigned expenses): A **+₹4,300** · B **+₹4,300** · C **+₹2,400**

**Remaining 50 kg Tomato** → contractor chooses **Move to Inventory**: lot = 50 kg @ ₹30 original rate = **₹1,500 value** (not a loss, not revenue).

**Expenses (day-level, unassigned):** Transport ₹1,000 · Labour ₹1,000 · Packaging ₹500 = **₹3,000**.

**Final financial result:**

| Field | Value |
|-------|-------|
| New Purchase | ₹34,000 |
| Inventory Used Cost | ₹0 |
| Additional Expenses | ₹3,000 |
| **Total Cost** | **₹37,000** |
| **Revenue** | **₹43,500** |
| **Profit** | **+₹6,500** |
| Profit Margin | 6,500 ÷ 43,500 = **14.94%** |

**Vegetable-wise:** Tomato ₹18,550 − ₹15,000 = **+₹3,550** · Potato ₹13,650 − ₹10,000 = **+₹3,650** · Onion ₹11,300 − ₹9,000 = **+₹2,300** → sums to the ₹9,500 gross purchase profit ✓ (43,500 − 34,000).

**Check:** 43,500 − 37,000 = 6,500 ✓. Consistency gate: (Revenue − Institution gross profit) must always reconcile: 43,500 − 11,000 = 32,500 = allocated purchases ✓.

## 16.3 Day 2 — 21 October 2026 (Inventory Reuse, No Double Counting)

Need: **Tomato 200 kg**. Inventory has: **Tomato 50 kg** (original ₹30/kg).

App shows: **Existing Inventory: 50 kg** · **New Purchase Needed: 150 kg @ ₹32 = ₹4,800** → contractor chooses **Use Inventory + Purchase New**.

- Total supply: 50 + 150 = 200 kg
- New Purchase: ₹4,800 · Inventory Used Cost: **₹1,500** (recognized once — no duplicate purchase)
- **Delivered Cost Pool** = 4,800 + 1,500 = ₹6,300 over 200 kg → **blended rate ₹31.50/kg** (formula 4)
- Allocation: A 100 kg × ₹40 = ₹4,000 · B 100 kg × ₹45 = ₹4,500 → **Revenue ₹8,500**
- Allocated cost (blended): A 100 × ₹31.50 = **₹3,150** → **+₹850** · B 100 × ₹31.50 = **₹3,150** → **+₹1,350** (source mix is informational — money draws from the pool)
- Total Cost = 4,800 + 1,500 = **₹6,300** → **Profit +₹2,200** (margin 25.88%); check: 3,150 + 3,150 = 6,300 ✓
- Inventory tomato lot: 50 − 50 = **0 kg** (fully used; history updated)

## 16.4 Rejection Handling (Standalone Example)

> Standalone exercise of the §6.6 prompt — its rows are not added to the §16.2–16.3 cart totals; the branch chosen below is what §16.9's 22-Oct sale consumes.

Institution A receives 200 kg Tomato → rejects **30 kg** → prompt: **"What should happen to the rejected 30 kg?"**

- **Move to Inventory** → lot +30 kg @ original ₹30 = ₹900 value *(this branch is the lot sold on 22 Oct — §16.9)*.
- **Mark as Loss/Waste** → reason entered → **Loss ₹900** → contributes to daily loss and overall loss.

## 16.5 Inventory Sale (Standalone Example)

Sell the 50 kg Tomato lot @ ₹35/kg → **Revenue ₹1,750**, Cost (50 × ₹30) ₹1,500, **Profit ₹250**, date recorded. Lot becomes Sold.

> Standalone exercise — not an extra event in the §16.2–16.3 chain (which consumed the 50 kg lot on Day 2). Its figures are exactly the flow §8.5 shows; §16.9's 22-Oct sale is a separate standalone row using §16.4's branch lot.

## 16.6 Inventory Waste (Standalone Example)

Mark 20 kg @ ₹30 as **Spoiled** → **Loss ₹600** (formula 12) → Daily Loss and Overall Loss updated. The contractor's deliberate choice — never automatic (§2.6).

## 16.7 Overall Business Illustration (Lifetime Figures)

| Field | Value |
|-------|-------|
| Total Investment | ₹5,00,000 |
| Total Revenue | ₹6,20,000 |
| Total Expenses | ₹40,000 |
| **Overall Profit** | **₹80,000** (negative → −₹80,000 Overall Loss) |

## 16.8 Payments & Receivables Example (Standalone, §7.5)

Institution A's tender period: **1–31 October 2026** (payment terms: paid after the tender period; Overdue Alert Days = 7). **Institutions B and C carry tenders whose periods likewise cover their supply days (1–31 Oct); their Day 1–2 supplies stay unbilled accrued in this example.**

| Date | Event | Amount |
|------|-------|--------|
| 20 Oct (Day 1) | Supply confirmed — Tomato ₹8,000 + Potato ₹5,250 + Onion ₹3,800 | ₹17,050 accrued |
| 21 Oct (Day 2) | Supply confirmed — Tomato ₹4,000 | ₹4,000 accrued |
| 21 Oct | Institution asks for proof of supply → transaction memo generated on demand, shared as screenshot, marked **Shared ✓** | — |
| 5 Nov | Period billed (Period Total = 17,050 + 4,000) | **₹21,050** · Bill Issued |
| 20 Dec | At collection: institution reports damaged stock → **Deduction recorded** (reason: quality; linked Day 2 transaction) | **−₹500** → Net to collect **₹20,550** |
| 20 Dec | Payment received, UTR recorded → FIFO applies to the Oct bill | **₹15,000** collected |
| 20 Dec | Residual pending, aging 45 days (since the 5 Nov bill — §7.5.1) | **₹5,550 pending** |

- Overall pending split as of 20 Dec (§6.1, §7.5.6): **Pending ₹36,500 = Billed pending ₹5,550 + Unbilled (accrued) ₹30,950** — accrued = B ₹20,050 (Day 1 ₹15,550 + Day 2 ₹4,500) + C ₹10,900 (§16.2–16.3).
- P&L recognized the ₹21,050 as revenue across 20–21 Oct. The 20 Dec deduction surfaces as a **−₹500 "Deductions" line in P&L dated 20 Dec** (formula 15) — the institution's effective revenue reads ₹20,550 without rewriting the supply days. The ₹15,000 collection only becomes cash, never profit again (§2.9). Over the month, Effective Revenue = Supply Revenue − Deductions keeps aggregate totals truthful.
- If the contractor had instead received ₹5,550 on 20 Dec and the rest later, the bill stays **Partially Paid → pending ₹15,000**.
- Memos/bills are generated **only when the institution asks** (his workflow — never daily-shared). Shared ✓ markers trigger the outdated-bill warning if amounts later change (§7.5.5). At payment time with deductions, the bill shows: Supply ₹21,050 − Deductions ₹500 = **Net to collect ₹20,550** (§7.5.3).
- **Contract-wise P&L (§11)** renders the same contract as one line: *Institution A · Tender 1 · 1–31 Oct — Supply ₹21,050 · Effective revenue ₹20,550 (−Deductions ₹500) · Allocated cost & expenses as recorded in the linked transactions → contract profit/loss and margin for the whole tender at a glance.* If margin reads thin after the deduction, that's the renegotiate-or-drop signal.

## 16.9 Other-Entry Days (Standalone, §10.1 & §9.1)

> Standalone rows demonstrating Other Day Entries: the 22/23 Oct lots come from §16.4's Move-to-Inventory branch and an older prior-period onion lot respectively — none of these rows change the §16.2–16.3 cart totals.

| Date | Event | P&L effect |
|------|-------|------------|
| 22 Oct (no cart) | A buyer takes the 30 kg rejected tomato lot (§16.4's Move-to-Inventory branch) @ ₹28 → Other Day Entry **Sale**: Revenue ₹840, Cost 30 × ₹30 = ₹900, lot 30 → 0 | **−₹60** for the day (Sale row) |
| 23 Oct (no cart) | 15 kg of an old onion lot (leftover from a prior period, original rate ₹30) marked **Spoiled** → Waste entry: 15 × ₹30 | **−₹450** loss day (Waste row) |
| 24 Oct (no cart) | Settle the mandi seller — the on-credit tomato ₹15,000 from 20 Oct | **₹0 in P&L** — cash-out only (the ₹15,000 was already inside 20 Oct's Investment; §9.1) |

All three render as compact badge-rows in History (Sale/Waste/Settlement), anchor to their own dates in daily/monthly aggregates, and never disturb cart-transaction history — no ₹840, ₹450, or ₹15,000 ghost totals appear in the 20–21 Oct supply days.

---

# 17. MASTER SCREEN & STATE MAP

> Grouped per screen with states/sub-flows (replaces the former flat 313-item list — nothing was dropped, only organized).
> Every screen must also support the **Global States** (§17 last row).

| Module | Screen | States / Sub-flows included |
|--------|--------|------------------------------|
| Entry & Setup | Splash | Loading, first-run vs returning |
| | Welcome / Intro | Slides, Skip |
| | First-Time Setup | Business profile, currency ₹, default unit |
| | Initial Items | Pre-seeded vegetables/groceries, custom item |
| | Initial Institutions | Add institution + tender prices |
| | Setup Complete | Confirm → Home |
| | **Home Dashboard** | Empty (no data) · Active Cart · Completed Day · Attention badges · **pending split (billed / unbilled accrued, §6.1)** · **Correction Mode chip (§2.8)** |
| Items | Item Home / All Items | Category tabs (Vegetables/Groceries/Other), Added ✓ badge |
| | Search / Filter Items | No-results state |
| | Item Detail (Quantity Entry) | Qty (purchase-unit toggle) · rate with last-rate prefill · optional purchase source · **quality/grade note (§6.3)** · **paid now / on-credit flag (§9.1)** · auto purchase cost · keypad |
| | Inventory Availability Check | 0 · some · choice sheet inventory/new/both |
| | Add to Cart | Success toast · duplicate-item prompt (edit/separate) · error state |
| | Add/Edit Custom Item | Image mgmt (bundled seed set or camera/gallery, §5.3), unit selection, deactivate confirmation, deactivated list, item history |
| Cart | **Cart** | Empty · Active · Cart item detail · Edit qty · Edit rate · Remove confirmation · Investment summary · validation/error state · Continue to Allocate · **Correction Mode never here — Cart tab = today's cart only (§2.8)** |
| Institutions | Institution List / Profile | Empty · search · add · edit · **tenders (add/edit multiple — own price lists, periods, optional demanded quantities, §7.2)** · deactivation (confirmed; confirmation names open tenders/dues; history preserved; Receivables & Contract P&L stay intact and collectable, §7.4) |
| | Fixed Tender Prices | List · add · edit · delete confirmation · price history · missing-price sheet |
| | Tender Management | List/add/edit tenders (§7.2) · periods & alert settings · optional demanded quantities · ended-tender state (daily rates stop, dues history kept) |
| Payments & Receivables | Institution Receivables | Accrued per tender/period · aging · overdue badges · period billing (`Billed − Deductions − Received`) · **Record Payment (amount received + deduction lines, FIFO)** · **Post-Lock Adjustments (§7.5.7)** · Payment history · **memo/bill generation on demand + Shared ✓ marker + re-share warning** · "Period ended — prepare bill?" chip |
| Payments & Dues | Dues Overview (More) | Per-institution rows: accrued / billed pending / **credit balance** / oldest aging · split pending totals · quick Record Payment |
| Allocation | Allocate per Item | Multi-institution entry · auto fixed rate · custom override · missing price (add or custom) · **tender picker when multiple active (§7.2)** · allocation % progress · revenue/cost/profit live update · overrun validation |
| | Remaining Quantity | Move-to-inventory confirmation · mark-waste confirmation + reason |
| | Rejected Quantity | Partial entry · reason · inventory vs waste decision · completion feedback |
| Inventory | **Inventory Dashboard** | Empty · status filters (Available/Partial/Rejected/Damaged/Waste) · search · attention panel |
| | Inventory Item Detail | Source, cost, qty, status, movement history |
| | Use Inventory | Select qty · **FEFO oldest-lot default with manual pick** · **reservations held while cart open, released on discard/revision** · partial/full · inventory+new-purchase breakdown · cost calc (consumed lot's rate) · confirmation |
| | Sell Inventory | Qty · rate · review · confirm · complete |
| | Mark Waste | Qty · reason · loss preview · confirm · complete |
| Expenses | Expense List/day | Empty · add · category select · amount keypad · note · assignment · edit · delete confirm · summary |
| Review | **Review Everything** | Item summary · purchase summary · inventory used · new purchases · institution summary (rates, revenue, cost, P/L) · remaining · rejected · inventory movement · loss/waste · expenses · vegetable-wise P/L · institution-wise P/L · overall summary · warnings · validation errors |
| Finalize | Final Confirmation | Full summary · confirm dialog · Edit Cart path |
| Completion | Cart Completed ✓ | Final result · transaction reference (R-suffix after correction-window revision) · View Details · View History · Back to Home |
| History | History Dashboard | Date cards · filters (Today/Yesterday/7d/This/Prev Month/Custom) · search · empty · **other-entry rows (Sale/Waste/Adjustment/Settlement badges)** |
| | Transaction Detail | Item / institution / expense / inventory / P-L detail views (frozen rates) · correction-window Edit / Locked badge (§2.8) · **revision selector (R1/R2…)** · **linked adjustments (§7.5.7)** · **memo/bill generation on demand (§7.5.5)** · optional **photo attachments** (§10) |
| | Other Day Entries | Sale · waste · adjustment · settlement rows (date-anchored — on no-cart days and alongside carts) · detail views · daily/monthly P&L aggregation (§10.1) |
| Payables | Seller Payables (More) | Seller rows: owed / aging / status (Open · Partly settled · Settled) · purchase flag paid/credit · settlements FIFO · due-date alerts (§9.1) |
| Profit & Loss | P/L Dashboard | Overall · Daily · Weekly · Monthly · Custom · Institution-wise · **Contract-wise (Tender)** · Vegetable-wise · expense impact · revenue/investment breakdown · margin · loss analysis · best/worst performer |
| Reports | Reports Dashboard | Daily/Weekly/Monthly/Institution/**Contract**/Vegetable/Inventory/Expense/P-L/Custom · **Payment Dues** · **Cash-vs-Accrued** · **Payables** · **Spoilage by Item** · detail · export |
| Settings | Settings Dashboard | Institutions · Item management (incl. purchase units, shelf-life ages) · Business (name/currency/units) · Expense categories · Overdue Alert Days · backup reminder interval · Appearance · About |
| Data Mgmt | Backup / Restore / Export / Import | Confirmations, destructive warnings, progress, completion feedback, Delete All Data (double confirm) |
| Notifications | Attention Center | All alerts from §14 |
| Modals / Sheets | Selectors & Confirmations | Institution · Item · Unit · Expense category · Inventory action · Loss reason · Rejection reason · **Select tender** · **Record payment (received + deductions)** · **Settle seller payment** · **Add post-lock adjustment** · Edit qty · Edit rate · Add expense · Remove item · Mark waste · Inventory usage · Inventory sale · Cart completion · **Discard cart** · Data restore · Data deletion |
| Global States | Applied to every screen | Loading · Skeleton · Empty · Error · Offline · Success · Validation error · Toast/Snackbar · Bottom sheet · Modal · Full-screen error recovery |

---

# 18. UX STATE RULE

For every major action, design and connect the relevant:

**Normal → Edit → Cancel → Error → Empty → Success → Confirmation → Back Navigation**

- No dead-end screens. No important button without a complete flow. Never design only the happy path.
- All financial calculations update instantly on any edit (§4.2, §6.4, §6.5, §16 examples).

---

# 19. MANDATORY BUILD CHECKLIST

- [ ] Build the application **end-to-end** from this document as the single source of truth.
- [ ] Not a landing page, not only a dashboard, not disconnected mockups.
- [ ] No invented business features beyond this spec; no e-commerce marketplace; no complicated ERP.
- [ ] Keep: **Inventory** module, **Institution-specific Fixed Tender Pricing**, **Cart-based workflow**, **Review → Confirm & Complete once**, **Payments & Receivables (§7.5)**.
- [ ] Every screen connects logically; every flow has all §18 states; every button has a destination.
- [ ] Every calculation follows §2.2 formulas and updates from entered data.
- [ ] No double-counting inventory (§2.3); historical rates preserved (§2.4); **correction window until end of the next day, then permanent lock with full revision snapshots (§2.8)**; profit never mixed with cash (§2.9).
- [ ] Rejected/remaining stock is **never** auto-loss — contractor decides (§2.6); post-lock discoveries go through **Adjustments**, not history edits (§7.5.7).
- [ ] One active cart; confirm whole cart once; expenses allocated per §2.5.
- [ ] **Mobile-phone-only layout** — bottom-nav phone UX; no tablet/desktop sidebar variant (§3.1).
- [ ] Fast, minimal-typing UX: visual selection, auto rates, auto inventory detection, instant totals, bag-based purchase entry, **last-rate prefill** (§4.2, §5, §6.3).
- [ ] **English-only UI** — one language everywhere including memos/bills, alerts and documents; no language switcher (§5.4).
- [ ] **Hugeicons is the single icon system** — Stroke Rounded family only, one pinned version, icon+text pairs on every action, one icon per alert type (§5.5, §17 map).
- [ ] Memos/bills generate **only when the institution asks** — screenshot/PDF, Shared ✓ marker, outdated re-share warning (§7.5.5).
- [ ] Dues must reconcile: **Billed − Deductions − Received = 0** for "Fully Paid"; deductions also hit P&L on their own date (§7.5.3, §7.5.7, formulas 14–15).
- [ ] **Multiple tenders per institution** supported (picker, per-tender price lists and dues periods) (§7.2).
- [ ] FEFO oldest-lot consumption default; backup reminder alert (§8.4, §13–14).
- [ ] **Correction Mode is a separate cart type** that can coexist with the daily cart (§2.8); blended delivered-cost allocation for mixed-source items (§2.2 formula 4).
- [ ] Pending figure always **split: Accrued vs Billed pending**; revisions re-map payments FIFO; excess collections become **credit balances** auto-applied next bill — recorded only through the **explicit overpayment intent step** ("Record it", §7.5.3, §15.1); thresholds live in **Settings → App** with defaults Long-Pending 30 / Tender-Ending 7 / Seller-Payable 3 days (§7.5.3, §7.5.6, §13, §14).
- [ ] **Attention Center is a full-screen list** behind the Home badge; alerts are **in-app only in v1** (OS notifications deferred per §21 rule 4); **Discard-cart confirmation** exists as a §17 modal (§14, §15.2, §17).
- [ ] Non-cart money events (sale/waste/adjustment/settlement) are date-anchored **Other Day Entries** in History with correct daily/monthly P&L aggregation (§10.1, §16.9).
- [ ] **Payables**: purchase payment flag (paid/credit + seller + due date), seller dues aging, settlements move cash only — never P&L (§9.1, §16.9).
- [ ] Cart-open **inventory reservations** deduct from availability and release on discard/revision; lots deplete only at Confirm (§8.4).
- [ ] **Tender picker per allocation row** with the period-covering default rule; Add Price asks which tender (§6.5).
- [ ] Search matches **item names** (§5.4, §6.2); optional **photo evidence** per transaction/adjustment (§10); **version-tagged backups** with restore validation (§13); **Spoilage by Item** report (§12); **Contract-wise P&L** — one profit/loss line per tender per institution incl. deductions (§11).
- [ ] **Data Recording Standard honored everywhere**: typed rows with date · item · qty · rate · source · reason codes; no free-text numbers; append-only ledger with ID integrity; export mirrors the ledger columns (§2.11).
- [ ] Standalone entries (sale/waste/adjustment/settlement) share the cart correction window; post-window fixes are compensating entries (§2.8).
- [ ] Deactivating an institution keeps receivables & contract P&L intact; new allocations blocked; confirmation names open tenders/dues (§7.4).
- [ ] No future-roadmap/AI feature ships in v1 (§21) — but the ledger captures its training data from day one (§2.11).

Non-negotiable core system (one line here — its only expanded walk-through is the §4 diagram):

**ITEM → QUANTITY → PURCHASE RATE → CART → INSTITUTION → FIXED TENDER RATE → ALLOCATION → REVENUE → EXPENSES → PROFIT/LOSS → REJECTED/REMAINING → INVENTORY (reuse/sell/waste) → REVIEW EVERYTHING → TODAY'S RESULT → CONFIRM & COMPLETE ONCE → HISTORY + INVENTORY + P/L UPDATED.**

And every one of those steps writes a clean, dated, attributed ledger row (§2.11) — the dataset the future AI will learn from (§21).

And cash follows on its own track, months later:

**TENDER PERIOD ENDS → (WHEN ASKED) MEMO/BILL GENERATED & SHARED AS SCREENSHOT → PAYMENT COLLECTED (PARTIAL OK, DEDUCTIONS RECORDED) → DUES CLEARED — profit already counted at supply; payment never re-enters P&L (§7.5, §2.9). Seller khata settles on its own little track (§9.1).**

---

# 20. FINAL PRODUCT EXPERIENCE

The finished app should feel like this:

> "I open the app." → "I see my current business status in seconds."
> "I tap the vegetable image." → "I enter how much I bought and what I paid."
> "I add it to my cart." → "I select the institution — the app already knows its fixed tender price."
> "I say how much goes where." → "The app calculates revenue, allocated cost, and profit."
> "A rejection? I choose inventory or loss." → "Old stock? I reuse it — no double cost."
> "Supplies done. When the institution asks for proof, one tap gives me the memo as a screenshot — in English."
> "I review everything — items, institutions, inventory, expenses."
> "Everything looks right." → "I press Confirm & Complete once. (Typo? I correct it by tomorrow evening — then it's locked.)"
> "The transaction is saved. Inventory, history, and profit/loss are updated."
> "Payment? It comes one or two months after the tender period — the app shows who owes me and reminds me to collect."
> "They cut some money for damaged bags at collection — I record the deduction, and dues zero out."
> "Quiet day? I still log a stock sale or waste from History, and mandi-seller credit sits in Payables until I settle it."

The contractor's time goes to **Selecting → Entering → Allocating → Reviewing** — never to manual calculation.

**The essential experience:**
**SELECT → ENTER → ALLOCATE → CALCULATE → HANDLE INVENTORY → REVIEW EVERYTHING → CONFIRM ONCE → SAVE EVERYTHING — then BILL THE PERIOD AND COLLECT, MONTHS LATER.**

---

# 21. FUTURE ROADMAP — DATA → INSIGHT → AI (Not in the v1 Build)

The app's stated long-term goal: **learn from the accumulated ledger — prices, quantities, contracts, payments, waste — to increase profits.** Sections 1–20 are the build contract; this section is the recognized direction, deliberately **excluded from the mandatory v1 build (§19)**. The only v1 obligation is the enabler: record data per **§2.11 from day one** so nothing must be re-keyed or inferred later.

| Phase | Scope | Reads from (§2.11 rows) |
|-------|-------|--------------------------|
| **1 · v1 build** | Exact spec — screens, formulas, reports, receivables, payables | — |
| **2 · Insights (post-v1, same app)** | Requests beyond reports: purchase-rate **volatility per item/seller/season**, margin trend per **contract (§11)**, waste rate by item/reason, institution payment-behavior profiles, Tender Price Benchmark ("Institution B pays ₹45/kg for tomato while A pays 40 — renegotiate before period start") | purchase lines (rate, source, quantity, date) · supply allocations & tender outcomes |
| **3 · Predictive** | "what to buy tomorrow" — demand patterns per institution (weekday/season effects), price forecasts per item from mandi history, contract net-profit projections incl. expected deductions & payment timing | full transaction ledger including payment/aging history |
| **4 · AI assistant** | ask: *"Which tender made losses? Tomato price trend? Buy onions tomorrow?"* — answers and suggestions generated from the contractor's own ledger; every suggestion routes through the app's normal decision points (§4.3) — the contractor always decides | reads only §2.11 ledger rows (photos and master-data settings are not read) |

Standing rules for all future phases (binding, though not v1-scoped):

1. **Advisory only — AI never writes to the books.** Every suggested buy/price/decision enters through the exact same manual screens and confirmation flows of §1–§20; no autonomous purchase, rate, or allocation.
2. **Privacy-first and single-user (§1.4)** — all computation on the contractor's own device/export; no institution data is ever shared with third parties.
3. **The ledger stays the source of truth (§2.11)** — analysis may only consume it, never mutate it; labels stay human-confirmable.
4. **Also post-v1:** OS-level local notifications (permission prompt + scheduling for the §14 alerts) — v1 keeps alerts in-app only (§14).
4. **Profit-maximization framing (§1.1)** — every phase is judged by one question: *"did this make the contractor more money or fewer losses?"*
