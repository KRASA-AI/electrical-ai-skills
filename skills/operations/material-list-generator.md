---
name: "Material List / BOM Generator"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/list"
version: 1.1
last_eval_score: 8.90
---

# 📋 Material List / BOM Generator

## Purpose

Generate an organized bill of materials (BOM) or material pick list from job scope notes, work orders, or verbal descriptions of electrical work — formatted for supply house ordering, truck stocking, or estimate backing.

## When to Use

Use this skill when you need to:
- Build a material list from a scope of work, job notes, or takeoff summary
- Create a supply house order for an upcoming job
- Verify you have all materials before dispatching a crew
- Back up an estimate with a detailed material breakdown
- Generate a material list for a service truck restock based on common job types

## Required Input

Provide the following:

1. **Job description** — What electrical work is being performed (e.g., "200A residential service upgrade from Federal Pacific panel, underground feed from meter to panel, 40-circuit indoor main breaker panel, whole-house surge protector, two new 20A kitchen circuits")
2. **Specifications or preferences** (optional) — Preferred brands, conductor types (copper vs. aluminum for feeders), conduit type (PVC, EMT, rigid), box fill preferences
3. **Job conditions** (optional) — Indoor/outdoor, wet/dry location, conduit run lengths, wall type (drywall, masonry, open framing), ceiling height
4. **Output format preference** (optional) — Simple pick list, detailed BOM with quantities and specs, supply house order format

## Instructions

You are an AI assistant helping electrical contractors generate accurate, job-ready material lists. Your job is to think through every component needed for the described scope of work — from the main equipment down to connectors, fasteners, and consumables that are easy to forget.

**Before you start:**
- Load `config.yml` from the repo root for company name, the **supplier preference matrix** (`config.yml.suppliers` — preferred / alternate / banned, segmented by material category and by region; a `banned` supplier is one the firm has decided not to buy from regardless of price for cause documented in `config.yml.suppliers.banned_reason`), the **standard truck-stock list** (`config.yml.stock.truck_stock_list` — what every service truck carries by default so the BOM excludes those items), and the **standard waste factors** (`config.yml.bom.waste_factors` — the firm's actual waste-and-overage history, e.g., 12% for residential branch wire, 8% for commercial conduit, 15% for fire-stop foam).
- Reference `knowledge-base/terminology/` for correct product nomenclature.
- Reference `knowledge-base/regulations/` for NEC requirements that affect material selection.
- Reference `knowledge-base/regulations/material-tariffs-2026.md` for the Tariff-Aware Material-Sourcing block (see §5 below) — Section 232 / 301 / IEEPA tariff exposure on copper, aluminum, switchgear, transformers, panelboards, conduit, and busway as of 2026-Q2.

**Supplier preference matrix (run on every BOM):**

For each line item in the generated BOM, attempt to assign a preferred supplier from `config.yml.suppliers` in this order:

1. **Preferred supplier for this category in this region** — pull `config.yml.suppliers.{category}.{region}.preferred`. If matched, stamp the line with the supplier name and the standard PO turnaround time.
2. **Alternate supplier for this category in this region** — used when preferred is out of stock or quoted lead time exceeds the project schedule.
3. **Banned supplier check** — if the only available source is on `config.yml.suppliers.banned`, do NOT silently substitute. Surface the situation in the Notes block with the `banned_reason` and require a one-line decision from the user (substitute another product, override the ban for this job, escalate to ownership).
4. **Manufacturer-direct fallback** — for a line item with no supplier match in config (typically specialty / custom items), flag for procurement and recommend a manufacturer-direct quote.

The supplier matrix is the difference between a BOM that prices in the firm's actual purchasing reality and a BOM that prices a generic quote from the catalog. A BOM the firm cannot actually buy at the prices listed is worse than no BOM.

**Generate a material list with these sections:**

### 1. Job Summary
- Restate the scope in 2-3 sentences
- Note any NEC-driven material requirements (e.g., AFCI breakers for bedrooms per NEC 210.12, GFCI protection per NEC 210.8, weather-resistant receptacles per NEC 406.9)

### 2. Material List — Organized by Category

Group materials into these standard categories:

**A. Main Equipment**
- Panels, switchgear, transformers, disconnects, transfer switches, generators
- Include amperage ratings, bus configuration, number of spaces/circuits

**B. Overcurrent Protection**
- Breakers (type, amperage, pole count — specify AFCI, GFCI, dual-function, CAFCI where required)
- Fuses (type, amperage, voltage rating)

**C. Conductors & Cable**
- Wire gauge, insulation type (THHN/THWN, NM-B, UF-B, USE-2, MC cable), conductor count
- Estimated footage with 10-15% overage allowance
- Clearly distinguish between branch circuit wire and feeder/service entrance conductors

**D. Raceway & Fittings**
- Conduit type and size (EMT, PVC, rigid, flex, liquidtight)
- Connectors, couplings, elbows, straps, hangers, pull boxes, junction boxes
- Estimated footage with fittings count

**E. Boxes, Covers & Trim**
- Device boxes (single-gang, double-gang, 4-square, etc.) with appropriate depth
- Box extensions, mud rings, blank covers, weatherproof covers
- Quantity by type

**F. Devices & Trim**
- Receptacles (standard, GFCI, weather-resistant, tamper-resistant, USB)
- Switches (single-pole, 3-way, 4-way, dimmer, occupancy sensor)
- Cover plates (color, material)

**G. Grounding & Bonding**
- Ground rods, clamps, grounding electrode conductor
- Bonding bushings, jumpers, equipment grounding conductors
- Per NEC Article 250 requirements

**H. Fasteners, Supports & Consumables**
- Straps, clamps, hangers, beam clamps, threaded rod
- Anchors (toggle bolts, Tapcons, drop-in anchors) based on wall/ceiling type
- Wire nuts, terminal lugs, heat shrink, electrical tape, cable ties, labels
- Fire caulk / fire stop if penetrating rated assemblies

**I. Specialty / Job-Specific**
- Surge protectors, disconnects, whips, seismic bracing
- Low-voltage items (thermostat wire, data cable, fire alarm wire) if in scope

### 3. Notes & Reminders
- Flag items that may require long lead times or special ordering
- Note NEC code sections that drove specific material choices
- Remind about permit-related materials (panel stickers, arc flash labels)
- Suggest "while you're there" add-ons for efficiency (e.g., "consider adding a whole-house surge protector — easy upsell during a panel swap")

### 4. Quantity Summary Table

| Item | Spec / Description | Qty | Unit | Category | Preferred Supplier | Lead Time | Tariff Exposure |
|------|-------------------|-----|------|----------|--------------------|-----------|------------------|

**Formatting rules:**
- Use standard trade terminology (not manufacturer marketing names)
- Include wire gauge in AWG or kcmil
- Specify conduit sizes in trade sizes (1/2", 3/4", 1", etc.)
- Round quantities up to standard package sizes where practical (e.g., wire in 250' or 500' spools, not 237')
- Apply the firm's actual waste factors from `config.yml.bom.waste_factors` rather than a generic 10–15% — the firm's history is more accurate than a default
- Clearly mark any item where the user needs to verify a specific model or brand
- Stamp every long-lead item (≥ 8 weeks) with the lead-time-quote date — lead times go stale within days, and a BOM with a stale lead time will mis-schedule the project

### 5. Tariff-Aware Material-Sourcing Block

For every BOM, run the Tariff-Aware Material-Sourcing pass against `knowledge-base/regulations/material-tariffs-2026.md`. The pass produces a one-table summary that surfaces which line items are exposed to the April 6, 2026 Section 232 restructure, the Section 301 China-origin tariffs, IEEPA actions, or HTS reclassification — and recommends a sourcing posture for each.

| Material category | Tariff tier (2026-Q2) | Recommended posture | Rationale |
|---|---|---|---|
| Copper conductor (THHN, XHHW-2, USE-2, MV-90/MV-105) | Section 232 50% on raw + impact passed through copper-rod basis differential | Lock the supplier price for 30+ days at PO release; verify the supplier's price-validity window in writing | Q2 2026 copper electric wire $416.11/MLF; +83.7% since Feb 2020. A two-week PO delay on a $40K wire line is a $2.5K–$4K material drift. |
| Aluminum conductor (XHHW-2, USE-2, AL SER) | Section 232 derivative tier 25% | Cross-check against copper for the feeder spec; aluminum is increasingly the cost-effective long-feeder choice in 2026 | Compare cost per ampacity at the spec's ambient-temp and conduit-fill derating. AL feeder upsizes by 1–2 wire sizes vs Cu but the 2026 cost spread often makes it the right call. |
| EMT, IMC, RMC conduit | Section 232 derivative tier 25% | Verify the supplier's mill source on the next quote; foreign mill stock may carry the 25% pass-through | Domestic EMT supply has tightened since the April 6 restructure. PVC conduit is unaffected by Section 232 but may be the right substitute for non-corrosive applications. |
| Panelboards (residential and commercial — Square D, Eaton, Siemens, GE) | Section 232 finished-equipment tier 15% (effective through end of 2027) | Lock the panel price at PO release with the supplier's price-validity window in writing | Section 232 finished-equipment tier explicitly covers panelboards through 2027. A spec-listed BoD panel may be 4–8 weeks out as of Q2 2026. |
| Switchgear, switchboards, MCCs | Section 232 finished-equipment tier 15% + 26–32 week lead time on MV switchgear | Long-lead early-release submittal at the time the BOM is finalized; cross-reference `skills/operations/submittal-package-compiler.md` for the early-release form and `skills/admin/material-tariff-escalation-clause-drafter.md` for the contract attachment | The lead time itself is the bigger schedule risk than the tariff. The escalation clause and the early-release submittal together close the schedule and the cost gap. |
| Distribution transformers (dry-type and pad-mount) | Section 232 finished-equipment tier 15% + 52–104 week lead time | Long-lead early-release. Verify the manufacturer's ship date in writing on the date the BOM is finalized. | Distribution transformers are the longest-lead Division 26 item in 2026 — longer than switchgear. |
| Specialty wire (MV cable, hospital-grade, control cable, instrumentation) | Section 232 finished-equipment tier 15% + 4–12 week lead time | Verify the supplier's stock posture (mill stock vs distribution stock); MV cable cuts to length, so verify the cut footage matches the spec | Specialty wire often has a higher tariff exposure than commodity copper because the finished-equipment tier captures more of the value-add. |
| Lighting fixtures (LED, T-bar troffer, downlight, area lighting) | Section 232 derivative tier 25% on imported aluminum housings + Section 301 China-origin tier 25% (overlap) | Verify country of origin on the cut sheet; specify a US-mill or domestic-assembled fixture if the spec allows | Some LED-driver-and-housing combos are dual-tariffed (Section 232 derivative on the housing + Section 301 on the driver). |

When the BOM is for a job under an in-flight Tariff Event clause (per `skills/admin/material-tariff-escalation-clause-drafter.md`), the cover-sheet of the BOM should also include the Tariff Event reference number and the date of the most recent supplier-quote-as-substantiation.

### 6. Hard rules (output rules — these forbid common BOM-quality failures)

The output must NOT do any of the following. If asked to do any of these, refuse and state why.

1. **No fabricated manufacturer part numbers.** If the spec or input doesn't name a part number, render the line as a description (e.g., "200 A 40-space main-breaker panel, 22 kAIC, indoor") rather than inventing a part number.
2. **No "as required" quantities on countable items.** A BOM that says "wire — as required" is not a BOM. State the quantity with the waste factor applied; if the run length is unknown, flag the gap and route to the takeoff or the foreman.
3. **No quantities below the package size for items that come in fixed packages.** Wire nuts come in 100s, not 17s; 1/2" EMT couplings come in boxes of 100, not 8s. Round up to the package size.
4. **No "best brand" or "premium" quality language.** Specify the brand by name from `config.yml.suppliers.preferred_brands` or by the spec's BoD listing — never with a marketing adjective.
5. **No silent substitution of a banned supplier or a banned brand.** The Notes block surfaces the situation; the user decides.
6. **No omission of grounding-electrode conductor or equipment-grounding conductor sizing.** GEC and EGC are NEC Article 250 line items that are commonly forgotten and become first-cycle inspection failures. Compute per NEC Table 250.66 (GEC) and 250.122 (EGC) for the service rating and the upstream OCPD.
7. **No omission of arc-flash labeling, panel directory, or the field-marked available-fault-current label per NEC 110.24** for any service ≥ 1200 A or any unit substation.
8. **No omission of fire-stop / fire-caulk on any rated-assembly penetration the BOM implies** (e.g., a feeder run from a panel to a remote disconnect that crosses a 2-hour rated wall — fire-caulk goes on the BOM).
9. **No reliance on the manufacturer's "general purpose" lug rating when the spec calls for AL9CU.** Aluminum-feeder applications require AL9CU lugs; specify them by listing.
10. **No customer-facing or marketing-style prose anywhere on the BOM.** A BOM is a procurement document.
11. **No phrase listed in `config.yml.voice.banned_phrases`.**

### 7. Cross-references

- `skills/operations/panel-schedule-documenter.md` — Used when the BOM includes a panelboard and the line-by-line panel schedule is needed for the procurement-and-install package.
- `skills/operations/submittal-package-compiler.md` — Downstream of the BOM. The BOM's manufacturer + part numbers should match the submitted product.
- `skills/admin/material-tariff-escalation-clause-drafter.md` — Used when the BOM is for a job with a Tariff Event clause in the contract.
- `skills/sales/bid-summary-writer.md` — Upstream of the BOM. The bid's takeoff numbers feed the BOM's quantities.
- `knowledge-base/regulations/material-tariffs-2026.md` — Primary KB reference for the Tariff-Aware Sourcing pass (§5).
- `knowledge-base/regulations/nec-2026-key-changes.md` — Used for NEC 2026 Article 220 → 120 and 750 → 130 renumbering when the BOM cites NEC sections.

## Worked Example 1 — Residential 200 A Service Upgrade

**Inputs:** Replace 100 A FPE Stab-Lok with 200 A Square D QO 40-space main-breaker panel; new 4/0 AL SER cable from outdoor disconnect to indoor panel (~20 ft); whole-house Type-2 SPD; (2) new 20 A kitchen circuits to receptacles ~30 ft from panel; outdoor service disconnect required per NEC 230.85; AHJ Cleveland OH, NEC 2026.

**`config.yml` excerpts:**

```yaml
suppliers:
  panelboards:
    ohio:
      preferred: Cleveland Wholesale Electric (Square D dealer)
      alternate: Graybar Cleveland (Eaton dealer)
  conductors:
    ohio:
      preferred: Cleveland Wholesale Electric
      alternate: Graybar Cleveland
  banned: []
bom:
  waste_factors:
    branch_wire_residential: 0.12
    feeder_residential: 0.05
    consumables_residential: 0.20
stock:
  truck_stock_list: [wire nuts assorted, electrical tape, romex staples, ground rod clamps, anti-oxidant compound, GFCI test plug, voltage tester]
```

**Output — BOM (full):**

| Item | Spec / Description | Qty | Unit | Category | Preferred Supplier | Lead Time | Tariff Exposure |
|------|--------------------|-----|------|----------|--------------------|-----------|-----------------|
| Main breaker panel | Square D QO140M200PRB, 200 A 40-space indoor MB, 22 kAIC | 1 | ea | Main Equipment | Cleveland Wholesale | In stock | 15% finished-equipment tier |
| Outdoor service disconnect | Square D QO200A NEMA 3R, 200 A 2P, 22 kAIC | 1 | ea | Main Equipment | Cleveland Wholesale | In stock | 15% finished-equipment tier |
| Meter base | Milbank U2980-XL-200, 200 A 4-jaw RL meter base, NEMA 3R | 1 | ea | Main Equipment | Cleveland Wholesale | In stock | — |
| SER cable, 4/0-4/0-2/0 AL | Service-entrance cable, 20 ft + 5% waste = 21 ft → 25 ft cut | 25 | ft | Conductors (Feeder) | Cleveland Wholesale | In stock | 25% derivative tier (AL) |
| 12/2 NM-B w/ ground | Branch-circuit, kitchen circuits 30 ft × 2 = 60 ft + 12% waste = 67 ft → 100 ft (cut to bundle pull) | 100 | ft | Conductors (Branch) | Cleveland Wholesale | In stock | 50% raw tier on Cu basis |
| 20 A 1P CAFCI/GFCI dual-function breaker | Square D QO120CAFI-DF, dual-function for kitchen circuits per NEC 210.8 + 210.12 | 2 | ea | Overcurrent | Cleveland Wholesale | In stock | 15% finished-equipment tier |
| 15 A 1P CAFCI breaker | Square D QO115CAFI, re-land existing branch-circuit re-terminations | 12 | ea | Overcurrent | Cleveland Wholesale | In stock | 15% finished-equipment tier |
| 20 A 1P GFCI breaker | Square D QO120GFI, re-land existing GFCI-required branch circuits (bath, garage, outdoor, laundry) | 4 | ea | Overcurrent | Cleveland Wholesale | In stock | 15% finished-equipment tier |
| Type-2 whole-house SPD | Square D HEPD80, 80 kA, lugs to panel busbar | 1 | ea | Specialty | Cleveland Wholesale | In stock | — |
| Ground rod, copper-clad | 5/8" × 8 ft, UL listed | 2 | ea | Grounding | Cleveland Wholesale | In stock | 50% raw tier on Cu basis |
| Ground rod clamp, acorn | UL-listed direct-burial, fits 5/8" rod, AL9CU lug | 2 | ea | Grounding | Cleveland Wholesale | In stock | 50% raw tier on Cu basis |
| #4 bare Cu (GEC) | Grounding electrode conductor — sized per NEC Table 250.66 for 200 A service with #4/0 AL feeder = #4 Cu | 25 | ft | Grounding | Cleveland Wholesale | In stock | 50% raw tier on Cu basis |
| #6 stranded Cu (EGC) | Equipment grounding conductor, panel-to-disconnect bond, sized per NEC Table 250.122 for 200 A OCPD = #6 Cu | 30 | ft | Grounding | Cleveland Wholesale | In stock | 50% raw tier on Cu basis |
| Intersystem bonding terminal | Outside the meter base per NEC 250.94 | 1 | ea | Grounding | Cleveland Wholesale | In stock | — |
| Anti-oxidant compound | For AL feeder lug terminations | 1 | tube | Consumables | Truck Stock | — | — |
| Panel directory label set | Pre-printed, adhesive, NEC 408.4(A) compliant | 1 | set | Consumables | Cleveland Wholesale | In stock | — |
| Arc-flash warning label | NEC 110.16(A), residential service entrance | 1 | ea | Consumables | Cleveland Wholesale | In stock | — |
| Available-fault-current field-marked label | NEC 110.24, calculation date and value | 1 | ea | Consumables | Truck Stock | — | — |

**Notes & Reminders:**

- NEC 2026 Cleveland adopted 2026-01-01 — outdoor service disconnect (NEC 230.85) is required, not optional. Outdoor disconnect added as a separate Main Equipment line.
- Dual-function breakers (CAFCI + GFCI) used on the new kitchen circuits — NEC 210.12 (AFCI) and 210.8(A)(7) (GFCI for kitchen) both apply, dual-function is the lower-cost compliance path.
- GEC sized per NEC Table 250.66: 200 A service with #4/0 AL feeder → #4 Cu GEC. EGC per NEC Table 250.122 for 200 A OCPD → #6 Cu (this is the panel-to-disconnect bond).
- Available-fault-current label per NEC 110.24 — required field marking; the label-and-Sharpie pen is on truck stock, but the BOM lists it as a discrete line so the foreman doesn't forget.
- FPE Stab-Lok panel removal — handled per the firm's standard demo and disposal procedure; haul-away on the work order, not on the BOM.

## Worked Example 2 — Commercial TI Feeder Run (480 V to 208 V step-down)

**Inputs:** 14,500 SF office TI; new panel LP-2A 208Y/120 V 225 A 42-ckt fed from existing 480 V distribution panel via new 75 kVA dry-type transformer; feeder run ~120 ft total (60 ft 480 V primary + 60 ft 208 V secondary); 28 branch circuits; Title 24 lighting controls scope on one zone (controls covered by a separate skill); AHJ Portland OR, NEC 2023 (Oregon adopted 2023 NEC effective 2024; 2026 adoption pending 2027).

**`config.yml` excerpts:**

```yaml
suppliers:
  panelboards:
    oregon:
      preferred: Consolidated Electrical Distributors Portland (Square D dealer)
      alternate: Platt Electric Supply (Eaton dealer)
  transformers:
    oregon:
      preferred: Consolidated Electrical Distributors Portland
      alternate: Platt Electric Supply
  conductors:
    oregon:
      preferred: Consolidated Electrical Distributors Portland
      alternate: Platt Electric Supply
  banned:
    - name: AcmePower
      banned_reason: "Repeated quality-control failures on dry-type transformers in 2024-2025; firm decision 2025-08."
bom:
  waste_factors:
    feeder_commercial: 0.05
    branch_wire_commercial: 0.10
    conduit_commercial: 0.08
```

**Output — BOM (excerpt; conductors and main equipment shown):**

| Item | Spec / Description | Qty | Unit | Category | Preferred Supplier | Lead Time | Tariff Exposure |
|---|---|---|---|---|---|---|---|
| 75 kVA dry-type transformer | Square D EE75T3H, 480 Δ – 208Y/120 V, 3Φ, K-13 rated, NEMA 1, indoor | 1 | ea | Main Equipment | Consolidated Electrical Distributors Portland | **6 weeks** (quote 2026-04-26) | 15% finished-equipment tier — verify quote validity at PO |
| LP-2A panelboard | Square D NQ442L1C, 225 A 42-ckt, 208Y/120 V 3Φ 4W, MLO with 225 A main lug, copper bus, 22 kAIC | 1 | ea | Main Equipment | Consolidated | **8 weeks** (quote 2026-04-26) | 15% finished-equipment tier |
| 480 V primary feeder | (3) #4 Cu THHN-2 + (1) #8 Cu EGC; 60 ft × 1.05 waste = 63 ft → 75 ft pull (cut to bundle); per NEC 310.16 75 °C column #4 Cu = 85 A; primary OCPD 100 A; transformer primary FLA 90 A; protected per NEC 450.3(B) at 125% = 113 A → 125 A OCPD | 75 | ft | Conductors (Feeder) | Consolidated | In stock | 50% raw tier on Cu basis |
| 208 V secondary feeder | (4) 4/0 Cu THHN-2 + (1) #4 Cu EGC; 60 ft × 1.05 waste = 63 ft → 75 ft pull; per NEC 310.16 75 °C column 4/0 Cu = 230 A; transformer secondary FLA 208 A; OCPD on LP-2A is the panel main; conductor protected per NEC 240.4(B) | 75 | ft | Conductors (Feeder) | Consolidated | In stock | 50% raw tier on Cu basis |
| 1 1/4" EMT (primary) | 60 ft × 1.08 waste = 65 ft → 75 ft (cut to length, in 10 ft sticks) | 75 | ft | Raceway | Consolidated | In stock | 25% derivative tier on imported steel — verify mill on quote |
| 2 1/2" EMT (secondary) | 60 ft × 1.08 waste = 65 ft → 75 ft | 75 | ft | Raceway | Consolidated | In stock | 25% derivative tier |
| 1 1/4" EMT couplings | 1 ea per 10 ft = 8 ea + 20% spare = 10 → box of 25 | 1 | bx | Raceway Fittings | Consolidated | In stock | — |
| 2 1/2" EMT couplings | 1 ea per 10 ft = 8 ea + 20% spare = 10 → box of 25 | 1 | bx | Raceway Fittings | Consolidated | In stock | — |
| Transformer floor pad / housekeeping | 4" concrete pad, 36" × 36" (typically by GC; flag for confirmation) | 1 | ea | Specialty (GC scope) | n/a | — | — |
| #4 Cu GEC | Per NEC Table 250.66 for 75 kVA transformer with 4/0 Cu secondary | 25 | ft | Grounding | Consolidated | In stock | 50% raw tier on Cu basis |
| AL9CU lug kit (panel main) | Square D listed lug for 4/0 Cu — verify panel as-shipped vs added kit | 1 | kit | Specialty | Consolidated | In stock | — |
| Available-fault-current label, NEC 110.24 | Field-marked; secondary calc 11.4 kAIC at LP-2A per upstream short-circuit study (Kessler & Daoud rev 2026-04-08) | 1 | ea | Consumables | Truck Stock | — | — |

**Notes & Reminders:**

- AcmePower is on the banned-supplier list (`config.yml.suppliers.banned`) for cause; the BOM uses Square D from Consolidated as the preferred path. Do not silently substitute AcmePower if Consolidated quotes a longer lead time — escalate to ownership for a decision.
- LP-2A panelboard is an 8-week lead item. If the project schedule requires earlier energization, consider an Eaton Pow-R-Line PRL1A from Platt Electric Supply (alternate supplier, 4-week lead time as of 2026-04-26) — but verify the substitution doesn't trigger an "Or Equal" submittal review per `skills/operations/submittal-package-compiler.md`.
- 75 kVA dry-type lead time is short relative to the Q2 2026 distribution-transformer baseline (52–104 weeks) because it's a small dry-type, not a pad-mount or MV unit. Verify the quote validity at PO release.
- The transformer pad is GC scope per the spec; the BOM flags it for confirmation rather than including the concrete.
- The K-13 transformer is specified for the IT load harmonic profile; if the spec doesn't call out K-rating, the standard non-K dry-type drops the cost ~12%.

## Worked Example 3 — Industrial MCC Retrofit (480 V VFD bucket replacement)

**Inputs:** Paper-mill MCC retrofit; replace (4) existing 100 hp Class E2 starters with (4) Allen-Bradley PowerFlex 755T VFD buckets in existing Eaton MCC structure; existing 480 V 3Φ distribution; copper bus 800 A; harmonic mitigation via PowerFlex 755T's TotalFORCE active front end (no added line reactor or phase-shift transformer); existing AIC bus rating 65 kAIC; AHJ Mosinee WI, NEC 2023.

**`config.yml` excerpts:**

```yaml
suppliers:
  vfd_drives:
    wisconsin:
      preferred: Werner Electric Supply (Allen-Bradley distributor, factory-direct relationship)
      alternate: Pieper Electric Distribution
  mcc_buckets:
    wisconsin:
      preferred: Werner Electric Supply
      alternate: Pieper Electric Distribution
bom:
  waste_factors:
    industrial_control_wire: 0.10
    industrial_branch: 0.08
```

**Output — BOM (excerpt; main equipment + control wire shown):**

| Item | Spec / Description | Qty | Unit | Category | Preferred Supplier | Lead Time | Tariff Exposure |
|---|---|---|---|---|---|---|---|
| PowerFlex 755T VFD bucket | Allen-Bradley 20G11ND170JN0NNNNN — 100 hp, 480 V, TotalFORCE active front end, 24-pulse equivalent harmonic profile, NEMA 12, MCC frame N | 4 | ea | Main Equipment | Werner Electric Supply | **18 weeks** (quote 2026-04-25 — verify validity at PO release) | 15% finished-equipment tier; AB factory may pass through |
| MCC bucket adapter kit (Eaton-to-AB) | Werner-supplied adapter kit, factory-tested combination per UL 845, listed for the Eaton MCC structure | 4 | ea | Main Equipment | Werner Electric Supply | **6 weeks** | — |
| Disconnect / fused for VFD upstream | 200 A 600 V Class L fuses (matched to factory tested combination on the AB listing per UL 845) | 4 | ea (3 per disconnect = 12 total) | Overcurrent | Werner Electric Supply | In stock | — |
| #4/0 Cu THHN-2 — VFD output to motor | 4-conductor + #2 EGC, ~80 ft per drive × 4 = 320 ft + 8% waste = 346 ft → 400 ft (one 1000 ft spool split four ways with 200 ft remaining for stock) | 400 | ft | Conductors | Werner Electric Supply | In stock | 50% raw tier on Cu basis |
| 16/4 stranded shielded control cable | TIA-485-A compliant for the EtherNet/IP fiber-to-Cu drop; 4 buckets × 30 ft = 120 ft + 10% waste = 132 ft → 150 ft | 150 | ft | Control / Low Voltage | Werner Electric Supply | In stock | — |
| Cat6A shielded patch cable | EtherNet/IP control to the line-side fiber drop; 4 ea × 25 ft per bucket | 4 | ea | Control / Low Voltage | Werner Electric Supply | In stock | — |
| Available-fault-current label | NEC 110.24; AIC at MCC line side per project short-circuit study rev 2026-04-12 = 48 kAIC (existing 65 kAIC bus rating complies) | 1 | ea | Consumables | Truck Stock | — | — |
| Arc-flash label — updated | NEC 110.16(A); per IEEE 1584-2018 calculation, incident energy at the new VFD buckets = 4.2 cal/cm² (Cat 2 PPE); label set per the firm's labeling spec | 4 | ea | Consumables | Werner Electric Supply | In stock | — |

**Notes & Reminders:**

- 18-week PowerFlex 755T lead time is the schedule driver — recommend long-lead early-release submittal per `skills/operations/submittal-package-compiler.md` and the Tariff-Aware contract attachment per `skills/admin/material-tariff-escalation-clause-drafter.md`.
- The TotalFORCE active front end on the 755T eliminates the phase-shift transformer or line reactor that would otherwise be on the BOM for harmonic mitigation per IEEE 519. If the spec calls out a separate phase-shift transformer, RFI to confirm whether the spec was written before the 755T's harmonic profile was published; the substitution-request package may save the project ~$11K per drive.
- The Eaton-to-AB adapter kit is a UL 845-listed factory-tested combination — without the listing, the substitution is a rejection trigger on the SCCR coordination per the spec's series-rating clause.
- IEEE 1584-2018 incident-energy calculation done at 4.2 cal/cm² (Cat 2 PPE) — verify the calculation against the firm's arc-flash study rev date before finalizing the label set.
- Existing 65 kAIC bus complies with the 48 kAIC available fault current; no bus-bracing upgrade required.

---

*v1.1 — 2026-04-28 — added config-driven supplier preference matrix, three takeoff worked examples (residential service upgrade, commercial TI feeder run, industrial MCC retrofit), Tariff-Aware Material-Sourcing block, hard rules, and cross-references to the new submittal-package-compiler.md skill.*
