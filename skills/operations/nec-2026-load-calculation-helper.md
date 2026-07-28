---
name: "NEC 2026 Load Calculation Helper"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~45 min/calc"
version: 2.0
last_eval_score: null   # v1.x graded 2026-07-13; v2.0 pending re-eval next cycle
---

# 🧮 NEC 2026 Load Calculation Helper

## Purpose

Walk an electrical contractor through a residential or small-commercial service and feeder load calculation under the 2026 NEC — which reorganized load-calc rules out of Article 220 into the new **Article 120**, moved energy management systems out of Article 750 into the new **Article 130**, and expanded the recognition of EMS and power control systems (PCS) as a code-compliant way to add load to an already-loaded service without upsizing it.

This skill collects intake, shows every step of the math (no hidden arithmetic), tests whether a service upgrade can honestly be avoided, and produces a one-page calc sheet that a PE or master electrician can sign and drop into the permit package.

It is a drafting and sanity-check aid, not a stamped engineering calculation.

> **⚠ PROVISION VERIFICATION GATE — read before first use.**
> The 2026 cycle renumbered the load-calc article, but this skill does **not** carry a verified copy of the published Article 120 demand tables, and the repo knowledge base does not either (`nec-2026-key-changes.md` §7 describes the reorganization qualitatively, not numerically).
>
> Therefore: **every numeric demand factor, VA/sq-ft density, tier break, demand-table value, AND every Article 120 section and table designation used in a 2026-cycle calc must be read off the published Article 120 text or the AHJ's own load-calc worksheet before that calc goes on a permit.** The skill must never assert a 2026 numeric factor *or section number* from memory. The §120.xx citations that appear throughout this file are working placeholders carried over from the Article 220 structure — treat them as unconfirmed until you have the book open.
>
> Any factor the user has not confirmed is printed in the output as `[UNVERIFIED — confirm against published Art. 120 before permit submission]`. A calc sheet with one or more UNVERIFIED factors is a *design aid only* and must not be signed.
>
> This gate does not apply to 2020/2023-cycle calcs, where Article 220's factors are settled and this skill applies them directly. **Most jurisdictions are still on 2020 or 2023** (see KB §9) — that is the normal path, not the exception.

## When to Use

Use this skill when you need to:

- Size or re-size a service to a 1- or 2-family dwelling adding an EV charger, heat pump, induction range, or tankless electric water heater
- Run a quick load calc before quoting a panel upgrade vs. an EMS-based solution
- Document feeder sizing for a multi-family building under the new Article 120 demand factors
- Translate a 2023-NEC calc (Article 220) into the 2026 equivalent (Article 120) during a jurisdiction's adoption lag
- Compare the **standard method** (§120.42 and .43, formerly 220.42/.83) against the **optional method** (§120.82, formerly 220.82) on an individual dwelling and pick the lower result
- Spot an opportunity to apply an EMS (**Article 130** in the 2026 cycle; Article 750 in 2020/2023) or a §705 power control system to add load on an existing service without upsizing
- Catch common apprentice-level mistakes (double-counting EVSE, forgetting the range demand table, running the heating/AC load through the optional-method demand factor)

Do not use this skill as the final sign-off calculation on any service over 400 A, any building with parallel sets, anything involving a microgrid or grid-forming inverter, or anything a local AHJ has flagged for PE review. Route those to your engineer of record.

## Required Input

Provide the following:

1. **Occupancy type** — Single-family dwelling, two-family dwelling, multi-family (with unit count), or small commercial ≤ 400 A. If commercial, describe the occupancy (office, retail, restaurant, etc.).
2. **Jurisdiction and NEC cycle adopted** — State + county/city, and whether they have adopted the 2026 NEC yet. If not, which cycle is in force (2020, 2023). This skill will refuse to produce a 2026-cycle calc for a permit in a jurisdiction still on 2023 — it will produce the 2023 version and note the 2026 delta.
3. **Conditioned square footage** — Heated/cooled area, excluding uninhabitable space like garages. Required for the general lighting load.
4. **Appliance inventory** — For each: nameplate volts, nameplate VA (or watts + PF), whether it is continuous-duty, and whether it is fastened in place. Include: range/cooktop/oven (give nameplate kW), dryer, water heater (tank vs tankless, kW), dishwasher, disposal, microwave, compactor, central AC (list tons + nameplate MCA or RLA+FLA), heat pump (heating + cooling nameplates), electric furnace or baseboard (kW per zone), pool/spa, well pump, EVSE (Level 1 vs Level 2, nameplate amps at which voltage).
5. **Small-appliance and laundry branch circuit count** — Typically 2 small-appliance + 1 laundry for a dwelling; 1 at-least-15A in each multi-family laundry area.
6. **EVSE details** — For each Level 2 charger: nameplate amps, voltage, single or paired, and whether it is an "EV energy management system" (EVEMS) per §625.42 with a documented throttle-down behavior.
7. **Existing service (if applicable)** — Service amperage, panel manufacturer/model, existing calculated load vs. existing metered peak (if you have it from the utility), age.
8. **Method preference** — Standard (§120.42/.43), optional dwelling (§120.82), or "run both and show me the lower result." Default: both.
9. **Any EMS / PCS equipment already specified or being considered** — Manufacturer, model, mode (load-shed vs. monitor-only), and what loads it controls.

## Instructions

You are an AI assistant helping an electrical contractor perform a service and feeder load calculation **under the NEC cycle actually in force at the AHJ** — Article 220 for a 2020/2023 jurisdiction (the common case), Article 120 for a 2026 jurisdiction. You show your math. You never hide a step. You cite the section number next to each factor you apply, and on a 2026 calc you tag any section or factor you have not confirmed against the published text.

**Before you start:**

- Load `config.yml` for the contractor's state, license number, and preferred voltages.
- Reference `knowledge-base/regulations/nec-2026-key-changes.md` for the Article 220 → 120 migration, the Article 750 → 130 EMS migration, and the **state-by-state adoption snapshot in §9**. Note the KB's explicit warning that several commercial adoption aggregators overclaim states as "adopted" that are not yet in force — Oregon is the named example.
- **Resolve the code cycle before touching the math.** The cycle in force at the AHJ governs. If the jurisdiction is on 2020 or 2023, produce the calc under **Article 220** using that cycle's settled factors, then add a short qualitative "2026 delta" note. Do not produce an Article 120 calc for a jurisdiction that has not adopted 2026 — the skill's own hard rules forbid it.
- If the jurisdiction *is* on 2026, apply the **Provision Verification Gate** above: ask the user to confirm each numeric factor against the published Article 120 text or the AHJ worksheet, and tag anything unconfirmed as `[UNVERIFIED]` in the output.

**Dwelling-Defaults Pre-load Block (config-driven, optional):**

Most residential calcs in the same service area share the same defaults — typical conditioned sq ft for the firm's lane, standard appliance schedule the firm spec's by default, and AHJ-specific derates the firm has already validated. The Dwelling-Defaults Pre-load Block eliminates the per-calc friction of re-typing the same numbers. The block is config-driven; if absent, the skill behaves exactly as v1.0 did.

`config.yml.nec_load_calculation_helper`:

```yaml
nec_load_calculation_helper:

  # Primary service area defaults — used when the user's intake omits the field
  primary_service_area:
    state: "OR"
    metro: "Portland"
    typical_sf_range: "1,800 – 2,800"        # informational; used in the Defaults Echo
    default_voltage: "120/240V split-phase"  # vs. "120/208V network" for select urban infill
    default_grounding_electrode_system: "two ground rods, 6 ft apart, §250.53(A)(3)"

  # AHJ-specific derate / amendment library — keyed by AHJ name
  #
  # `cycle_verified_on` is REQUIRED on every entry. It is the date a human last confirmed
  # the adopted cycle against a primary source (state building-code office or the NFPA
  # enforcement map — NOT a commercial aggregator; see KB §9 on aggregator overclaims).
  # An entry without `cycle_verified_on` is treated as unverified and forces the skill to
  # ask, not assume.
  ahj_amendments:
    "City of Portland, OR":
      adopted_nec_cycle: "2023"              # 2023 OESC, effective 2023-10-01
      effective_date: "2023-10-01"
      cycle_verified_on: "2026-07-13"
      next_expected_cycle: "2026 OESC — ANTICIPATED 2026-10-01, NOT YET IN FORCE"
      caution: >
        Oregon adopts the OESC statewide through the Building Codes Division; a city is not
        an independent NEC adopter. Several commercial aggregators list Oregon as already on
        2026 — it is not. Calcs for Oregon permits pulled before the OESC effective date run
        under Article 220 (2023), not Article 120.
      local_amendments:
        - "PBOT permit threshold: any service > 200 A requires utility coordination"
      derate_defaults:
        ambient_temperature_C: 30
        conductor_grouping_factor: "use Table 310.15(C)(1) per cycle"

    "Commonwealth of Massachusetts":
      adopted_nec_cycle: "2026"              # 527 CMR 12.00, first state in force
      effective_date: "2026-04-24"
      cycle_verified_on: "2026-07-13"
      caution: >
        NEC 2026 is genuinely in force here, so Article 120 applies — but the Provision
        Verification Gate still applies: confirm each Article 120 numeric factor against the
        published text plus the Massachusetts state amendments before signing a permit calc.
      local_amendments:
        - "Massachusetts state amendments to NFPA 70 apply on top of the 2026 base text —
           read 527 CMR 12.00 alongside Article 120"

    "City of Seattle, WA":
      adopted_nec_cycle: "2023"
      effective_date: "2024-07-01"
      cycle_verified_on: "2026-07-13"
      next_expected_cycle: "2026 — effective 2026-12-31 per Washington adoption"
      local_amendments:
        - "SDCI service entry: SE conductors must be in raceway from meter to first means
           of disconnect"

  # Default appliance schedule — what the firm typically spec's on a Pacific NW remodel
  default_appliance_schedule:
    range_kw: 10           # Bosch HEI8054U or equivalent induction; nameplate kW
    dryer_kw: 5            # gas dryer common in PDX, electric here as fallback
    water_heater:
      type: "tank"         # vs. "tankless" or "heat_pump"
      kw: 4.5
    dishwasher_va: 1200
    disposal_va: 800
    ac_tons: 3
    ac_nameplate_amps: 21  # at 240 V; ≈ 5,040 VA
    furnace_blower_va: 500 # gas furnace blower motor

  # Small-appliance and laundry — almost always the same on a single-family dwelling
  default_branch_circuits:
    small_appliance: 2     # §120.52(B)(1) — minimum 2 SAC
    laundry: 1             # §120.52(B)(2) — minimum 1 laundry

  # EVSE defaults — the firm's default install
  default_evse:
    nameplate_amps: 48     # Wallbox Pulsar Plus, Tesla Wall Connector, etc.
    voltage: 240
    evems_capable: true    # vast majority of 2024+ Level 2 chargers are EVEMS-capable
    evems_throttle_amps: 32  # default throttle setpoint when EVEMS engaged

  # Method-preference default — most firms run both and pick the lower
  default_method: "both"   # "standard" / "optional" / "both"

  # Cache freshness — config-driven AHJ entries staler than this trigger a warning
  cache_staleness_threshold_days: 90
```

The Pre-load Block (1) populates the intake with the default voltage, default appliance schedule, default SAC/laundry counts, and default EVSE configuration when the user's intake omits the field; (2) pulls the AHJ-adopted NEC cycle and effective date from `ahj_amendments` keyed by the user's AHJ name; (3) surfaces any `local_amendments` in the calc-sheet Notes block (e.g., Portland's EVEMS-label exception); (4) echoes every defaulted value in a **Defaults Echo block** at the top of the output so the user can spot a mis-default before signing.

**Defaults Echo block (always print when any default was applied):**

```
DEFAULTS APPLIED (from config.yml.nec_load_calculation_helper)
- Voltage:              120/240V split-phase  [DEFAULT — confirm before sign-off]
- Range:                10 kW (induction)     [DEFAULT — verify nameplate]
- Dryer:                5 kW                  [DEFAULT — verify nameplate]
- EVSE:                 48 A @ 240 V, EVEMS-capable, 32 A throttle setpoint  [DEFAULT]
- SAC / Laundry:        2 / 1                 [DEFAULT — §120.52(B)]
- AHJ-adopted cycle:    2026 (Portland, OR — effective 2025-10-01)  [CACHE HIT]
- Local amendments:     PBOT > 200 A utility-coord threshold;
                        Multnomah EVEMS-label panel-side exception
```

If `cycle_verified_on` for the AHJ is older than `cache_staleness_threshold_days` (default 90), emit a Cache Freshness Warning at the top of the output but still answer against the cached cycle:

```
⚠ CACHE FRESHNESS WARNING
The AHJ cycle for "City of Portland, OR" was last verified on 2025-10-01, which is
> 90 days ago. The answer below uses the cached cycle (NEC 2026). Re-verify
adoption status before submitting permit. Update config.yml.nec_load_calculation_helper.ahj_amendments after verification.
```

The skill never invents a defaulted value. If `nec_load_calculation_helper` is absent from `config.yml`, the skill behaves exactly as v1.0 did and prompts the user for every intake item.

**Decision-First Quick Pass (run before the full calc):**

Many residential calcs are not "what size service should I install" — they are "do I need to upgrade at all?" The Decision-First Quick Pass is a 60-second triage that lands one of three answers before the full calc runs. It surfaces the upgrade-avoidance opportunity at the *start* of the conversation rather than at step 6 of a 9-step process, so the rest of the calc is framed by the decision the homeowner actually faces.

```
DECISION-FIRST QUICK PASS

Question 1: Rough-size the optional-method load. Use the SAME structure as the full
  optional-method calc — do not use a shortcut formula, because the shortcut is where
  the heating/AC error creeps in:

    general pool VA = (sq ft × lighting density) + (1,500 × SAC/laundry circuits)
                      + Σ nameplate VA of fastened-in-place appliances
                      + Σ nameplate VA of EVSE
                      + Σ nameplate VA of other permanently connected motors
    demand VA       = (100% of the first tier) + (remainder × remainder factor)
                      + 100% × the LARGER of the heating load or the A/C load   ← OUTSIDE the pool
    amps            = demand VA ÷ service voltage

  Take the tier break and remainder factor from the cycle in force (Art. 220 for
  2020/2023; verified Art. 120 for 2026 — see the Provision Verification Gate).

  → If DEMAND ≤ existing service rating with > 15% margin: SCOPE = "no upgrade — confirm with full calc"
  → If DEMAND > existing service rating × 1.3:             SCOPE = "upgrade required — full calc to size"
  → Otherwise (close to the ceiling):                      SCOPE = "upgrade-avoidance candidate — run full calc with EMS/PCS variant"

  A quick pass that lands within ±10% of the service rating is NOT a pass. It is a
  "run the full calc" — never report an upgrade-avoidance finding off the quick pass alone.

Question 2: Does the customer have an existing utility-metered peak (last 12-month interval data)?
  → If YES: cross-check the metered peak against the calculated load.
            If metered peak < calculated load × 0.6, flag the calc as conservative
            and recommend the EMS/PCS variant.
  → If NO:  recommend pulling 12-month interval data from the utility before
            authorizing a service upgrade > 200 A. Most utilities (PGE, PSE, SCE, ConEd)
            provide this within 5 business days.

Question 3: Is the customer adding ANY of: EV charger, heat-pump water heater, induction range,
            heat-pump space heat, electric dryer (where previously gas), pool/spa, or another sub-panel?
  → If YES: §625.42 EVEMS, an EMS (Art. 130 in 2026 / Art. 750 in 2020–2023), or a §705 PCS
            is a real option — run the EMS/PCS variant (optional method with the controlled load
            throttled) as a separate column in the calc table.
            Do NOT assume the EMS variant will succeed. Throttling the EVSE only removes VA from
            the 40%-remainder pool, so it returns roughly 40 cents on the dollar. On a service
            that is already near its ceiling before the new load, an EMS frequently does NOT
            close the gap — report that plainly when it happens.
  → If NO:  full calc only; EMS/PCS variant unnecessary.

Surface the answer in a one-line headline at the top of the output:
  SCOPE: [no upgrade — confirm / upgrade required / upgrade-avoidance candidate]
  EMS/PCS variant: [recommended / not applicable]
  Utility peak data: [available / pull before proceeding]
```

The Decision-First headline lands at the top of the calc sheet, *before* the load table, so the homeowner / EC reads the answer before reading the math.

**Process:**

1. **Run the Decision-First Quick Pass** (above) using the intake plus any Dwelling-Defaults Pre-load. Land the SCOPE headline at the top of the output. Then confirm the intake. If required fields are missing AND no Pre-load default exists for them, list them and stop. Do **not** guess kW values that are not in the Pre-load.
2. Classify the calculation path:
   - Single-family dwelling ≤ 400 A → offer both §120.42 (standard) and §120.82 (optional). Run both.
   - Two-family or multi-family → §120.42/.43 + the applicable demand tables.
   - Small commercial ≤ 400 A → §120.40 and the relevant occupancy-specific table under Part IV of Article 120.
3. **Build the load list, line by line, in a table:**
   - Column 1: Load description (e.g., "General lighting, 2,450 sq ft × 2 VA").
   - Column 2: Nameplate VA (before demand).
   - Column 3: NEC 2026 section applied (e.g., "§120.41(A), Table 120.42").
   - Column 4: Demand factor applied, with the method named (e.g., standard-method general-load tier, or optional-method first tier + remainder). Do not borrow the optional method's remainder factor into a standard-method table — they are different factors. Tag `[UNVERIFIED]` on a 2026 calc until confirmed.
   - Column 5: Resulting demand VA.
4. Sum the demand column. Convert to amperes at the service voltage. Round up to the next standard service size (100, 125, 150, 200, 225, 300, 400 A).
5. **If the standard method and the optional method were both requested, show both results side-by-side** and identify the lower one. Use the lower per §120.82 allowance.
6. **Service-upgrade-avoidance check:**
   - If the calculated load exceeds the existing service but the overrun is ≤ ~30% of the existing service, test whether an EMS (**Article 130** in 2026; **Article 750** in 2020/2023) covering EVSE, water heater, or dryer — or a §705 PCS covering inverters/interconnected sources — could legally reduce the calculated load below the existing service rating.
   - For an EVSE that is an EVEMS per §625.42, cite the "controlled load" allowance and show the calc with the throttled value.
   - **Run the variant calc all the way to amps before recommending it.** Under the optional method, a throttled load sits in the 40%-remainder pool, so shedding X VA of EVSE only removes ~0.4X VA of demand. The EMS often fails to close the gap. If it fails, say so and recommend the upgrade — do not present a marginal or negative result as a save.
   - If the EMS variant lands within 5% of the existing service rating, it is **not** a recommendation. Report it as "numerically compliant, no practical margin" and recommend the upgrade: any future load addition re-opens the calc, and a homeowner who later adds a heat pump will pay for the panel anyway plus the stranded EMS.
   - Suggest product categories, not brands — e.g., "smart panel with whole-home EMS", "240 V circuit-level load manager installed ahead of the EV circuit", "dedicated EV energy-management relay with CT-based monitoring".
   - Produce the EMS/PCS variant calc as a separate table so the customer can see both paths priced side by side, including the honest outcome where the EMS does not work.
7. **Apprentice-mistake checklist** — run and report:
   - **In the optional method, did we keep the heating/AC load OUT of the demand-factor pool and add it at 100% afterward?** This is the single most common and most dangerous error in a dwelling calc — running the A/C through the 40% remainder understates the service by double-digit amps and can green-light an undersized service. The heating/AC load is a separate 100% adder in *both* the Article 220 optional method and its Article 120 successor. **Verify this line every time.**
   - Did we count the **larger** of the heating load vs. the A/C load, never both?
   - Did we treat the range per the range demand table, not full nameplate?
   - Did we apply the dryer demand table?
   - Did we add the small-appliance and laundry circuits at 1,500 VA each?
   - Did we forget any continuous motor load, or the largest-motor 25% adder where required (§430.24)?
   - **On a 2026-cycle calc: is every numeric factor above confirmed against the published Article 120 text, or is it tagged `[UNVERIFIED]`?**
8. Produce the one-page calc sheet. Template (Markdown, print-clean):
   - Header: job name/address, date, preparer, method used, NEC cycle applied.
   - Load table (from step 3).
   - Total demand VA, total demand A at service voltage, recommended service size.
   - Side-by-side comparison (if both methods run).
   - EMS/PCS avoidance option (if applicable).
   - Sign-off block: "Prepared by _____. Reviewed by _____ (master electrician or PE). This calculation is not a stamped engineering document unless the reviewer affixes a license seal."
9. Close with the disclaimer block: "This calculation assumes the nameplate data provided is accurate. Field-verify nameplates before ordering gear. A licensed master electrician or professional engineer must review and sign before permit submission. This skill does not substitute for AHJ-specific worksheet requirements."

**Hard rules (do not bend):**

- **Never run the heating or air-conditioning load through the optional method's demand factor.** It is added at 100%, after the pool. Getting this wrong understates the service and is the error most likely to put a customer on an undersized service.
- **Never assert a 2026 Article 120 numeric factor from memory.** Confirm it against the published text or the AHJ worksheet, or tag it `[UNVERIFIED]` and mark the sheet as a design aid, not a signable calc. (Provision Verification Gate.)
- Never publish a 2026 Article 120 calc on a permit package for a jurisdiction that has not adopted 2026. Use the cycle in force (Article 220), with a qualitative 2026 delta note. **Verify adoption against a primary source, not a commercial aggregator** — KB §9 documents aggregators overclaiming adoption.
- Never skip the range and dryer demand tables — raw nameplate in a residential calc is almost always wrong.
- Never recommend an EMS/PCS solution without naming the controlled loads, the mode (monitor vs. shed), the cycle-correct citation (§625.42 EVEMS; Art. 130 EMS in 2026 / Art. 750 in 2020–2023; §705 PCS), **and the variant calc carried through to amps.**
- Never present an EMS result with <5% margin as an upgrade-avoidance win.

## Example Output

**Inputs (abbreviated):**

- Single-family dwelling, 2,450 sq ft, **Newton MA** (Massachusetts is in force on NEC 2026 per 527 CMR 12.00, effective 2026-04-24 — KB §9; this is why an Article 120 calc is legitimate here and would *not* be in Oregon).
- Existing service: 100 A, 120/240 V split-phase.
- Appliances: 10 kW range, 5 kW dryer, 4.5 kW electric water heater, 1,200 VA dishwasher, 800 VA disposal, 3-ton AC (nameplate 21 A @ 240 V), gas furnace (500 VA blower motor).
- EVSE: 48 A @ 240 V Level 2, EVEMS-capable.
- 2 small-appliance + 1 laundry branch circuits.
- Method: both standard and optional.
- EMS consideration: customer open to it.

**Output:**

### Load calculation — 123 Main St, Newton MA — 2026-07-13

**SCOPE: UPGRADE REQUIRED.** The optional method lands at **112.5 A** on a 100 A service. The EMS/EVEMS variant was run and **does not close the gap** at any practical charge rate. Recommendation: upgrade the service. Utility peak data: pull 12-month Eversource interval data to inform the final size, but it does not change the compliance answer — the calculated load, not the metered peak, governs the permit.

```
DEFAULTS APPLIED (from config.yml.nec_load_calculation_helper)
- Voltage:              120/240V split-phase                     [DEFAULT]
- AHJ-adopted cycle:    2026 (Massachusetts — eff. 2026-04-24)   [CACHE HIT, cycle_verified_on 2026-07-13]
- Local amendments:     527 CMR 12.00 state amendments apply on top of the 2026 base text
- SAC / Laundry:        2 / 1                                    [DEFAULT]
- (All other appliance values from user intake, not defaulted.)

⚠ PROVISION VERIFICATION GATE — 5 factor groups UNVERIFIED
The following Article 120 values were supplied by the user's intake and have NOT been
confirmed against the published Art. 120 text or the MA state amendments:
  - general-lighting density (2 VA/sq ft)                        [UNVERIFIED]
  - optional-method first tier + remainder (8,000 VA @ 100%, 40%) [UNVERIFIED]
  - standard-method tier + remainder (8,000 VA @ 100%, 35%)       [UNVERIFIED]
  - range demand-table value (8,000 VA from a 10 kW nameplate)    [UNVERIFIED]
  - dryer demand-table value (5,000 VA, single unit)              [UNVERIFIED]
  - ALL §120.xx section and table designations cited below        [UNVERIFIED]
This sheet is a DESIGN AID. Confirm every item above against the published Article 120
and 527 CMR 12.00 before any licensee signs it for permit submission.

Note what the gate does NOT undermine: the STRUCTURE of the calc — heating/AC added at
100% outside the demand pool, largest-of-heat-or-cool, EVSE inside the pool, demand tables
for range and dryer — is stable across cycles and is the part that flipped this answer.
The tier numbers move; the structure does not.
```

**Method A — Standard**

| # | Load | Nameplate VA | Demand factor | Demand VA |
|---|------|--------------|---------------|-----------|
| 1 | General lighting (2,450 sq ft × 2 VA) | 4,900 | — | — |
| 2 | Small-appliance circuits (2 × 1,500) | 3,000 | — | — |
| 3 | Laundry circuit (1 × 1,500) | 1,500 | — | — |
| 4 | Subtotal lines 1–3 | 9,400 | 100% first 8,000; 35% of 1,400 remainder | 8,490 |
| 5 | Range | 10,000 | range demand table | 8,000 |
| 6 | Dryer | 5,000 | dryer demand table (single unit) | 5,000 |
| 7 | Water heater | 4,500 | 100% | 4,500 |
| 8 | Dishwasher | 1,200 | 100% | 1,200 |
| 9 | Disposal | 800 | 100% | 800 |
| 10 | A/C (21 A × 240 V) | 5,040 | 100% (larger of heat or cool) | 5,040 |
| 11 | Gas-furnace blower | 500 | 100% | 500 |
| 12 | EVSE (48 A × 240 V) | 11,520 | 100% | 11,520 |
| | **Total demand** | | | **45,050 VA** |

45,050 VA ÷ 240 V = **187.7 A → 200 A service.**

**Method B — Optional dwelling**

Note the structure: the heating/AC load is **not** in the demand-factor pool. It is added at 100% *after* the pool is factored. Running it through the 40% remainder is the classic error and it understates this service by ~13 A.

| # | Load | Nameplate VA | Treatment | Demand VA |
|---|------|--------------|-----------|-----------|
| 1 | General lighting + small-appliance + laundry | 9,400 | into general pool | — |
| 2 | Fastened-in-place appliances (range + dryer + WH + DW + disposal, at nameplate) | 21,500 | into general pool | — |
| 3 | EVSE | 11,520 | into general pool | — |
| 4 | Gas-furnace blower motor | 500 | into general pool | — |
| | **General pool subtotal** | **42,920** | | |
| 5 | First tier | — | 100% of first 8,000 VA | 8,000 |
| 6 | Remainder (42,920 − 8,000 = 34,920) | — | × 40% | 13,968 |
| 7 | **A/C — larger of heat vs. cool** | 5,040 | **100%, OUTSIDE the pool** | 5,040 |
| | **Total demand** | | | **27,008 VA** |

27,008 VA ÷ 240 V = **112.5 A → exceeds the existing 100 A service.**

**Method choice:** the optional method yields the lower of the two results (112.5 A vs. 187.7 A) and is permitted for this dwelling. Either way, **the existing 100 A service is inadequate.** Next standard size above 112.5 A is 125 A; see the recommendation below on why 200 A is the better buy.

### Service-upgrade-avoidance check — RESULT: EMS DOES NOT SAVE THIS SERVICE

The customer asked whether an EV energy management system could let them keep the 100 A service. It cannot. Here is the math, run all the way through rather than asserted:

| EVEMS throttle setpoint | EVSE VA in pool | General pool | Pool demand (8,000 + 40% rem.) | + A/C @100% | Total | Amps @ 240 V | Verdict |
|---|---|---|---|---|---|---|---|
| None (48 A) | 11,520 | 42,920 | 21,968 | 5,040 | 27,008 | **112.5 A** | ❌ over |
| 32 A | 7,680 | 39,080 | 20,432 | 5,040 | 25,472 | **106.1 A** | ❌ still over |
| 16 A | 3,840 | 35,240 | 18,896 | 5,040 | 23,936 | **99.7 A** | ⚠ 0.3% margin |

Why the EMS underperforms here: a throttled EVSE sits inside the **40% remainder pool**, so shedding 1,000 VA of charger only removes ~400 VA of calculated demand. It returns forty cents on the dollar. This house is already at 93 A *before* the EV is added — there is simply not enough headroom for the EMS to buy back.

The 16 A setpoint is numerically under 100 A, but per the skill's hard rules a **0.3% margin is not an upgrade-avoidance win**: it delivers roughly 3.8 kW of charging (~12 miles of range per hour — an overnight charge adds about 40 kWh, which is *not* a full fill on a typical 60–80 kWh pack), and any future load — a heat-pump water heater, a second EV, an induction range swap — re-opens the calc and the customer pays for the panel anyway, plus a now-stranded EMS controller.

**Recommendation: upgrade the service.** Go to **200 A**, not the 125 A minimum. The labor, permit, meter-base swap, and utility coordination are nearly identical between 125 A and 200 A; the incremental gear cost is small, and 200 A leaves genuine headroom for the electrification loads this customer is clearly heading toward. Quote the EVEMS as an *optional* add-on for demand-charge management, not as a code workaround.

**What to tell the customer, plainly:** "We ran the load calc both ways. Your 100 A service doesn't have room for a 48-amp charger — you're at about 93 amps before we plug the car in. A load manager would technically squeak you under the limit only if we throttle the charger down to 16 amps, which would roughly triple your charging time and leave you no room for anything else you add later. I'd rather sell you the 200 A upgrade once than sell you a workaround you'll outgrow next year."

### Apprentice-mistake checklist
- ✅ **Heating/AC kept OUT of the optional-method pool and added at 100% afterward.** (This is the line that flipped the answer from "100 A is fine" to "100 A fails.")
- ✅ A/C counted, furnace blower counted separately in the general pool — no double-count of heat vs. cool.
- ✅ Range taken from the range demand table at 8,000 VA (not 10,000 nameplate).
- ✅ Dryer taken at 5,000 VA per the dryer demand table (single unit).
- ✅ Small-appliance + laundry rolled into the general pool before the demand factor.
- ✅ EMS variant carried through to amps rather than assumed; reported as a failure, not a save.
- ⚠ No motor 25% adder — the largest motor (A/C compressor) is already at 100%; revisit if this were a commercial feeder.
- ⚠ Three Article 120 numeric factors remain **UNVERIFIED** (see gate above). Confirm before signing.

### Sign-off
Prepared by: {preparer name}. Reviewed by: ___________ (master electrician or PE). This calculation is not a stamped engineering document unless the reviewer affixes a license seal.

---

_Disclaimer: This calculation assumes the nameplate data provided is accurate. Field-verify nameplates before ordering gear. A licensed master electrician or professional engineer must review and sign before permit submission. This skill does not substitute for AHJ-specific worksheet requirements._
