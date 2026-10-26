# SCREENS MASTER LIST — Vegetable Contractor Management App

> **v1.3 · 23 Oct 2026 · Derived from `vegetable_contractor_complete_end_to_end_prompt.md` (SRS v2.9)** — the single source of truth.
>
> **Provenance note:** The file `opencode_generate_screens_master_list_prompt.md` referenced by the task **does not exist in the repo** (at generation time the repo held only the SRS and `AGENTS.md`; this file itself is the generated deliverable). The "screen blueprint" lives inside the SRS itself: **§17 Master Screen & State Map** (the grouped screen/state map) plus the screen definitions in §6–§14. This document is built strictly from the SRS (v1.0 from v2.6; v1.1 from v2.7 for the English-only change — S03 Language Selection retired; v1.2 from v2.8 for the mobile-phone-only change — Tablet/Desktop §3.2 sidebar variant retired; v1.3 from v2.9 — all ten gaps G-0–G-10 resolved in the spec, register updated in §10). The SRS was not modified to produce the MASTER LIST itself (SRS edits were the v2.9 gap decisions); all figures quoted below derive from the §16 canonical dataset.

---

## 0. Method & Classification Legend

Every SRS statement about a screen, field, action, state, dialog, or alert was extracted (§4, §6–§15, §17) and organized **feature-first** (by feature module, ordered by the business lifecycle). Every §17 map row is accounted for (§8.1 cross-check), and every requirement from §2, §7, §9–§14, §15 is mapped in the traceability matrix (§8 of this file).

Classification:

| Class | Meaning |
|-------|---------|
| **S** — Full screen | Navigable page/destination (has its own layout, states, back navigation). |
| **BS** — Bottom-sheet flow | Sheet-driven sub-flow with its own §18 state machine (up to full-screen on mobile). |
| **DL** — Dialog | Confirmation / choice / notice dialog (simple, usually Cancel + destructive or options). |
| **V** — State variant | Not a separate surface; a documented state of the owning screen. |
| **G** — Global component | Reused surface present across screens (nav bar, keypad, toasts, global states). |

Every screen and flow must honor **§18**: `Normal → Edit → Cancel → Error → Empty → Success → Confirmation → Back Navigation` and the **§17 Global States** row (Loading · Skeleton · Empty · Error · Offline · Success · Validation error · Toast/Snackbar · Bottom sheet · Modal · Full-screen error recovery) — applied to **all 41 screens** (defined once below; mapped screen-by-screen in the **§9 universal-element compliance matrix**).

**Design constraints applied to every screen (not repeated per-screen):**
English-only UI — one language everywhere incl. documents, no language switcher (§5.4) · Hugeicons Stroke Rounded only, icon always paired with text (§5.5, §17) · frosted-glass app bar / bottom nav / sheets / modals with solid fallback (§5.1) · numeric keypad for all money & quantity input (§5.1, §5.2) · green = profit, red = loss everywhere (§5.2) · ₹ & Indian digit grouping (§2.7) · full-width single-column cards, mobile phone only — no tablet/desktop variant (§5.1, §3.1) · one canonical item image reused (§5.3) · gentle confirmation behind **every** Delete — one CRUD pattern everywhere (§5.1, §15.3) · alert and document text in plain English (§14, §5.4) · offline-first, sync/backup optional and non-blocking (§2.8 rule 3, §15.2).

---

## 1. Count Summary

| Metric | Count |
|--------|-------|
| Feature modules | **17** (M01–M17; M06 is flows-only by design) |
| Full screens (S01–S42) | **41** (S03 retired in SRS v2.7 — English-only; numbering unchanged) |
| Bottom-sheet flows (BS-01–BS-22) | **22** |
| Dialog definitions (DL-01–DL-14) | **14** |
| Overlay definitions total (BS + DL) | **36** (covers all **22** items of §17's "Modals / Sheets" row — now including **Discard cart**, SRS v2.9 — plus 14 additional overlays mandatory in the SRS body: §6.3, §6.6, §7.5.2, §7.5.5, §8.3, §8.5–8.6, §9.1, §10, §12, §13, §15.2, §17 image-mgmt/revision-selector) |
| Global components | **10** (G1–G10) |
| Workflows documented | **11** (WF-01–WF-11) |
| SRS §17 map rows covered | **40/40 screens + 2 meta rows (modals, global states)** |
| Universal-element compliance matrix (§9) | **41 screens × 13 elements** (7 always-on + 6 conditional checks per screen) |

---

## 2. Feature Modules Overview

| Module | Name | Screens | Flow surfaces |
|--------|------|---------|---------------|
| M01 | Onboarding & Setup | S01–S07 (6 active; S03 retired) | — |
| M02 | Home Dashboard & Navigation | S08 (1) | G1, G2, G6 |
| M03 | Items & Purchase Entry | S09–S11 (3) | BS-01, BS-16*, BS-21, DL-08 |
| M04 | Daily Cart | S12 (1) | BS-16, DL-01, DL-09, DL-10, G2 |
| M05 | Allocation | S13 (1) | BS-14, BS-15 |
| M06 | Remaining & Rejected Handling | — (flows belong to allocation/cart) | BS-02, BS-03, BS-04 |
| M07 | Review & Finalization | S14–S16 (3) | DL-06 |
| M08 | Institutions & Tenders | S17–S21 (5) | DL-03, DL-04, DL-05 |
| M09 | Payments & Receivables | S22–S23 (2) | BS-09, BS-10, BS-11, BS-12, BS-21 (photo attach) |
| M10 | Inventory | S24–S25 (2) | BS-05, BS-06, BS-07, DL-11 |
| M11 | Expenses | S26 (1) | BS-08, DL-02 |
| M12 | Seller Payables | S27 (1) | BS-13 |
| M13 | History & Other Day Entries | S28–S30 (3) | DL-13 (revision picker), DL-14 (attachment viewer), BS-21 (photo attach) |
| M14 | Profit & Loss | S31 (1) | — |
| M15 | Reports | S32–S33 (2) | BS-22 (export / share) |
| M16 | Settings & Data Management | S34–S41 (8) | BS-17, BS-18, BS-20, DL-07 |
| M17 | Attention Center | S42 (1) | — |

\* BS-16 (Edit qty/rate) serves both Cart and Item Detail.

**Bottom navigation (§3.1, G1):** Home (S08) · Vegetables (S09) · Cart (S12) · Inventory (S24) · More → Institutions (S17), Payments & Dues (S23), Seller Payables (S27), Profit & Loss (S31), History (S28), Reports (S32), Settings (S34). Attention Center (S42) is badged on Home. Dues Overview (S23) has one hub target — both labels ("Payments & Dues" §3.1, "Payments & Dues overview" §7.5.6) refer to the same screen (register G-9: documented, no change needed).

---

## 3. Screen Inventory

### M01 · Onboarding & Setup (§4.1, §17 "Entry & Setup")

#### S01 · Splash
- **SRS:** §17 (Splash: "Loading, first-run vs returning")
- **Purpose:** Loading; route first-run users to onboarding, returning users to Home.
- **Fields:** — (brand mark).
- **Actions:** None (automatic).
- **Nav:** out → S02 (first run) / S08 (returning).
- **States:** Loading; branch on first-run vs returning; offline-safe (local-first app, §2.8.3 — no network gate).
- **Requirements:** Route rule only; no blocking network calls (§15.2 Offline).

#### S02 · Welcome / Intro
- **SRS:** §4.1, §17 ("Slides, Skip")
- **Fields:** Intro slides — **copy defined in SRS v2.9 (§4.1):** ① *"Know your price before you buy."* (fixed tender rates protect the margin) · ② *"Enter once — every rupee tracked."* (cart → confirm-once → dues/waste on record); **Skip always visible** (was Gap G-5).
- **Actions:** Next; **Skip**.
- **Nav:** out → S04.
- **States:** Normal; skipped → jumps to S04; back navigation honored (§18).

#### S03 · (Retired — numbering intentionally left unused)
- **Status:** Removed in SRS v2.7 — the app is **English-only** (§5.4); the first-run Language selection step no longer exists (§4.1). The S03 identifier is not reused; no screen maps to it.

#### S04 · Business Profile Setup (First-Time Setup)
- **SRS:** §4.1, §17 ("First-Time Setup: Business profile, currency ₹, default unit"), §13 Business group
- **Fields:** Business Name* · Currency (**₹ default**) · Default Unit (**kg**) · default shelf-life policy (§13).
- **Actions:** Continue.
- **Nav:** out → S05.
- **States:** Required-field validation (§15.1 "Enter a valid amount." pattern for money defaults); edit later in S36.

#### S05 · Initial Items
- **SRS:** §4.1, §17 ("Pre-seeded vegetables/groceries, custom item")
- **Fields:** Seeded catalog — **enumerated in SRS v2.9 (§6.2, resolves G-6):** Vegetables: Tomato, Potato, Onion, Carrot, Cabbage, Cauliflower, Brinjal, Beans, Green Chilli, Lady's Finger, Coriander, Spinach · Groceries: Rice, Dal, Oil, Sugar (each with image, name, unit; §16.1 canonical overrides: Tomato 3d · Potato 14d · Onion 30d; leafy 1d; other veg default 7d; groceries none — editable per item §8.1); anything beyond is a custom item.
- **Actions:** Edit item (→ S11 shared form); Add custom item; Continue.
- **Nav:** out → S06.
- **States:** Seeded list; empty-if-skipped variant; custom-item add form inline.

#### S06 · Initial Institutions
- **SRS:** §4.1, §17 ("Add institution + tender prices"), §7.2
- **Fields:** Institution Name*, contact, tender(s) (name, number, start/end), payment terms, fixed tender price list per tender (item · unit · rate).
- **Actions:** Add institution; add tender; add item price; Continue.
- **Nav:** out → S07.
- **States:** 0–n institutions (§7.2 "Most institutions carry one tender…"); validation of required fields.
- **Requirements:** Fixed Tender Rate is per-institution, per-tender (§2.1, §7.2) — never one global selling rate.

#### S07 · Setup Complete
- **SRS:** §4.1, §17 ("Confirm → Home")
- **Actions:** Confirm.
- **Nav:** out → S08 (Empty variant).
- **States:** Success; cannot re-trigger unless data deleted (§13 Delete All → DL-07).

---

### M02 · Home Dashboard & Navigation (§6.1, §3.1, §17 "Home Dashboard")

#### S08 · Home Dashboard
- **SRS:** §6.1, §3.1, §10.1, §17 (Empty · Active Cart · Completed Day · Attention badges · **pending split** · **Correction Mode chip**)
- **Purpose:** Understand today's situation within seconds; launch everything.
- **Content / Fields:**
  - **TODAY — four financial cards** (§16, §6.1): Investment ₹34,000 (= New Purchase Cost + Inventory Used Cost for the day) · Revenue ₹43,500 · Expenses ₹3,000 · **Profit/Loss +₹6,500** (green; negative → Loss −₹5,000 red, cf. 18 Oct in §10). TODAY cards aggregate cart **+ Other Day Entries for the current date** (§10.1).
  - **Pending split figure** (§6.1, §7.5.6, §16.8): `Pending ₹36,500 · Billed pending ₹5,550 (A's Oct bill: 21,050 − 500 deductions − 15,000 collected) · Unbilled (accrued) ₹30,950 (B ₹20,050 + C ₹10,900)`. Profit ≠ cash (§2.9).
  - **Today's Cart card** (V: Active Cart): "3 Items · Total Quantity 1,200 kg · Investment ₹34,000 · Expected Revenue ₹43,500 · Expected Profit +₹9,500 (before expenses)" + **Open Cart** → S12. Empty variant: "Start Today's Supply" + **Browse Vegetables** → S09.
  - **Inventory summary** card: "50 kg Available · Stock Value ₹1,500 · 1 Item" + **View Inventory** → S24; "N items need attention" (§8.1 attention count).
  - **Overall Performance:** Total Investment ₹5,00,000 · Total Revenue ₹6,20,000 · Total Expenses ₹40,000 · **Overall Profit ₹80,000** (negative → Overall Loss) + **View Profit/Loss** → S31.
  - **Quick Actions:** Browse Vegetables (S09) · Open Cart (S12) · Inventory (S24) · Institutions (S17).
  - **Attention / Notifications** section: badge-linked to §14 alerts (→ S42). Includes payment-due and old-stock alerts.
- **Nav:** in: launch; back from anywhere. out: S09, S12, S24, S17, S31, S42 (badge), attention targets (S23, S27).
- **States (V):** **Empty** (no data) · **Active Cart** · **Completed Day** — window open (§2.8): button **"Edit Today's Cart"** → Correction Mode for today's transaction; window closed: **"Completed & Locked"** badge · **Correction Mode chip**: persistent **"Correcting TRX 20-Oct (R2)"** while a correction is open, even beside an active cart; the four TODAY cards always reflect **today's own figures**, never the corrected transaction's.
- **Requirements:** §6.1 exactly; floating cart indicator G2 always visible (§3.1); alerts feed §14; Correction Mode chip rule §2.8.2 + §6.1.

---

### M03 · Items & Purchase Entry (§6.2, §6.3, §13, §17 "Items")

#### S09 · Item Home / All Items
- **SRS:** §6.2, §17 (Item Home; Search/Filter row), §5.1/§5.3
- **Content:** Full-width single-column card grid — mobile phone only (§5.1, §3.1); product-style cards: **large hero image** (4:3/1:1 crop), Name, category, default unit, **live price hint**, state badge **"Added ✓"** and "500 kg in Cart" when added, tap-to-continue.
- **Search (V, §17 "Search / Filter Items" row):** `Search vegetables or items…` — matches **item names** (§5.4, §6.2), groceries and custom items; **No-results state** (§17).
- **Filters (V):** All · Vegetables · Groceries · Other Items. Deactivated items hidden (visible only via Settings → Item Management, S35 — §6.2, §13).
- **Actions:** Tap card → S10; (from S35) also entry into S11 editor context.
- **Nav:** in: G1 tab, S08 quick action. out: S10.

#### S10 · Item Detail — Quantity & Purchase Entry
- **SRS:** §6.3, §17 (Item Detail row: "Qty (purchase-unit toggle) · rate with last-rate prefill · optional purchase source · quality/grade note · paid now / on-credit flag · auto purchase cost · keypad"), §8.4, §9.1
- **Fields:**
  - **Large hero image** (§5.3).
  - **Quantity** — up to **2 decimals** (§2.7, §15.2); **numeric keypad** (G3).
  - **Unit** — item's base unit (kg; one unit per item, §15.2).
  - **Purchase-unit shortcut (V/BS, optional per item):** toggle entering in bags/sacks/crates/boxes defined in Item Master (e.g., Potato 1 bag = 50 kg): enter `8 bags` → instant "8 bags = 400 kg"; stored in base unit; rate stays ₹/base-unit (§6.3).
  - **Today's Purchase Rate** — **prefilled** with last recorded rate, labeled "Last: ₹28/kg · 19 Oct", always editable; entered fresh daily (§6.3, §15.2 Purchase rate prefill).
  - **Purchase Cost** — auto: `Quantity × Purchase Rate` (formula 1) → ₹15,000.
  - **Purchase source (optional)** — free-text mandi/seller name per purchase line (§6.3; feeds history/reports §2.11).
  - **Quality/grade note (optional)** — free text ("A-grade", "semi-loosu"); never affects math (§6.3).
  - **Purchase payment flag (§9.1):** **Paid now** (default) · **On-credit →** seller name (free-text with **autocomplete suggestions from previously used sellers** — §9.1 "free-text person list") + optional due date (feeds S27; cash-only semantics).
- **Inventory availability check (→ BS-01):** automatic; shows "Existing Inventory Available: 50 kg" (availability already excludes open-cart reservations, §8.4) with options Use Inventory / Purchase New / Both.
- **Actions:** **Add to Cart** (primary); toast "Added to Cart ✓" (§15.4). Duplicate add → **DL-08** (Edit existing entry / Add a separate purchase line). Invalid data → §15.1 errors ("Enter a quantity before continuing." · "Enter today's purchase rate." · "Up to 2 decimal places allowed."). ₹0 rate allowed but Review-flagged (§15.2).
- **Nav:** in: S09. out: S12.

#### S11 · Add / Edit Custom Item (Item Master editor)
- **SRS:** §17 ("Add/Edit Custom Item: Image mgmt, unit selection, deactivate confirmation, deactivated list, item history"), §13 Items, §6.2
- **Fields:** Name* · Category (Vegetables / Groceries / Other Items) · Unit (kg/litre/sack — one per item, §15.2) · **Purchase-unit conversion** (optional: bag/sack/crate/box = N base units, §6.3) · **Shelf-life alert age (days)** (defaults leafy 1 / veg 7 / groceries none; §16.1 canonical overrides: Tomato 3 · Potato 14 · Onion 30; §8.1, §16.1) · **Image — one canonical per item (§5.3)** — set/replace via **BS-21** Image Picker (**mechanism resolved in SRS v2.9**: bundled seed set or camera/gallery pick with thumbnail preview — was Gap G-4) · Active flag.
- **Actions:** Save; **Deactivate** → DL-05 (never delete; ID integrity for historical rows, §2.11.4, §13); view deactivated list (from S35 only, §6.2); view **item history** (past purchase rates/usage — read-only ledger view, §17).
- **Nav:** in: S09 context; S35. out: back to list.
- **States:** Normal / Edit / Cancel / Error / Success toast ("Institution Created ✓"-style feedback §15.4 pattern) / Deactivated view.

---

### M04 · Daily Cart (§6.4, §17 "Cart")

#### S12 · Cart (TODAY'S CART)
- **SRS:** §6.4, §17 ("Empty · Active · Cart item detail · Edit qty · Edit rate · Remove confirmation · Investment summary · validation/error state · Continue to Allocate · Correction Mode never here — Cart tab = today's cart only, §2.8"), §2.8.4, §15.2
- **Content:**
  - Title **TODAY'S CART** + cart date = day **started** (business date, §2.8.4).
  - Per purchase line: item name, quantity (with purchase-unit hint: `400 kg · 8 bags @ 50 kg`), purchase rate, purchase cost. Example: Tomato — 500 kg — ₹30/kg — ₹15,000.
  - Multi-line items: **combined quantity + weighted-average rate** display; allocations draw from the combined pool (§15.2 Duplicate item rule) — never silently merge (§6.3).
  - Bottom: **Total Quantity** (1,200 kg) · **Total Investment** (₹34,000) — instant recalc on any edit (§18).
  - Persistent indicator (G2): "Cart · 3 Items · Investment ₹34,000".
- **Actions:** Per line **Edit** (→ BS-16 qty and/or rate) · **Remove** (→ DL-01 "Remove Tomato from today's cart?" Cancel/Remove). Add item → S09/S10. **Continue → Allocate** → S13. Empty state: "Your cart is empty — start by selecting vegetables." + Browse Vegetables (§6.4).
- **Validation on continue:** missing rates/quantities listed per §15.1.
- **Cross-midnight rule (§15.2 Cart across days):** on open with an unconfirmed old cart → **DL-09** "Continue yesterday's cart?" or "Start fresh" (old cart must be confirmed or explicitly discarded first → DL-10).
- **Reservations (§8.4):** while open, consuming cart items reserve inventory; discard/edit releases & re-binds automatically; lots deplete only at Confirm (§6.8).
- **Correction Mode:** **never reachable here** (§2.8.2) — the Cart tab always shows the daily cart; corrections open from S08 or S29 only.
- **Nav:** G1 tab (always daily cart). out: S13, S09.

---

### M05 · Institution Allocation (§6.5, §17 "Allocate per Item")

#### S13 · Allocate per Item
- **SRS:** §6.5, §17 (multi-institution entry · auto fixed rate · custom override · missing price · **tender picker §7.2** · allocation % progress · revenue/cost/profit live update · overrun validation), §16.2
- **Content per cart item:**
  - Total quantity header (e.g., Tomato 500 kg; combined-pool if multiple lines).
  - Allocation row per institution: quantity → **automatic rate display**: "₹40/kg — **Fixed Tender Rate**" (A), ₹45 (B), ₹38 (C) — no manual entry (§6.5).
  - **Custom Rate override** (optional, e.g., ₹42) — labeled **"Custom Rate"**, this allocation only; never rewrites the saved institution price (§6.5, §2.4).
  - **Missing price** → **BS-15** ("No fixed tender price found" → Add Price to Institution [asks which tender §7.2] or Use Custom Rate). Never borrow another institution's rate (§4.3).
  - **Tender selection per allocation row** (§6.5, §7.2): default = tender whose period **covers today** AND **holds a price** for that item; latest start date wins ties; picker sheet (BS-14) only when ambiguous; error "Choose the tender for this supply." (§15.1). Selected tender stored in the frozen snapshot (§2.4).
  - **Allocation %** — progress bars, computed precision, display 1 decimal (§2.7).
  - **Revenue per institution** (formula 3) · **Allocated cost per institution** (formula 4, blended delivered-cost pool when item mixes inventory + fresh purchase) · **institution gross profit inline**.
  - **Live allocation/cost/revenue totals** (§18; §6.5 example: Tomato → A 200 kg (40%) ₹8,000 · B 150 kg (30%) ₹6,750 · C 100 kg (20%) ₹3,800 · Remaining 50 kg (10%)).
- **Validation:** allocations cannot exceed total item quantity (§15.1 "Allocated quantity cannot exceed available quantity."); deactivated institutions cannot be allocated (§15.2).
- **Nav:** in: S12 (Continue → Allocate). out: **BS-03/BS-04** per item (remaining/rejected), then next item; when all handled → S26/S14 per §4.2 order.
- **Requirements:** §4.3 decision points honored — never auto-decided; §6.5 fully.

---

### M06 · Remaining & Rejected Handling (§6.6, §17 Remaining/Rejected rows) — flows only

#### BS-03 · Remaining Quantity Decision
- See §4 BS registry. Total 500 − Allocated 450 = **Remaining 50 kg** → prompt: **Move to Inventory** (→ BS-02 confirmation sheet: item, quantity, original purchase rate ₹30/kg, original cost ₹1,500, source, date → **Add to Inventory**) or **Mark as Loss/Waste** (reason required → loss recorded, formula 12). **Never auto-loss** (§2.6, §4.3).

#### BS-04 · Rejected Quantity Decision
- Per institution receipt (e.g., A rejects 30 kg of 200 kg Tomato): **"What should happen to the rejected 30 kg?"** — exact partial quantities supported; Move to Inventory (usable, +30 kg, → BS-02) or Mark as Loss/Waste (unusable → loss = original cost of 30 kg = ₹900; reason required — spoiled, damaged, …; **Loss Reason from editable dropdown**, §2.11.2). Every decision recorded with **source institution and date** (§6.6).
- Rejections normally recorded **before Confirm**; discovered after lock → **BS-11 Post-Lock Adjustment** (§6.6 note, §7.5.7).

---

### M07 · Review & Finalization (§6.7, §6.8, §17 Review/Finalize/Completion rows)

#### S14 · Review Everything (Most Important Screen)
- **SRS:** §6.7, §17 (all review sections), §2.5, §16
- **Sections:**
  1. **Item Summary** — per item: total quantity, purchase rate, purchase cost, **inventory used (with source breakdown)**, new purchase quantity.
  2. **Institution Summary** — per institution: items, quantities, tender rates (fixed/custom), allocation %, revenue, allocated cost, **assigned expenses** (assigned only — never auto-distributed, §2.5), profit. Canonical: A +₹4,300 · B +₹4,300 · C +₹2,400 (§16.2).
  3. **Inventory Movement** — remaining/rejected decisions, inventory in/out, waste (§16.4 branch: rejecting 30 kg → inventory ₹900 value or loss ₹900).
  4. **Expenses** — list with edit/delete (→ BS-08 / DL-02); total (Day 1: Transport ₹1,000 · Labour ₹1,000 · Packaging ₹500 = ₹3,000).
  5. **Analysis (pre-confirmation):** Vegetable-wise (Tomato +₹3,550 · Potato +₹3,650 · Onion +₹2,300, sums to gross ₹9,500 ✓) · Institution-wise (A/B/C above) · **Overall final financial summary**.
  6. **Warnings / Validation** — unallocated quantity, items without rates, **₹0-rate flagged for review** (§15.2), missing prices, allocation overruns — each with a path to fix (§15.1 last row).
- **Bottom — TODAY'S FINANCIAL RESULT** (canonical §16.2): Total Investment (New Purchase) ₹34,000 · Inventory Used Cost ₹0 (Day 1) · Additional Expenses ₹3,000 · **Total Cost ₹37,000** · Expected Revenue ₹43,500 · **Expected Profit +₹6,500** · Profit Margin **14.94%**. Negative → **Expected Loss −₹X** prominently red (§5.2).
- **Nav:** in: S13 chain + S26. Actions: per-section edit; **Edit Cart** → S12; **Confirm** → S15.
- **States:** Normal / Edit / Cancel / Validation-error list / Empty-section handling (§18 — no dead ends).

#### S15 · Final Confirmation
- **SRS:** §6.8, §17 ("Full summary · confirm dialog · Edit Cart path"), §4.3
- **Actions:** Secondary **Edit Cart** → S12. Primary **Confirm & Complete** → **DL-06** confirmation dialog summarizing the result, requiring explicit confirm. **ONE confirmation for the entire daily cart — never per item, never per institution** (§6.8, §4.3).
- **On completion:** "Cart Completed ✓" (→ S16); full revision snapshot stored on every re-confirm (§2.8.5); already-shared memo/bill (if any) triggers outdated re-share warning (§7.5.5).

#### S16 · Cart Completed ✓
- **SRS:** §6.8, §17 ("Final result · transaction reference (R-suffix after correction-window revision) · View Details · View History · Back to Home"), §15.4
- **Content:** Today's Final Result (Investment / Revenue / Expenses / Profit) · transaction date + **reference number** (e.g., TRX-20261020; **R2** suffix after a correction-window re-confirm) — the immutable snapshot: date, items, quantities, purchase rates/costs, inventory usage & original costs, institutions, allocations, tender rates (incl. custom), revenue, expenses, total cost, profit/loss, remaining quantities, all inventory movements (§6.8).
- **Memos/bills are NOT auto-generated** — produced only on demand (§7.5.5, §6.8 note).
- **Actions:** View Details → S29 · View History → S28 · Back to Home → S08.
- **Post-state:** Inventory updated (lots deplete **at Confirm**, §8.4) · History updated · P&L updated (§4.2).

---

### M08 · Institutions & Tenders (§7.1–§7.4, §17 Institutions rows)

#### S17 · Institution List
- **SRS:** §7.1, §17 ("Empty · search · add · …")
- **Content:** Cards: name, contact, contract info, number of tender prices; **search**; empty state "No institutions yet — create one to start allocating supplies." + **Add Institution**.
- **Entry points:** More (§3.1) · Settings → Institutions (§13) · Home quick action (§6.1).
- **Actions:** Add → S18; tap → S19.
- **States:** Empty / filled with search; deactivated institutions still listed (history preserved §7.4) with visual inactive treatment (§15.2).

#### S18 · Create / Edit Institution
- **SRS:** §7.2, §17 (constitution of "Institution List / Profile" row + Tender Management row)
- **Field groups:**
  | Group | Fields |
  |---|---|
  | Basic | Institution Name* · Contact Person · Phone · Address · Location |
  | Tenders (list — add/edit **multiple**, §7.2) | **+ Add Tender** (same editor as Tender Management S20): Tender/Contract Name · Tender Number · Start Date · End Date · **Payment Terms** (paid after tender period ends — usually 1–2+ months later) · **Overdue Alert Days** (default 7 — drives Payment Due alert §14) · **Optional demanded quantities per item** (supplied-vs-required check for the period, off by default) · own **Fixed Tender Price list** (S21) |
  | Notes | Optional notes |
- **Actions:** Save (toast ✓); cancel; add/edit/delete-tender within group (see S20 rules); delete price (→ S21 actions).
- **Nav:** in: S17. out: S19.
- **States:** §18 complete set; multiple tenders per institution supported (§7.2 — picker at allocation, dues/bills per tender).

#### S19 · Institution Profile
- **SRS:** §7.3, §17 ("Institution List / Profile" row)
- **Sections/content:**
  - **Details** — basic + contract info.
  - **Tenders** — list of tenders with periods, payment terms, alert days; add/edit → S20 (list/add/edit; §7.2). **Ended tender** state: stops offering daily rates; keeps dues/payments history intact (§7.2).
  - **Fixed Tender Prices (per tender)** — → S21.
  - **Price History** — past rate changes per item, per tender (S21 sub-view).
  - **Today's Supply** — current allocations if cart in progress.
  - **Financial Performance** (lifetime + per period): Total Supply qty · Revenue · Allocated Cost · Assigned Expenses · Profit/Loss · Profit Margin · Supply History (§7.3).
  - **Receivables & Payments** — full sub-module → S22 (accrued dues, aging, payment records §7.5.1; Payments summary row: Total Billed · Collected · Pending in the profile header, §7.5.4).
  - **Memos & Bills (on demand)** — documents generated from this institution's supplies → BS-12 flow; section listing shared docs with **Shared ✓** markers (§7.5.5).
  - **Item-wise Performance / Monthly Performance** — inside the analytics view (§7.3).
- **Actions:** Edit (→ S18); **Deactivate** → **DL-04** (§7.4: confirmation names open business state — "Institution A has 1 active tender and ₹21,050 accrued dues"; historical references preserved; Receivables & Contract P&L stay intact and collectable; only new carts/allocations blocked, §15.2).
- **Nav:** in: S17. out: S18, S20, S21, S22, memo flow, performance views.

#### S20 · Tender Management (per institution)
- **SRS:** §7.2, §17 ("List/add/edit tenders · periods & alert settings · optional demanded quantities · ended-tender state")
- **Content:** Tender list per institution: name, number, period (start/end), payment terms, Overdue Alert Days, demanded quantities (per item, optional — supplies-vs-required view for the period), status (Active / **Ended** — rates stop, dues history kept).
- **Actions:** + Add Tender (editor shared with S18); Edit; fields as in S18 group table.
- **Nav:** in: S18/S19. out: tender editor (same form); link to price list S21; dues of that tender inside S22.
- **States:** Active / Ended / Empty list; edit without rewriting historical transactions (§2.4).

#### S21 · Fixed Tender Prices (per tender)
- **SRS:** §7.3, §17 ("Fixed Tender Prices: List · add · edit · delete confirmation · price history · missing-price sheet")
- **Content:** Per-item price list of the selected tender (Tomato ₹40/kg · Potato ₹35/kg · Onion ₹38/kg — §16.1); rate per item with unit.
- **Actions:** **+ Add Item Price** (Item · Unit · Fixed Tender Rate); **Edit** price (₹40 → ₹42 — applies to **future transactions only**, §7.3/§2.4); **Delete** price → DL-03 (confirm; historical transactions unaffected); **Price History** per item per tender.
- **Nav:** in: S19/S20. out: DL-03; BS-15 (missing-price "Add Price" route shares this form).
- **States:** Empty per tender ("no prices yet — add item prices"); edit/error/success toasts ("Tender Price Updated ✓" §15.4).

---

### M09 · Payments & Receivables (§7.5, §2.9–§2.10, §17 Payments rows)

#### S22 · Institution Receivables (embedded in Institution Profile)
- **SRS:** §7.5.1–§7.5.7, §17 ("Accrued per tender/period · aging · overdue badges · period billing · Record Payment · Post-Lock Adjustments · Payment history · memo/bill generation on demand + Shared ✓ + re-share warning · 'Period ended — prepare bill?' chip")
- **Content:**
  - **Per tender / per period groups** (§7.5.1): accrued dues (all supplied-and-confirmed amounts not yet paid, grouped per tender and period); each group: `Billed − Deductions − Received = Pending` (formula 14) + **aging** ("Oct period · ₹21,050 billed · 45 days pending"); **status badges**: Accrued (Not Billed) · Bill Issued · Partially Paid · Fully Paid. Fully Paid ⇔ Billed − Deductions − Received = 0 (§7.5.3).
  - **"Period ended — prepare bill?" chip** when a period ends with unbilled supplies → BS-10. Billing stays **manual**, never automatic (§7.5.2/§7.5.5). Preceded by "Tender Ending Soon" alert (§14).
  - **Credit balance display:** "Advance from Institution A: ₹X" — excess collections / downward-revision excesses; **auto-applied FIFO to the next period's bill** (§7.5.3).
  - **Payment History (section, §7.5.4):** all receipts per institution with period allocations; running pending totals; **Payments summary row** (Total Billed · Collected · Pending).
  - **Post-Lock Adjustments (section, §7.5.7):** linked list ("Original + Adjustments") also mirrored in S29.
  - **Payment-overdue badges** (Payment Due Alert trigger §14).
- **Actions:** **Record Payment** → BS-09 (amount received + deduction lines, FIFO w/ manual targeting override) · **Prepare Period Bill** → BS-10 · **Record Adjustment** → BS-11 · **Generate memo/bill** → BS-12.
- **Nav:** in: S19 (Receivables & Payments), S23 target. out: BS-09/10/11/12.
- **States:** Empty (no dues — cash buyer); per-period states; **"Shared ✓" / Outdated markers** on previously shared memos/bills after a revision or adjustment (§7.5.5); validation §15.1 (deductions+received > pending → **overpayment intent step**, SRS v2.9; shared-bill edit warning; downward revision credit notice — G-1 resolved).
- **Requirements:** Deactivated institution's dues remain visible and collectable (§7.4, §15.2); dues formulas exact; FIFO oldest-period-first default (§7.5.3).

#### S23 · Dues Overview (More)
- **SRS:** §7.5.6, §17 ("Per-institution rows: accrued / billed pending / credit balance / oldest aging · split pending totals · quick Record Payment")
- **Content:** Per-institution rows: accrued, billed pending, credit balance, oldest aging; **split pending totals** — canonical (§16.8): `Pending ₹36,500 = Billed pending ₹5,550 + Unbilled (accrued) ₹30,950`; quick **Record Payment** → BS-09; alert-driven (Payment Due Alert, Long-Pending Dues §14); Cash-vs-Accrued report link (§12).
- **Nav:** in: More (S08/G1). out: S22, BS-09.
- **States:** Empty ("all clear — nothing pending"); per-institution rows w/ aging (green→red aging emphasis allowed only as status color discipline: green=profit/red=loss, §5.2).

---

### M10 · Inventory (§8, §17 Inventory rows)

#### S24 · Inventory Dashboard
- **SRS:** §8.1, §17 ("Empty · status filters (Available/Partial/Rejected/Damaged/Waste) · search · attention panel"), §2.6, §14
- **Content:**
  - **Summary:** total available quantity · total stock value (formula 11 — **neither revenue, profit, loss nor cost**, §2.6) · item count.
  - **Filters:** All · Available · Partially Used · **Rejected** (lots sourced from rejections; status may still be Available) · Damaged · Waste/Loss. Search supported.
  - **Attention section (§8.1, §14):** "N items need attention" (rejected awaiting decision) + **Review Inventory**; **Old Stock detection**: any Available/Partially Used lot whose age exceeds the item's **shelf-life alert age** gets an **"Old Stock"** badge → quick actions **Use in Supply · Sell · Mark Waste** (→ DL-11 → BS-05/06/07).
  - **Empty state:** "No inventory available — rejected or remaining reusable stock will appear here."
- **Actions:** tap card → S25 · attention review · **+ Add stock manually** → BS-02 (source = "manual add", §8 intro) · quick menu DL-11 · filters/search inline.
- **Nav:** in: G1 tab, S08 quick action/card. out: S25, DL-11 flows.
- **Requirements:** §8.8 distinctions; rejected/remaining never auto-loss (§2.6, §4.3); old-stock alerts on quiet days too (§4.2).

#### S25 · Inventory Item Detail (per lot)
- **SRS:** §8.2, §8.7, §17 ("Source, cost, qty, status, movement history"), §15.2 (Same item, multiple lots)
- **Content (lot card fields):** Available **50 kg** · Original Cost **₹30/kg** · Stock Value **₹1,500** · Source **"Institution A — Rejected"** · Age **1 day** (shelf-life alert: 3 days for vegetables — §16.1) · Status (**Available** / Partially Used / Sold / Damaged / Waste·Loss — §2.6) · **Movement history** (§8.7): per movement date, ±quantity, source/destination ("+50 kg · Institution A Rejected" / "−30 kg · Used for Institution B" / "−20 kg · Wasted (Spoiled)"), running remaining.
- **Multiple lots:** kept as **separate lots per original rate**; app picks/asks which lot to consume (FEFO default, §8.4); each lot keeps its cost and history (§15.2).
- **Actions:** **Use in Supply** (→ BS-05) · **Sell** (→ BS-06) · **Mark Waste** (→ BS-07). Photo reused from Item Master (§5.3).
- **Nav:** in: S24. out: BS-05/06/07.
- **States:** Normal / attention (old-stock badge) / used-up → Sold / history empty state.

---

### M11 · Expenses (§9, §17 "Expense List/day")

#### S26 · Expenses (Per-Day List)
- **SRS:** §9, §17 ("Empty · add · category select · amount keypad · note · assignment · edit · delete confirm · summary"), §2.5
- **Content:** Per-day expense entries: type · amount · note · assignment. Example day: Transport ₹1,000 · Labour ₹1,000 · Packaging ₹500 → **Total ₹3,000**. **Daily Expense Breakdown** also visible in Review (S14 §6 item) and History (S29 expense detail).
- **Actions:** **Add expense** → BS-08; Edit → BS-08; **Delete** → DL-02 (confirm; immediate recalculation everywhere); read expense summary.
- **States:** Empty state; edit/delete **before confirmation only** (§9 — the cart expense set freezes at Confirm; later fixes via correction window, §2.8).
- **Rules:** Assignment optional — **Institution/Item assigned or Day-level only** (default day-level); **unassigned expenses never auto-distributed** (§2.5, §4.3); institution profit displays assigned expenses only (§2.5, S14/S19/S31).
- **Nav:** in: cart workflow step (§4.2 after remaining/rejected), S14, triggers recalculation. out: S14.

---

### M12 · Seller Payables (§9.1, §17 "Payables" row)

#### S27 · Seller Payables
- **SRS:** §9.1, §17 ("Seller rows: owed / aging / status (Open · Partly settled · Settled) · purchase flag paid/credit · settlements FIFO · due-date alerts")
- **Content:** Per-seller rows: amount owed · purchase date(s) · oldest aging · status (**Open · Partly settled · Settled**); data originates from purchase-line flag on S10 (Paid now default / On-credit → seller name + optional due date).
- **Actions:** **Settle open lines** → BS-13 (payment amount + reference, FIFO); quick view of unsettled/on-credit lines; Seller Payable Due alert surface (§14; alert lead **3 days, set in Settings → App — SRS v2.9**, resolves G-2).
- **Semantics:** P&L unchanged — daily Investment and Total Cost remain recognized at **purchase date**; settlements move **cash only** ("money in hand today" = receipts − settlements − expenses; §9.1, §2.9, §16.9: 24-Oct settlement of the ₹15,000 credit tomato = ₹0 in P&L).
- **Nav:** in: More. out: BS-13.
- **States:** Empty if he always buys cash (§9.1 "stays empty — zero extra effort").

---

### M13 · History & Other Day Entries (§10, §10.1, §17 History rows)

#### S28 · History Dashboard
- **SRS:** §10, §17 ("Date cards · filters · search · empty · other-entry rows"), §2.11, §10.1
- **Content:**
  - **Cart cards by date:** Investment / Revenue / Expenses / Result (green **+₹6,500**; red −₹5,000, §5.2). Canonical table: 20 Oct ₹34,000/₹43,500/₹3,000/+₹6,500 · 19 Oct ₹28,000/₹32,000/₹2,000/+₹2,000 · 18 Oct ₹35,000/₹32,000/₹2,000/−₹5,000.
  - **Other-entry rows (date-anchored, §10.1):** compact rows with type badge **Sale · Waste · Adjustment · Settlement** → detail views (S30). Rendered on **no-cart days and alongside carts** (§16.9: 22-Oct sale −₹60 · 23-Oct waste −₹450 · 24-Oct settlement ₹0 P&L).
  - **Filters:** Today · Yesterday · Last 7 Days · This Month · Previous Month · **Custom Date Range** (G7 picker) — **period totals shown for selection** (§10).
  - **Search:** across transactions (item names, institutions, dates) — hits fetched on demand (§10, §2.11).
  - **Built for years of data:** month-paginated date cards, virtualized ledger rows (§10) + precomputed aggregates (§2.11).
  - **Photos (V, optional, off by default):** weight-slip/delivery/transport photo attached to a transaction or adjustment — visible in Transaction Detail (S29) (§10).
- **Nav:** in: More, S16, S08. out: S29 (cart card), S30 (badge row).
- **States:** Empty ("no transactions yet"); filtered/search results; error/offline handled by G5.

#### S29 · Transaction Detail (frozen rates)
- **SRS:** §10, §17 ("Item / institution / expense / inventory / P-L detail views (frozen rates) · correction-window Edit / Locked badge · revision selector (R1/R2…) · linked adjustments · memo/bill generation on demand · optional photo attachments"), §2.4, §2.8
- **Content (immutable snapshot, §2.4):** item list with actual rates as used · institution allocations with tender rates as used (incl. custom) · inventory movements — usage & original costs, remaining/rejected decisions · expenses · final result · **sub-views:** item detail · institution detail · expense detail · inventory detail · P/L detail · **ref & revision:** TRX-20261020 (-R2, …) with **revision selector R1/R2/…** — dues/memos always follow the **latest** revision (§2.8.5) · **linked post-lock adjustments** section ("Original + Adjustments", §7.5.7) · **photo attachments** (optional; §10) — attach via **BS-21** picker (weight-slip/delivery/transport), view full-screen via **DL-14**.
- **Actions (window-period gated, §2.8.1):** window open → **Re-open & Correct** (Correction Mode; banner: *"Editing TRX 20-Oct (R2) — all changes stay date-stamped to the transaction's original date."* §2.8.2) + **Memo/bill generation** (→ BS-12, on demand only §7.5.5). Window closed → **Locked badge**; later fixes only via BS-11 post-lock adjustments or compensating entries (§2.10) — history never edited.
- **Nav:** in: S28. out: BS-11, BS-12, BS-21 (photo attach), DL-13 (revision picker), DL-14 (photo preview), correction flow (→ editing surfaces date-stamped to original date).
- **States:** Normal / Edit (within window) / Locked / revision variants R1–Rn / with-photos / with-adjustments / **outdated shared-bill warning banner** ("A shared bill exists — amounts have changed. Regenerate and re-share the updated version." §15.1, §7.5.5).

#### S30 · Other Day Entries (detail views)
- **SRS:** §10.1, §17 ("Sale · waste · adjustment · settlement rows (date-anchored — on no-cart days and alongside carts) · detail views · daily/monthly P&L aggregation")
- **Content:** A standalone entry's detail view per type:
  - **Sale** — revenue + lot reduction (formulas 8, 13) — e.g., 22-Oct 30 kg @ ₹28 → ₹840 revenue, ₹900 cost, **−₹60 day** (§16.9).
  - **Waste** — loss (formula 12) — e.g., 23-Oct 15 kg onions @ ₹30 spoiled → **−₹450 loss day** (margin blank per §15.2 Revenue = 0 rule).
  - **Adjustment** — inventory in and/or **Deductions line in P&L** (formula 15) dated on the adjustment's date (§7.5.7).
  - **Settlement** — **cash-out only** (₹0 in P&L — cost already recognized at purchase, §9.1).
- **Correction semantics (§2.8.6):** same window as carts — editable **until end of the calendar day following its date**, then locked; post-window wrong amounts fixed by a **compensating counter-entry** dated on the repair day (§2.10) — never history edits.
- **Aggregation:** feeds daily/monthly P&L aggregates exactly like cart effects (§10.1); Home TODAY cards aggregate them for the current date (§6.1, §10.1).
- **Nav:** in: S28 badge row. out: BS-05/BS-06/BS-07/BS-13/BS-11 as applicable for origin.
- **States:** Normal / Edit (window) / Locked / compensating-entry linked view.

---

### M14 · Profit & Loss (§11, §17 "P/L Dashboard")

#### S31 · P/L Dashboard
- **SRS:** §11, §17, §2.9, §5.2
- **Content:**
  - **KPIs:** Total Investment ₹5,00,000 · Total Revenue ₹6,20,000 · Total Expenses ₹40,000 · **Overall Profit ₹80,000** (or Overall Loss red).
  - **Tabs:** Overall · Daily · Weekly · **Institution-wise** · **Contract-wise (Tender)** · **Vegetable-wise** · Monthly (+ **custom period**).
  - **Institution-wise:** per institution — total supply qty, revenue, allocated cost, assigned expenses (§2.5), profit/loss, margin; **best & worst performer highlighted**.
  - **Contract-wise (per tender, per institution; §11, §16.8):** each tender agreement gets its own P&L line — supply quantity, supply revenue at the tender's fixed rates, allocated cost, **deductions from that contract's collections**, **net profit/loss + margin for the contract as a whole** (e.g., *A · Tender 1 · 1–31 Oct — Supply ₹21,050 · Effective ₹20,550 (−Deductions ₹500) → contract P/L*). Renegotiate/drop signal. Adjustments included as Effective Revenue.
  - **Vegetable-wise:** per item — total quantity, total investment, total revenue, assigned expenses, waste loss, overall P/L (e.g., Tomato: 5,000 kg · ₹1,50,000 · ₹1,85,000 · ₹5,000 · **+₹30,000**; best & worst highlighted).
  - **Breakdowns:** revenue breakdown · investment breakdown · expense impact · loss analysis.
  - **Deductions as their own line** on the adjustment's date (formula 15); aggregates show Effective Revenue = Supply − Deductions (§7.5.7).
  - **Profit is transaction-based** (recognized at confirm) — never mixes payment collections (§2.9); cash belongs to Receivables.
  - **Built for years:** precomputed, incrementally updated aggregates; deep histories paginate (§11).
  - **Charts:** simple only (§5.2 — Daily P/L, Institution comparison, Vegetable comparison, Monthly trend); loss analysis etc. stay number-forward.
- **Nav:** in: More, S08. out: report links (S33 Contract / Dues / Cash-vs-Accrued views share the math); institution rows → S19; item rows → S09.

---

### M15 · Reports (§12, §17 "Reports Dashboard")

#### S32 · Reports Dashboard
- **SRS:** §12, §17
- **Content:** Report-type list (**14 types**): Daily · Weekly · Monthly · Institution · **Contract (tender profitability per §11)** · Vegetable · Inventory · Expense · P/L · **Payment Dues** · **Cash-vs-Accrued** · **Payables** · **Spoilage by Item** · Custom Date Range. Goal framing: every report exists to **spot and stop profit leaks** (§12).
- **Actions:** Open report → S33.
- **Nav:** in: More. out: S33.
- **States:** Empty per report type (e.g., no data for range).

#### S33 · Report Detail
- **SRS:** §12, §17 ("detail · export")
- **Content (structured summary per type):**
  - **Payment Dues:** per institution — billed, collected, pending, aging (§7.5.6).
  - **Cash-vs-Accrued:** collections vs supplied revenue by period (§7.5, §2.9).
  - **Payables:** per seller — owed, aging, settlements (§9.1).
  - **Spoilage by Item:** waste/loss frequencies — the "what not to buy" view (§12; feeds §21 later, but views only §2.2 rows).
  - **Contract:** per-tender profitability incl. deductions (same math as S31 Contract-wise).
  - **Daily/Weekly/Monthly:** day/period P&L aggregations incl. Other Day Entries (§10.1).
  - **Institution/Vegetable:** same math as S31 tabs.
  - **Inventory:** stock value, aging, waste (§8.1/§8.8 data).
  - **Expense:** expense breakdown by type/assignment (§9).
  - **P/L:** the S31 dashboard as a document.
  - **Custom Date Range:** any range (G7 picker).
- **Actions:** **Export** (→ **BS-22** overlay: share / print / PDF / CSV) — no report editing, no report builder (§12).
- **Nav:** out: **BS-22** (export/share overlay); back to S32.

---

### M16 · Settings & Data Management (§13, §17 "Settings" + "Data Mgmt" rows)

#### S34 · Settings Dashboard
- **SRS:** §13 (groups table), §17 ("Settings Dashboard: Institutions · Item management · Business · Expense categories · Overdue Alert Days · backup reminder interval · Appearance · About")
- **Content:** Hub rows → Institutions (S17) · Items (S35) · Business (S36) · App (S37, incl. Expense Category Editor S38 and Reason Editor S39) · Data (S40) · About (S41).
- **Nav:** pure hub; out per row.

#### S35 · Item Management (Settings)
- **SRS:** §13 Items, §17 (Settings row), §6.2 (deactivated list view)
- **Content:** Full item list incl. **Deactivated** filtering ("show deactivated only via Settings → Item Management", §6.2); each item: name (§5.4), images (canonical mgmt §5.3 · replace via **BS-21**), units, **purchase-unit conversion** (bag/sack/crate = N base units §6.3), **shelf-life alert age (days)** per item.
- **Actions:** Add/Edit (→ S11 shared form); **Deactivate Item** → DL-05 (**never delete; keeps historical references — old transactions still show the item**, §13, §2.11.4); reactivate.
- **Nav:** out: S11.

#### S36 · Business Settings
- **SRS:** §13 Business, §17
- **Fields:** Business Name · Currency (₹ default) · Default Unit (kg) · **default shelf-life policy**.
- **Actions:** Save. No data-rewrite side effects (master edits never rewrite transactions, §2.4/§2.11).

#### S37 · App Settings
- **SRS:** §13 App, §14 thresholds, §17
- **Fields:** Appearance/Theme · **default Overdue Alert Days (default 7 — global fallback pre-filling the Add-Tender field, §7.2)** · **alert thresholds — Long-Pending Dues aging (default 30 days) · Tender Ending Soon N days (default 7) · Seller Payable alert N days (default 3)** (SRS v2.9, resolves G-2) · **backup reminder interval** (default 7 days) · links to Expense Category Editor (S38) and Reason Editor (S39).

#### S38 · Expense Category Editor
- **SRS:** §13 ("Expense category editor"), §9 (Transport, Labour, Loading, Unloading, Packaging, Other — configurable)
- **Content:** Editable category list (add/rename/remove — configuring, not rewriting history; old rows resolve by ID §2.11).
- **States:** Empty-guard (at least one category; Other default), duplicate-name validation.

#### S39 · Reason Editor (single shared editable set)
- **SRS:** §13, §2.11.2 ("Reasons come from editable dropdown sets"), §7.5.3 (deduction defaults: Quality · Damage · Quantity/short weight · Return · Other), §6.6/§8.6 (rejection & waste reasons)
- **Content:** **One editable reason set shared by** payment deductions (BS-09), inventory waste/loss marks (BS-07), and rejection reasons (BS-04) — never typed prose anywhere (§2.11.2; §15.2 Free-text values rule).
- **States:** Empty-guard; defaults preset.

#### S40 · Data Management
- **SRS:** §13 Data group, §17 ("Backup / Restore / Export / Import: Confirmations, destructive warnings, progress, completion feedback, Delete All Data (double confirm)"), §2.11.5
- **Actions:**
  - **Backup** → **BS-17**: confirmation → progress → complete feedback; **version-tagged** files (schema version + timestamp).
  - **Restore** → **BS-18**: validates version **before** confirming; destructive warning.
  - **Export Data** → **BS-19**: CSV/JSON — **columns mirror the §2.11 ledger** (one row per purchase line, allocation, expense, payment, settlement, waste, sale, adjustment).
  - **Import Data** → **BS-20**: validation + confirm.
  - **Delete All Data** → **DL-07**: **double confirmation**, warns of permanent loss.
- **Backup reminder** surfaces as Backup Due alert (§14, interval from S37).

#### S41 · About
- **SRS:** §13 About
- **Content:** App info, version.

---

### M17 · Attention Center (§14, §17 "Notifications")

#### S42 · Attention Center
- **SRS:** §14 (13 alert types), §17 ("All alerts from §14"), §6.1 (badged section on Home)
- **Classification note (resolved — was G-7):** §17 lists it as a module row; SRS v2.9 (§14) confirms the **badge on Home opens a full-screen Attention Center list (S42)**. It is app-surface #41 in this doc.
- **Content — all §14 alerts (each with icon from §5.5 per-alert slot, plain-English copy per §5.4):**
  | # | Alert | Trigger (per §14) | Surfaces |
  |---|-------|-------------------|----------|
  | 1 | Rejected Stock Alert | Rejected qty awaiting inventory/waste decision | S08, S24 |
  | 2 | Incomplete Cart Alert | Previous day's cart never confirmed | S08 + DL-09 on open |
  | 3 | Missing Institution Price | Allocation attempted without fixed tender price | S13 → BS-15 |
  | 4 | Inventory Attention | Damaged stock / items awaiting decision | S08, S24 |
  | 5 | Old Stock Alert | Lot age > item shelf-life alert age (§8.1) | S24, S08 |
  | 6 | Payment Due Alert | Period end + Overdue Alert Days passed, unpaid billing (§7.5.6) | S08, S22, S23 |
  | 7 | Long-Pending Dues | Aging beyond threshold (default 30 days — Settings → App, SRS v2.9 §13) (§7.5.6) | S23, S08 |
  | 8 | Backup Due | No backup in N days (default 7, §13) | S08 + S40 |
  | 9 | Tender Ending Soon | N days (default 7 — Settings → App) before End Date: "Tender 'X' ends 31 Oct — bill the period" | S08, S19/S22 ("prepare bill?" chip) |
  | 10 | Seller Payable Due | On-credit purchase due date approaching/passed, unsettled (alert lead default 3 days — Settings → App) (§9.1) | S08, S27 |
  | 11 | Price Updated | Fixed tender price changed (informational) | toast/info row |
  | 12 | Transaction Completed | Cart confirmed (informational) | toast/info row |
  | 13 | Waste/Loss Alert | Waste action recorded (informational) | toast/info row |
- **Nav:** in: S08 badge. out: per-alert target (cart, inventory, receivables, settings…). All text in plain English (§14, §5.4).
- **States:** Empty ("all clear"); **in-app only in v1** — OS-local notifications deferred post-v1 (§14, §21 rule 4 — G-8 resolved).

---

## 4. Bottom-Sheet Flow Registry (BS-01–BS-22)

Each flow carries its own §18 state chain and validation. Shared styling: frosted glass, 24-px top radius (§5.1), Hugeicons, plain-English copy (§5.4).

| ID | Flow | SRS | Contents / steps & states |
|----|------|-----|---------------------------|
| BS-01 | Inventory Availability Choice | §6.3, §8.4, §17 | Automatic on Item Detail when stock exists: "Existing Inventory Available: 50 kg" (reservation-aware) → options **Use Inventory / Purchase New (or both, not mutually exclusive)**; leads to qty entry (S10) and/or lot usage (BS-05). States: no-data (never shown), choice, confirm. |
| BS-02 | Add to Inventory (confirmation sheet) | §8.3, §6.6, §17 | item · quantity · **original purchase rate** · original cost · source (institution/remaining/manual) · date → **Add to Inventory**. Merger rule: identical item+unit from same source/day may merge; otherwise separate lots with own rate & history. Success: "Inventory Updated ✓" (§15.4). |
| BS-03 | Remaining Quantity Decision | §6.6, §17 "Remaining Quantity" | After allocation, per item: **Move to Inventory** (→ BS-02) or **Mark as Loss/Waste** (**reason required** → loss = remainder × original rate, formula 12). Partial remainders supported. Never auto-decided (§4.3). |
| BS-04 | Rejected Quantity Decision | §6.6, §17 "Rejected Quantity" | Per institution: enter exact rejected qty (partial OK) → Move to Inventory (→ BS-02) or Mark Loss/Waste + reason (dropdown, editable). Records source institution + date. Normally pre-Confirm; post-lock → BS-11 instead (§6.6). Completion feedback per §18. |
| BS-05 | Use Inventory in Supply | §8.4, §17 "Use Inventory" | Lot selection: **FEFO oldest-lot default with manual pick**; partial/full usage; source breakdown: Required 200 kg → From Inventory 50 kg (cost ₹1,500, already invested — no new money today, even if on credit) + New Purchase 150 kg @ today's rate; blended delivered-cost preview (**formula 4**: pool ÷ qty, e.g., ₹31.50/kg §16.3); confirmation. Reservations held while cart open; lots deplete at Confirm. Inventory cost recognized **exactly once** (§2.3). |
| BS-06 | Sell Inventory | §8.5, §17 "Sell Inventory" | select quantity → selling rate → review (revenue, original cost, **profit formula 13** — e.g., 50 @ ₹35 → ₹1,750 rev, ₹1,500 cost, **+₹250**) → confirm → complete. Date-anchored (Other Day Entry on no-cart days §10.1). Negative sale shows red. |
| BS-07 | Mark Waste / Damaged | §8.6, §8.1, §17 "Mark Waste" | select quantity → **reason (dropdown, editable via S39)** → **loss preview (formula 12)** — e.g., 20 kg @ ₹30 → Loss ₹600 → confirm **"Are you sure this stock is unusable?"** (§15.3) → complete; lot → Waste/Loss status; date-anchored. |
| BS-08 | Add / Edit Expense | §9, §17 "Add expense" | Expense Type (configurable set S38) · Amount (numeric keypad G3) · Optional note · **Optional assignment** to institution/item (default **day-level**, §2.5). Validations §15.1. Edit before confirm only. |
| BS-09 | Record Payment (with Deductions) | §7.5.3, §17 "Record payment" | Fields: **Amount received** · Reference (UTR/cheque/DD/voucher no.) · note · date. **Deduction lines** (add one or more): reason (editable set — Quality · Damage · Quantity/short weight · Return · Other, S39) · amount · optional link to transaction/adjustment. Net: dues reduce by Received + Deductions (formula 14). **Applied oldest pending period first (FIFO) w/ manual targeting override.** Overpay/downward-revision → credit balance ("Advance from Institution A: ₹X", auto-applied FIFO next bill) with warning: *"₹15,000 is already collected against this bill — revised total is ₹13,000. ₹2,000 becomes credit balance."* **Overpayment intent step (SRS v2.9, resolves G-1):** when Deductions + Received > pending, confirm *"Received ₹X exceeds pending ₹Y — ₹Δ becomes credit balance. Record it?"* → **Record it** (credit path) / **Adjust** (return to form; plain validation applies). Validation otherwise: "Deductions plus received cannot exceed the pending amount." (§15.1) |
| BS-10 | Prepare Period Bill | §7.5.2, §17 | Aggregate confirmed supplies within a tender's period (auto-stops at period end; **supports partial-period billing**); items summary, amounts, issue date, optional notes; display `Supply ₹X − Deductions ₹Y = Net to collect ₹Z`; confirm issue → **Bill record** ("Bill Issued"). Generated **only on demand** from the chip/S22. |
| BS-11 | Record Post-Lock Adjustment | §7.5.7, §2.10, §17 "Add post-lock adjustment", §10 | institution · linked transaction/tender/period · type (**Rejected/Short quantity · Accepted-weight difference · Quality deduction · Other**) · quantity and/or amount · reason (editable set) · date = **discovery date** · **optional photo evidence** (weight-slip/delivery/transport — off by default, §10/§15.2). Effects preview: rejected/short qty → **inventory lot** "Institution X — Post-Supply Rejected" (then normal Use/Sell/Waste); money → **dues deduction** + **P&L Deductions line** on that date (formula 15). Appears linked in S29 ("Original + Adjustments") and S22. |
| BS-12 | Memo / Bill Generate & Share | §7.5.5, §17 | **Only when the institution asks** (from Transaction Detail S29 or profile S19/S22). Choose type: **Transaction memo** (institution, supply date, memo number, items, quantity, **frozen tender rates**, amount per line, total, signature space) or **Period bill** (Supply − Deductions = Net to collect). On-screen preview (§5.4); **every generation timestamped**; numbering sequential per institution (A-2026-10-001; revision suffixes per §6.8); share as **screenshot/image or PDF**; optional **"Shared ✓"** marker; shared docs later outdated by revision/adjustment → warning: *"This shared bill is outdated — regenerate and share the updated version."* (§7.5.5). |
| BS-13 | Record Seller Settlement | §9.1, §17 "Settle seller payment" | Per seller: open on-credit lines; payment amount + reference; **FIFO** across lines; confirm. **Cash-only** — never P&L (§9.1, §16.9). Updates S27 statuses (Open → Partly settled → Settled). |
| BS-14 | Tender Picker | §6.5, §7.2, §15.1 | Per allocation row when multiple active tenders are ambiguous: choose tender with the **default preselected** (period covers today AND holds a price; latest-start tiebreak). Error: "Choose the tender for this supply." Each allocation stores its tender in the frozen snapshot (§2.4). |
| BS-15 | Missing Tender Price | §6.5, §15.1, §4.3 | "No fixed tender price found" → **Add Price to Institution** (asks **which tender** when multiple, §7.2 — shares the S21/S20 price form) or **Use Custom Rate** (this allocation only). Never borrow another institution's rate. Error text: "No fixed tender price is available for this institution." (§15.1) |
| BS-16 | Edit Cart Item (qty / rate) | §6.4, §17 "Edit qty · Edit rate" | Keypad edit of quantity (with purchase-unit toggle) and/or rate; live recalc of line cost, item totals, cart totals (§18). Also used by Correction Mode surfaces (date-stamped edits, §2.8.2). |
| BS-17 | Backup | §13, §17 | Confirm → progress → completion feedback; file is **version-tagged** (schema version + timestamp). |
| BS-18 | Restore | §13, §17 "Data restore" | Pick version-tagged file → **validate version before confirming** → destructive warning → confirm → progress → feedback. |
| BS-19 | Export Data | §13, §2.11.5, §17 | Choose **CSV/JSON** → generate → share/save. Columns map directly to §2.11 ledger rows (purchase line · allocation · expense · payment · settlement · waste · sale · adjustment) — no re-keying later (§21 dependency). |
| BS-20 | Import Data | §13, §17 | Pick file → **validation (+ schema/version)** → confirm → progress → feedback. |
| BS-21 | Image Picker (item images + photo evidence) | §17 ("Image mgmt"), §5.3, §10, §15.2 (**mechanism resolved in SRS v2.9**) | One shared pick/attach overlay used by **two features**: ① **item canonical image** — set/replace in S11 (Add/Edit Item) and S35 (Item Management) — **bundled seed image set or camera/gallery capture/pick, thumbnail preview before save, one image per item, no in-app editing** (§5.3, was Gap G-4) — canvas never modified; ② **photo evidence** — attach a weight-slip / delivery / transport photo to a **confirmed transaction** (S29) or a **post-lock adjustment** (BS-11), optional and off by default (§10/§15.2). Options: camera capture · gallery/file pick · remove/none; thumbnail preview before save; reuses the same Hugeicons slot everywhere (§5.5). Nothing numeric — never affects any formula. |
| BS-22 | Report Export / Share | §12, §17 ("detail · export") | From S33 report detail: choose output **Share · Print · PDF · CSV** (structured summary rendered in English §5.4, ₹ grouped §2.7); generation is read-only — no report editing, no report builder. Success feedback per §18. |

---

## 5. Dialog Registry (DL-01–DL-14)

> Every **Delete** anywhere in the app obeys the single §5.1 CRUD rule: gentle confirmation dialog first (§15.3 enumerate the destructive cases below).

| ID | Dialog | SRS | Reachable from | Todo |
|----|--------|-----|----------------|------|
| DL-01 | Remove cart item ("Remove Tomato from today's cart?" — Cancel/Remove) | §6.4, §15.3 | S12 line → Remove | Editable replacement of the line remains available via BS-16. |
| DL-02 | Delete expense (confirm) | §9, §15.3 | S26/S14 expense rows | |
| DL-03 | Delete tender price (confirm; historical transactions unaffected) | §7.3, §15.3 | S21 | |
| DL-04 | Deactivate institution — names open business state: "Institution A has 1 active tender and ₹21,050 accrued dues" | §7.4, §17, §15.2 | S19 (or list row action) | New carts/allocations blocked; dues intact & collectable. |
| DL-05 | Deactivate item (confirm; never delete; references kept) | §13, §17 | S35 (and S11) | |
| DL-06 | **Confirm & Complete** — whole-cart summary requiring explicit confirm (ONE per cart, §4.3) | §6.8, §15.3 | S15 | After confirm → S16; re-confirms gain R-suffix (§2.8). |
| DL-07 | Delete All Data — **double confirmation**, warns permanent loss | §13, §15.3 | S40 | |
| DL-08 | Duplicate item in cart — "Edit existing entry" / "Add a separate purchase line" | §6.3, §15.2, §17 | S10 on repeat add | Multiple lines allowed; combined qty + weighted-avg rate shown in S12. |
| DL-09 | Continue old cart? — "Continue yesterday's cart?" or "Start fresh" | §15.2 (Cart across days) | S12 open with stale date | Old cart must be confirmed or **discarded** first (discard confirm = DL-10, §17 modal row SRS v2.9). |
| DL-10 | **Discard cart** confirmation ("Discard today's cart? Entered items will be removed." — Cancel/Discard) | §15.2, **§17 modal row (officially added SRS v2.9 — resolves G-3)** | DL-09 "Start fresh" path | Destructive-ish confirm; releases reservations (§8.4). |
| DL-11 | Inventory quick-action menu — Use in Supply · Sell · Mark Waste (§17 "Inventory action") | §8.2, §8.1 | S25 / S24 card | Routes to BS-05/06/07. |
| DL-12 | Generic selector sheets (reusable pick-lists): Institution · Item · Unit · Expense category · Loss reason · Rejection reason | §17 modal row; editable sets §13 | expense entry (type), assignment, reason pickers, unit | All selections come from editable dropdown sets — **no free-text reasons** (§2.11.2, §15.2). |
| DL-13 | **Revision picker (R1/R2/…)** | §17 ("revision selector (R1/R2…)"), §2.8.5, §6.8 | S29 Revision row (transactions with revisions after re-confirms) | Selector overlay listing all stored revision snapshots with per-revision reference (TRX-20261020-R2…); dues & memos always follow the **latest** revision (§2.8.5); view-only switch — switching revisions never edits history. |
| DL-14 | **Attachment viewer (full-screen photo preview)** | §10 (photos "visible in Transaction Detail"), §15.2 | S29 photo thumbnails (also entered from BS-21 attach preview) | Full-screen overlay for transaction/adjustment photo evidence (weight-slip/delivery/transport) with capture/attach date — "available when disputes arise at collection time" (§10); back/close honored (§18). Implied visual affordance of §10's documentation; deletion of a photo follows the §5.1 gentle-confirm pattern. |

---

## 6. Global Components & Global States (G1–G10)

| ID | Component | SRS |
|----|-----------|-----|
| G1 | **Bottom navigation** — floating frosted glass, 5 destinations: Home · Vegetables · Cart (**badge = item count**) · Inventory · More. Mobile phone only — no tablet/desktop sidebar variant (SRS v2.8). | §3.1, §5.1, §17 |
| G2 | **Floating cart indicator** — glass; "Cart · 3 Items · Investment ₹34,000"; Hugeicons cart icon paired with text (never emoji); on Home and other screens. **Variants (§2.8.2, §6.1):** Active Cart ("Open · n items") · Completed-Day Home state ("Completed today — Edit Today's Cart" while window open / "Completed & Locked") · **Correction chip** "Correcting TRX 20-Oct (R2)" persistent beside any active cart. | §3.1, §5.1, §5.5, §2.8.2 |
| G3 | **Numeric keypad** — mandatory for **all** money & quantity input (not the OS keyboard); plain-English labels (§5.4); up to 2 decimals (§2.7). | §5.1, §5.2 |
| G4 | **Toast/Snackbars** (§15.4 exact set): Added to Cart ✓ · Institution Created ✓ · Tender Price Updated ✓ · Inventory Updated ✓ · Cart Completed ✓ — short, unobtrusive, no excessive animation. | §15.4 |
| G5 | **Global states applied to every screen** (§17 last row): Loading · Skeleton · Empty · Error · Offline · Success · Validation error · Toast/Snackbar · Bottom sheet · Modal · Full-screen error recovery. Enforced by §18: **no dead ends; every action gets all states.** | §17, §18 |
| G6 | **Financial visualization kit** — large numbers primary (profit green / loss red everywhere); simple charts only (Daily P/L, Institution, Vegetable, Monthly trend); allocation % progress bars. | §5.2 |
| G7 | **Calendar / date-range pickers** — filters (§10), custom range (§10/§11/§12), tender periods (S20), payment dates (BS-09/13), adjustment dates (BS-11). | §10–§12 |
| G8 | **Offline & sync posture** — fully offline (local-first); sync/backup optional and non-blocking; offline indicator + export/backup access when back online. | §2.8.3, §15.2 |
| G9 | **Date cards (Calendar/History browsing)** — clean rounded date cards everywhere history is browsed (S28 & future list contexts). | §5.1 ("Calendar/History browsing renders as clean rounded date cards") |
| G10 | **Icon system** — Hugeicons only, **Stroke Rounded** family, one pinned version; color/weight carries state (never style-switch); icons always paired with plain-English text (§5.4); 24 px nav/primary, 20 px inline, 4-px grid, `currentColor`; one icon per §14 alert type; **§17 map assigns every slot**. | §5.5, §17, §19 |

---

## 7. End-to-End Workflows (screen-step sequences)

### WF-01 · First-Time Setup (once) — §4.1
`S01 Splash → S02 Welcome → S04 Business Profile (₹, kg) → S05 Initial Items (seeded + custom) → S06 Initial Institutions (+ tenders + fixed prices) → S07 Setup Complete → S08 Home (Empty)` (S03 retired in SRS v2.7 — English-only, no language step)

### WF-02 · Canonical Daily Supply Flow — §4.2 (the only workflow diagram in the SRS)
```
S08 Home (Start Today's Supply)
→ S09 Item list (search, filters)
→ S10 Item Detail (BS-01 if stock exists)
→ S12 Cart (repeat items; BS-16 edit anytime; DL-01 remove)
→ S12 Continue → Allocate
→ S13 Allocate per item (BS-14 tender picker · BS-15 missing price)
→ BS-03 Remaining 50 kg (→ BS-02 Add to Inventory)   [per item]
→ BS-04 Rejected 30 kg (inventory vs waste + reason)  [per institution]
→ S26 Expenses (BS-08 add; DL-02 delete)
→ S14 Review Everything (warnings; TODAY'S RESULT ₹37,000 / ₹43,500 / +₹6,500 / 14.94%)
→ S15 Final Confirmation → DL-06 ONE confirm
→ S16 Cart Completed ✓ (TRX saved; inventory/History/P&L updated)
→ S08 Home (Completed Day; "Edit Today's Cart" while window open)
```

### WF-03 · Correction Window Flow — §2.8, §6.8, §7.5.5/§7.5.3
`S08 "Edit Today's Cart" (today's transaction only) OR S29 "Re-open & Correct" (any within-window)`
`→ Correction Mode (banner: "Editing TRX 20-Oct (R2) — all changes stay date-stamped to the transaction's original date."; Persistent chip "Correcting TRX 20-Oct (R2)" on S08; the Cart tab NEVER shows it)`
`→ edits via BS-16 / item add / expense / inventory decisions → totals recalc → re-confirm (S15/DL-06)`
`→ reference → TRX-20261020-R2; full revision snapshot stored (§2.8.5)`
`→ collected-money check: payments/deductions re-run FIFO against the latest revision; downward revision → credit balance + warning (BS-09)`
`→ if any memo/bill was shared: BS-12 outdated warning → regenerate & re-share`
Window closes at the **end of the calendar day following the transaction date** → S29 Locked badge (§2.8.1).

### WF-04 · Tender-Period Billing & Collection — §7.5, §2.9, §16.8
`Supply confirmed (revenue recognized at confirm; P&L only) → S22 accrued per tender/period (badges Accrued→Bill Issued)`
`→ 21 Oct: institution asks for proof → S29/BS-12 transaction memo → screenshot → Shared ✓`
`→ 31 Oct period ends → 5 Nov: BS-10 bill ₹21,050 issued ("Period ended — prepare bill?" chip path)`
`→ 20 Dec: collection → BS-09: ₹15,000 received + ₹500 deduction (reason: quality, linked Day-2 transaction) → Net to collect ₹20,550 → FIFO → Oct bill now billed 21,050 − 500 − 15,000 = pending ₹5,550 (aging 45 days)`
`→ S23 pending split ₹36,500 (Billed 5,550 + Accrued 30,950) → P&L: −₹500 Deductions line dated 20 Dec (formula 15); the ₹15,000 never re-enters P&L (§2.9)`
Partial payments keep residual visible (Partially Paid); full clearance ⇔ formula 14 = 0.

### WF-05 · Post-Lock Adjustment — §7.5.7, §2.10
`Discovery after lock (next-day delivery / late rejection / gate-weigh 200→198 / damage at unloading) → S29 (Locked badge) → BS-11: type, qty and/or amount, reason, date = discovery day → effects: inventory lot "Institution X — Post-Supply Rejected" and/or dues deduction + P&L Deductions line (formula 15) → linked in S29 ("Original + Adjustments") and S22 → History row (S30/S28). Locked transaction itself is never edited.`

### WF-06 · Inventory Lifecycle — §8
`BS-03/BS-04 → BS-02 lot (+50 kg @ ₹30 = ₹1,500) → S25 card (age, source, status) → old-stock badge per §8.1 when age > shelf-life → next supply: S10 → BS-01 → BS-05 (FEFO; partial usage rules §8.4; reservations) or DL-11 → BS-06 (+₹250) → BS-07 (−₹600) → movement history per lot (S25/S28/S30)`

### WF-07 · Inventory Reuse Without Double Counting (Day 2, §16.3) — §2.3, §2.2 f4
`S10 Tomato (need 200 kg) → BS-01: Existing 50 kg → choose Use + Purchase New → BS-05: 50 kg from lot (₹1,500, already invested) + 150 kg @ ₹32 (₹4,800) → delivered pool ₹6,300 → blended ₹31.50/kg → S13: A 100 kg × ₹40 = ₹4,000 · B 100 kg × ₹45 = ₹4,500 → allocation at blended ₹3,150 each → S14: profit +₹2,200 (margin 25.88%) → check 3,150+3,150 = 6,300 ✓ → Confirm depletes lot 50→0 (S25 history)`

### WF-08 · On-Credit Purchase & Settlement (Payables) — §9.1, §16.9
`S10 tomato line flagged On-credit → seller + due date (Day 1, ₹15,000, due 27 Oct) → P&L unaffected (already inside 20-Oct Investment) → S27 row (Open, aging) → Seller Payable Due alert → 24 Oct: BS-13 settle ₹15,000 (FIFO) → cash-out only: ₹0 in P&L (S30 settlement row)`

### WF-09 · Other Day Entries on Quiet Days — §10.1, §16.9
`No-cart day → sale/waste/adjustment/settlement from M10/M09/M12 flows → History badge-rows (S28) with own dates → per-type detail (S30) → daily/monthly P&L aggregation (sale-only day: normal margin rules; waste-only day: red loss, margin blank) → Home TODAY aggregates same-date entries`

### WF-10 · Institution Deactivation Safety — §7.4, §15.2
`S19 → Deactivate → DL-04 names open state ("1 active tender, ₹21,050 accrued dues") → confirmed: cannot allocate (S13 excludes); existing allocations kept; Receivables (S22) & Contract P&L (S31) intact and collectable; deactivated list visible (S17)`

### WF-11 · Data Management — §13
`S40 → BS-17 backup (version-tagged; reminder alert §14) → BS-18 restore (validate → destructive confirm) → BS-19 export (CSV/JSON mirroring §2.11 ledger) → BS-20 import (validate+confirm) → DL-07 delete-all (double confirm)`

**Cross-midnight note (§15.2):** opening the app with an unconfirmed old cart → DL-09 ("Continue yesterday's cart?" / "Start fresh" → DL-10) before any new cart.

---

## 8. Traceability Matrix (SRS → screens)

### 8.1 §17 Master Screen & State Map — row-by-row proof of coverage (nothing dropped)

| §17 Module / Screen row | Covered by |
|--------------------------|------------|
| Splash | S01 |
| Welcome / Intro | S02 |
| First-Time Setup | S04 (§4.1 flow; Language step removed in v2.7) |
| Initial Items | S05 |
| Initial Institutions | S06 |
| Setup Complete | S07 |
| **Home Dashboard** (Empty · Active Cart · Completed Day · attention badges · pending split · Correction chip) | S08 |
| Item Home / All Items | S09 |
| Search / Filter Items (No-results) | V of S09 |
| Item Detail (Quantity Entry) | S10 |
| Inventory Availability Check | BS-01 |
| Add to Cart (toast · duplicate prompt · error) | S10 + DL-08 + G4 |
| Add/Edit Custom Item (Image mgmt — bundled set or camera/gallery §5.3 SRS v2.9, unit, deactivate confirmation, deactivated list, item history) | S11 (+S35 deactivated list; DL-05; BS-21) |
| **Cart** (Empty · Active · item detail · edit qty/rate · remove confirmation · investment summary · validation · Continue · no Correction Mode here) | S12 (+BS-16, DL-01) |
| Institution List / Profile (empty · search · add · edit · tenders multi · deactivation w/ open state named) | S17 / S18 / S19 (DL-04) |
| Fixed Tender Prices (list · add · edit · delete confirm · price history · missing-price sheet) | S21 (+BS-15, DL-03) |
| Tender Management (list/add/edit · periods & alerts · demanded quantities · ended state) | S20 |
| Institution Receivables (accrued per tender/period · aging · overdue badges · period billing · Record Payment · Post-Lock Adjustments · payment history · memo/bill on demand + Shared ✓ + re-share warning · "prepare bill?" chip) | S22 (+BS-09/10/11/12) |
| Dues Overview (per-institution rows · credit balance · split totals · quick payment) | S23 (+BS-09) |
| Allocate per Item (auto rate · custom · missing price · tender picker · progress % · live revenue/cost/profit · overrun validation) | S13 (+BS-14/15) |
| Remaining Quantity (inventory confirm · waste confirm + reason) | BS-03 (+BS-02) |
| Rejected Quantity (partial entry · reason · inventory vs waste · completion feedback) | BS-04 (+BS-02) |
| Inventory Dashboard (empty · filters incl. Rejected · search · attention panel) | S24 |
| Inventory Item Detail (source, cost, qty, status, movement history) | S25 |
| Use Inventory (qty · FEFO default + manual pick · reservations · partial/full · mixed-source breakdown · cost of consumed lot · confirmation) | BS-05 |
| Sell Inventory (qty · rate · review · confirm · complete) | BS-06 |
| Mark Waste (qty · reason · loss preview · confirm · complete) | BS-07 |
| Expense List/day (empty · add · category · keypad · note · assignment · edit · delete confirm · summary) | S26 (+BS-08, DL-02) |
| **Review Everything** (all 6 section groups incl. warnings) | S14 |
| Final Confirmation (full summary · confirm dialog · Edit Cart path) | S15 (+DL-06) |
| Cart Completed ✓ (result · reference & R-suffix · View Details/History/Home) | S16 |
| History Dashboard (date cards · filters · search · empty · other-entry badges) | S28 (+S30) |
| Transaction Detail (sub-views · Edit/Locked · revision selector · linked adjustments · memo generation · photos) | S29 (+BS-11/12, DL-13/14) |
| Other Day Entries (4 badge types · detail views · P&L aggregation) | S30 |
| Seller Payables (rows · flag · settlements FIFO · due alerts) | S27 (+BS-13) |
| P/L Dashboard (Overall · Daily · Weekly · Monthly · Custom · Institution-wise · Contract-wise · Vegetable-wise · breakdowns · best/worst) | S31 |
| Reports Dashboard (13 listed type groups ↔ §12's 14 types incl. Payment Dues, Cash-vs-Accrued, Payables, Spoilage by Item · detail · export) | S32/S33 (+BS-22) |
| Settings Dashboard (all groups) | S34 (+S35–S37, S41) |
| Data Mgmt (confirmations · destructive warnings · progress · completion · Delete All double confirm) | S40 (+BS-17–20, DL-07) |
| Attention Center (all §14 alerts) | S42 (+S08 badge section) |
| Modals / Sheets row (22 items — SRS v2.9 added "Discard cart") | §4–§5 registries: Institution DL-12 · Item DL-12 · Unit DL-12 · Expense category DL-12 · Inventory action DL-11 · Loss reason DL-12 · Rejection reason DL-12 · Select tender BS-14 · Record payment BS-09 · Settle seller BS-13 · Add post-lock adjustment BS-11 · Edit qty/rate BS-16 (one sheet covering the row's two separate items "Edit qty" and "Edit rate") · Add expense BS-08 · Remove item DL-01 · Mark waste BS-07 · Inventory usage BS-05 · Inventory sale BS-06 · Cart completion DL-06 · **Discard cart DL-10** · Data restore BS-18 · Data deletion DL-07 · **beyond-row overlays:** BS-21 (image picker/photo evidence) · BS-22 (report export/share) · DL-13 (revision picker) · DL-14 (attachment viewer) |
| Global States row (11 states, every screen) | G5 (+per-screen States lists) |

### 8.2 §2 accounting rules

| Requirement | SRS | Covered by |
|---|---|---|
| Two price types, never one global rate | §2.1 | S10 (purchase rate) vs S20/S21 (fixed tender price per institution/tender) · S13 auto rate display |
| f1 Purchase Cost | §2.2.1 | S10 auto · S12 totals |
| f2 Allocation % | §2.2.2 | S13 progress bars (1-decimal display, exact calc §2.7) |
| f3 Institution Revenue | §2.2.3 | S13 · S14 |
| f4 Allocated cost / blended delivered-cost pool | §2.2.4, §16.3 | BS-05 preview · S13/S14 allocation at blended ₹31.50 · S31 |
| f5–f7 Cost composition | §2.2.5–7 | S14 TODAY'S RESULT (₹34,000 + ₹0 + ₹3,000 = ₹37,000) |
| f8–f10 Revenue/Profit/Margin | §2.2.8–10 | S08 TODAY cards · S14 · S16 · S31 (margin blank at revenue 0, §15.2) |
| f11 Inventory Value | §2.2.11 | S24 summary (neither revenue/profit/loss/cost — §2.6) |
| f12 Waste Loss | §2.2.12 | BS-07 preview (20 kg @ ₹30 → ₹600) |
| f13 Inventory Sale Profit | §2.2.13 | BS-06 review (₹250) |
| f14 Net Collectable | §2.2.14 | S22/S23 · BS-09 |
| f15 Deduction P&L line | §2.2.15 | BS-11 · S31 Effective Revenue line · S30 |
| No double counting | §2.3 | BS-01/BS-05 + WF-07 (₹6,300 check) |
| Historical rate preservation | §2.4 | S29 frozen views · S21 edit applies future-only |
| Expense allocation rule | §2.5 | BS-08 assignment field (default day-level) · S14/S19/S31 display assigned-only |
| Rejected/remaining never auto-loss | §2.6, §4.3 | BS-03/BS-04 contractor decision |
| Precision & ₹ Indian grouping | §2.7 | G3 keypad (≤2 decimals), displays everywhere (allocation % 1-decimal; ₹5,00,000 grouping on S31) |
| Correction window + two cart types + local-first + business-date + revision snapshots + standalone windows | §2.8 | S12 (daily cart only) · S08/S29 Correction entry · banner chip · S30 standalone window · G8 offline · BS-16/17/18/19/20 · snapshots S16/S29 |
| Revenue vs cash (accrued vs billed; khata cash-only) | §2.9 | S08 pending split · S22/S23 · S27 · S31 "never mixes cash" |
| Post-lock principle | §2.10 | BS-11 · S29 linked list |
| Data Recording Standard (typed rows · dropdown reasons · append-only · ID integrity · export mirrors ledger) | §2.11 | S10 (purchase-line source/quality fields) · S13 (tender per row) · BS-04/07/09 (reason dropdowns) · DL-04/DL-05 deactivation never delete · BS-19 export · append-only S29 revisions/compensating entries |

### 8.3 §3 navigation · §4 flows · §5 design system

| Requirement | Covered by |
|---|---|
| 5-tab bottom nav + More contents (incl. Payments & Dues, Seller Payables) | G1 (S08/S09/S12/S24 + More hub) |
| Persistent cart indicator | G2 |
| First-time setup flow | S01/S02 + S04–S07 (WF-01) |
| Daily supply flow + supply irregularity | WF-02 + S12/S30 (carts only on supply days; dues/aging/alerts run on quiet days §4.2) |
| Decision points never automated | §4.3 mapped: BS-01 (inventory vs buy), BS-03 (remaining), BS-04 (rejected), BS-15 (missing price), DL-06 (one confirm) |
| Soft-modern glass, full-width rows, rounding, shadows | §5.1 (global constraint header) |
| Component library incl. keypads, sheets, toasts | §5.1 + G3/G4 |
| Financial visualization rules | §5.2 + G6 |
| Image usage (hero 4:3/1:1, no marketplace styling) | §5.3 on S09/S10/S25; canonical image mgmt S11/S35 |
| English-only + simple-language rule (§5.4) | all screens (plain-English strings; no language switcher); memos/bills in English BS-12 |
| Hugeicons single system (Stroke Rounded, icon+text, per-slot map) | G10 + §17 map assignment |

### 8.4 §6–§10 screens — see per-screen SRS refs in §3 (each screen cites its section) and §8.1 mapping above.

### 8.5 §11 tabs & §12 reports

| Requirement | SRS | Covered by |
|---|---|---|
| P/L tabs Overall/Daily/Weekly/Institution-wise/Contract-wise/Vegetable-wise/Monthly (+custom) | §11 | S31 |
| Contract-wise incl. deductions, renegotiate-or-drop signal | §11, §16.8 | S31 (Contract-wise) · S33 |
| Best/worst performer highlights | §11 | S31 (Institution & Vegetable tabs) |
| Deductions line / Effective Revenue in aggregates | §7.5.7, §11 | S31/S33 |
| Precomputed aggregates & pagination | §11, §10 | S28/S31 |
| 14 report types + detail + export, no builder | §12 | S32/S33 (+G7 picker) |
| Report inventory values chain | §12/§8.8 | S33 Inventory, Expense, P/L, etc. |

### 8.6 §13 settings groups → S34 hub: Institutions S17 · Items S35 · Business S36 · App S37 (expense categories S38 · reason editor S39 · overdue alert days · **alert thresholds Long-Pending/Tender-Ending/Seller-Payable, SRS v2.9** · backup interval; no Language row — English-only §5.4) · Data S40 · About S41. Deactivation safeties DL-04/DL-05. §2.11 export mirror BS-19.

### 8.7 §14 alerts — mapped 13/13 in S42 table (S08 badge hosts the same list).

### 8.8 §15 validation & edge cases

**§15.1 messages → surfaces:**

| Message | Shown at |
|---|---|
| "Enter a quantity before continuing." | S10, S15 path |
| "Enter today's purchase rate." | S10 |
| "Allocated quantity cannot exceed available quantity." | S13 |
| "No fixed tender price is available for this institution." | S13 (BS-15) |
| "Enter a valid amount." | S10, BS-08, BS-09, BS-13 |
| "Quantity must be greater than zero." | S10, BS-03/04/05/06/07 |
| "Up to 2 decimal places allowed." | S10 keypad, BS-16 |
| Confirm-with-warnings (unallocated / no rates / rejections) | S14 warnings list |
| "Choose the tender for this supply." | BS-14 |
| "Deductions plus received cannot exceed the pending amount." | BS-09 (with the **overpayment intent step** — "Record it" → credit balance; SRS v2.9, G-1 resolved) |
| "A shared bill exists — amounts have changed. Regenerate and re-share the updated version." | BS-12 / during correction of a shared transaction |
| "₹15,000 is already collected… ₹2,000 becomes credit balance." | BS-09 (downward revision) |

**§15.2 edge cases → handling surface:** fractional ≤2 dec (G3/S10) · rounding (§2.7 displays) · one unit per item (S11, S10) · duplicate item (DL-08/S12) · ₹0 rate flagged (S14) · multiple lots FEFO (S25/BS-05/§8.4) · cart across days (DL-09/DL-10) · offline (G8) · loss day display (S14/S16/S28/S30/G6) · margin blank at revenue 0 (S30 waste-only day) · allocation remainder (BS-03 + S14 warning) · inventory more than required (BS-05 partial) · deactivated w/ open allocation (DL-04/S13) · irregular supply (S30/S23 quiet-day operation) · rate typo after confirm (S29 window/Locked) · rejection after lock (BS-11) · multiple tenders (BS-14) · lot consumption order (BS-05 FEFO) · rate prefill ("Last: ₹28/kg · 19 Oct" S10) · purchase in bags (S10 toggle) · inventory aging (S24/S25) · partial payments FIFO (BS-09) · supply once paid months later (S31/S33) · Correction Mode beside daily cart (S08/S12, §2.8) · blended allocation (BS-05/WF-07) · revision re-mapping FIFO + credit balance (BS-09) · period ended chip (S22) · no-cart money events (S30) · reservations (S12/S24/BS-05) · credit purchases (S10/S27) · standalone entries window (S30) · free-text values (DL-12 → dropdowns only) · photo evidence (S29/BS-11 attach via BS-21, optional default-off).

**§15.3 confirmations → DL-01, BS-07 (waste confirm), DL-02, DL-03, BS-18, DL-07.  §15.4 success toasts → G4.**

### 8.9 §16 canonical dataset — acceptance placement of every figure

| Canonical figure | Must appear on |
|---|---|
| Day 1: purchases ₹34,000 (Tomato 500×₹30 · Potato 400×₹25 · Onion 300×₹30) | S12/S14/S16 |
| Allocation: A 200/150/100 · B 150/150/100 · C 100/100/100; revenue-total ₹43,500 (A 17,050 · B 15,550 · C 10,900) | S13/S14/S29 |
| Allocated cost ₹32,500 (A 12,750 · B 11,250 · C 8,500); institution profit +4,300/+4,300/+2,400; consistency gate 43,500−11,000=32,500 | S14 (pre-confirm analysis) · S29 |
| Remaining 50 kg → lot ₹1,500 (not loss, not revenue) | BS-03 → BS-02 → S24/S25 |
| Result: cost ₹37,000 · revenue ₹43,500 · profit +₹6,500 · margin 14.94% | S14/S16 (Home cards Day 1: 34,000/43,500/3,000/+6,500) |
| Vegetable-wise +3,550/+3,650/+2,300 (sums to gross 9,500 ✓) | S14 & S31 Vegetable tab |
| Day 2 blended: pool ₹6,300, blended rate ₹31.50/kg (formula 4), revenue ₹8,500, profit +₹2,200, margin 25.88%, lot 50→0 | S10/BS-05/S13/S14/S25 |
| Rejection 30 kg → ₹900 inventory OR ₹900 loss (standalone, not added to Day 1–2 totals) | BS-04 + branch consumed by WF-09's 22-Oct sale |
| Inventory sale 50 kg @ ₹35 → +₹250 (standalone exercise) | BS-06 |
| Waste 20 kg @ ₹30 → −₹600 | BS-07 |
| Overall: ₹5,00,000 / ₹6,20,000 / ₹40,000 / +₹80,000 | S08 Overall card · S31 |
| Receivables chain: memo Shared ✓ → 5-Nov bill ₹21,050 → −₹500 deduction → ₹15,000 collected → pending ₹5,550 (45-day aging from 5-Nov bill) · pending split ₹36,500 = 5,550 + 30,950 (B 20,050 + C 10,900) · Contract line Supply 21,050 → Effective 20,550 | S22/S23/S08/S29/BS-09/BS-10/BS-12/S31 |
| Other-entry days: 22-Oct sale −₹60 · 23-Oct waste −₹450 · 24-Oct settlement ₹0 P&L; no ghost totals (no ₹840 / ₹450 / ₹15,000) inside the 20–21 Oct supply days | S28/S30 · S31 daily/aggregates · WF-09 |
| Display rounding: Onion 33.3%×3 = 99.9% display but stored 100% exact | S13 (progress %) |

*Numbers above are typed verbatim from the §16 canonical dataset — consistency gate: Revenue − Institution gross profit must always reconcile (43,500 − 11,000 = 32,500 = allocated purchases ✓).*

### 8.10 §19 build checklist → covering IDs (acceptance gate)

| Checklist item (§19) | Covered by |
|---|---|
| End-to-end build; not a dashboard-only/landing/mocks; no invented features; no marketplace; no ERP | All modules M01–M17; scope guardrails §1.4 |
| Keep Inventory, Fixed Tender Pricing, cart workflow, Review→Confirm once, Receivables | M10 · S21 · M04–M07 · M09 |
| Every screen connects; §18 states; every button has a destination | §18 + per-screen States/Nav (no dead ends) |
| §2.2 formulas; instant updates | WF-02/07; S10/S12/S13/S14 |
| No double counting; frozen history; correction window + lock + snapshots; profit ≠ cash | BS-05 · S29 · §2.8 cluster · §2.9 cluster |
| Never auto-loss; post-lock via adjustments | BS-03/04 · BS-11 |
| One active cart; confirm once; expenses per §2.5 | S12/S15/DL-06 · BS-08 |
| Mobile phone only; no tablet/desktop sidebar variant (SRS v2.8) | §3.1 · G1 |
| Fast minimal-typing UX: visual selection, auto rates, auto inventory detection, instant totals, bag entry, last-rate prefill | S09/S10/BS-01/BS-16 |
| English-only UI incl. memos (§5.4) | all screens · BS-12 |
| Hugeicons single system; icon+text; alert icons | G10 |
| Memos only on demand; screenshot/PDF; Shared ✓; outdated re-share | BS-12 |
| Dues reconcile (formula 14=0 ⇒ Fully Paid); deductions hit P&L on their date | BS-09 · BS-11 · S22 |
| Multiple tenders (picker; per-tender price lists & dues) | S20/S21/BS-14 |
| FEFO default; backup reminder | BS-05 · §14 + S37/S40 |
| Correction Mode separate & coexisting; blended allocation | S08/S29/S12 · BS-05 |
| Pending split; FIFO re-mapping; credit balances auto-apply (via overpayment intent step); thresholds in Settings → App | S23 · BS-09 · S37 |
| Other Day Entries date-anchored with correct aggregation | S30/S28 |
| Payables (flag, aging, cash-only settlements) | S10/S27/BS-13 |
| Reservations deducted; released on discard/revision; deplete at Confirm | BS-05/S12/S24 |
| Tender picker per row + default rule; Add Price asks which tender | BS-14/BS-15 |
| Search; photo evidence; version-tagged backups; Spoilage report; Contract-wise P&L | S09 · S29 · BS-17/18 · S33 · S31 |
| §2.11 standard honored; export mirrors ledger | S10/S13/BS-* · BS-19 |
| Standalone entries share correction window; post-window compensating entries | S30 |
| Deactivation keeps receivables/P&L; blocks new allocations | DL-04/S13 |
| No §21 features in v1 (but ledger capture from day one) | §21 excluded; BS-19/S10 provide training data |
| Attention Center full-screen behind Home badge; alerts in-app only v1; Discard-cart modal exists | S42 · S08 badge · DL-10 · G5 |

### 8.11 §21 — explicit non-scope (noted once): no Insight/Predictive/AI-assistant feature ships in v1 (§19 — "No future-roadmap/AI feature ships in v1"); the only v1 obligation is capturing §2.11-shaped ledger data from day one (typed rows everywhere + BS-19 export mirrors the ledger).

---

## 9. Per-Screen Universal-Element Compliance Matrix ("what must be on every screen")

This matrix verifies, screen by screen, that every universal constraint from §3.1, §5.1–§5.5, §2.7–§2.8, §15 and §18 of the SRS is either **present** or **explicitly not applicable**. It is the companion checklist to §19 for the build phase; a screen missing any enforced row here fails acceptance.

### 9.1 Universal elements (defined once, checked everywhere)

| # | Element | Source | Regime |
|---|---------|--------|--------|
| U1 | Glass app bar with screen title (frosted surface + solid fallback) | §5.1, G6 | Always (except S01 splash) |
| U2 | Persistent navigation — bottom nav, same 5 destinations (mobile phone only; retired sidebar never returns) | §3.1, G1 | Always from S08 (post-setup shell) |
| U3 | Global states row (Loading · Skeleton · Empty · Error · Offline · Success · Validation error · Toast · Sheet · Modal · Full-screen recovery) | §17 global row, G5 | Always |
| U4 | Hugeicons Stroke Rounded only; every icon paired with text; plain-English labels everywhere (§5.4) | §5.5, §5.4, G10 | Always |
| U5 | Offline-first: local-first operation, non-blocking sync indicator, no blocking errors offline | §2.8.3, §15.2, G8 | Always |
| U6 | §18 state chain per action (Normal→Edit→Cancel→Error→Empty→Success→Confirmation→Back); back always resolves to a parent, never a dead end | §18 | Always |
| U7 | Scope guard: contractor-only, no marketplace/ERP idioms, no invented features | §1.4, §19 | Always |
| U8 | Floating cart indicator chip — Active Cart / Completed-Day Home / Correction Mode variants | §3.1, §2.8.2, G2 | Conditional — visible whenever a cart exists today; ◐ = chip merges into the screen's own subject |
| U9 | Numeric keypad for every money/quantity field (OS keyboard suppressed) | §5.1, §5.2, G3 | Conditional — typed on this screen (●) or via linked overlay (◐) |
| U10 | Success toasts — §15.4 fixed wording (●) or generic §18 success feedback (◐) | §15.4, G4, §18 | Conditional |
| U11 | ₹ figures with Indian digit grouping; 2-decimal quantities; totals computed from exact stored values | §2.7 | Conditional — screen displays money |
| U12 | Profit = green palette, Loss = red palette on every displayed result | §5.2 | Conditional — screen displays P/L |
| U13 | Large touch targets; a primary action per screen state; sticky CTA on long forms | §3.1, §5.2 | Conditional |

Symbols: ● = present/enforced · ◐ = conditional (see note) · ○ = not applicable by design.

### 9.2 Always-on elements (U1–U7) — 41 screens

| Screen | U1 Bar | U2 Nav | U3 States | U4 Icons/Lang | U5 Offline | U6 §18 | U7 Scope |
|--------|--------|--------|-----------|----------------|------------|--------|----------|
| S01 Splash | ○ | ○ | ● | ● | ● | ● | ● |
| S02 Welcome/Intro | ◐ | ○ | ● | ● | ● | ● | ● |
| S04 Business Profile | ● | ○ | ● | ● | ● | ● | ● |
| S05 Initial Items | ● | ○ | ● | ● | ● | ● | ● |
| S06 Initial Institutions | ● | ○ | ● | ● | ● | ● | ● |
| S07 Setup Complete | ● | ○ | ● | ● | ● | ● | ● |
| S08–S42 (all daily-app screens) | ● | ● | ● | ● | ● | ● | ● |

Notes: S02 is full-bleed intro slides — only a skip/close chrome is required (◐). Setup (S04–S07) keeps the glass step-header with back; the G1 shell enters at S08. Bottom sheets and dialogs render atop the underlying screen, which keeps its bar/nav (§9.4).

### 9.3 Conditional elements (U8–U13) — 41 screens

| Screen | U8 Chip | U9 Keypad | U10 Toast | U11 ₹ | U12 G/R | U13 Primary | Note |
|--------|---------|-----------|-----------|-------|---------|--------------|------|
| S01 | ○ | ○ | ○ | ○ | ○ | ○ | pre-shell |
| S02 | ○ | ○ | ○ | ○ | ○ | ◐ Skip | |
| S04 | ○ | ○ | ○ | ○ | ○ | ● Continue | |
| S05 | ○ | ● inline | ◐ | ○ | ○ | ● | custom-item form numeric fields |
| S06 | ○ | ● | ◐ | ● | ○ | ● | price entry keypad; institution-add generic success feedback |
| S07 | ○ | ○ | ○ | ○ | ○ | ● Confirm | |
| S08 | ● | ○ | ○ | ● | ● | ◐ | primary = context (Open Cart / Correction chip / Browse; §6.1) |
| S09 | ● | ○ | ○ | ◐ hint | ○ | ◐ | tap cards; "Added to Cart ✓" toast fires at S10 |
| S10 | ● | ● | ● Added to Cart ✓ | ● | ○ | ● Add to Cart | |
| S11 | ● | ● | ◐ | ○ | ○ | ● Save | |
| S12 | ◐ | ◐ via BS-16 | ○ | ● | ○ | ● | chip redundant — screen itself is the cart |
| S13 | ● | ● | ○ | ● | ● | ● | inline institution P/L colored |
| S14 | ● | ◐ via BS-16 | ○ | ● | ● | ● Confirm | |
| S15 | ● | ○ | ○ | ● | ● | ● Confirm | |
| S16 | ◐ | ○ | ● Cart Completed ✓ | ● | ● | ◐ links | completion card carries its own state |
| S17 | ● | ○ | ● Institution Created ✓ | ○ | ○ | ◐ Add Institution | toast lands here on return from S18 |
| S18 | ● | ● | ◐ | ◐ price list | ○ | ● Save | |
| S19 | ● | ○ | ○ | ● | ● | ○ | read-first dashboard |
| S20 | ● | ● | ◐ | ◐ | ○ | ● | |
| S21 | ● | ● | ● Tender Price Updated ✓ | ● | ○ | ● + Add Item Price | |
| S22 | ● | ◐ via BS-09 | ○ | ● | ◐ status | ◐ Record Payment | |
| S23 | ● | ◐ via BS-09 | ○ | ● | ◐ status | ◐ quick pay | |
| S24 | ● | ◐ via BS-02 | ● Inventory Updated ✓ | ● | ○ | ◐ + Add stock manually | stock value neutral (§2.6) |
| S25 | ● | ● sheets | ● Inventory Updated ✓ | ● | ◐ loss rows | ◐ Use/Sell/Waste | |
| S26 | ● | ◐ via BS-08 | ◐ | ● | ○ | ● Add Expense | |
| S27 | ● | ◐ via BS-13 | ◐ | ● | ○ | ◐ Settle | cash-neutral (SRS §9.1/§2.9) |
| S28 | ● | ○ | ○ | ● | ● | ○ | browse |
| S29 | ● | ○ | ◐ memo/adjust | ● | ● | ◐ Re-open/Generate | |
| S30 | ● | ◐ edit flows | ◐ | ● | ● | ◐ edit in window | |
| S31 | ● | ○ | ○ | ● | ● | ◐ tabs/custom | |
| S32 | ● | ○ | ○ | ○ | ○ | ◐ open report | |
| S33 | ● | ○ | ◐ export done | ● | ● | ● Export | Export → BS-22 |
| S34 | ● | ○ | ○ | ○ | ○ | ○ | hub — rows navigate |
| S35 | ● | ● | ◐ | ○ | ○ | ◐ Add/Edit | |
| S36 | ● | ○ | ◐ | ○ | ○ | ● Save | |
| S37 | ● | ● | ◐ | ○ | ○ | ● Save | |
| S38 | ● | ○ | ◐ | ○ | ○ | ● Save | |
| S39 | ● | ○ | ◐ | ○ | ○ | ● Save | |
| S40 | ● | ○ | ◐ per-flow | ○ | ○ | ● | Backup/Export/Import buttons |
| S41 | ● | ○ | ○ | ○ | ○ | ○ | static |
| S42 | ● | ○ | ○ | ○ | ◐ severity | ◐ rows act | |

### 9.4 Overlay inheritance + matrix maintenance

- Bottom sheets (BS) and dialogs (DL) inherit every universal element of their underlying screen — bar, nav, states remain; the overlay renders atop.
- Overlays that themselves carry money/quantity/profit content must enforce U9–U12 **inside the overlay**:
  - **Keypad (U9):** BS-02 · BS-05 · BS-06 · BS-07 · BS-08 · BS-09 · BS-10 · BS-11 · BS-13 · BS-16
  - **₹ (U11):** the same set plus BS-12 (document preview)
  - **Green/red (U12):** BS-06 (profit preview) · BS-07 (loss preview) · BS-09 (net status) · BS-11 (± adjustment preview)
- Maintenance rule: every new screen/overlay must be registered in §1 (counts), §3/§4/§5 (definitions), §8 (traceability) **and here** before build sign-off; this matrix is the per-screen acceptance gate of §19.

---

## 10. Gap Register — all resolved (decisions recorded in SRS v2.9)

| # | Gap (as originally logged) | SRS anchor | **Resolution adopted (SRS v2.9)** | Status |
|---|-----|-----------|--------|--------|
| G-0 | `opencode_generate_screens_master_list_prompt.md` missing from repo. | repo state | **Closed by decision:** no separate prompt file will be supplied — this MASTER LIST + the SRS (§17 as the screen blueprint) remain the two documents of record. | Closed |
| G-1 | Overpayment vs §15.1 validation tension. | §7.5.3 vs §15.1 | **Explicit overpayment intent step** in BS-09 — when Deductions + Received exceed pending, confirm *"Received ₹X exceeds pending ₹Y — ₹Δ becomes credit balance. Record it?"* (Record = credit path / **Adjust** = return to form, plain validation then applies). (§7.5.3, §15.1) | Resolved |
| G-2 | Three alert thresholds lacked a Settings home. | §7.5.6/§14/§9.1 vs §13 | **Settings → App (S37)** with defaults: Long-Pending Dues aging **30** · Tender Ending Soon **7** · Seller Payable alert **3** days (§13, §14 rows now cite it). | Resolved |
| G-3 | Discard-cart confirmation missing from §17's modal list. | §15.2 vs §17 | **"Discard cart" officially added to §17's modal row** (21 → 22 items) — §15.2 ↔ DL-10 aligned. | Resolved |
| G-4 | Custom-item image mechanism unspecified. | §17, §5.3 | **Bundled seed image set or camera/gallery pick with thumbnail preview** — one canonical image per item, no in-app editing (§5.3, §17; BS-21 updated). | Resolved |
| G-5 | Welcome/Intro slide contents unspecified. | §17, §4.1 | **2 plain-English slides + always-visible Skip:** "Know your price before you buy." / "Enter once — every rupee tracked." (§4.1; S02 updated) | Resolved |
| G-6 | Seed catalog not enumerated. | §6.2/§16.1 | **Seed set = exactly the §6.2-named items** (12 vegetables + 4 groceries); §16.1 canonical overrides (Tomato 3 · Potato 14 · Onion 30; leafy 1; other veg default 7; groceries none); all else = custom items (§6.2). | Resolved |
| G-7 | Attention Center surface type ambiguous. | §17 vs §14 | **Confirmed: Home badge opens a full-screen Attention Center list (S42)** — not an inline expansion (§14, §17). | Resolved |
| G-8 | Local notifications "optionally" — v1 scope unclear. | §14 | **In-app only in v1**; OS-local notifications deferred post-v1 as an explicit §21 item (rule 4) (§14, §21). | Resolved |
| G-9 | Dues Overview duplicate entry wording. | §3.1/§7.5.6 | **Documented:** one hub target (S23); both labels refer to it — no change needed. | Closed (none) |
| G-10 | Standalone (non-cart) expense rows not among §10.1 types. | §9 vs §10.1 | **Decided: no standalone expenses in v1** — expenses attach to a cart day (§9, §2.5); explicit note added to §10.1; revisit only as a post-v1 addition (not silent scope creep). | Resolved |

---

## 11. Summary Report

- **Feature modules:** 17 — Onboarding & Setup · Home · Items & Purchase Entry · Daily Cart · Allocation · Remaining & Rejected (flows-only) · Review & Finalization · Institutions & Tenders · Payments & Receivables · Inventory · Expenses · Seller Payables · History & Other Day Entries · P&L · Reports · Settings & Data Mgmt · Attention Center.
- **Actual screen count:** **41 full screens** (S01–S42; S03 retired in SRS v2.7 — English-only), plus 22 bottom-sheet flows (BS-01–BS-22), 14 dialogs (DL-01–DL-14), and 10 global components (G1–G10). Screen-level state variants (V) are documented inline per screen.
- **Overlay definitions:** **36** — covers every item of §17's "Modals / Sheets" row (**22**, incl. **Discard cart** per SRS v2.9) plus 14 overlays the SRS body mandates (inventory choice · add-to-inventory · remaining · rejected · period bill · memo/bill generator · missing price · backup · export · import · image picker/photo evidence · report export/share · revision picker · attachment viewer).
- **Universal-element coverage:** §9 maps every screen against 13 universal elements (U1–U13: 7 always-on + 6 conditional); BS/DL overlays inherit their parent screen's elements and carry their own keypad/₹/profit-loss constraints where relevant (§9.4).
- **Traceability:** §17 map covered **40/40** rows (+2 meta rows); §14 alerts **13/13**; §15.1 messages **12/12**; §12 reports **14/14**; §11 tabs **7/7**; §13 groups **6/6**; §16 canonical figures mapped to their display screens.
- **Unresolved gaps: none** — the register (§10) records **11 decisions, all closed** (G-0–G-10) as SRS v2.9 changes; the largest behavioral one: overpayment intent step in Record Payment (G-1, BS-09).
- **No app code generated** (per AGENTS.md) — spec-only repo. Doc updated in place: v1.1 mirrored SRS v2.7's English-only change; v1.2 mirrored SRS v2.8's mobile-phone-only change; v1.3 mirrored SRS v2.9's gap resolutions (register in §10). All §16 canonical figures unchanged.
